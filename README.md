const [isViewOnly, setIsViewOnly] = useState(false);

useEffect(() => {
  setIsViewOnly(hasViewAccess && !hasEditAccess);
}, [hasViewAccess, hasEditAccess]);

console.log("User has only view access:", isViewOnly);
