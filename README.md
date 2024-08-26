<Window x:Class="MaRSRiskServerGateway.UI.MainWindow"
        xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
        xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
        xmlns:d="http://schemas.microsoft.com/expression/blend/2008"
        xmlns:mc="http://schemas.openxmlformats.org/markup-compatibility/2006"
        xmlns:viewmodels="clr-namespace:MaRSRiskServerGateway.UI.ViewModels"
        mc:Ignorable="d"
        Title="MaRS Risk Server Gateway" Height="450" Width="800">

    <Window.Resources>
        <Style TargetType="Button">
            <Setter Property="Margin" Value="5"/>
            <Setter Property="Padding" Value="10,5"/>
        </Style>
        <Style TargetType="TextBlock">
            <Setter Property="Margin" Value="5"/>
        </Style>
    </Window.Resources>

    <Grid Margin="20">
        <Grid.RowDefinitions>
            <RowDefinition Height="Auto"/>
            <RowDefinition Height="Auto"/>
            <RowDefinition Height="Auto"/>
            <RowDefinition Height="*"/>
        </Grid.RowDefinitions>

        <Button Grid.Row="0" 
                Content="Select Excel File" 
                Command="{Binding SelectFileCommand}"
                HorizontalAlignment="Left"/>

        <TextBlock Grid.Row="1" 
                   Text="{Binding SelectedFilePath}" 
                   TextWrapping="Wrap"/>

        <Button Grid.Row="2" 
                Content="Process Excel" 
                Command="{Binding ProcessExcelCommand}"
                HorizontalAlignment="Left"/>

        <TextBlock Grid.Row="3" 
                   Text="{Binding StatusMessage}" 
                   TextWrapping="Wrap"/>
    </Grid>
</Window>
