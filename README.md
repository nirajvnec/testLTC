.config-container {
  display: grid;
  grid-template-rows: auto 1fr auto;
  height: 100vh;
  padding: 1rem;
  font-family: Arial, sans-serif;
}

.top-bar {
  display: flex;
  justify-content: space-between;
  align-items: center;
  margin-bottom: 1rem;
}

.config-dropdown {
  width: 250px;
  padding: 0.5rem;
}

.button-group button {
  margin-left: 10px;
  padding: 0.5rem 1rem;
}

.grid-area {
  border: 1px solid #ccc;
  background-color: #f9f9f9;
  display: flex;
  justify-content: center;
  align-items: center;
  height: 300px;
}

.grid-message {
  background-color: #f5e9c7;
  padding: 1rem;
  box-shadow: 0 0 5px #aaa;
  max-width: 300px;
  text-align: center;
  font-size: 14px;
}

.footer-bar {
  display: flex;
  justify-content: space-between;
  margin-top: 1rem;
  align-items: center;
  font-size: 14px;
}

.page-size select {
  margin-left: 5px;
  padding: 0.2rem;
}

.pagination button {
  margin: 0 5px;
  padding: 0.3rem 0.6rem;
}



<div class="config-container">
  <div class="top-bar">
    <select class="config-dropdown">
      <option value="">Configuration table</option>
    </select>
    <div class="button-group">
      <button>Save</button>
      <button>Add</button>
      <button>Edit</button>
    </div>
  </div>

  <div class="grid-area">
    <div class="grid-message">If no Configuration table selected – empty grid</div>
  </div>

  <div class="footer-bar">
    <div class="page-size">
      Page Size:
      <select>
        <option>20</option>
        <option>50</option>
        <option>100</option>
      </select>
    </div>
    <div class="pagination">
      <span>1 to 2 of 2</span>
      <button>&lt;</button>
      <span>Page 1 of 1</span>
      <button>&gt;</button>
    </div>
  </div>
</div>

