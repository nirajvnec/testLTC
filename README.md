const userRoles = (userContext.assignedRoles || []).map(role => 
    role.replace("MIM_ABT-GWG_", "")
);

console.log("User Roles:", userRoles); // Check the modified roles in the console

console.log("User Roles:", userRoles); // Log all assigned roles

const hasEditAccess = userRoles.includes("APP_MTRM_RCO");
console.log("Has Edit Access:", hasEditAccess); // Check if edit access is granted

const hasViewAccess = userRoles.some(role => 
    ["APP_MTRM_MRC", "APP_MTRM_USER", "APP_MTRM_RRA"].includes(role)
);
console.log("Has View Access:", hasViewAccess); // Check if view access is granted
