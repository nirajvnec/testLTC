
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
