using System;
using System.Collections.Generic;
using System.Linq;

public static class ListExtensions
{
    public static List<string> AddSpaceBeforeCapitalLetters(this List<string> inputList)
    {
        if (inputList == null)
            throw new ArgumentNullException(nameof(inputList));

        return inputList.Select(input => System.Text.RegularExpressions.Regex.Replace(input, "([A-Z])", " $1").Trim()).ToList();
    }
}

class Program
{
    static void Main()
    {
        List<string> inputList = new List<string>
        {
            "READRUN",
            "READONLY",
            "READWRITE"
        };

        List<string> resultList = inputList.AddSpaceBeforeCapitalLetters();

        foreach (string result in resultList)
        {
            Console.WriteLine(result);
        }
    }
}
