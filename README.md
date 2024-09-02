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
