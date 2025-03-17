<div className={styles.gridMessageBoxesContainer}>
  <div className={styles.buttonWrapper}>
    {/* Alert Positioned Absolutely */}
    <Alert className={styles.alertBadge}>9</Alert>

    {/* Button */}
    <Button
      size={Button.size.LARGE}
      type={Button.type.SECONDARY}
      icon={<Envelope16px />}
      disabled={readOnly}
      id="btnAddnewreportconfiguration"
      aria-label="Request Email For CoB"
      onClick={() => {
        addReportConfig();
      }}
    />
  </div>
</div>
