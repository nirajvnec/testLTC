using System;
using System.Threading.Tasks;
using Grpc.Net.Client;
using Mars.Proto.RiskServer;

class Program
{
    static async Task Main(string[] args)
    {
        // Use the unsecure endpoint for this example
        using var channel = GrpcChannel.ForAddress("http://gbld9065051.eu.hedani.net:54911");
        var client = new BridgeG2C.BridgeG2CClient(channel);

        try
        {
            // Prepare the request
            var request = new ExchangeRateRequest
            {
                NumeratorCcy = "INR",
                DenominatorCcy = "GBP",
                CommonVarContextCtx = "3"
            };

            // Call the aggregatedVaR method
            var responseVaR = await client.AggregatedVaRAsync(request);
            Console.WriteLine($"AggregatedVaR response: {responseVaR.CommonVarNumberOut}");

            // Call the aggregatedPL method
            var responsePL = await client.AggregatedPLAsync(request);
            Console.WriteLine($"AggregatedPL response: {responsePL.CommonVarNumberOut}");
        }
        catch (Exception ex)
        {
            Console.WriteLine($"Error: {ex.Message}");
        }
    }
}
