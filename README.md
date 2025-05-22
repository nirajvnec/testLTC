SELECT 
    SERVERPROPERTY('EngineEdition') AS EngineEdition,
    SERVERPROPERTY('Edition') AS Edition,
    SERVERPROPERTY('ProductVersion') AS ProductVersion,
    SERVERPROPERTY('InstanceName') AS InstanceName;