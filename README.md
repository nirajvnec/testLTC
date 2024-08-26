using System;
using System.ComponentModel;
using System.Runtime.CompilerServices;
using System.Windows.Input;
using Microsoft.Win32;
using MaRSRiskServerGateway.Core.Interfaces;
using MaRSRiskServerGateway.UI.Commands;

namespace MaRSRiskServerGateway.UI.ViewModels
{
    public class MainViewModel : INotifyPropertyChanged
    {
        private readonly IExcelProcessor _excelProcessor;
        private string _selectedFilePath;
        private string _statusMessage;

        public string SelectedFilePath
        {
            get => _selectedFilePath;
            set
            {
                _selectedFilePath = value;
                OnPropertyChanged();
                ((RelayCommand)ProcessExcelCommand).RaiseCanExecuteChanged();
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

        public ICommand SelectFileCommand { get; }
        public ICommand ProcessExcelCommand { get; }

        public MainViewModel(IExcelProcessor excelProcessor)
        {
            _excelProcessor = excelProcessor;
            SelectFileCommand = new RelayCommand(SelectFile);
            ProcessExcelCommand = new RelayCommand(ProcessExcel, CanProcessExcel);
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
                StatusMessage = "File selected: " + SelectedFilePath;
            }
        }

        private void ProcessExcel()
        {
            try
            {
                _excelProcessor.ProcessExcelData(SelectedFilePath);
                StatusMessage = "Excel file processed successfully.";
            }
            catch (Exception ex)
            {
                StatusMessage = "Error processing Excel file: " + ex.Message;
            }
        }

        private bool CanProcessExcel()
        {
            return !string.IsNullOrEmpty(SelectedFilePath);
        }

        public event PropertyChangedEventHandler PropertyChanged;

        protected virtual void OnPropertyChanged([CallerMemberName] string propertyName = null)
        {
            PropertyChanged?.Invoke(this, new PropertyChangedEventArgs(propertyName));
        }
    }
}
