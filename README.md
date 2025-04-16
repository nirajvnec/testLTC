const item = {
  headerName: r[Constants.COLUMN_NAME],
  field:
    r[Constants.LOV_SQL] !== null && r[Constants.LOV_SQL] !== ''
      ? r[Constants.COLUMN_NAME].replace(Constants.ID, Constants.NAME)
      : r[Constants.COLUMN_NAME],
  width: 200,
  sortable: true,
  headerClass:
    r[Constants.UPDATABLE] === Constants.CONST_NO ? Constants.GREY_HEADER : '',
  cellClass:
    r[Constants.UPDATABLE] === Constants.CONST_NO &&
    r[Constants.COLUMN_TYPE] === Constants.DATEFORMAT_DDMONYYYY
      ? Constants.GREY_FORMAT_CELL
      : r[Constants.COLUMN_TYPE] === Constants.DATEFORMAT_DDMONYYYY
        ? Constants.FORMAT_DATE_TO_CELL
        : r[Constants.UPDATABLE] === Constants.CONST_NO
          ? r[Constants.LOV_SQL] !== null && r[Constants.LOV_SQL] !== ''
            ? ''
            : Constants.GREY_CELL
          : '',
  cellStyle:
    r[Constants.UPDATABLE] === Constants.CONST_NO
      ? r[Constants.LOV_SQL] !== null && r[Constants.LOV_SQL] !== ''
        ? {}
        : { backgroundColor: Constants.GREY_HEX }
      : {},
  // ✅ valueFormatter added here
  valueFormatter:
    r[Constants.COLUMN_NAME] === 'VALID_FROM'
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
};
