public partial class App : Application
{
    private IServiceProvider _serviceProvider;

    protected override void OnStartup(StartupEventArgs e)
    {
        base.OnStartup(e);
        var serviceCollection = new ServiceCollection();
        ConfigureServices(serviceCollection);
        _serviceProvider = serviceCollection.BuildServiceProvider();

        var mainWindow = _serviceProvider.GetRequiredService<MainWindow>();
        mainWindow.Show();
    }

    private void ConfigureServices(IServiceCollection services)
    {
        // Register your view models, services, and MainWindow
        services.AddSingleton<IExcelProcessor, ExcelProcessor>(); // Assuming IExcelProcessor is implemented by ExcelProcessor
        services.AddSingleton<MainViewModel>();
        services.AddSingleton<MainWindow>();
    }
}
