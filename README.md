
using System;

public static class StringExtensions
{
    public static string ToFormattedDate(this string input)
    {
        if (DateTime.TryParse(input, out DateTime tempDate))
        {
            // Return the date in YYYYMMDD format
            return tempDate.ToString("yyyyMMdd");
        }
        else
        {
            // Return an empty string or any error message
            return string.Empty;
        }
    }
}



using System;
using System.Collections.Generic;
using Microsoft.Office.Interop.Excel;
using System.Runtime.InteropServices;

public List<KeyValuePair<string, string>> PopulateFromExcelRange(Range excelRange)
{
    List<KeyValuePair<string, string>> result = new List<KeyValuePair<string, string>>();

    try
    {
        // Assume the first column is the key and the second column is the value
        for (int row = 1; row <= excelRange.Rows.Count; row++)
        {
            string key = Convert.ToString((excelRange.Cells[row, 1] as Range).Value2);
            string value = Convert.ToString((excelRange.Cells[row, 2] as Range).Value2);

            // Add the key-value pair to the list
            result.Add(new KeyValuePair<string, string>(key, value));
        }
    }
    catch (Exception ex)
    {
        Console.WriteLine($"Error reading Excel data: {ex.Message}");
    }
    finally
    {
        // Clean up COM objects
        if (excelRange != null)
            Marshal.ReleaseComObject(excelRange);
    }

    return result;
}
```

To use this method, you would call it like this:

```csharp
// Assuming you have already opened the Excel workbook and worksheet
Worksheet worksheet = (Worksheet)workbook.Worksheets[1];
Range range = worksheet.UsedRange;

List<KeyValuePair<string, string>> myList = PopulateFromExcelRange(range);

// Don't forget to clean up COM objects when you're done
Marshal.ReleaseComObject(worksheet);
Marshal.ReleaseComObject(workbook);
Marshal.ReleaseComObject(excelApp);
```
