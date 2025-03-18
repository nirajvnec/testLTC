import React, { useState } from "react";
import CoBEmailRequestPopup from "./CoBEmailRequestPopup";

const App = () => {
  const [showPopup, setShowPopup] = useState(false);

  return (
    <div>
      <button onClick={() => setShowPopup(true)}>Open Popup</button>
      {showPopup && <CoBEmailRequestPopup show={showPopup} onClose={() => setShowPopup(false)} />}
    </div>
  );
};

export default App;
