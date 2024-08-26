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
            var startAddress = new ExcelAddress(startCell);
            var endAddress = new ExcelAddress(endCell);

            for (int col = startAddress.Start.Column; col <= endAddress.End.Column; col++)
            {
                headers.Add(worksheet.Cells[startAddress.Start.Row, col].Text);
            }
            return headers;
        }

        public List<List<string>> GetData(string sheetName, string startCell, string endCell)
        {
            var worksheet = _excelPackage.Workbook.Worksheets[sheetName];
            var data = new List<List<string>>();
            var range = worksheet.Cells[startCell + ":" + endCell];

            for (int row = range.Start.Row; row <= range.End.Row; row++)
            {
                var rowData = new List<string>();
                for (int col = range.Start.Column; col <= range.End.Column; col++)
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
            return worksheet.Cells[range].Text;
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

        public void Close()
        {
            _excelPackage.Dispose();
        }
    }
}
