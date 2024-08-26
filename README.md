using System.Collections.Generic;
using System.ComponentModel;
using System.Runtime.CompilerServices;
using System.Threading.Tasks;
using System.Windows.Input;
using MaRSRiskServerGateway.Core.Interfaces;

namespace MaRSRiskServerGateway.UI.ViewModels
{
    public class MainViewModel : INotifyPropertyChanged
    {
        private readonly IExcelReader _excelReader;
        private readonly IExcelProcessor _excelProcessor;

        public ICommand ProcessExcelCommand { get; }

        private string _statusMessage;
        public string StatusMessage
        {
            get => _statusMessage;
            set
            {
                _statusMessage = value;
                OnPropertyChanged();
            }
        }

        public MainViewModel(IExcelReader excelReader, IExcelProcessor excelProcessor)
        {
            _excelReader = excelReader;
            _excelProcessor = excelProcessor;
            ProcessExcelCommand = new RelayCommand(async () => await ProcessExcelAsync());
        }

        private async Task ProcessExcelAsync()
        {
            StatusMessage = "Processing...";
            
            // This is where you would typically open a file dialog to select an Excel file
            // For this example, we'll assume a file path
            string filePath = @"C:\path\to\your\excel\file.xlsx";

            // Read Excel data
            var headers = _excelReader.GetHeaders("Sheet1", "A1", "Z1");
            var data = _excelReader.GetData("Sheet1", "A2", "Z100");

            // Process data
            _excelProcessor.ProcessExcelData(headers, data);

            // Calculate risk metric
            double riskMetric = _excelProcessor.CalculateRiskMetric(data);

            // Update status
            StatusMessage = $"Processing complete. Risk Metric: {riskMetric}";
        }

        public event PropertyChangedEventHandler PropertyChanged;

        protected virtual void OnPropertyChanged([CallerMemberName] string propertyName = null)
        {
            PropertyChanged?.Invoke(this, new PropertyChangedEventArgs(propertyName));
        }
    }

    // You'll need to implement this RelayCommand class or use an existing implementation
    public class RelayCommand : ICommand
    {
        private readonly Func<Task> _execute;

        public RelayCommand(Func<Task> execute)
        {
            _execute = execute;
        }

        public event EventHandler CanExecuteChanged;

        public bool CanExecute(object parameter) => true;

        public async void Execute(object parameter)
        {
            await _execute();
        }
    }
}
