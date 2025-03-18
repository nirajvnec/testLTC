.buttonWrapper {
  position: relative;  /* Needed for absolute positioning of children */
  display: inline-block; /* Prevents it from taking full width */
}

/* 🔴 Red Alert Badge (Top-Right Corner) */
.alertBadge {
  position: absolute;
  top: -5px;   /* Adjust distance from top */
  right: -5px; /* Adjust distance from right */
  background-color: red;
  color: white;
  font-size: 12px;
  font-weight: bold;
  padding: 2px 6px;
  border-radius: 50%;
}

/* ✅ Mark Tick Icon (Bottom-Right Corner) */
.markTickIcon {
  position: absolute;
  bottom: -5px;  /* Adjust distance from bottom */
  right: -5px;   /* Adjust distance from right */
  background-color: white; /* Optional: Background for visibility */
  border-radius: 50%;
  padding: 2px;
}
