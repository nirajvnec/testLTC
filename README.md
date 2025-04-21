.gridTopActionButtons {
  grid-row: 4; /* Keep on the 4th row */
  grid-column: 1 / span 8; /* Span across all columns to contain everything */
  display: flex;
  justify-content: space-between; /* This creates space between left and right button groups */
  align-items: center;
}

.gridFirstButton {
  grid-column: 2; /* First content column (after the 5vh margin column) */
  justify-self: start;
  /* Remove any grid-row property here as positioning is handled by parent */
}

.gridActionButtonsContainer {
  display: grid;
  grid-template-columns: auto auto auto;
  gap: 5px;
  justify-self: end; /* Align to the right */
  /* Position in columns before the right margin */
  grid-column: 5 / span 3; /* Columns 5-7 (before the final 5vh margin) */
}
