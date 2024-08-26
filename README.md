public class AsyncRelayCommand : ICommand
{
    private readonly Func<Task> _execute;
    private readonly Func<bool> _canExecute;
    private bool _isExecuting;

    public AsyncRelayCommand(Func<Task> execute, Func<bool> canExecute = null)
    {
        _execute = execute ?? throw new ArgumentNullException(nameof(execute));
        _canExecute = canExecute;
    }

    public event EventHandler CanExecuteChanged
    {
        add { CommandManager.RequerySuggested += value; }
        remove { CommandManager.RequerySuggested -= value; }
    }

    public bool CanExecute(object parameter)
    {
        return !_isExecuting && (_canExecute?.Invoke() ?? true);
    }

    public async void Execute(object parameter)
    {
        if (CanExecute(parameter))
        {
            try
            {
                _isExecuting = true;
                await _execute();
            }
            finally
            {
                _isExecuting = false;
            }
        }

        RaiseCanExecuteChanged();
    }

    public void RaiseCanExecuteChanged()
    {
        CommandManager.InvalidateRequerySuggested();
    }
}




public class MainViewModel : INotifyPropertyChanged
{
    private readonly IExcelReader _excelReader;
    private string _statusMessage;

    public ICommand SelectFileCommand { get; }
    public ICommand ProcessExcelCommand { get; }
    public ICommand GetMaRSAggregatedVARCommand { get; }

    public MainViewModel(IExcelReader excelReader)
    {
        _excelReader = excelReader;
        SelectFileCommand = new RelayCommand(SelectFile);
        ProcessExcelCommand = new AsyncRelayCommand(ProcessExcelAsync, CanProcessExcel);
        GetMaRSAggregatedVARCommand = new AsyncRelayCommand(GetMaRSAggregatedVARAsync, CanGetMaRSAggregatedVAR);
    }

    private async Task GetMaRSAggregatedVARAsync()
    {
        StatusMessage = "Calculating MaRS Aggregated VAR...";
        try
        {
            // Simulating a long-running operation
            await Task.Delay(2000); // Replace this with your actual async calculation
            StatusMessage = "MaRS Aggregated VAR calculation completed.";
            // Add your actual calculation logic here
        }
        catch (Exception ex)
        {
            StatusMessage = $"Error calculating MaRS Aggregated VAR: {ex.Message}";
        }
    }

    private bool CanGetMaRSAggregatedVAR()
    {
        return !string.IsNullOrEmpty(SelectedFilePath);
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

    // Implement INotifyPropertyChanged
    public event PropertyChangedEventHandler PropertyChanged;
    protected virtual void OnPropertyChanged([CallerMemberName] string propertyName = null)
    {
        PropertyChanged?.Invoke(this, new PropertyChangedEventArgs(propertyName));
    }

    // Other properties and methods...
}
