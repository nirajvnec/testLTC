.configContainer {
  display: grid;
  grid-template-rows: auto auto 1fr auto; // title + top bar + grid + pagination
  height: 100vh;
  padding: 1rem;
  font-family: Arial, sans-serif;
}

.pageTitle {
  margin-bottom: 0.5rem;

  h2 {
    font-size: 1.5rem;
    font-weight: bold;
    margin: 0;
  }
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
  {/* First Row - Page Title */}
  <div className={layoutStyles.pageTitle}>
    <h2>Configure Attributes</h2>
  </div>

  {/* Second Row - Top Bar with Dropdown and Buttons */}
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

