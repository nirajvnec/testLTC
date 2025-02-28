import { useEffect } from "react";

const MyComponent = () => {
  useEffect(() => {
    console.log("DOM fully loaded, applying styles...");

    // Select the div with the dynamic class
    const divElement = document.querySelector('[class^="UWR_Accordion_header-content"]');

    if (divElement) {
      (divElement as HTMLElement).style.marginTop = "14px";
    }

  }, []); // Runs only once after component mounts

  return <div>My Component</div>;
};

export default MyComponent;
