using System;
using Microsoft.Office.Interop.Excel;
using CreditSuisseRiskServer;

public class RiskCalculator
{
    // Constants (assuming these were defined elsewhere in the original VBA code)
    private const string M_STRID_SENSTYPE_LEGACY = "SENSTYPE_LEGACY";
    private const string M_STRID_SENSTYPE = "SENSTYPE";
    private const string M_STRID_SENSVALUE_LEGACY = "SENSVALUE_LEGACY";
    private const string M_STRID_SENSVALUE = "SENSVALUE";
    private const string M_STRID_SENSSURFACE = "SENSSURFACE";
    private const string M_STRID_SENSPLDSTRIP = "SENSPLDSTRIP";

    private void ConstructParameterObjects(
        Range rngHeadingRow,
        Range rngRepriceableSpec,
        Range rngContext,
        out MarsRiskRepriceables objRepriceableListOut,
        out MarsRiskContext objVARContextOut)
    {
        objRepriceableListOut = ConstructSensDetailList(rngHeadingRow, rngRepriceableSpec);
        objVARContextOut = ConstructVARContext(rngContext);
    }

    private MarsRiskRepriceables ConstructSensDetailList(Range rngHeadingRow, Range rngRepriceableSpec)
    {
        var objList = new MarsRiskRepriceables();

        for (int lngSourceRowIndex = 1; lngSourceRowIndex <= rngRepriceableSpec.Rows.Count; lngSourceRowIndex++)
        {
            var objSensDetail = ConstructSensDetail(rngHeadingRow, rngRepriceableSpec, lngSourceRowIndex);
            objList.Add(objSensDetail);
        }

        return objList;
    }

    private MarsRiskRepriceable ConstructSensDetail(Range rngHeadingRow, Range rngRepriceableSpec, int lngSourceRowIndex)
    {
        var objSensDetail = new MarsRiskRepriceable();
        string strSurfaceRef = ""; // No surface found yet.
        string strStripRef = ""; // No strip found yet.

        int columnCount = rngHeadingRow.Columns.Count;

        for (int lngColumn = 1; lngColumn <= columnCount; lngColumn++)
        {
            if (rngRepriceableSpec.Cells[lngSourceRowIndex, lngColumn].Value2 != null)
            {
                bool blnAddAttribute = true;
                string strAttributeNameGiven = rngHeadingRow.Cells[1, lngColumn].Value2?.ToString() ?? string.Empty;
                string strAttributeNameUse = strAttributeNameGiven;

                switch (strAttributeNameGiven.ToUpper())
                {
                    case M_STRID_SENSTYPE_LEGACY:
                        strAttributeNameUse = M_STRID_SENSTYPE;
                        break;
                    case M_STRID_SENSVALUE_LEGACY:
                        strAttributeNameUse = M_STRID_SENSVALUE;
                        break;
                    case M_STRID_SENSSURFACE:
                        if (string.IsNullOrEmpty(strSurfaceRef))
                        {
                            strSurfaceRef = rngRepriceableSpec.Cells[lngSourceRowIndex, lngColumn].Value2?.ToString() ?? string.Empty;
                            blnAddAttribute = string.IsNullOrWhiteSpace(strSurfaceRef);
                        }
                        break;
                    case M_STRID_SENSPLDSTRIP:
                        if (string.IsNullOrEmpty(strStripRef))
                        {
                            strStripRef = rngRepriceableSpec.Cells[lngSourceRowIndex, lngColumn].Value2?.ToString() ?? string.Empty;
                            blnAddAttribute = string.IsNullOrWhiteSpace(strStripRef);
                        }
                        break;
                }

                if (blnAddAttribute)
                {
                    objSensDetail.Add(strAttributeNameUse, rngRepriceableSpec.Cells[lngSourceRowIndex, lngColumn].Value2);
                }
            }
        }

        if (!string.IsNullOrEmpty(strSurfaceRef))
        {
            string strDecodedAddress = DecodeRef(strSurfaceRef);
            var rngSurface = Globals.ThisAddIn.Application.Range[strDecodedAddress];
            SetSensDetailSurface(rngSurface, objSensDetail);
        }

        if (!string.IsNullOrEmpty(strStripRef))
        {
            string strDecodedAddress = DecodeRef(strStripRef);
            var rngStrip = Globals.ThisAddIn.Application.Range[strDecodedAddress];
            SetSensDetailPLDStrip(rngStrip, objSensDetail);
        }

        return objSensDetail;
    }

    private void SetSensDetailSurface(Range rngSurface, MarsRiskRepriceable objRepriceable)
    {
        // Surface is described in Range: First row - axis labels, each subsequent row - point on surface.
        int lngRowCount = rngSurface.Rows.Count;
        int lngColumnCount = rngSurface.Columns.Count;

        if ((lngRowCount >= 2) && (lngColumnCount >= 1))
        {
            string[] astrLabel = new string[lngColumnCount];
            double[,] adblPoint = new double[lngRowCount - 1, lngColumnCount];

            for (int lngColumn = 1; lngColumn <= lngColumnCount; lngColumn++)
            {
                astrLabel[lngColumn - 1] = rngSurface.Cells[1, lngColumn].Value2?.ToString() ?? string.Empty;
            }

            for (int lngRow = 2; lngRow <= lngRowCount; lngRow++)
            {
                for (int lngColumn = 1; lngColumn <= lngColumnCount; lngColumn++)
                {
                    adblPoint[lngRow - 2, lngColumn - 1] = Convert.ToDouble(rngSurface.Cells[lngRow, lngColumn].Value2);
                }
            }

            objRepriceable.SetSurface(adblPoint, astrLabel);
        }
    }

    private void SetSensDetailPLDStrip(Range rngStrip, MarsRiskRepriceable objRepriceable)
    {
        // Strip is described in Range: First column - dates, second column P&L value.
        // Note: No labels.

        int lngRowCount = rngStrip.Rows.Count;
        int lngColumnCount = rngStrip.Columns.Count;

        if (lngColumnCount != 2)
        {
            throw new InvalidOperationException("Input P&L strip must have two columns: date and P/L.");
        }

        if (lngRowCount >= 1)
        {
            DateTime[] adteCOBDate = new DateTime[lngRowCount];
            double[] adblPL = new double[lngRowCount];

            for (int lngRow = 1; lngRow <= lngRowCount; lngRow++)
            {
                adteCOBDate[lngRow - 1] = Convert.ToDateTime(rngStrip.Cells[lngRow, 1].Value2);
                adblPL[lngRow - 1] = Convert.ToDouble(rngStrip.Cells[lngRow, 2].Value2);
            }

            objRepriceable.SetInputPLStrip(adteCOBDate, adblPL);
        }
    }

    private MarsRiskContext ConstructVARContext(Range rngVARContext)
    {
        MarsRiskContext objVARContext = new MarsRiskContext();
        string strValue;

        for (int lngRow = 1; lngRow <= rngVARContext.Rows.Count; lngRow++)
        {
            if (rngVARContext.Cells[lngRow, 1].Value2 != null)
            {
                var cellValue = rngVARContext.Cells[lngRow, 2].Value2;
                if (cellValue is DateTime)
                {
                    strValue = ((DateTime)cellValue).ToString("yyyyMMdd");
                }
                else if (cellValue is bool)
                {
                    strValue = ((bool)cellValue).ToString().ToUpper();
                }
                else
                {
                    strValue = cellValue?.ToString() ?? string.Empty;
                }

                objVARContext.Add(
                    Parameter_Name: rngVARContext.Cells[lngRow, 1].Value2?.ToString() ?? string.Empty,
                    Parameter_Value: strValue
                );

                // Uncomment the following line if you want to keep the debug print
                // Console.WriteLine($"{rngVARContext.Cells[lngRow, 1].Value2}={strValue}");
            }
        }

        return objVARContext;
    }

    private string DecodeRef(string reference)
    {
        // Implementation needed
        throw new NotImplementedException();
    }
}
