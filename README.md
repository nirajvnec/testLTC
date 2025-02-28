import { useEffect } from "react";

const MyComponent = () => {
  useEffect(() => {
    console.log("DOM fully loaded, running JavaScript...");

    // Example: Modify button height after DOM is ready
    document.querySelectorAll("#accordionEvents, #accordionSubscriptions").forEach((btn) => {
      (btn as HTMLElement).style.height = "80px";
      (btn as HTMLElement).style.minHeight = "80px";
    });

  }, []); // Empty dependency array ensures this runs only once

  return <div>My Component</div>;
};

export default MyComponent;
