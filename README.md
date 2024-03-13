using System;
using System.Linq;
using System.Net.Http;
using System.Threading.Tasks;

class Program
{
    static async Task Main(string[] args)
    {
        string[] urls = {
            "https://jsonplaceholder.typicode.com/posts/1",
            "https://api.github.com/users/octocat",
            "https://dog.ceo/api/breeds/image/random",
            "https://api.coindesk.com/v1/bpi/currentprice.json",
            "https://api.chucknorris.io/jokes/random",
            "https://api.openweathermap.org/data/2.5/weather?q=London&appid=YOUR_API_KEY",
            "https://api.nasa.gov/planetary/apod?api_key=YOUR_API_KEY",
            "https://api.themoviedb.org/3/movie/popular?api_key=YOUR_API_KEY",
            "https://api.tvmaze.com/shows/1",
            "https://api.exchangerate-api.com/v4/latest/USD"
        };

        var httpClient = new HttpClient();

        var tasks = urls.AsParallel()
            .Select(async url =>
            {
                try
                {
                    var response = await httpClient.GetAsync(url);

                    if (response.IsSuccessStatusCode)
                    {
                        var content = await response.Content.ReadAsStringAsync();
                        return $"URL: {url}\nResponse: {content}\n";
                    }
                    else
                    {
                        return $"Request failed for URL: {url}\nStatus Code: {response.StatusCode}\n";
                    }
                }
                catch (Exception ex)
                {
                    return $"Exception occurred for URL: {url}\nMessage: {ex.Message}\n";
                }
            });

        var results = await Task.WhenAll(tasks);

        foreach (var result in results)
        {
            Console.WriteLine(result);
        }
    }
}
