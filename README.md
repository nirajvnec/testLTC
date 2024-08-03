using Microsoft.Web.WebView2.WinForms;
using Microsoft.Web.WebView2.Core;
using System;
using System.Threading;
using System.Windows.Forms;
using System.Collections.Generic;

namespace WinFormsWebView2
{
    public class WinFormsWebView : IBrowser
    {
        // ... (previous code remains unchanged)

        private void CoreWebView2OnClientCertificateRequested(object sender, 
            CoreWebView2ClientCertificateRequestedEventArgs e)
        {
            e.Handled = true;

            if (!AllowToSelectCertificate)
            {
                // Automatically select a certificate if not allowing user selection
                e.SelectedCertificate = e.MutuallyTrustedCertificates[1];
                return;
            }

            var resetEvent = new ManualResetEvent(false);
            CoreWebView2ClientCertificate selectedCert = null;

            WebView.Invoke((MethodInvoker)delegate
            {
                selectedCert = ShowCertificateSelectionDialog(e.MutuallyTrustedCertificates);
                resetEvent.Set();
            });

            resetEvent.WaitOne();

            if (selectedCert != null)
            {
                e.SelectedCertificate = selectedCert;
            }
            else
            {
                // User cancelled or didn't select a certificate
                e.Cancel = true;
            }
        }

        private CoreWebView2ClientCertificate ShowCertificateSelectionDialog(IReadOnlyList<CoreWebView2ClientCertificate> certificates)
        {
            using (var form = new Form())
            {
                form.Text = "Select a certificate for authentication";
                form.StartPosition = FormStartPosition.CenterScreen;
                form.Size = new System.Drawing.Size(400, 300);

                var listBox = new ListBox
                {
                    Dock = DockStyle.Fill
                };

                foreach (var cert in certificates)
                {
                    listBox.Items.Add($"{cert.Subject} (Issuer: {cert.Issuer})");
                }

                var okButton = new Button
                {
                    Text = "OK",
                    DialogResult = DialogResult.OK,
                    Dock = DockStyle.Bottom
                };

                var cancelButton = new Button
                {
                    Text = "Cancel",
                    DialogResult = DialogResult.Cancel,
                    Dock = DockStyle.Bottom
                };

                form.Controls.Add(listBox);
                form.Controls.Add(okButton);
                form.Controls.Add(cancelButton);

                if (form.ShowDialog() == DialogResult.OK && listBox.SelectedIndex != -1)
                {
                    return certificates[listBox.SelectedIndex];
                }

                return null;
            }
        }

        // ... (rest of the class remains unchanged)
    }
}
