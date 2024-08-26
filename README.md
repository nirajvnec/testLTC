<Window x:Class="MaRSRiskServerGateway.UI.MainWindow"
        xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
        xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
        xmlns:d="http://schemas.microsoft.com/expression/blend/2008"
        xmlns:mc="http://schemas.openxmlformats.org/markup-compatibility/2006"
        xmlns:viewmodels="clr-namespace:MaRSRiskServerGateway.UI.ViewModels"
        mc:Ignorable="d"
        Title="MaRS Risk Server Gateway" Height="450" Width="800">

    <Window.DataContext>
        <viewmodels:MainViewModel/>
    </Window.DataContext>

    <Grid>
        <StackPanel VerticalAlignment="Center" HorizontalAlignment="Center">
            <Button Content="Process Excel" 
                    Command="{Binding ProcessExcelCommand}" 
                    Width="200" 
                    Height="50" 
                    Margin="0,10,0,10"/>
            
            <TextBlock Text="{Binding StatusMessage}" 
                       Margin="0,20,0,0" 
                       HorizontalAlignment="Center" />
            
            <!-- Placeholder for future DataGrid or other data display -->
            <TextBlock Text="Data will be displayed here" 
                       Margin="0,20,0,0" 
                       HorizontalAlignment="Center" />
        </StackPanel>
    </Grid>
</Window>





using System.ComponentModel;
using System.Runtime.CompilerServices;
using System.Threading.Tasks;
using System.Windows.Input;
using MaRSRiskServerGateway.Core.Interfaces;
using MaRSRiskServerGateway.UI.Commands;

namespace MaRSRiskServerGateway.UI.ViewModels
{
    public class MainViewModel : INotifyPropertyChanged
    {
        private IExcelReader _excelReader;
        private IExcelProcessor _excelProcessor;

        public ICommand ProcessExcelCommand { get; private set; }

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

        // Add a public parameterless constructor
        public MainViewModel()
        {
            // Initialize with default implementations or null
            // You'll need to set these later, perhaps through a method
            _excelReader = null;
            _excelProcessor = null;
            ProcessExcelCommand = new RelayCommand(ProcessExcelAsync);
        }

        // Keep the existing constructor
        public MainViewModel(IExcelReader excelReader, IExcelProcessor excelProcessor)
        {
            _excelReader = excelReader;
            _excelProcessor = excelProcessor;
            ProcessExcelCommand = new RelayCommand(ProcessExcelAsync);
        }

        private async Task ProcessExcelAsync()
        {
            // Existing implementation
        }

        public event PropertyChangedEventHandler PropertyChanged;

        protected virtual void OnPropertyChanged([CallerMemberName] string propertyName = null)
        {
            PropertyChanged?.Invoke(this, new PropertyChangedEventArgs(propertyName));
        }
    }
}
