using System.Collections.Generic;
using System.Linq;
using Microsoft.Office.Interop.Excel;
using MaRSRiskServerGateway.Core.Interfaces;

namespace MaRSRiskServerGateway.Core.Services
{
    public class ExcelReader : IExcelReader
    {
        private readonly Application _excelApp;
        private readonly Workbook _workbook;

        public ExcelReader(string filePath)
        {
            _excelApp = new Application();
            _workbook = _excelApp.Workbooks.Open(filePath);
        }

        public List<string> GetHeaders(string sheetName, string startCell, string endCell)
        {
            var sheet = GetWorksheet(sheetName);
            var headers = new List<string>();

            for (char col = startCell[0]; col <= endCell[0]; col++)
            {
                var cellAddress = $"{col}{startCell.Substring(1)}";
                Range cell = sheet.Range[cellAddress];
                headers.Add(cell.Value2?.ToString() ?? string.Empty);
            }

            return headers;
        }

        public List<List<string>> GetData(string sheetName, string startCell, string endCell)
        {
            var sheet = GetWorksheet(sheetName);
            var data = new List<List<string>>();

            for (int row = int.Parse(startCell.Substring(1)); row <= int.Parse(endCell.Substring(1)); row++)
            {
                var rowData = new List<string>();

                for (char col = startCell[0]; col <= endCell[0]; col++)
                {
                    var cellAddress = $"{col}{row}";
                    Range cell = sheet.Range[cellAddress];
                    rowData.Add(cell.Value2?.ToString() ?? string.Empty);
                }

                data.Add(rowData);
            }

            return data;
        }

        public Range GetRange(string sheetName, string range)
        {
            var sheet = GetWorksheet(sheetName);
            return sheet.Range[range];
        }

        public List<string> GetSheetNames()
        {
            return _workbook.Sheets.Cast<Worksheet>().Select(ws => ws.Name).ToList();
        }

        private Worksheet GetWorksheet(string sheetName)
        {
            return _workbook.Sheets[sheetName] as Worksheet;
        }

        // Cleanup method to release Excel COM objects and close the workbook
        public void Close()
        {
            _workbook.Close(false);
            _excelApp.Quit();

            // Release COM objects to avoid memory leaks
            System.Runtime.InteropServices.Marshal.ReleaseComObject(_workbook);
            System.Runtime.InteropServices.Marshal.ReleaseComObject(_excelApp);
        }
    }
}
