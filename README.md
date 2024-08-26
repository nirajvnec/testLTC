<Window x:Class="MaRSRiskServerGateway.UI.Views.MainWindow"
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
