
using OfficeOpenXml;
using System;
using System.Data;
using System.IO;
using System.Threading.Tasks;

namespace MaRSRiskServerGateway.Services
{
    public class EPPlusExcelDataService : IExcelDataService
    {
        public EPPlusExcelDataService()
        {
            ExcelPackage.LicenseContext = LicenseContext.NonCommercial;
        }

        public async Task<DataTable> GetSensitivitiesDataAsync(string filePath)
        {
            using (var package = new ExcelPackage(new FileInfo(filePath)))
            {
                await package.LoadAsync(filePath);
                var worksheet = package.Workbook.Worksheets["Sensitivities"];
                return await ExtractDataFromWorksheetAsync(worksheet, "A10:W70");
            }
        }

        public async Task<DataTable> GetVaRDataAsync(string filePath)
        {
            using (var package = new ExcelPackage(new FileInfo(filePath)))
            {
                await package.LoadAsync(filePath);
                var worksheet = package.Workbook.Worksheets["VaR"];
                return await ExtractDataFromWorksheetAsync(worksheet, "B20:C35");
            }
        }

        private async Task<DataTable> ExtractDataFromWorksheetAsync(ExcelWorksheet worksheet, string range)
        {
            return await Task.Run(() =>
            {
                var dataTable = new DataTable();
                var excelRange = worksheet.Cells[range];

                // Add columns
                for (int col = excelRange.Start.Column; col <= excelRange.End.Column; col++)
                {
                    dataTable.Columns.Add(worksheet.Cells[excelRange.Start.Row, col].Value?.ToString() ?? $"Column {col}");
                }

                // Add data
                for (int row = excelRange.Start.Row + 1; row <= excelRange.End.Row; row++)
                {
                    var dataRow = dataTable.NewRow();
                    for (int col = excelRange.Start.Column; col <= excelRange.End.Column; col++)
                    {
                        dataRow[col - excelRange.Start.Column] = worksheet.Cells[row, col].Value?.ToString() ?? string.Empty;
                    }
                    dataTable.Rows.Add(dataRow);
                }

                return dataTable;
            });
        }
    }
}


using System.Data;
using System.Threading.Tasks;

namespace MaRSRiskServerGateway.Services
{
    public interface IExcelDataService
    {
        Task<DataTable> GetSensitivitiesDataAsync(string filePath);
        Task<DataTable> GetVaRDataAsync(string filePath);
    }
}




using System.Data;
using System.Windows.Input;
using System.Threading.Tasks;
using MaRSRiskServerGateway.Services;

namespace MaRSRiskServerGateway.ViewModels
{
    public class MainViewModel : INotifyPropertyChanged
    {
        private readonly IExcelDataService _excelDataService;
        private string _selectedFilePath;
        private DataTable _sensitivitiesData;
        private DataTable _varData;
        private string _statusMessage;

        public MainViewModel(IExcelDataService excelDataService)
        {
            _excelDataService = excelDataService;
            SelectFileCommand = new RelayCommand(SelectFile);
            ProcessExcelCommand = new AsyncRelayCommand(ProcessExcelAsync, CanProcessExcel);
            GetMaRSAggregatedVARCommand = new AsyncRelayCommand(GetMaRSAggregatedVARAsync, CanGetMaRSAggregatedVAR);
        }

        public ICommand SelectFileCommand { get; }
        public ICommand ProcessExcelCommand { get; }
        public ICommand GetMaRSAggregatedVARCommand { get; }

        public string SelectedFilePath
        {
            get => _selectedFilePath;
            set
            {
                _selectedFilePath = value;
                OnPropertyChanged();
                ProcessExcelAsync().ConfigureAwait(false);
            }
        }

        public DataTable SensitivitiesData
        {
            get => _sensitivitiesData;
            set
            {
                _sensitivitiesData = value;
                OnPropertyChanged();
            }
        }

        public DataTable VaRData
        {
            get => _varData;
            set
            {
                _varData = value;
                OnPropertyChanged();
            }
        }

        public string StatusMessage
        {
            get => _statusMessage;
            set
            {
                _statusMessage = value;
                OnPropertyChanged();
            }
        }

        private async Task ProcessExcelAsync()
        {
            if (string.IsNullOrEmpty(SelectedFilePath) || !File.Exists(SelectedFilePath))
            {
                StatusMessage = "Please select a valid Excel file.";
                return;
            }

            try
            {
                StatusMessage = "Processing Excel file...";
                SensitivitiesData = await _excelDataService.GetSensitivitiesDataAsync(SelectedFilePath);
                VaRData = await _excelDataService.GetVaRDataAsync(SelectedFilePath);
                StatusMessage = "Excel file processed successfully.";
            }
            catch (Exception ex)
            {
                StatusMessage = $"Error processing Excel file: {ex.Message}";
            }
        }

        private async Task GetMaRSAggregatedVARAsync()
        {
            // Implement your GetMaRSAggregatedVAR logic here
        }

        private bool CanProcessExcel() => !string.IsNullOrEmpty(SelectedFilePath) && File.Exists(SelectedFilePath);
        private bool CanGetMaRSAggregatedVAR() => SensitivitiesData != null && VaRData != null;

        // Implement INotifyPropertyChanged
        public event PropertyChangedEventHandler PropertyChanged;
        protected virtual void OnPropertyChanged([CallerMemberName] string propertyName = null)
        {
            PropertyChanged?.Invoke(this, new PropertyChangedEventArgs(propertyName));
        }

        private void SelectFile()
        {
            var openFileDialog = new OpenFileDialog
            {
                Filter = "Excel Files|*.xlsx;*.xls",
                Title = "Select an Excel file"
            };

            if (openFileDialog.ShowDialog() == true)
            {
                SelectedFilePath = openFileDialog.FileName;
            }
        }
    }
}
