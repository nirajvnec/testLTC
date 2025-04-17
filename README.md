const rollToDateLabel = Utilities.formatMMDDYYYYorYYYYMMDD(props.toDate);

return (
  <span className={styles.label}>
    Roll to : {rollToDateLabel}
  </span>
);
