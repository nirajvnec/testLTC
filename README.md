.formRow {
  grid-row: 4;
  grid-column: 2 / span 5;
  display: flex;
  justify-content: space-between; /* <-- This is key */
  align-items: flex-end;
}


.buttonGroup {
  display: flex;
  gap: 10px;
  margin-left: auto; /* Pushes buttons to the right */
}


<div className={styles.formRow}>
  <Dropdown ... />

  <div className={styles.buttonGroup}>
    <Button ... />
    <Button ... />
    <Button ... />
  </div>
</div>
