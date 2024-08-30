using System;
using System.Reflection;

class Program
{
    static void Main(string[] args)
    {
        // 1. Load the DLL
        string dllPath = @"path\to\your\dll\file.dll";
        Assembly assembly = Assembly.LoadFrom(dllPath);

        // 2. Get all types from the assembly
        Type[] types = assembly.GetTypes();

        // 3. Iterate through each type and its methods
        foreach (Type type in types)
        {
            Console.WriteLine($"Class: {type.FullName}");

            // Get all methods of the type
            MethodInfo[] methods = type.GetMethods(BindingFlags.Public | BindingFlags.NonPublic | BindingFlags.Instance | BindingFlags.Static);

            foreach (MethodInfo method in methods)
            {
                Console.WriteLine($"  Method: {method.Name}");
            }

            Console.WriteLine();
        }
    }
}
