valueFormatter:
  ['VALID_FROM', 'VALID_TO', 'LAST_UPDATED_TIMESTAMP'].includes(r[Constants.COLUMN_NAME])
    ? (params: any) =>
        params.value
          ? moment(params.value).isValid()
            ? moment(params.value).format('DD-MMM-YYYY')
            : params.value
          : ''
    : undefined,
