.configContainer {
  display: grid;
  grid-template-rows: auto 1fr auto;
  height: 100vh;
  padding: 1rem;
  font-family: Arial, sans-serif;
}

.topBar {
  display: flex;
  justify-content: space-between;
  align-items: center;
  margin-bottom: 1rem;
}

.configDropdown {
  width: 250px;
  padding: 0.5rem;
}

.buttonGroup button {
  margin-left: 10px;
  padding: 0.5rem 1rem;
}


<div className={layoutStyles.configContainer}>
  <div className={layoutStyles.topBar}>
    <select className={layoutStyles.configDropdown}>
      <option value="">Configuration table</option>
    </select>
    <div className={layoutStyles.buttonGroup}>
      <button>Save</button>
      <button>Add</button>
      <button>Edit</button>
    </div>
  </div>
</div>

