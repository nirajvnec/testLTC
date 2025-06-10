Subject: Interview Feedback – Tarun Kumar

Hi Shiva,

We have completed our evaluation of Tarun Kumar and find him to be a suitable candidate for the position.

He brings strong experience in cloud technologies and API/backend development. In addition, he has worked with various front-end tools and libraries such as Kendo UI and Bootstrap. While he does not have prior hands-on experience with React, he has expressed a strong willingness to learn and work with it.

Overall, we believe he would be a valuable addition to our team, and we recommend moving forward with him.

Best regards,
Niraj







Subject: Interview Feedback – Vargavish Thakur

Dear [Recipient's Name],

We have completed our evaluation of Mr. Vargavish Thakur for the current role. After careful consideration, we have found that he is not a suitable fit for this position at this time.

Both Nancy and I assessed him across front-end, back-end, and database areas. We appreciate his time and effort in participating in the interview process.

We wish him all the very best in his future endeavors.

Best regards,
Shiva
[Your Job Title]
[Your Contact Info, if needed]







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
