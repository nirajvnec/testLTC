=TestCar()


Function TestCar() As String
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
