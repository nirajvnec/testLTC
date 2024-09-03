using System;
using System.Threading.Tasks;
using Grpc.Net.Client;
using Phonebook;

namespace GrpcClient
{
    class Program
    {
        static async Task Main(string[] args)
        {
            using var channel = GrpcChannel.ForAddress("http://localhost:50051");
            var client = new ContactService.ContactServiceClient(channel);

            // Add a contact
            var newContact = new Contact
            {
                Name = "John Doe",
                Email = "john.doe@example.com"
            };
            newContact.PhoneNumbers.Add("123-456-7890");
            newContact.PhoneNumbers.Add("098-765-4321");

            var addResponse = await client.AddContactAsync(newContact);
            Console.WriteLine($"Add Contact Response: {addResponse.Message}");

            // Get all contacts
            var allContacts = await client.GetAllContactsAsync(new Empty());
            Console.WriteLine("All Contacts:");
            foreach (var contact in allContacts.Contacts)
            {
                Console.WriteLine($"- {contact.Name}: {string.Join(", ", contact.PhoneNumbers)}");
            }

            // Search contacts
            var searchResponse = await client.SearchContactsAsync(new SearchRequest { Name = "John" });
            Console.WriteLine("Search Results:");
            foreach (var contact in searchResponse.Contacts)
            {
                Console.WriteLine($"- {contact.Name}: {string.Join(", ", contact.PhoneNumbers)}");
            }

            Console.WriteLine("Press any key to exit...");
            Console.ReadKey();
        }
    }
}








using System;
using System.Collections.Generic;
using System.Linq;
using System.Threading.Tasks;
using Grpc.Core;
using Phonebook;
using Microsoft.Extensions.Logging;

namespace GrpcServer
{
    public class ContactServiceImpl : ContactService.ContactServiceBase
    {
        private readonly ILogger<ContactServiceImpl> _logger;
        private readonly List<Contact> contacts = new List<Contact>();

        public ContactServiceImpl(ILogger<ContactServiceImpl> logger)
        {
            _logger = logger;
        }

        public override Task<ContactResponse> AddContact(Contact request, ServerCallContext context)
        {
            _logger.LogInformation($"Adding contact: {request.Name}");
            contacts.Add(request);
            return Task.FromResult(new ContactResponse
            {
                Success = true,
                Message = $"Contact {request.Name} added successfully."
            });
        }

        public override Task<ContactList> GetAllContacts(Empty request, ServerCallContext context)
        {
            _logger.LogInformation("Getting all contacts");
            var response = new ContactList();
            response.Contacts.AddRange(contacts);
            return Task.FromResult(response);
        }

        public override Task<ContactList> SearchContacts(SearchRequest request, ServerCallContext context)
        {
            _logger.LogInformation($"Searching contacts for: {request.Name}");
            var matchingContacts = contacts
                .Where(c => c.Name.Contains(request.Name, StringComparison.OrdinalIgnoreCase))
                .ToList();

            var response = new ContactList();
            response.Contacts.AddRange(matchingContacts);
            return Task.FromResult(response);
        }
    }

    public class Program
    {
        const int Port = 50051;

        public static void Main(string[] args)
        {
            var logger = LoggerFactory.Create(logging =>
            {
                logging.AddConsole();
                logging.SetMinimumLevel(LogLevel.Information);
            }).CreateLogger<ContactServiceImpl>();

            Server server = new Server
            {
                Services = { ContactService.BindService(new ContactServiceImpl(logger)) },
                Ports = { new ServerPort("localhost", Port, ServerCredentials.Insecure) }
            };

            server.Start();

            Console.WriteLine($"gRPC server listening on port {Port}");
            Console.WriteLine("Press any key to stop the server...");
            Console.ReadKey();

            server.ShutdownAsync().Wait();
        }
    }
}




=UseCarProcessor(A4:B4, A5:B7)

Public m_objCar As Object

Public Function UseCarProcessor(headerRange As Range, valueRange As Range) As String
    ' Create an instance of the Car object using CreateObject
    Set m_objCar = CreateObject("MyCarLibrary.Car")
    
    Dim result As String

    ' Call the GetFormattedData method with the provided ranges
    result = m_objCar.GetFormattedData(headerRange, valueRange)

    ' Return the result
    UseCarProcessor = result
End Function


=UseCarProcessor(A4:B4, A5:B7)


Public m_objCar As MyCarLibrary.Car

Public Function UseCarProcessor(headerRange As Range, valueRange As Range) As String
    ' Create an instance of the Car object
    Set m_objCar = New MyCarLibrary.Car
    
    Dim result As String

    ' Call the GetFormattedData method with the provided ranges
    result = m_objCar.GetFormattedData(headerRange, valueRange)

    ' Return the result
    UseCarProcessor = result
End Function



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
