using System;
using System.Linq;
using System.Text;

public static class StringExtensions
{
    public static string InsertSpaceBeforeUppercase(this string input)
    {
        if (string.IsNullOrEmpty(input))
            return input;

        StringBuilder result = new StringBuilder(input.Length * 2);
        result.Append(input[0]);

        for (int i = 1; i < input.Length; i++)
        {
            if (char.IsUpper(input[i]) && (i != 0 && !char.IsUpper(input[i - 1]) || (i < input.Length - 1 && !char.IsUpper(input[i + 1]))))
                result.Append(' ');

            result.Append(input[i]);
        }

        return result.ToString();
    }

    using System.Collections.Generic;
using System.Linq;

public static class ListExtensions
{
    public static List<string> TransformList(this List<string> list)
    {
        return list.Select(str => str.InsertSpaceBeforeUppercase()).ToList();
    }
}

}

public class Program
{
    public static void Main()
    {
        var strings = new[] {"READRUN", "READONLY", "READWRITE"};
        
        foreach(var str in strings)
        {
            Console.WriteLine(str.InsertSpaceBeforeUppercase());
        }
    }
}
