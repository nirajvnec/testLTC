<div className={layoutStyles.configurationToolbar}>
  <div className={layoutStyles.leftDropdown}>
    <select>
      <option value="">Select Option</option>
    </select>
  </div>

  <div className={layoutStyles.gridTopActionButtons}>
    <div className={styles.gridActionButtonsContainer}>
      <Button>Save</Button>
      <Button>Add</Button>
      <Button>Edit</Button>
    </div>
  </div>
</div>



.configurationToolbar {
  display: grid;
  grid-template-columns: 1fr 1fr;
  grid-row: 4;
  align-items: center;
}



.leftDropdown {
  grid-column: 1;
  justify-self: start;
}

.gridTopActionButtons {
  grid-column: 2;
  justify-self: end;
}

.gridActionButtonsContainer {
  display: grid;
  grid-template-columns: auto auto auto;
  gap: 5px;
}
