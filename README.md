.buttonWrapper {
  position: relative;  /* Ensures Alert is positioned relative to this container */
  display: inline-block; /* Prevents expanding to full width */
}

.alertBadge {
  position: absolute;
  top: -5px;   /* Move slightly above */
  right: -5px; /* Move to the right */
  background-color: red; /* Ensures visibility */
  color: white;
  font-size: 12px;
  font-weight: bold;
  padding: 2px 6px;
  border-radius: 50%; /* Circular appearance */
}
