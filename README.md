using System;
using System.Runtime.InteropServices;

class Program
{
    private const string DllPath = @"C:\Users\nkuma152\aggregator.dll";

    [DllImport(DllPath, CallingConvention = CallingConvention.Cdecl)]
    static extern int GetClassCount();

    [DllImport(DllPath, CallingConvention = CallingConvention.Cdecl)]
    static extern IntPtr GetClassName(int index);

    [DllImport(DllPath, CallingConvention = CallingConvention.Cdecl)]
    static extern int GetMethodCount(int classIndex);

    [DllImport(DllPath, CallingConvention = CallingConvention.Cdecl)]
    static extern IntPtr GetMethodName(int classIndex, int methodIndex);

    static string PtrToString(IntPtr ptr) => Marshal.PtrToStringAnsi(ptr);

    static void Main(string[] args)
    {
        try
        {
            int classCount = GetClassCount();
            for (int i = 0; i < classCount; i++)
            {
                string className = PtrToString(GetClassName(i));
                Console.WriteLine($"Class: {className}");
                int methodCount = GetMethodCount(i);
                for (int j = 0; j < methodCount; j++)
                {
                    string methodName = PtrToString(GetMethodName(i, j));
                    Console.WriteLine($"  Method: {methodName}");
                }
            }
        }
        catch (DllNotFoundException)
        {
            Console.WriteLine($"Error: Could not find the DLL at {DllPath}");
        }
        catch (EntryPointNotFoundException ex)
        {
            Console.WriteLine($"Error: Could not find a required function in the DLL. Details: {ex.Message}");
        }
        catch (Exception ex)
        {
            Console.WriteLine($"An unexpected error occurred: {ex.Message}");
        }
    }
}
