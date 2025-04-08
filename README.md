<div className={styles.gridMessageBoxesTopContainer}>
  <div className={styles.gridMessageBoxesContainer}>
    
    {/* Add New */}
    <Button
      size={Button.size.MEDIUM}
      type={Button.type.SECONDARY}
      id="btnAddNewReportConfiguration"
      aria-label="Add New"
      onClick={addReportConfig}
      className={styles.buttonWrapper}
    >
      <div className={styles.iconWrapper}>
        <Envelope16px />
        <span className={styles.alertBadge}>{emailStatus.sent}</span>
        <MarkTick12px className={styles.markTickIcon} />
      </div>
    </Button>

    {/* Edit */}
    <Button
      size={Button.size.MEDIUM}
      type={Button.type.SECONDARY}
      id="btnEditReportConfiguration"
      onClick={editReportConfig}
      className={styles.buttonWrapper}
    >
      <div className={styles.iconWrapper}>
        <Envelope16px />
        <span className={styles.alertBadge}>{emailStatus.pending}</span>
        <RiskHigh16px className={styles.riskIcon} />
      </div>
    </Button>

    {/* Delete */}
    <Button
      size={Button.size.MEDIUM}
      type={Button.type.SECONDARY}
      id="btnDeleteReportConfiguration"
      onClick={deleteReportConfig}
      className={styles.buttonWrapper}
    >
      <div className={styles.iconWrapper}>
        <Envelope16px />
        <span className={styles.alertBadge}>{emailStatus.failed}</span>
        <MarkClose12px className={styles.closeIcon} />
      </div>
    </Button>

  </div>
</div>





.iconWrapper {
  position: relative;
  display: inline-block;
  line-height: 1;
}

.alertBadge {
  position: absolute;
  top: -10px;
  right: -10px;
  background-color: #367588;
  color: white;
  font-size: 10px;
  font-weight: bold;
  padding: 2px 6px;
  border-radius: 12px;
  min-width: 20px;
  text-align: center;
  z-index: 2;
}

.markTickIcon,
.riskIcon,
.closeIcon {
  position: absolute;
  bottom: -8px;
  left: -6px;
  background-color: white;
  border-radius: 50%;
  padding: 1px;
  font-size: 12px;
  color: inherit;
}
