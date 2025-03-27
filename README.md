 // Role-based access flags
    const userRoles = userContext.assignedRoles || [];
    const hasEditAccess = userRoles.includes("APP_MTRM_RCO");
    const hasViewAccess = userRoles.some(role => 
        ["APP_MTRM_MRC", "APP_MTRM_USER", "APP_MTRM_RRA"].includes(role) 
    );
