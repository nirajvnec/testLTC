using System.Collections.Generic;
using System.IO;
using OfficeOpenXml;
using MaRSRiskServerGateway.Core.Interfaces;

namespace MaRSRiskServerGateway.Core.Services
{
    public class ExcelReader : IExcelReader
    {
        private readonly ExcelPackage _excelPackage;

        public ExcelReader(string filePath)
        {
            ExcelPackage.LicenseContext = LicenseContext.NonCommercial;
            var fileInfo = new FileInfo(filePath);
            _excelPackage = new ExcelPackage(fileInfo);
        }

        public List<string> GetHeaders(string sheetName, string startCell, string endCell)
        {
            var worksheet = _excelPackage.Workbook.Worksheets[sheetName];
            var headers = new List<string>();

            int startColumn = ExcelCellAddress.GetColumn(startCell);
            int startRow = ExcelCellAddress.GetRow(startCell);
            int endColumn = ExcelCellAddress.GetColumn(endCell);

            for (int col = startColumn; col <= endColumn; col++)
            {
                headers.Add(worksheet.Cells[startRow, col].Text);
            }

            return headers;
        }

        public List<List<string>> GetData(string sheetName, string startCell, string endCell)
        {
            var worksheet = _excelPackage.Workbook.Worksheets[sheetName];
            var data = new List<List<string>>();

            int startColumn = ExcelCellAddress.GetColumn(startCell);
            int startRow = ExcelCellAddress.GetRow(startCell);
            int endColumn = ExcelCellAddress.GetColumn(endCell);
            int endRow = ExcelCellAddress.GetRow(endCell);

            for (int row = startRow; row <= endRow; row++)
            {
                var rowData = new List<string>();

                for (int col = startColumn; col <= endColumn; col++)
                {
                    rowData.Add(worksheet.Cells[row, col].Text);
                }

                data.Add(rowData);
            }

            return data;
        }

        public string GetRange(string sheetName, string range)
        {
            var worksheet = _excelPackage.Workbook.Worksheets[sheetName];
            var cells = worksheet.Cells[range];
            return cells.Text;
        }

        public List<string> GetSheetNames()
        {
            var sheetNames = new List<string>();

            foreach (var worksheet in _excelPackage.Workbook.Worksheets)
            {
                sheetNames.Add(worksheet.Name);
            }

            return sheetNames;
        }
    }
}
