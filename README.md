const { 
  referenceDataService, 
  service 
}: { 
  referenceDataService: IReferenceDataService; 
  service: IPreverificationReportService; 
} = Utilities.defaults({
  referenceDataService: Utilities.isTruthy(true)
    ? new ReferenceDataServiceMock(logger)
    : new ReferenceDataService(logger),
  service: Utilities.isTruthy(getMarvelControlsEnv().MARVEL_APP_USE_REFERENCEDATASERVICE_MOCK)
    ? new PreverificationReportServiceMock()
    : new PreverificationReportService(logger, userContext),
});
