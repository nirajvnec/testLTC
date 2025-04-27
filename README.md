interface ColumnMeta {
  field: string;
  type: 'string' | 'number' | 'date';
  flex?: number;
}

type ColumnOverrides = {
  [fieldName: string]: Partial<any>;
};

export const buildColumnDefs = (columns: ColumnMeta[], overrides?: ColumnOverrides) => {
  return columns.map(col => {
    const colDef: any = {
      field: col.field,
      headerName: col.field,
      flex: col.flex || 1,
      minWidth: 100,
      sortable: true,
      resizable: true,
    };

    if (col.type === 'number') {
      colDef.type = 'numericColumn';
      colDef.filter = 'agNumberColumnFilter';
    } else if (col.type === 'string') {
      colDef.filter = 'agTextColumnFilter';
    } else if (col.type === 'date') {
      colDef.filter = 'agDateColumnFilter';
      colDef.valueFormatter = (params: any) => {
        if (!params.value) return '';
        return new Date(params.value).toLocaleDateString();
      };
    }

    // If there is an override for this column, apply it
    if (overrides && overrides[col.field]) {
      Object.assign(colDef, overrides[col.field]);
    }

    return colDef;
  });
};
