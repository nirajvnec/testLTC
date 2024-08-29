
Sub TestCar()
    Dim myCar As Object
    Set myCar = CreateObject("MyCarLibrary.Car")

    ' Check if the object was created successfully
    If Not myCar Is Nothing Then
        MsgBox "Number of wheels: " & myCar.GetNumberOfWheel
    Else
        MsgBox "Failed to create MyCarLibrary.Car object"
    End If
End Sub



Avish/Niraj:
Proof of concept (POC) for consuming a C# DLL within an XLA file or through VBA code. The objective is to maintain the same method names as those used in the existing C++ code to assess the feasibility of integrating the C# DLL without requiring modifications to the VBA code.

Priyanka:
Exploring the deployment process for a C# DLL on user machines. This includes investigating possible solutions, such as utilizing WIX scripts to determine the feasibility of registering the assembly or DLL on user machines.

Jaisy:
Kindly schedule a session to review the C++ COM API currently used by the MaRSRiskServer Excel Add-in.
