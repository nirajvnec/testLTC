using ClosedXML.Excel;
using System.Collections.Generic;

namespace MaRSRiskServerGateway.Core.Interfaces
{
    public interface IExcelReader
    {
        (List<string> headers, List<List<string>> data, IXLRange varRange) ReadExcelData(string filePath);
        List<string> GetHeaders(IXLWorksheet sheet, string startCell, string endCell);
        List<List<string>> GetData(IXLWorksheet sheet, string startCell, string endCell);
        IXLRange GetRange(IXLWorksheet sheet, string range);
    }
}
