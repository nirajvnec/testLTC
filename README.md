valueFormatter:
  columnName === 'VALID_FROM' || columnName === 'VALID_TO' || columnName === 'LAST_UPDATED_TIMESTAMP'
    ? (params: any) => {
        const date = new Date(params.value);
        return date instanceof Date && !isNaN(date.getTime())
          ? date.toLocaleDateString('en-GB', {
              day: '2-digit',
              month: 'short',
              year: 'numeric',
            }).replace(/ /g, '-')
          : params.value;
      }
    : undefined,
