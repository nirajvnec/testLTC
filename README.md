private void ConfigureServices(IServiceCollection services)
{
    // Register the ExcelReader with the specific file path
    services.AddSingleton<IExcelReader>(provider => 
        new ExcelReader("C:/Users/nkuma152/Complex_user.xlsm"));

    // Register other services that might depend on IExcelReader
    services.AddSingleton<IExcelProcessor, ExcelProcessor>();

    // Register your view models and MainWindow
    services.AddSingleton<MainViewModel>();
    services.AddSingleton<MainWindow>();
}
