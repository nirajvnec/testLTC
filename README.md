using System.Windows;
using MaRSRiskServerGateway.UI.ViewModels;

namespace MaRSRiskServerGateway.UI
{
    public partial class MainWindow : Window
    {
        public MainWindow(MainViewModel viewModel)
        {
            InitializeComponent();
            DataContext = viewModel;
        }
    }
}


using System.Windows;
using MaRSRiskServerGateway.UI.ViewModels;

namespace MaRSRiskServerGateway.UI
{
    public partial class MainWindow : Window
    {
        public MainWindow(MainViewModel viewModel)
        {
            InitializeComponent();
            DataContext = viewModel;
        }
    }
}
