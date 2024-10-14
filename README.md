public static class StringExtensions
{
    public static string ExtractPid(this string input)
    {
        string pattern = @"\((.*?)\)";
        Match match = Regex.Match(input, pattern);
        return match.Success ? match.Groups[1].Value : string.Empty;
    }
}


string input = "Process (12345) completed";
string pid = input.ExtractPid();

Console.WriteLine(pid);  // Output: 12345
