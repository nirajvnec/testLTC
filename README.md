MyForm form = new MyForm();
FormHandler.SafeShow(form);

dialogResult result = FormHandler.SafeShowDialog(form);




public static class FormHandler
{
    public static void SafeShow(Form form)
    {
        try
        {
            form.Show();
        }
        catch (System.ComponentModel.Win32Exception e)
        {
            if (e.Message.Contains("Cannot create a window handle"))
            {
                GC.Collect();
                GC.WaitForPendingFinalizers();

                // Reattempt to show the form after garbage collection
                form.Show();
            }
            else
            {
                // Handle other Win32 exceptions or rethrow
                throw;
            }
        }
    }

    public static DialogResult SafeShowDialog(Form form)
    {
        try
        {
            return form.ShowDialog();
        }
        catch (System.ComponentModel.Win32Exception e)
        {
            if (e.Message.Contains("Cannot create a window handle"))
            {
                GC.Collect();
                GC.WaitForPendingFinalizers();

                // Reattempt to show the form as dialog after garbage collection
                return form.ShowDialog();
            }
            else
            {
                // Handle other Win32 exceptions or rethrow
                throw;
            }
        }
    }
}


MyWindow window = new MyWindow();
WindowHandler.SafeShow(window);

// or

bool? result = WindowHandler.SafeShowDialog(window);


using System;
using System.Windows;
using System.ComponentModel;

public static class WindowHandler
{
    public static void SafeShow(Window window)
    {
        try
        {
            window.Show();
        }
        catch (Win32Exception e)
        {
            if (e.Message.Contains("Cannot create a window handle"))
            {
                GC.Collect();
                GC.WaitForPendingFinalizers();

                // Reattempt to show the window after garbage collection
                window.Show();
            }
            else
            {
                // Handle other Win32 exceptions or rethrow
                throw;
            }
        }
    }

    public static bool? SafeShowDialog(Window window)
    {
        try
        {
            return window.ShowDialog();
        }
        catch (Win32Exception e)
        {
            if (e.Message.Contains("Cannot create a window handle"))
            {
                GC.Collect();
                GC.WaitForPendingFinalizers();

                // Reattempt to show the window as dialog after garbage collection
                return window.ShowDialog();
            }
            else
            {
                // Handle other Win32 exceptions or rethrow
                throw;
            }
        }
    }
}









using System.Windows.Forms;

public static class FolderBrowserDialogExtensions
{
    public static DialogResult ShowAndDispose(this FolderBrowserDialog dialog)
    {
        try
        {
            return dialog.ShowDialog();
        }
        finally
        {
            dialog.Dispose();
        }
    }
}





using Microsoft.Win32;

public static class DialogExtensions
{
    public static bool? ShowAndDispose(this OpenFileDialog dialog)
    {
        try
        {
            return dialog.ShowDialog();
        }
        finally
        {
            dialog.Dispose();
        }
    }

    public static bool? ShowAndDispose(this SaveFileDialog dialog)
    {
        try
        {
            return dialog.ShowDialog();
        }
        finally
        {
            dialog.Dispose();
        }
    }
}





using System.Windows;

public static class WindowExtensions
{
    public static void ShowAndDisposeWhenClosed(this Window window)
    {
        window.Closed += (sender, e) => window.Close();
        window.Show();
    }
}



public static class FormExtensions
{
    public static DialogResult ShowDialogAndDispose(this Form form)
    {
        using (form)
        {
            return form.ShowDialog();
        }
    }
}



var myForm = new MyDialogForm();
if (myForm.ShowDialogAndDispose() == DialogResult.OK)
{
    // process results
}



using System.Windows.Forms;

public static class FormExtensions
{
    public static void ShowAndDisposeWhenClosed(this Form form)
    {
        form.FormClosed += (sender, e) => form.Dispose();
        form.Show();
    }
}


YourForm formInstance = new YourForm();
formInstance.ShowAndDisposeWhenClosed();










public class DisposableBase : IDisposable
{
    private bool _disposed = false;

    protected virtual void Dispose(bool disposing)
    {
        if (!_disposed)
        {
            if (disposing)
            {
                // Dispose managed resources here.
            }

            // Dispose unmanaged resources here if any.

            _disposed = true;
        }
    }

    public void Dispose()
    {
        Dispose(true);
        GC.SuppressFinalize(this);
    }

    ~DisposableBase()
    {
        Dispose(false);
    }
}

public class DerivedClass : DisposableBase
{
    private SomeResource _resource;

    protected override void Dispose(bool disposing)
    {
        if (disposing)
        {
            // Dispose of any local managed resources specific to this derived class.
            if (_resource != null)
            {
                _resource.Dispose();
                _resource = null;
            }
        }

        base.Dispose(disposing); // Ensure base disposal is also called.
    }
}





public async Task<List<TreeViewItem>> SaveZipAndExtract(IFormFile file) 
{
    // Save the file
    string targetPath = Path.Combine(@"C:\Temp", file.FileName);
    using (var stream = new FileStream(targetPath, FileMode.Create)) 
    {
        await file.CopyToAsync(stream);
        stream.Close(); // Explicitly close the stream.
    }

    // Unzip 
    string unzipFolderPath = Path.Combine(@"C:\Users\nkuma152\Documents", Path.GetFileNameWithoutExtension(file.FileName));
    ZipFile.ExtractToDirectory(targetPath, unzipFolderPath);

    // If you have a specific XML file in the unzipped folder to read, you can do so here.
    string xmlFilePath = Directory.GetFiles(unzipFolderPath, "MetaData.xml").FirstOrDefault();

    // ... rest of your logic ...

    return someList; // replace "someList" with your actual return data.
}
