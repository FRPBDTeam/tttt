using System;
using System.Collections.Generic;
using System.ComponentModel;
using System.Data;
using System.Drawing;
using System.Linq;
using System.Net;
using System.Text;
using System.Threading.Tasks;
using System.Windows.Forms;
using System.Diagnostics;
using System.IO;
using System.Reflection;

namespace GsmDownLoader
{
    public partial class Form1 : Form
    {
        private string[] downloadUrls = new string[] {
            "https://www.frpbdteam.com/GsmDownLoader/Meow_Login_Tool_v2.exe",
            "https://www.frpbdteam.com/GsmDownLoader/Meow_Login_Tool_v0.2.1.exe",
            "https://www.frpbdteam.com/GsmDownLoader/MsmDownloadTool-V2.0.71-rcsm.exe",
            "https://www.frpbdteam.com/GsmDownLoader/MsmDownloadTool_V2.0.67-Rcsm.exe",
            "https://www.frpbdteam.com/GsmDownLoader/MsmDownloadTool_v2.0.63_rcsm.exe",
            "https://www.frpbdteam.com/GsmDownLoader/MsmDownloadTool%20for%20RMX3363.exe",
            "https://www.frpbdteam.com/GsmDownLoader/MsmDownloadTool%20for%20GT%20Neo2%20RMX3370.exe",
            "https://www.frpbdteam.com/GsmDownLoader/DownloadTool_Rcsm_V1.2.23.exe",
            "https://www.frpbdteam.com/GsmDownLoader/JRT.exe"
        };

        private string[] downloadOptions = new string[] {
            "Meow_Login_Tool_v2",
            "Meow_Login_Tool_v0.2.1",
            "MsmDownloadTool-V2.0.71-rcsm",
            "MsmDownloadTool_V2.0.67-Rcsm",
            "MsmDownloadTool_v2.0.63_rcsm",
            "MsmDownloadTool for RMX3363",
            "MsmDownloadTool for GT Neo2 RMX3370",
            "DownloadTool_Rcsm_V1.2.23",
            "JRT"
        };

        private WebClient client;
        private DateTime startTime;

        public Form1()
        {
            InitializeComponent();

            // Initialize the ComboBox with download options
            ComboBox1.Items.AddRange(downloadOptions);
            ComboBox1.SelectedIndex = 0;

            // Set the progress bar properties
            ProgressBar1.Minimum = 0;
            ProgressBar1.Maximum = 100;
            ProgressBar1.Step = 1;
            ProgressBar1.Visible = false;

            // Customize the label properties
            Label1.Text = "Downloading... 0%";
            Label2.Text = "Speed: 0.0 MB/s";
            Label3.Text = "Total Size: 0 MB";
        }

        private void Button1_Click(object sender, EventArgs e)
        {
            string selectedDownload = ComboBox1.SelectedItem.ToString();

            client = new WebClient();
            string url = downloadUrls[ComboBox1.SelectedIndex];
            string fileName = selectedDownload + ".exe";

            try
            {
                ServicePointManager.SecurityProtocol = SecurityProtocolType.Tls12; // Set security protocol to TLS 1.2

                // Show the progress bar and start the download
                ProgressBar1.Visible = true;
                ProgressBar1.Value = 0;

                startTime = DateTime.Now;
                client.DownloadProgressChanged += new DownloadProgressChangedEventHandler(client_DownloadProgressChanged);
                client.DownloadFileCompleted += new AsyncCompletedEventHandler(client_DownloadFileCompleted);
                client.DownloadFileAsync(new Uri(url), fileName);
            }
            catch (Exception ex)
            {
                MessageBox.Show("An error occurred while downloading the file: " + ex.Message);
            }
        }

        private void client_DownloadProgressChanged(object sender, DownloadProgressChangedEventArgs e)
        {
            // Update the progress bar and label
            ProgressBar1.Value = e.ProgressPercentage;
            TimeSpan timeSpan = DateTime.Now - startTime;
            double speed = e.BytesReceived / timeSpan.TotalSeconds / 1048576;
            Label1.Text = $"Downloading... {e.ProgressPercentage}%";
            Label2.Text = $"Speed: {speed:0.0} MB/s";
            Label3.Text = $"Total Size: {e.TotalBytesToReceive / 1048576.0:0.00} MB";
        }

        private void client_DownloadFileCompleted(object sender, AsyncCompletedEventArgs e)
        {
            if (e.Error != null)
            {
                MessageBox.Show("An error occurred while downloading the file: " + e.Error.Message);
            }
            else
            {
                MessageBox.Show("Download Successfully.");
            }

            // Hide the progress bar
            ProgressBar1.Visible = false;
            Label1.Text = "Download Completed.";
        }

        private void ProgressBar1_Click(object sender, EventArgs e)
        {

        }

        private void Label3_Click(object sender, EventArgs e)
        {

        }

        private void Label2_Click(object sender, EventArgs e)
        {

        }

        private void ComboBox1_SelectedIndexChanged(object sender, EventArgs e)
        {

        }

    }
}
