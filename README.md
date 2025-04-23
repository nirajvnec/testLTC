import React, { useEffect, useState } from 'react';

const MyComponent = () => {
  const [allTableNames, setAllTableNames] = useState<string[]>([]);
  const [selectedTableColumnNames, setSelectedTableColumnNames] = useState<string[]>([]);

  useEffect(() => {
    const fetchData = async () => {
      const allTables = await attributeConfigurationService.getAllConfigTables();
      const tableNamesOnly = allTables.map(t => t.tableName);
      setAllTableNames(tableNamesOnly);
      console.log('allTableNames', tableNamesOnly);

      const columns = await attributeConfigurationService.getSelectedTableInfo(
        'reference.attribute_filter_master_key'
      );
      const columnNamesOnly = columns.map(col => col.name);
      setSelectedTableColumnNames(columnNamesOnly);
      console.log('selectedTableColumnNames', columnNamesOnly);

      const data = await attributeConfigurationService.getSelectedTableColumnData(
        'reference.attribute_filter_master_key'
      );
      console.log('selectedTableData', data);
    };

    fetchData();
  }, []);
  
  return <div>Check console output!</div>;
};
