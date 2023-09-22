this.pictureBoxSpinner.Dock = System.Windows.Forms.DockStyle.Fill;
this.pictureBoxSpinner.SizeMode = System.Windows.Forms.PictureBoxSizeMode.StretchImage;
this.pictureBoxSpinner.Image = Image.FromFile(imagePath);
this.pictureBoxSpinner.BringToFront();
this.pictureBoxSpinner.Visible = true;
this.pictureBoxSpinner.Refresh();





string imagePath = Path.Combine(Application.StartupPath, "images", "hourglass.png");
bool fileExists = File.Exists(imagePath);
// Set a breakpoint here and inspect `fileExists`



this.pictureBoxSpinner.Image = Image.FromFile(Path.Combine(Application.StartupPath, "images", "hourglass.png"));
this.pictureBoxSpinner.Dock = System.Windows.Forms.DockStyle.Fill;

// Remove these lines:
// this.pictureBoxSpinner.Location = new System.Drawing.Point(90, 30);
// this.pictureBoxSpinner.Size = new System.Drawing.Size(100, 100);


this.labelLoadingText.Dock = System.Windows.Forms.DockStyle.Bottom;
this.labelLoadingText.TextAlign = ContentAlignment.MiddleCenter;

















public partial class LoadingForm : Form
{
    public LoadingForm()
    {
        InitializeComponent();

        pictureBoxLoading.Image = Image.FromFile(Path.Combine(Application.StartupPath, "images", "hourglass.png"));
    }
}










using System.Drawing;
using System.IO;
using System.Windows.Forms;




private void loadingTimer_Tick(object sender, EventArgs e)
{
    loadingTimer.Stop();
    
    if (_loadingForm != null && !_loadingForm.IsDisposed)
    {
        if (_loadingForm.InvokeRequired)
        {
            _loadingForm.Invoke((MethodInvoker)delegate
            {
                _loadingForm.Close();
            });
        }
        else
        {
            _loadingForm.Close();
        }
    }
}


public partial class MainForm : Form
{
    private LoadingForm _loadingForm;

    public MainForm()
    {
        InitializeComponent();
    }

    private void ShowLoadingForm()
    {
        // Initialize the loading form
        _loadingForm = new LoadingForm();

        // Start the timer
        loadingTimer.Start();

        // Show the loading form modally
        _loadingForm.ShowDialog(this);
    }

    private void loadingTimer_Tick(object sender, EventArgs e)
    {
        // Stop the timer
        loadingTimer.Stop();

        // Close the loading form
        if (_loadingForm != null && !_loadingForm.IsDisposed)
        {
            _loadingForm.Close();
        }
    }

    // Assume this is triggered when you want to start showing the loading screen
    private void buttonStartLoading_Click(object sender, EventArgs e)
    {
        ShowLoadingForm();
    }
}












public partial class LoadingForm : Form
{
    public LoadingForm()
    {
        InitializeComponent();

        // To prevent closing the form using Alt+F4 or Close button
        this.FormClosing += LoadingForm_FormClosing;
    }

    private void LoadingForm_FormClosing(object sender, FormClosingEventArgs e)
    {
        // This ensures that the form can't be closed by the user.
        if (e.CloseReason == CloseReason.UserClosing)
        {
            e.Cancel = true;
        }
    }
}






namespace YourNamespace
{
    partial class LoadingForm
    {
        /// <summary>
        /// Required designer variable.
        /// </summary>
        private System.ComponentModel.IContainer components = null;

        /// <summary>
        /// Clean up any resources being used.
        /// </summary>
        /// <param name="disposing">true if managed resources should be disposed; otherwise, false.</param>
        protected override void Dispose(bool disposing)
        {
            if (disposing && (components != null))
            {
                components.Dispose();
            }
            base.Dispose(disposing);
        }

        #region Windows Form Designer generated code

        /// <summary>
        /// Required method for Designer support - do not modify
        /// the contents of this method with the code editor.
        /// </summary>
        private void InitializeComponent()
        {
            this.pictureBoxSpinner = new System.Windows.Forms.PictureBox();
            this.labelLoadingText = new System.Windows.Forms.Label();
            ((System.ComponentModel.ISupportInitialize)(this.pictureBoxSpinner)).BeginInit();
            this.SuspendLayout();
            // 
            // pictureBoxSpinner
            // 
            this.pictureBoxSpinner.Image = global::YourNamespace.Properties.Resources.spinner; // Assuming you've added spinner.gif to your project resources
            this.pictureBoxSpinner.Location = new System.Drawing.Point(90, 30);
            this.pictureBoxSpinner.Name = "pictureBoxSpinner";
            this.pictureBoxSpinner.Size = new System.Drawing.Size(100, 100);
            this.pictureBoxSpinner.SizeMode = System.Windows.Forms.PictureBoxSizeMode.StretchImage;
            this.pictureBoxSpinner.TabIndex = 0;
            this.pictureBoxSpinner.TabStop = false;
            // 
            // labelLoadingText
            // 
            this.labelLoadingText.AutoSize = true;
            this.labelLoadingText.Location = new System.Drawing.Point(115, 140);
            this.labelLoadingText.Name = "labelLoadingText";
            this.labelLoadingText.Size = new System.Drawing.Size(50, 13);
            this.labelLoadingText.TabIndex = 1;
            this.labelLoadingText.Text = "Loading...";
            // 
            // LoadingForm
            // 
            this.AutoScaleDimensions = new System.Drawing.SizeF(6F, 13F);
            this.AutoScaleMode = System.Windows.Forms.AutoScaleMode.Font;
            this.ClientSize = new System.Drawing.Size(284, 181);
            this.Controls.Add(this.labelLoadingText);
            this.Controls.Add(this.pictureBoxSpinner);
            this.FormBorderStyle = System.Windows.Forms.FormBorderStyle.FixedDialog;
            this.MaximizeBox = false;
            this.MinimizeBox = false;
            this.Name = "LoadingForm";
            this.StartPosition = System.Windows.Forms.FormStartPosition.CenterScreen;
            this.Text = "Please wait...";
            this.TopMost = true;
            ((System.ComponentModel.ISupportInitialize)(this.pictureBoxSpinner)).EndInit();
            this.ResumeLayout(false);
            this.PerformLayout();
        }

        #endregion

        private System.Windows.Forms.PictureBox pictureBoxSpinner;
        private System.Windows.Forms.Label labelLoadingText;
    }
}




using System;
using System.Linq;
using System.Windows.Forms;

public partial class MainForm : Form
{
    public MainForm()
    {
        InitializeComponent();

        TabPage tabPage = FindControlByName(this, "query_builder_tab") as TabPage;

        if (tabPage != null)
        {
            var control = FindControlByType<CtlCalculationReportFormat>(tabPage);

            if (control != null)
            {
                // If the control exists, toggle its visibility.
                control.Visible = !control.Visible;

                string status = control.Visible ? "visible" : "hidden";
                MessageBox.Show($"The control is now {status}.");
            }
            else
            {
                MessageBox.Show("The control of the given type does not exist in the tab page.");
            }
        }
        else
        {
            MessageBox.Show("The tab page with the specified name does not exist.");
        }
    }

    private Control FindControlByName(Control parent, string controlName)
    {
        return parent.Controls.Find(controlName, true).FirstOrDefault();
    }

    private T FindControlByType<T>(Control parent) where T : Control
    {
        return parent.Controls.OfType<T>().FirstOrDefault();
    }
}



using System;
using System.Linq;
using System.Windows.Forms;

public partial class MainForm : Form
{
    public MainForm()
    {
        InitializeComponent();

        TabPage tabPage = FindControlByName(this, "query_builder_tab") as TabPage;

        if (tabPage != null)
        {
            var control = FindControlByType<CtlCalculationReportFormat>(tabPage);
            if (control == null)
            {
                var newControl = new CtlCalculationReportFormat();
                tabPage.Controls.Add(newControl);
                MessageBox.Show("Added the control to the tab page!");
            }
            else
            {
                MessageBox.Show("The control of the given type already exists in the tab page.");
            }
        }
        else
        {
            MessageBox.Show("The tab page with the specified name does not exist.");
        }
    }

    private Control FindControlByName(Control parent, string controlName)
    {
        return parent.Controls.Find(controlName, true).FirstOrDefault();
    }

    private T FindControlByType<T>(Control parent) where T : Control
    {
        return parent.Controls.OfType<T>().FirstOrDefault();
    }
}




using System;
using System.Linq;
using System.Windows.Forms;

public partial class MainForm : Form
{
    public MainForm()
    {
        InitializeComponent();
        
        // Try to get the control of the given type
        var control = GetControlByType<MyUserControl>(this);
        
        if (control != null)
        {
            if (!control.Visible)
            {
                control.Visible = true;
                MessageBox.Show("The control of the given type was not visible, but now it is!");
            }
            else
            {
                MessageBox.Show("The control of the given type is already visible.");
            }
        }
        else
        {
            MessageBox.Show("The control of the given type does not exist.");
        }
    }

    private T GetControlByType<T>(Control parent) where T : Control
    {
        return parent.Controls.OfType<T>().FirstOrDefault();
    }
}




using System;
using System.Linq;
using System.Windows.Forms;

public partial class MainForm : Form
{
    public MainForm()
    {
        InitializeComponent();
        
        // Check if the control of the given type exists
        bool controlExists = DoesControlExistByType<MyUserControl>(this);
        
        if (controlExists)
        {
            MessageBox.Show("The control of the given type exists!");
        }
        else
        {
            MessageBox.Show("The control of the given type does not exist.");
        }
    }

    private bool DoesControlExistByType<T>(Control parent) where T : Control
    {
        return parent.Controls.OfType<T>().Any();
    }
}



using System;
using System.Windows.Forms;

public partial class MainForm : Form
{
    private bool isUserControlVisible = false;
    private UserControl myUserControl;

    public MainForm()
    {
        InitializeComponent();

        CreateMyUserControl();

        toggleButton.Text = "Show UserControl";
        toggleButton.Click += ToggleButtonClick;

        myUserControl.Visible = false; // Make sure UserControl starts hidden
    }

    private void CreateMyUserControl()
    {
        myUserControl = new MyUserControl(); // Assuming MyUserControl is the type of your UserControl
        myUserControl.Visible = false;
        this.Controls.Add(myUserControl); // You might want to set its location and other properties before adding it
    }

    private void ToggleButtonClick(object sender, EventArgs e)
    {
        isUserControlVisible = !isUserControlVisible; // Toggle the state

        if (isUserControlVisible)
        {
            if (myUserControl.IsDisposed)
            {
                CreateMyUserControl(); // Recreate the UserControl if it's disposed
            }

            myUserControl.Visible = true;
            toggleButton.Text = "Hide UserControl";
        }
        else
        {
            myUserControl.Visible = false;
            myUserControl.Dispose(); // Dispose of the UserControl when it's hidden
            toggleButton.Text = "Show UserControl";
        }
    }
}



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
