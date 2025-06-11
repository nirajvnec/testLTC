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
