import { useEffect } from "react";

const MyComponent = () => {
  useEffect(() => {
    console.log("DOM fully loaded, applying styles...");

    // Select all divs whose class starts with "UWR_Accordion_header-content"
    const divElements = document.querySelectorAll('[class^="UWR_Accordion_header-content"]');

    divElements.forEach((div) => {
      (div as HTMLElement).style.marginTop = "14px";
    });

  }, []); // Runs only once after component mounts

  return <div>My Component</div>;
};

export default MyComponent;
