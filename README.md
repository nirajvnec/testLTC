using ClosedXML.Excel;
using System.Collections.Generic;

namespace MaRSRiskServerGateway.Core.Interfaces
{
    public interface IExcelProcessor
    {
        void ProcessExcelData(string filePath);
        void ProcessSensitivitiesAndVaR(List<string> headers, List<List<string>> data, IXLRange varRange);
        double CalculateAggregatedVaR(List<List<string>> data);
        void UpdateVaRRange(IXLRange varRange, double aggregatedVaR);
    }
}



using ClosedXML.Excel;
using MaRSRiskServerGateway.Core.Interfaces;
using System;
using System.Collections.Generic;
using System.Linq;

namespace MaRSRiskServerGateway.Core.Services
{
    public class ExcelProcessor : IExcelProcessor
    {
        private readonly IExcelReader _excelReader;

        public ExcelProcessor(IExcelReader excelReader)
        {
            _excelReader = excelReader;
        }

        public void ProcessExcelData(string filePath)
        {
            (List<string> headers, List<List<string>> data, IXLRange varRange) = _excelReader.ReadExcelData(filePath);

            if (headers.Count > 0 && data.Count > 0 && varRange != null)
            {
                ProcessSensitivitiesAndVaR(headers, data, varRange);
            }
            else
            {
                Console.WriteLine("Failed to read data from the Excel file.");
            }
        }

        public void ProcessSensitivitiesAndVaR(List<string> headers, List<List<string>> data, IXLRange varRange)
        {
            double aggregatedVaR = CalculateAggregatedVaR(data);
            UpdateVaRRange(varRange, aggregatedVaR);

            Console.WriteLine($"Processed {data.Count} rows of sensitivity data with {headers.Count} columns");
            Console.WriteLine($"Aggregated VaR: {aggregatedVaR}");
        }

        public double CalculateAggregatedVaR(List<List<string>> data)
        {
            // This is a placeholder implementation. Replace with your actual VaR calculation logic.
            double sum = 0;
            foreach (var row in data)
            {
                foreach (var cell in row)
                {
                    if (double.TryParse(cell, out double value))
                    {
                        sum += Math.Abs(value); // Using absolute values for demonstration
                    }
                }
            }
            return sum / data.Count; // Simple average for demonstration
        }

        public void UpdateVaRRange(IXLRange varRange, double aggregatedVaR)
        {
            // This is a placeholder implementation. Replace with your actual logic for updating the VaR range.
            varRange.FirstCell().Value = aggregatedVaR;

            // Assuming you want to fill the entire range with the same value
            foreach (var cell in varRange.Cells())
            {
                cell.Value = aggregatedVaR;
            }

            // Save the changes to the workbook
            varRange.Worksheet.Workbook.Save();
        }
    }
}
