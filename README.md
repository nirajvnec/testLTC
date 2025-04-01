If I want to restrict access to the following route for any users with the roles listed below:


['APP_MTRM_RCO', 'APP_MTRM_MRC', 'APP_MTRM_USER', 'APP_MTRM_RRA']

where should I make the necessary changes?

Route definition:

routes: [
    {
        title: 'Create or Manage: Pre-Verification Report',
        content: ManageReports,
        path: '/controls/manage/pre-verification',
        exact: true,
        allowedRoles: [APP_ROLES.APP_MTRM_USER, APP_ROLES.APP_MTRM_PUBLISHER],
        order: 1,
        hideInProd: true,
    },
],
