using ClosedXML.Excel;

public class ExcelReader : IExcelReader
{
    private readonly IXLWorkbook _workbook;

    public ExcelReader(string filePath)
    {
        // Create the workbook using the provided file path
        _workbook = new XLWorkbook(filePath);
    }

    // Add methods here to read data from the workbook
}
