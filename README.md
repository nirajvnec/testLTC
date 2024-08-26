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
            var (headers, data, varRange) = _excelReader.ReadExcelData(filePath);

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
            // This is a placeholder implementation. You should replace this with your actual VaR calculation logic.
            // For demonstration purposes, let's assume the VaR is the sum of all numeric values in the data.
            double sum = 0;
            foreach (var row in data)
            {
                foreach (var cell in row)
                {
                    if (double.TryParse(cell, out double value))
                    {
                        sum += value;
                    }
                }
            }
            return sum;
        }

        public void UpdateVaRRange(IXLRange varRange, double aggregatedVaR)
        {
            // This is a placeholder implementation. You should replace this with your actual logic for updating the VaR range.
            // For demonstration purposes, let's just write the aggregated VaR to the first cell of the range.
            varRange.FirstCell().Value = aggregatedVaR;

            // Save the changes to the workbook
            varRange.Worksheet.Workbook.Save();
        }
    }
}
