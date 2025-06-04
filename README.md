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
