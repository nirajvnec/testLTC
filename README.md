using Microsoft.Web.WebView2.WinForms;
using Microsoft.Web.WebView2.Core;
using System;
using System.Threading;
using System.Threading.Tasks;
using System.Windows.Forms;
using System.Security.Cryptography.X509Certificates;

namespace WinFormsWebView2
{
    public class WinFormsWebView : IBrowser
    {
        // ... (previous code remains unchanged)

        private async void CoreWebView2OnClientCertificateRequested(object sender, 
            CoreWebView2ClientCertificateRequestedEventArgs e)
        {
            e.Handled = true;

            if (!AllowToSelectCertificate)
            {
                // Automatically select a certificate if not allowing user selection
                e.SelectedCertificate = e.MutuallyTrustedCertificates[1];
                return;
            }

            // Use Task.Run to show dialog on a separate thread
            var selectedCert = await Task.Run(() => ShowCertificateSelectionDialog(e.MutuallyTrustedCertificates));

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

        private X509Certificate2 ShowCertificateSelectionDialog(X509Certificate2Collection certificates)
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
                    listBox.Items.Add(cert.Subject);
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
