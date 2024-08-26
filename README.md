$SolutionName = "MaRSRiskServerGateway"
$projectNames = @("Core", "UI", "Tests")

# Create main directory
New-Item -ItemType Directory -Force -Path $SolutionName
Set-Location $SolutionName

# Create solution file
dotnet new sln -n $SolutionName

# Create projects
foreach ($project in $projectNames) {
    $projectPath = "src\$SolutionName.$project"
    New-Item -ItemType Directory -Force -Path $projectPath
    
    Set-Location $projectPath
    
    switch ($project) {
        "UI" { dotnet new wpf }
        "Tests" { dotnet new xunit }
        default { dotnet new classlib }
    }
    
    Set-Location -Path ..\..\..
}

# Add projects to solution
foreach ($project in $projectNames) {
    dotnet sln add "src\$SolutionName.$project\$SolutionName.$project.csproj"
}

# Add project references
dotnet add "src\$SolutionName.UI\$SolutionName.UI.csproj" reference "src\$SolutionName.Core\$SolutionName.Core.csproj"
dotnet add "src\$SolutionName.Tests\$SolutionName.Tests.csproj" reference "src\$SolutionName.Core\$SolutionName.Core.csproj"
dotnet add "src\$SolutionName.Tests\$SolutionName.Tests.csproj" reference "src\$SolutionName.UI\$SolutionName.UI.csproj"

# Install NuGet packages
dotnet add "src\$SolutionName.Core\$SolutionName.Core.csproj" package ClosedXML
dotnet add "src\$SolutionName.UI\$SolutionName.UI.csproj" package Unity

# Create additional folders
$additionalFolders = @(
    "src\$SolutionName.Core\Interfaces",
    "src\$SolutionName.Core\Models",
    "src\$SolutionName.Core\Services",
    "src\$SolutionName.UI\ViewModels",
    "src\$SolutionName.UI\Views",
    "src\$SolutionName.Tests\CoreTests",
    "src\$SolutionName.Tests\UITests"
)

foreach ($folder in $additionalFolders) {
    New-Item -ItemType Directory -Force -Path $folder
}

# Create README.md
New-Item -ItemType File -Force -Path "README.md"
Set-Content -Path "README.md" -Value "# $SolutionName`n`nThis is a Risk Server Gateway application for MaRS."

# Create basic interface files
$interfaceFiles = @{
    "IExcelReader.cs" = @"
using System.Collections.Generic;
using ClosedXML.Excel;

namespace $SolutionName.Core.Interfaces
{
    public interface IExcelReader
    {
        List<string> GetHeaders(string sheetName, string startCell, string endCell);
        List<List<string>> GetData(string sheetName, string startCell, string endCell);
        IXLRange GetRange(string sheetName, string range);
        List<string> GetSheetNames();
    }
}
"@
    "IRiskProcessor.cs" = @"
using System.Collections.Generic;
using ClosedXML.Excel;

namespace $SolutionName.Core.Interfaces
{
    public interface IRiskProcessor
    {
        void GetMaRSAggregatedVAR(List<string> headers, List<List<string>> data, IXLRange targetRange);
    }
}
"@
}

foreach ($file in $interfaceFiles.Keys) {
    $filePath = "src\$SolutionName.Core\Interfaces\$file"
    New-Item -ItemType File -Force -Path $filePath
    Set-Content -Path $filePath -Value $interfaceFiles[$file]
}

# Create basic implementation files
$implementationFiles = @{
    "ExcelReader.cs" = @"
using $SolutionName.Core.Interfaces;
using ClosedXML.Excel;
using System.Collections.Generic;
using System.Linq;

namespace $SolutionName.Core.Services
{
    public class ExcelReader : IExcelReader
    {
        private readonly IXLWorkbook _workbook;

        public ExcelReader(IXLWorkbook workbook)
        {
            _workbook = workbook;
        }

        public List<string> GetHeaders(string sheetName, string startCell, string endCell)
        {
            // Implementation here
            throw new System.NotImplementedException();
        }

        public List<List<string>> GetData(string sheetName, string startCell, string endCell)
        {
            // Implementation here
            throw new System.NotImplementedException();
        }

        public IXLRange GetRange(string sheetName, string range)
        {
            // Implementation here
            throw new System.NotImplementedException();
        }

        public List<string> GetSheetNames()
        {
            // Implementation here
            throw new System.NotImplementedException();
        }
    }
}
"@
    "RiskProcessor.cs" = @"
using $SolutionName.Core.Interfaces;
using ClosedXML.Excel;
using System.Collections.Generic;

namespace $SolutionName.Core.Services
{
    public class RiskProcessor : IRiskProcessor
    {
        public void GetMaRSAggregatedVAR(List<string> headers, List<List<string>> data, IXLRange targetRange)
        {
            // Implementation here
            throw new System.NotImplementedException();
        }
    }
}
"@
}

foreach ($file in $implementationFiles.Keys) {
    $filePath = "src\$SolutionName.Core\Services\$file"
    New-Item -ItemType File -Force -Path $filePath
    Set-Content -Path $filePath -Value $implementationFiles[$file]
}

# Create basic ViewModel file
$viewModelContent = @"
using System.ComponentModel;
using System.Runtime.CompilerServices;
using System.Windows.Input;
using $SolutionName.Core.Interfaces;

namespace $SolutionName.UI.ViewModels
{
    public class MainViewModel : INotifyPropertyChanged
    {
        private readonly IExcelReader _excelReader;
        private readonly IRiskProcessor _riskProcessor;

        public MainViewModel(IExcelReader excelReader, IRiskProcessor riskProcessor)
        {
            _excelReader = excelReader;
            _riskProcessor = riskProcessor;
            // Initialize properties and commands here
        }

        public event PropertyChangedEventHandler PropertyChanged;

        protected virtual void OnPropertyChanged([CallerMemberName] string propertyName = null)
        {
            PropertyChanged?.Invoke(this, new PropertyChangedEventArgs(propertyName));
        }
    }
}
"@

$viewModelPath = "src\$SolutionName.UI\ViewModels\MainViewModel.cs"
New-Item -ItemType File -Force -Path $viewModelPath
Set-Content -Path $viewModelPath -Value $viewModelContent

Write-Host "Project $SolutionName has been set up successfully!"
