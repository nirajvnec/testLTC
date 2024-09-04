
using System;
using System.Collections.Generic;

public class ExcelDataProcessor
{
    public static List<List<string>> GetFormattedData(
        string[,] headerRange,
        string[,] dataRange,
        string[,] additionalDataRange)
    {
        List<List<string>> formattedData = new List<List<string>>();

        // Process header
        List<string> header = new List<string>();
        for (int i = 0; i < headerRange.GetLength(1); i++)
        {
            header.Add(headerRange[0, i]);
        }
        formattedData.Add(header);

        // Process main data
        for (int row = 0; row < dataRange.GetLength(0); row++)
        {
            List<string> rowData = new List<string>();
            for (int col = 0; col < dataRange.GetLength(1); col++)
            {
                rowData.Add(dataRange[row, col]);
            }
            formattedData.Add(rowData);
        }

        // Process additional data
        Dictionary<string, string> additionalDataDict = new Dictionary<string, string>();
        for (int i = 0; i < additionalDataRange.GetLength(0); i++)
        {
            string key = additionalDataRange[i, 0];
            string value = additionalDataRange[i, 1];
            additionalDataDict[key] = value;
        }

        // Apply additional data to formatted data (assuming it modifies existing data)
        for (int i = 1; i < formattedData.Count; i++) // Start from 1 to skip header
        {
            string key = formattedData[i][0]; // Assume first column is the key
            if (additionalDataDict.ContainsKey(key))
            {
                formattedData[i].Add(additionalDataDict[key]);
            }
        }

        return formattedData;
    }
}







ProtoAttributor extension in Visual Studio

Grpc.Net.Client


using Grpc.Net.Client;
using Google.Protobuf.WellKnownTypes;
using YourNamespace; // The namespace where your generated classes are

// Create a gRPC channel
using var channel = GrpcChannel.ForAddress("https://your-server-address");

// Create a client
var client = new BridgeG2C.BridgeG2CClient(channel);

// Create and populate the input message
var input = new CalcInputRepriceables
{
    Repriceables = new Repriceables
    {
        Sequence = 
        {
            new Repriceable
            {
                TheType = "SomeType",
                TheExprCcy = "USD",
                TheValue = 100.0,
                TheSurface = new Surface
                {
                    Version = 1,
                    Label = new Strings { Sequence = { "Label1", "Label2" } },
                    Origin = new Doubles { Sequence = { 0.0, 0.0 } },
                    Points = new RevalPoints
                    {
                        Sequence = 
                        {
                            new RevalPoint
                            {
                                CoOrd = new Doubles { Sequence = { 1.0, 1.0 } },
                                Value = 10.0
                            }
                        }
                    }
                },
                TheStrip = new DatedPLStrip
                {
                    Sequence = 
                    {
                        new DatedPL
                        {
                            TheDate = new Date { Day = 1, Month = 1, Year = 2024 },
                            TheValue = 1000.0
                        }
                    }
                },
                TheAttributes = new RepriceableAttributes
                {
                    Sequence = 
                    {
                        new RepriceableAttribute { Name = "Attr1", Value = "Value1" }
                    }
                }
            }
        }
    },
    Ctx = new VaRContext
    {
        Sequence = 
        {
            new KeyValuePair { Key = "ContextKey", Value = "ContextValue" }
        }
    }
};

try
{
    // Call the gRPC method
    CalcOutputVaRNumber response = await client.aggregatedVaRAsync(input);

    // Process the response
    Console.WriteLine($"VaR: {response.Out.Val}");
    foreach (var record in response.Log.Sequence)
    {
        foreach (var kvp in record.Sequence)
        {
            Console.WriteLine($"{kvp.Key}: {kvp.Value}");
        }
    }
}
catch (RpcException e)
{
    Console.WriteLine($"RPC failed: {e.Status}");
}


using System;
using Microsoft.Office.Interop.Excel;
using System.Runtime.InteropServices;

namespace MyCarLibrary
{
    [ComVisible(true)]
    [Guid("Another-GUID-Here")]  // Replace with an actual GUID
    [ClassInterface(ClassInterfaceType.None)]
    public class Car : ICar
    {
        public string GetFormattedData(Range headerRange, Range valueRange)
        {
            string result = "";
            int rowCount = valueRange.Rows.Count;

            for (int i = 1; i <= rowCount; i++)
            {
                string header1 = headerRange.Cells[1, 1].Value.ToString();
                string value1 = valueRange.Cells[i, 1].Value.ToString();
                string header2 = headerRange.Cells[1, 2].Value.ToString();
                string value2 = valueRange.Cells[i, 2].Value.ToString();

                result += $"{header1}: {value1}, {header2}: {value2}\n";
            }

            return result;
        }

        // Implement other methods of ICar here if necessary
    }
}



Public m_objCar As MyCarLibrary.Car

Sub UseCarProcessor()
    ' Create an instance of the Car object
    Set m_objCar = New MyCarLibrary.Car
    
    Dim result As String

    ' Assuming headerRange and valueRange are already defined
    result = m_objCar.GetFormattedData(Range("A4:B4"), Range("A5:B7"))

    ' Output the result (you can also return it to a cell)
    MsgBox result
End Sub





Public m_objExcelProcessor As MyCarLibrary.ExcelDataProcessor

Sub UseProcessor()
    ' Create an instance of the ExcelDataProcessor object
    Set m_objExcelProcessor = New MyCarLibrary.ExcelDataProcessor
    
    Dim result As String

    ' Assuming headerRange and valueRange are already defined
    result = m_objExcelProcessor.GetFormattedData(Range("A4:B4"), Range("A5:B7"))

    ' Output the result (you can also return it to a cell)
    MsgBox result
End Sub





Public Function GetFormattedDataFromCSharp(headerRange As Range, valueRange As Range) As String
    Dim excelProcessor As Object
    Set excelProcessor = CreateObject("MyNamespace.ExcelDataProcessor")
    
    GetFormattedDataFromCSharp = excelProcessor.GetFormattedData(headerRange, valueRange)
End Function




=TestCar()


Public Function TestCar() As String
    Dim myCar As Object
    Set myCar = CreateObject("MyCarLibrary.Car")

    ' Check if the object was created successfully
    If Not myCar Is Nothing Then
        TestCar = "Number of wheels: " & myCar.GetNumberOfWheel
    Else
        TestCar = "Failed to create MyCarLibrary.Car object"
    End If
End Function




=GetFormattedData(A4:B4, A5:B7)



Function GetFormattedData(headerRange As Range, valueRange As Range) As String
    Dim result As String
    Dim i As Integer
    
    result = ""
    
    ' Loop through each row in the valueRange
    For i = 1 To valueRange.Rows.Count
        ' Concatenate header with values
        result = result & headerRange.Cells(1, 1).Value & ": " & valueRange.Cells(i, 1).Value & ", " & _
                         headerRange.Cells(1, 2).Value & ": " & valueRange.Cells(i, 2).Value & vbCrLf
    Next i
    
    ' Return the concatenated string
    GetFormattedData = result
End Function
