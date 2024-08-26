public interface IExcelReader
{
    List<string> GetSensitivitiesHeader(string filePath);
    List<string> GetSensitivitiesData(string filePath);
    List<string> GetVarData(string filePath);
}


using OfficeOpenXml;
using System;
using System.Collections.Generic;
using System.IO;
using System.Linq;

public class EPPlusExcelReader : IExcelReader
{
    public EPPlusExcelReader()
    {
        // Set the EPPlus license context
        ExcelPackage.LicenseContext = LicenseContext.NonCommercial;
    }

    public List<string> GetSensitivitiesHeader(string filePath)
    {
        using (var package = new ExcelPackage(new FileInfo(filePath)))
        {
            var worksheet = package.Workbook.Worksheets["Sensitivities"];
            var range = worksheet.Cells["A10:W10"];
            return range.Select(cell => cell.Text).ToList();
        }
    }

    public List<string> GetSensitivitiesData(string filePath)
    {
        using (var package = new ExcelPackage(new FileInfo(filePath)))
        {
            var worksheet = package.Workbook.Worksheets["Sensitivities"];
            var range = worksheet.Cells["A11:X70"];
            return range.Select(cell => cell.Text).ToList();
        }
    }

    public List<string> GetVarData(string filePath)
    {
        using (var package = new ExcelPackage(new FileInfo(filePath)))
        {
            var worksheet = package.Workbook.Worksheets["VaR"];
            var range = worksheet.Cells["B20:C35"];
            return range.Select(cell => cell.Text).ToList();
        }
    }
}



    private async Task GetMaRSAggregatedVARAsync()
    {
        try
        {
            _sensitivitiesHeader = _excelReader.GetSensitivitiesHeader(SelectedFilePath);
            _sensitivitiesData = _excelReader.GetSensitivitiesData(SelectedFilePath);
            _varData = _excelReader.GetVarData(SelectedFilePath);

            // Process the data as needed
            // ...

            StatusMessage = "MaRS Aggregated VAR calculation completed.";
        }
        catch (Exception ex)
        {
            StatusMessage = $"Error calculating MaRS Aggregated VAR: {ex.Message}";
        }
    
