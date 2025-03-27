const isViewOnly = hasViewAccess && !hasEditAccess;

if (isViewOnly) {
  console.log("User has only view access");
}
