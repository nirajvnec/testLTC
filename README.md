using System.Collections.Generic;
using ClosedXML.Excel;

namespace MaRSRiskServerGateway.Core.Interfaces
{
    public interface IExcelReader
    {
        List<string> GetHeaders(string sheetName, string startCell, string endCell);
        List<List<string>> GetData(string sheetName, string startCell, string endCell);
        IXLRange GetRange(string sheetName, string range);
        List<string> GetSheetNames();
    }
}


using System.Collections.Generic;
using System.Linq;
using ClosedXML.Excel;
using MaRSRiskServerGateway.Core.Interfaces;

namespace MaRSRiskServerGateway.Core.Services
{
    public class ExcelReader : IExcelReader
    {
        private readonly IXLWorkbook _workbook;

        public ExcelReader(IXLWorkbook workbook)
        {
            _workbook = workbook;
        }

        public List<string> GetHeaders(string sheetName, string startCell, string endCell)
        {
            var sheet = _workbook.Worksheet(sheetName);
            var headers = new List<string>();
            for (char col = startCell[0]; col <= endCell[0]; col++)
            {
                headers.Add(sheet.Cell($"{col}{startCell.Substring(1)}").GetValue<string>());
            }
            return headers;
        }

        public List<List<string>> GetData(string sheetName, string startCell, string endCell)
        {
            var sheet = _workbook.Worksheet(sheetName);
            var data = new List<List<string>>();
            for (int row = int.Parse(startCell.Substring(1)); row <= int.Parse(endCell.Substring(1)); row++)
            {
                var rowData = new List<string>();
                for (char col = startCell[0]; col <= endCell[0]; col++)
                {
                    rowData.Add(sheet.Cell($"{col}{row}").GetValue<string>());
                }
                data.Add(rowData);
            }
            return data;
        }

        public IXLRange GetRange(string sheetName, string range)
        {
            var sheet = _workbook.Worksheet(sheetName);
            return sheet.Range(range);
        }

        public List<string> GetSheetNames()
        {
            return _workbook.Worksheets.Select(ws => ws.Name).ToList();
        }
    }
}
