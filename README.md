=UseCarProcessor(A4:B4, A5:B7)



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
