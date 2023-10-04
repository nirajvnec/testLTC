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
