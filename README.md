const { referenceDataService }: { referenceDataService: IReferenceDataService } = Utilities.defaults({
    referenceDataService: Utilities.isTruthy(getMarvelControlsEnv().MARVEL_APP_USE_REFERENCATASERVICE_MOCK)
        ? new ReferenceDataServiceMock(logger)
        : new ReferenceDataService(logger),
});
