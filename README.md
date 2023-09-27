using System;
using System.Collections.Generic;
using System.Linq;
using System.Text;

public static class StringExtensions
{
    public static string InsertSpaceAfterRead(this string input)
    {
        if (string.IsNullOrEmpty(input))
            return input;

        if (input.StartsWith("READ"))
        {
            return "READ " + input.Substring(4);
        }
        return input;
    }
}

public static class ListExtensions
{
    public static List<string> TransformList(this List<string> list)
    {
        return list.Select(str => str.InsertSpaceAfterRead()).ToList();
    }
}

public class Program
{
    public static void Main()
    {
        var strings = new List<string> {"READRUN", "READONLY", "READWRITE"};
        
        var transformedStrings = strings.TransformList();
        
        foreach(var str in transformedStrings)
        {
            Console.WriteLine(str);
        }
    }
}
