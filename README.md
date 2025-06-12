
Hi Siva,

We conducted Ashutosh's interview today at around 2 PM. After evaluating his skills, we found that they do not align with our project requirements. Therefore, we will not be proceeding with his candidature.

We wish him all the best in his future endeavors.

Best regards,




public BlobServiceClient GetBlobServiceClient()
{
    var tenantAuthority = $"https://login.microsoftonline.com/{_tenantId}";
    string url = $"https://{_storageAccount}.blob.core.windows.net/";
    Uri blobUri = new Uri(url);

    TokenCredential credential;

#if DEBUG
    // Local development: use your AZ login credentials
    credential = new DefaultAzureCredential();
#else
    // Hosted environments (SIT, UAT, PROD): use app registration
    credential = new ClientSecretCredential(
        _tenantId,
        _clientId,
        _clientSecret,
        new TokenCredentialOptions
        {
            AuthorityHost = new Uri(tenantAuthority)
        });
#endif

    return new BlobServiceClient(blobUri, credential);
}



public async Task<Stream> DownloadFileAsync(string fileName, string fileExtention, string folderPath = "")
{
    // Use the injected service to determine report type
    string reportType = _reportTypeService.GetReportType(fileName);

    // Adjust folder path based on report type
    if (string.IsNullOrEmpty(folderPath))
    {
        folderPath = reportType; // Use PBI or PG as the path
    }
    else
    {
        folderPath = Path.Combine(folderPath, reportType); // Append PBI or PG
    }

    var blobServiceClient = GetBlobServiceClient();
    var containerClient = blobServiceClient.GetBlobContainerClient(folderPath);

    string fileN = fileName;
    string localFilePath = Path.Combine("./", fileN);

    var blobClient = containerClient.GetBlobClient(fileName + fileExtention);

    BlobDownloadInfo download = await blobClient.DownloadAsync();
    Stream stream = download.Content;

    return stream;
}







public class ADLSService : IADLSService
{
    private readonly RdlReportConfig _rdlConfig;
    private readonly string _tenantId;
    private readonly string _clientId;
    private readonly string _clientSecret;
    private readonly string _storageAccount;
    private readonly string _containerName;
    private readonly string _containerOutputName;

    private readonly IReportTypeService _reportTypeService;

    public ADLSService(
        IOptions<RdlReportConfig> rdlConfig,
        IOptions<MarvelPowerBI> marvelPowerBI,
        IReportTypeService reportTypeService // ⬅️ Inject here
    )
    {
        _rdlConfig = rdlConfig.Value;
        _tenantId = _rdlConfig.TenantId.ToString();
        _storageAccount = _rdlConfig.StorageAccount;
        _containerName = _rdlConfig.ContainerName;
        _containerOutputName = _rdlConfig.ContainerOutputName;

        _clientId = !string.IsNullOrWhiteSpace(marvelPowerBI.Value.ClientId) 
            ? marvelPowerBI.Value.ClientId 
            : throw new Exception("Power BI Client ID is null.");

        _clientSecret = !string.IsNullOrWhiteSpace(marvelPowerBI.Value.ClientSecret) 
            ? marvelPowerBI.Value.ClientSecret 
            : throw new Exception("Power BI Client secret is null.");

        _reportTypeService = reportTypeService; // ⬅️ Assign to private field
    }
}






public class ReportTypeService : IReportTypeService
{
    public string GetReportType(string fileName)
    {
        if (fileName.StartsWith("PBI_", StringComparison.OrdinalIgnoreCase))
            return "PBI";
        else if (fileName.StartsWith("PG_", StringComparison.OrdinalIgnoreCase))
            return "PG";
        else
            throw new InvalidOperationException("Unknown report type for file: " + fileName);
    }
}




public async Task<Stream> DownloadFileAsync(string fileName, string fileExtention, string folderPath = "")
{
    string reportType = GetReportType(fileName); // Determine report type: PBI or PG

    // Adjust folder path based on report type
    if (string.IsNullOrEmpty(folderPath))
    {
        folderPath = reportType; // Use PBI or PG as the path
    }
    else
    {
        folderPath = Path.Combine(folderPath, reportType); // Append PBI or PG
    }

    var blobServiceClient = GetBlobServiceClient();
    var containerClient = blobServiceClient.GetBlobContainerClient(folderPath);

    string fileN = fileName;
    string localFilePath = Path.Combine("./", fileN);

    var blobClient = containerClient.GetBlobClient(fileName + fileExtention);

    BlobDownloadInfo download = await blobClient.DownloadAsync();
    Stream stream = download.Content;

    return stream;
}

// Determines the report type based on the file name
private string GetReportType(string fileName)
{
    if (fileName.StartsWith("PBI_", StringComparison.OrdinalIgnoreCase))
        return "PBI";
    else if (fileName.StartsWith("PG_", StringComparison.OrdinalIgnoreCase))
        return "PG";
    else
        throw new InvalidOperationException("Unknown report type for file: " + fileName);
}




.Where(kvp => kvp.Key.Contains("MarvelReportConfig"))



Also, could you please share:

One sample PBI report and one sample PG report, so I can test the upload/download functionality via Swagger?

The Azure Storage container folder link where these files will appear after the upload?




Do we need to refer to the file name or extension to identify whether it's a PBI or PG report?



Hi Harshit,

I need some clarity on identifying the report type—whether it's a PBI or PG report. On the API side, I need to handle uploads and downloads based on the report type.

If the report is a PG report, then it should be uploaded to or downloaded from the PG folder.

If the report is a PBI report, then the action should target the PBI folder.


Could you please let me know how we can distinguish between these two report types?




Subject: Feedback on Shailesh's Evaluation

Hi Siva,

We completed the evaluation of Shailesh today. After careful consideration, we found that his skill set does not align with the requirements of the current role.

The assessment was based on .NET and other relevant technologies commonly used in our projects. It appears that Shailesh does not have hands-on experience with some of the key technologies we rely on.

As a result, we have decided not to move forward with his candidature. We wish him all the best in his future endeavors.


import { Utilities } from 'scripts/ts/utilities';

{
  field: 'thresholdValue',
  headerName: 'Threshold',
  filter: 'agTextColumnFilter',
  sortable: true,
  filterParams: {
    buttons: ['reset', 'apply'],
  },
  valueFormatter: Utilities.formatNullAsDash,
  cellStyle: () => textColumnStyle,
  flex: 0.5,
}





import { ValueFormatterParams } from 'ag-grid-community';

export class Utilities {
  // ...existing stuff...

  public static formatNullAsDash(params: ValueFormatterParams): string {
    return params.value == null ? '-' : params.value;
  }

  // ...more methods...
}





{
  field: 'thresholdValue',
  headerName: 'Threshold',
  filter: 'agTextColumnFilter',
  sortable: true,
  filterParams: {
    buttons: ['reset', 'apply'],
  },
  valueFormatter: (params: ValueFormatterParams) =>
    params.value == null ? '-' : params.value,
  cellStyle: () => textColumnStyle,
  flex: 0.5,
}



{
  field: 'thresholdValue',
  headerName: 'Threshold',
  filter: 'agTextColumnFilter',
  sortable: true,
  filterParams: {
    buttons: ['reset', 'apply'],
  },
  valueFormatter: ({ value }) => value == null ? '-' : value,
  cellStyle: () => textColumnStyle,
  flex: 0.5,
},
{
  field: 'standardDeviationCoeff',
  headerName: 'Standard Deviation Coeff',
  filter: 'agTextColumnFilter',
  sortable: true,
  filterParams: {
    buttons: ['reset', 'apply'],
  },
  valueFormatter: ({ value }) => value == null ? '-' : value,
  cellStyle: () => textColumnStyle,
  flex: 1.2,
},







refactor: update GetAvailableCards to return CardInfo for HTML detail support






await this.apiClient.get<ICardInfo>(
  '/HomeDashboard/GetCardInfoById',
  {
    params: { cardId: id }
  }
);



UPDATE [reference].[card_details]
SET 
    card_details = '{ "content": "<div style=''text-align: left;''>Hi Test</div>" }',
    card_header = 'Features Live on TRC Hub'
WHERE card_key = 1;



git stash apply "stash@{1}"


git stash apply stash@{1}

git sf threshold_enablenull


sf = "!f() { \
  curr=$(git rev-parse --abbrev-ref HEAD); \
  if [ \"$curr\" != 'development' ]; then \
    echo \"❌ You are on '$curr'. Please switch to 'development' branch first (git checkout development).\"; \
    exit 1; \
  fi; \
  t=$(date +%Y-%m-%d_%H:%M:%S); \
  git stash push -m 'AutoStash: '$curr' @ '$t && \
  git pull origin development && \
  git checkout -b feature/$1 && \
  git stash apply; \
}; f"






make ThresholdValue nullable in entity model

# 1. Make sure you're on the development branch
git checkout development

# 2. Stash your local changes (optional but safe)
git stash push -m "WIP - saving local changes before sync"

# 3. Pull the latest changes from remote development
git pull origin development

# 4. Create and switch to the new feature branch
git checkout -b feature/threshold_enablenull

# 5. Apply your previously stashed changes (without removing the stash)
git stash apply

# 6. Add all updated files
git add .

# 7. Commit your changes
git commit -m "Added threshold_enablenull feature logic"

# 8. Push the new branch to remote and set upstream
git push -u origin feature/threshold_enablenull



USE [at40482-mtrm-dev]
GO

UPDATE [config].[threshold]
SET [threshold] = 1
WHERE [threshold_key] = 14
GO


USE [at40482-mtrm-dev]
GO

UPDATE [config].[threshold]
SET
    [node_id] = 656096,
    [measure_id] = 12,
    [node_level] = 3,
    [threshold] = NULL,  -- setting threshold to NULL
    [standard_deviation_coeff] = 0.00000,
    [is_override_all] = 0,
    [comment] = 'demo',
    [created_at] = '2025-04-24 07:37:33.167',
    [created_by] = 'MARVEL-API',
    [modified_at] = NULL,
    [modified_by] = NULL
WHERE
    [threshold_key] = 14
GO


ALTER TABLE config.threshold
ALTER COLUMN threshold DECIMAL(18, 2) NULL;


ALTER TABLE config.threshold
ALTER COLUMN threshold DECIMAL(18, 2) NOT NULL;
