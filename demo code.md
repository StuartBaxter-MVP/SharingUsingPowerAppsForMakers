# SharingUsingPowerAppsForMakers

//Chosen user
Set(
    gblSelectedUser,
    drpSelectUser.Selected
);
//chosen team
Set(
    gblSelectedTeam,
    drpSelectTeam.Selected
);
//Add the user to the chosen team
Relate(
    gblSelectedUser.'Teams (teammembership_association)',
    gblSelectedTeam
);
//the guid for this app
Set(
    gblApp,
    First(
        Filter(
            PowerAppsforMakers.GetApps().value,
            properties.displayName = "Sharing is caring"
        )
    )
);//Share the app with the user
PowerAppsforMakers.EditAppRoleAssignment(
    gblApp.name,
    {
        put: {
            properties: {
                roleName: "canView",
                capabilities: [""],
                NotifyShareTargetOption: "Notify",
                principal: {
                    email: gblSelectedUser.'Primary Email',
                    id: gblSelectedUser.'Azure AD Object ID',
                    type: "User",
                    tenantId: "null"
                }
            }
        }
    }
) ;
