import React from "react";
import { Overlay } from "@uwr/react"; // Assuming correct import

// Define the type for props
type CoBEmailRequestPopupProps = {
  show: boolean;
  onClose: () => void;
};

function CoBEmailRequestPopup(props: CoBEmailRequestPopupProps) {
  return (
    <React.Fragment>
      <Overlay
        style={{ maxWidth: "95%", width: "1530px", marginTop: "5px", maxHeight: "auto" }}
        id="cob-email-request-dialog"
        show={props.show}
        closeLabel="Close"
        hasCloseButton
        onHide={props.onClose}
      >
        <Overlay.Header>
          <h2>Request for Historical CoB Smartclose Email</h2>
        </Overlay.Header>

        <Overlay.Body>
          <div style={{ display: "flex", flexDirection: "column", gap: "10px" }}>
            {/* Historical CoB Date Input */}
            <div>
              <label htmlFor="historical-cob-date">Historical CoB Date:</label>
              <input type="date" id="historical-cob-date" />
            </div>

            {/* Reason Input */}
            <div>
              <label htmlFor="reason">Reason:</label>
              <input type="text" id="reason" placeholder="Enter reason..." />
            </div>

            {/* Request Button */}
            <div>
              <button style={{ backgroundColor: "#007bff", color: "white", padding: "10px 15px", border: "none", cursor: "pointer" }}>
                Request
              </button>
            </div>
          </div>
        </Overlay.Body>
      </Overlay>
    </React.Fragment>
  );
}

export default CoBEmailRequestPopup;
