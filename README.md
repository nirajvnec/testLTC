<SubDeskSelector
  ...
  onError={(errors: string[]) => {
    setApiErrors(errors);
    setApiErrorsOverlayOpen(true);
  }}
/>