public void ConfigureServices(IServiceCollection services)
{
    // Register the ExcelReader with the specific file path
    services.AddSingleton<IExcelReader>(provider => 
        new ExcelReader("C:/Users/nkuma152/Complex_user.xlsm"));

    // Force initialization (optional)
    var serviceProvider = services.BuildServiceProvider();
    var excelReader = serviceProvider.GetRequiredService<IExcelReader>();

    // Other service registrations...
}
