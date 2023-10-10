foreach (CtlDragDropItem drag_drop_item in m_drag_drop_items)
{
    if (drag_drop_item == null)
    {
        continue; // skip to the next iteration if drag_drop_item is null
    }

    if (drag_drop_item.Header == null || current_heading == null)
    {
        continue; // skip to the next iteration if Header or current_heading is null
    }

    if ((drag_drop_item.Header.ToUpper() == current_heading.ToUpper() ||
         (current_heading == CsBreakDownHeading.HEADING_ALL + CsBreakDownHeading.HEADING_SUFFIX)) &&
        !drag_drop_item.Disabled && 
        !drag_drop_item.IsDraggedAway &&
        drag_drop_item.Header.ToString() != CsBreakDownHeading.HEADING_VARANALYSIS + CsBreakDownHeading.HEADING_SUFFIX &&
        drag_drop_item.Header.ToString() != CsBreakDownHeading.HEADING_IRCANALYSIS + CsBreakDownHeading.HEADING_SUFFIX)
    {
        drag_drop_item.Top = label_top;
        label_top += SIZE_ITEM_SPACING + drag_drop_item.Height; // assuming this is what you intended
        // item_found_in_header = true;  // uncomment and assign this variable appropriately
        drag_drop_item.Visible = true; // assuming there is a Visible property
    }
    else
    {
        // add appropriate handling for the else case
    }
}
 



private async Task AddBreakdownsToReportAsync(CtlDragDropList.DragDropItemCollection breakdownitems, CS.Mars.ReportDef.CsTableFormat table, CsReportDef.TableDimensionEnum dimension, CsReportDef report_def, CsETGlobalCache global_cache, bool checkScenarioExpression, bool isBreakdownHidden = false)
{ 
    foreach (CtlDragDropBreakdown drag_drop_breakdown in breakdownitems) 
    { 
        drag_drop_breakdown.Breakdown.IsBreakdownHidden = isBreakdownHidden ? "true" : null; 
        
        if (checkScenarioExpression && drag_drop_breakdown.Data is CsBreakdownScenarioExpression) 
        { 
            await drag_drop_breakdown.Breakdown.PopulateReportDefAsync(report_def, table, dimension, global_cache); 
        } 
        else if (!checkScenarioExpression && !(drag_drop_breakdown.Data is CsBreakdownScenarioExpression)) 
        { 
            if (drag_drop_breakdown.Breakdown.WantDisplayName) 
            { 
                if (drag_drop_breakdown.Breakdown.WantNodeNameDisplayName) 
                {
                    // I think some code is missing here, as the if statement is empty
                }
            } 
            
            drag_drop_breakdown.Breakdown.WantDisplayName = false; 
            string subreport_name = drag_drop_breakdown.Breakdown.SubreportName; 
            drag_drop_breakdown.Breakdown.SubreportName = subreport_name + "_NODE_NAME"; 
            
            await drag_drop_breakdown.Breakdown.PopulateReportDefAsync(report_def, table, dimension, global_cache); 
            
            drag_drop_breakdown.Breakdown.WantDisplayName = true; // assuming t_h_ was a typo and meant to reset WantDisplayName
            drag_drop_breakdown.Breakdown.SubreportName = subreport_name; 
        } 
        else if (drag_drop_breakdown.Data is CsBreakdownCobDate && report_def.CalculationMethod is CsCalcMethod) 
        { 
            report_def.BreakdownLevel = dimension.ToString();
            continue; 
        } 
        
        await drag_drop_breakdown.Breakdown.PopulateReportDefAsync(report_def, table, dimension, global_cache);
    } 
}




try 
{
    var context = new ApplicationContext(new FrmMain());
    Application.ThreadException += new ThreadExceptionEventHandler(Application_ThreadException);
    Application.Run(context);
} 
catch (Exception ex) 
{
    Console.WriteLine(ex.Message); // Print the exception message to the console, or handle it as needed
}





public void UpdateUI(Control new_item)
{
    if (new_item == null || label_area_panel == null)
    {
        return;
    }

    void AddControl()
    {
        label_area_panel.Controls.Add(new_item);
    }

    if (label_area_panel.InvokeRequired)
    {
        label_area_panel.BeginInvoke((MethodInvoker)delegate { AddControl(); });
    }
    else
    {
        AddControl();
    }
}



using System;
using System.Collections.Concurrent;
using System.Collections.Specialized;
using System.Linq;
using System.Threading.Tasks;
using System.Windows.Forms;

// ... Other necessary using directives and namespace, class definitions...

public class YourControlClass : Control // Replace with your actual base class
{
    private Label item_label = new Label();

    public override string Text 
    { 
        get 
        {
            if (this.InvokeRequired)
            {
                return (string)this.Invoke(new Func<string>(() => this.Text));
            }
            else
            {
                return item_label.Text;
            }
        }
        set 
        {
            if (this.InvokeRequired)
            {
                this.Invoke(new Action<string>((s) => this.Text = s), value);
            }
            else
            {
                item_label.Text = value;
            }
        }
    }
}

// ... The rest of your previous code ...






using System.Collections.Concurrent;
using System.Collections.Specialized;
using System.Linq;
using System.Threading.Tasks;

// ... Other necessary using directives and namespace, class definitions...

public void SetRiskTypeAttributes(StringCollection attributes)
{
    // Assuming this is thread-safe or making it thread-safe is needed.
    CsAttributeList positionAttributes = globalCache.AttributesDoc.GetPositionAttributes();

    CsBreakdownCollection positionAttributeBreakdowns = CsBreakdownCollection.AttributeBreakdowns(positionAttributes);
    
    // Using a ConcurrentBag to ensure thread-safety
    ConcurrentBag<CtlDragDropItem> safeListItems = new ConcurrentBag<CtlDragDropItem>(ListItems.Cast<CtlDragDropItem>());

    var tempPositionBreakdown = positionAttributeBreakdowns.Cast<CsBreakdown>()
                                                           .Select(b => b.DisplayText)
                                                           .Except(safeListItems.Select(i => i.Text.ToUpper()))
                                                           .ToArray();

    if (tempPositionBreakdown.Any())
    {
        var safePositionBreakdowns = new ConcurrentBag<CsBreakdown>(positionAttributeBreakdowns.Cast<CsBreakdown>()
                                                                                              .Where(b => tempPositionBreakdown.Contains(b.DisplayText)));

        Parallel.ForEach(safePositionBreakdowns, breakdown =>
        {
            AddDragDropBreakdownItem(breakdown); // Ensure this method is thread-safe
        });
    }
    
    CsBreakdownCollection sensitivityBreakdowns = CsBreakdownCollection.SensitivityAttributeBreakdowns(attributes);

    var tempSensitivity = sensitivityBreakdowns.Cast<CsBreakdown>()
                                               .Select(b => b.DisplayText)
                                               .Except(safeListItems.Select(i => i.DisplayText))
                                               .ToArray();

    if (tempSensitivity.Any())
    {
        var safeSensitivityBreakdowns = new ConcurrentBag<CsBreakdown>(sensitivityBreakdowns.Cast<CsBreakdown>()
                                                                                           .Where(b => tempSensitivity.Contains(b.DisplayText)));
        
        Parallel.ForEach(safeSensitivityBreakdowns, breakdown =>
        {
            AddDragDropBreakdownItem(breakdown); // Ensure this method is thread-safe
        });
    }

    // Added new section with thread-safety
    var safeAttributes = new ConcurrentBag<string>();
    Parallel.ForEach(positionAttributeBreakdowns.Cast<CsBreakdown>(), breakdown =>
    {
        var attribute = globalCache.AttributesDoc.GetAttribute(breakdown.AttributeName);
        safeAttributes.Add(attribute.AttributeName);
    });

    // Corrected this line to use the attributes parameter
    RemoveMissingAttributeBreakdowns(attributes); 
    attributes.AddRange(safeAttributes.ToArray());
}

// Make sure the methods RemoveMissingAttributeBreakdowns and AddDragDropBreakdownItem are thread-safe or are made thread-safe









public void UpdateUI(Control new_item)
{
    if (label_area_panel.InvokeRequired)
    {
        label_area_panel.BeginInvoke((MethodInvoker)delegate 
        {
            label_area_panel.Controls.Add(new_item);
        });
    }
    else
    {
        label_area_panel.Controls.Add(new_item);
    }
}




using System;
using System.Collections.Concurrent;
using System.Linq;

public class CtlDragDropBreakdown
{
    public Breakdown Breakdown { get; set; }
}

public class Breakdown
{
    public string DisplayText { get; set; }
}

public class BreakdownManager
{
    private readonly ConcurrentBag<CtlDragDropBreakdown> items = new ConcurrentBag<CtlDragDropBreakdown>();

    public bool FindBreakdownByName(string breakdownAttributeName)
    {
        var breakdown = items.FirstOrDefault(existingItem =>
            existingItem.Breakdown.DisplayText.Equals(breakdownAttributeName, StringComparison.OrdinalIgnoreCase));
        return breakdown != null;
    }

    public void AddBreakdown(CtlDragDropBreakdown breakdown)
    {
        items.Add(breakdown);
    }

    // Potentially other methods to modify and access the 'items' collection
}




private readonly ReaderWriterLockSlim rwLock = new ReaderWriterLockSlim();

public bool FindBreakdownByName(string breakdownAttributeName)
{
    this.rwLock.EnterReadLock();
    CtlDragDropBreakdown[] snapshot;
    try
    {
        snapshot = this.Cast<CtlDragDropBreakdown>().ToArray();
    }
    finally
    {
        this.rwLock.ExitReadLock();
    }

    var breakdown = snapshot.FirstOrDefault(existingItem =>
        existingItem.Breakdown.DisplayText.Equals(breakdownAttributeName, StringComparison.OrdinalIgnoreCase));
    
    return breakdown != null;
}

if (this.lbl_area_panel.InvokeRequired)
{
    this.lbl_area_panel.Invoke((MethodInvoker)delegate
    {
        this.lbl_area_panel.Controls.Add(new_item);
    });
}
else
{
    this.lbl_area_panel.Controls.Add(new_item);
}







private async Task UpdateAvailableAttributesAsync()
{
    try
    {
        CsAttributeList attributeList; // Assuming the initialization or declaration elsewhere
        StringCollection riskTypes = other_filter_grid.Empty 
            ? other_filter_grid.GetRiskTypes(1) 
            : other_filter_grid.GetRiskTypes();

        var riskTypeValidation = new RiskTypeValidation(m_report_def_helper, riskTypes);

        string errorMsg = riskTypeValidation.GetCommonAttributesForRiskTypes(
            other_filter_grid.IcEditMode,
            cob_datepicker.Value,
            m_global_cache.IsDstRefMethodSelected,
            ref attributeList);

        riskTypeValidation.AddSensitivityAttributeIfContainScenario(attributeList, m_global_cache);
        StringCollection attributeNames = attributeList.AttributeNames;

        if (!this.InvokeRequired)
        {
            if (!DslMarsSwitch.IsRefMethodDisabled)
            {
                ctlCalcReportFormat.SetRiskTypeAttributes(attributeNames);
                ctlCalcReportFormat.SetSearched();
            }
        }
        else
        {
            if (MslMarsSwitch.IsRefMethodDisabled)
            {
                ctlCalcReportFormat.SetRiskTypeAttributes(attributeNames);
                ctlCalcReportFormat.SetSearched();
            }

            await Task.Run(() =>
            {
                if (!Ds1MarsSwitch.IsRefMethodDisabled)
                    ctlCalcReportFormat.SetRiskTypeAttributes(attributeNames);
            });
        }
    }
    catch (Exception ex)
    {
        // Handle exception here
        Console.WriteLine(ex.Message); // Replace with actual exception handling
    }
}




public partial class YourForm : Form 
{
    public YourForm()
    {
        InitializeComponent();
        this.openToolStripMenuItem.Click += async (sender, e) => await FileOpenMenuAsync(sender, e);
    }

    private async Task FileOpenMenuAsync(object sender, EventArgs e)
    {
        OpenFileDialog openFileDialog = new OpenFileDialog();

        if (openFileDialog.ShowDialog() == DialogResult.OK) 
        {
            string fileName = openFileDialog.FileName;

            // Assuming that ReadFileContentAsync is a method that reads file content asynchronously
            string content = await ReadFileContentAsync(fileName);

            // Do something with the content...
        }
    }

    private async Task<string> ReadFileContentAsync(string fileName) 
    {
        using (StreamReader reader = new StreamReader(fileName))
        {
            return await reader.ReadToEndAsync();
        }
    }
}




private void InitializeBusyIndicator()
{
    // Initialize panel
    panelBusyIndicator = new Panel
    {
        Size = new Size(200, 100),
        BackColor = Color.FromArgb(50, 255, 255, 224), // semi-transparent yellow-white
        Visible = false
    };

    panelBusyIndicator.Paint += PanelBusyIndicator_Paint; // Add this line

    // The rest of your code remains the same...
}

// Add this method to your form’s code
private void PanelBusyIndicator_Paint(object sender, PaintEventArgs e)
{
    Control control = sender as Control;
    if (control != null)
    {
        using (Pen pen = new Pen(Color.Red, 2)) // Set the border color and width here
        {
            e.Graphics.DrawRectangle(pen, 0, 0, control.Width - 1, control.Height - 1);
        }
    }
}







public partial class MainForm : Form
{
    private Panel panelBusyIndicator;
    private Label lblPleaseWait;

    public MainForm()
    {
        InitializeComponent();
        InitializeBusyIndicator();
    }

    private void InitializeBusyIndicator()
    {
        // Initialize panel
        panelBusyIndicator = new Panel
        {
            Size = new Size(200, 100),
            BackColor = Color.FromArgb(128, 0, 0, 0), // semi-transparent black
        };

        // Initialize label with "Please wait..." text
        lblPleaseWait = new Label
        {
            Text = "Please wait...",
            ForeColor = Color.White, // making text visible against semi-transparent black background
            AutoSize = false,
            Size = panelBusyIndicator.Size,
            TextAlign = ContentAlignment.MiddleCenter,
        };

        panelBusyIndicator.Controls.Add(lblPleaseWait);
        this.Controls.Add(panelBusyIndicator);
    }

    private async void btnLoadData_Click(object sender, EventArgs e)
    {
        panelBusyIndicator.BringToFront(); // bring panel to the front

        panelBusyIndicator.Location = new Point(
            (this.ClientSize.Width - panelBusyIndicator.Width) / 2,
            (this.ClientSize.Height - panelBusyIndicator.Height) / 2);

        panelBusyIndicator.Visible = true;
        panelBusyIndicator.Enabled = false;

        await Task.Run(() => LoadData());

        panelBusyIndicator.Visible = false;
    }

    private void LoadData()
    {
        System.Threading.Thread.Sleep(5000);
    }
}







public partial class MainForm : Form
{
    private Panel panelBusyIndicator;
    private Label lblPleaseWait;

    public MainForm()
    {
        InitializeComponent();
        InitializeBusyIndicator();
    }

    private void InitializeBusyIndicator()
    {
        // Initialize panel
        panelBusyIndicator = new Panel
        {
            Size = new Size(200, 100), // Adjust size as needed
            BackColor = Color.FromArgb(128, 255, 255, 255), // Semi-transparent white
            Visible = false
        };

        // Initialize label
        lblPleaseWait = new Label
        {
            Text = "Please wait...",
            AutoSize = true,
            Location = new Point((panelBusyIndicator.Width - lblPleaseWait.Width) / 2, 
                                 (panelBusyIndicator.Height - lblPleaseWait.Height) / 2)
        };

        // Add label to panel
        panelBusyIndicator.Controls.Add(lblPleaseWait);

        // Add panel to form
        this.Controls.Add(panelBusyIndicator);
    }

    private async void btnLoadData_Click(object sender, EventArgs e)
    {
        // Position the busy indicator panel in the center of the MainForm
        panelBusyIndicator.Location = new Point(
            (this.ClientSize.Width - panelBusyIndicator.Width) / 2,
            (this.ClientSize.Height - panelBusyIndicator.Height) / 2);

        // Show busy indicator
        panelBusyIndicator.Visible = true;
        panelBusyIndicator.Enabled = false; // Disables the panel so the form is still interactive

        // Simulate a long-running operation
        await Task.Run(() => LoadData());

        // Hide busy indicator
        panelBusyIndicator.Visible = false;
    }

    private void LoadData()
    {
        // Simulate a delay to represent a long-running task.
        System.Threading.Thread.Sleep(5000);
    }
}




this.Load += async (sender, e) => await Form_Loaded(sender, e);

public partial class FrmLoadingReportsIndicator : Form
{
    private System.Windows.Forms.Label lblStatus = new System.Windows.Forms.Label();
    private System.Windows.Forms.ProgressBar progressBar = new System.Windows.Forms.ProgressBar();

    public FrmLoadingReportsIndicator()
    {
        InitializeComponent();
        this.StartPosition = FormStartPosition.CenterParent;
    }

    private void InitializeComponent()
    {
        this.lblStatus.Location = new System.Drawing.Point(12, 9);
        this.lblStatus.Name = "lblStatus";
        this.lblStatus.Size = new System.Drawing.Size(260, 23);
        this.lblStatus.TabIndex = 0;
        this.lblStatus.Text = "Status";

        this.progressBar.Location = new System.Drawing.Point(12, 35);
        this.progressBar.Name = "progressBar";
        this.progressBar.Size = new System.Drawing.Size(260, 23);
        this.progressBar.TabIndex = 1;

        this.AutoScaleDimensions = new System.Drawing.SizeF(6F, 13F);
        this.AutoScaleMode = System.Windows.Forms.AutoScaleMode.Font;
        this.ClientSize = new System.Drawing.Size(284, 70);
        this.Controls.Add(this.progressBar);
        this.Controls.Add(this.lblStatus);
        this.Name = "FrmLoadingReportsIndicator";
        this.Text = "Loading...";
    }

    public void UpdateStatus(string text, int progress)
    {
        lblStatus.Text = text;
        progressBar.Value = progress;
    }
}




private void LoadReportDef(string fileName)
{
    Task.Run(async () => 
    {
        CsReportDef report_def = await OpenReportDefFromFileAsync(fileName);

        if (report_def.CalculationMethod != null)
        {
            if (report_def.CalculationMethod.SupportsWhatIf && 
                !CsSessionData.GetInstance().IsErcNtcsUserConsent)
            {
                if (m_global_cache != null && 
                    m_global_cache.ConfigDoc != null)
                {
                    ctlCalcReportFormat.ShowErcNtcsMessage(m_global_cache.ConfigDoc.ErcNtcsLdprHelpLink);
                }
            }
        }

        CsRegistryHelper.SetOption("DefaultFolder", Path.GetDirectoryName(fileName));
        DslMarsSwitch.CalculationFormatReset(ctlCalcReportFormat.GetSelectedMethod(), report_def.CalculationMethod); // Renamed here
        ctlCalcReportFormat.ResetDerivationCriteria();
        UpdateRecentDocRegistry(fileName, RECENT_DOCS_REG);

        try
        {
            m_busy_form = new FrmBusyStatus(backgroundWorker, report_def);
            m_busy_form.Status = "Loading report...";
            await m_busy_form.ShowDialogAsync(this); // Assuming there's an async version
            CsSessionData.GetInstance().LoadedReportName = 
                string.IsNullOrWhiteSpace(report_def.ReportFileName) ? 
                report_def.ReportName : 
                report_def.ReportFileName;
            Text = MARS_ENQUIRY_TOOL_STUB + "- " + CsSessionData.GetInstance().LoadedReportName;
        }
        catch (Exception ex)
        {
            CsReportRunException exception = new CsReportRunException(ex.Message, ex, report_def.ToString(), report_def.ReportName);
            CsMarsErrorHelper.GetInstance().ShowError(exception, "Error populating from report definition", false);
        }
    });
}




sub.Click += async (sender, e) => await file_recent_docs_submenu_click(sender, e);



private async Task LoadReportDefAsync(string fileName)
{
    CsReportDef report_def = await OpenReportDefFromFileAsync(fileName);

    if (report_def.CalculationMethod != null)
    {
        if (report_def.CalculationMethod.SupportsWhatIf && 
            !CsSessionData.GetInstance().IsErcNtcsUserConsent)
        {
            if (m_global_cache != null && 
                m_global_cache.ConfigDoc != null)
            {
                ctlCalcReportFormat.ShowErcNtcsMessage(m_global_cache.ConfigDoc.ErcNtcsLdprHelpLink);
            }
        }
    }

    CsRegistryHelper.SetOption("DefaultFolder", Path.GetDirectoryName(fileName));
    Ds1MarsSwitch.CalculationFormatReset(ctlCalcReportFormat.GetSelectedMethod(), report_def.CalculationMethod);
    ctlCalcReportFormat.ResetDerivationCriteria();
    UpdateRecentDocRegistry(fileName, RECENT_DOCS_REG);

    try
    {
        m_busy_form = new FrmBusyStatus(backgroundWorker, report_def);
        m_busy_form.Status = "Loading report...";
        await m_busy_form.ShowDialogAsync(this); // Assuming there's an async version
        CsSessionData.GetInstance().LoadedReportName = 
            string.IsNullOrWhiteSpace(report_def.ReportFileName) ? 
            report_def.ReportName : 
            report_def.ReportFileName;
        Text = MARS_ENQUIRY_TOOL_STUB + "- " + CsSessionData.GetInstance().LoadedReportName;
    }
    catch (Exception ex)
    {
        CsReportRunException exception = new CsReportRunException(ex.Message, ex, report_def.ToString(), report_def.ReportName);
        CsMarsErrorHelper.GetInstance().ShowError(exception, "Error populating from report definition", false);
    }
}





private async Task<CsReportDef> OpenReportDefFromFileAsync(string filePath)
{
    CsReportDef report_def;

    Cursor.Current = Cursors.WaitCursor;

    try
    {
        using (StreamReader stream = new StreamReader(filePath))
        {
            XmlDocument xml_doc = new XmlDocument();
            string content = await stream.ReadToEndAsync();
            xml_doc.LoadXml(content);

            // Reset other parameters as needed before creating CsReportDef
            resetOtherParameters();

            report_def = new CsReportDef(m_report_def_helper)
            {
                ReportFileName = Path.GetFileName(filePath)
            };

            report_def.LoadXml(xml_doc);
            AddPropertyTypeSetsToCache(report_def);
        }
    }
    catch (XmlException ex)
    {
        throw new ApplicationException($"The file {filePath} contains invalid xml", ex);
    }

    return report_def;
}





Hi Santosh, 
This is not a realistic scenario. It is the limitation of the application. If the User Object Count reaches 10000 and the Handle count also goes more than 12500 32-bit application, Please create a release note that this is the limitation beyond which the Windows form application will crash.


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
