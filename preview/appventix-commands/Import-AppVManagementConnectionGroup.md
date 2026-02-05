---
layout: default
title: Import-AppVManagementConnectionGroup
nav_exclude: true
---

# Import-AppVManagementConnectionGroup

Imports App-V Connection Groups from the App-V Management Database to the AppVentiX configuration.

## Syntax

```powershell
Import-AppVManagementConnectionGroup
    [-SQLServer <String>]
    [-SQLInstance <String>]
    [-SQLDatabase <String>]
    [-SQLCredential <PSCredential>]
    [-MatchConnectionGroupWithMachineGroup]
    [-GUI]
    [<CommonParameters>]
```

## Description

The `Import-AppVManagementConnectionGroup` function retrieves Connection Groups from the App-V Management Database and imports them into AppVentiX. This function allows you to import Connection Groups from the Management Server to make them available for publishing.

For each Connection Group imported, the function:
1. Connects to the App-V Management database
2. Retrieves Connection Group details including member packages and AD group assignments
3. Matches packages to AppVentiX content shares
4. Creates the Connection Group in AppVentiX
5. Creates publishing tasks for the Connection Group

## Parameters

### -SQLServer

Specifies the SQL Server hostname or IP address where the App-V Management database is hosted.

| | |
|---|---|
| Type: | String |
| Position: | Named |
| Default value: | localhost |
| Accept pipeline input: | False |
| Accept wildcard characters: | False |

### -SQLInstance

Specifies the SQL Server instance name. If not specified, the default instance will be used.

| | |
|---|---|
| Type: | String |
| Position: | Named |
| Default value: | None (default instance) |
| Accept pipeline input: | False |
| Accept wildcard characters: | False |

### -SQLDatabase

Specifies the App-V Management database name.

| | |
|---|---|
| Type: | String |
| Position: | Named |
| Default value: | AppVManagement |
| Accept pipeline input: | False |
| Accept wildcard characters: | False |

### -SQLCredential

Specifies the PSCredential object for SQL Server authentication. If not specified, Windows Integrated Authentication will be used.

| | |
|---|---|
| Type: | PSCredential |
| Position: | Named |
| Default value: | None (Windows Authentication) |
| Accept pipeline input: | False |
| Accept wildcard characters: | False |

### -MatchConnectionGroupWithMachineGroup

When specified, automatically matches Connection Groups to AppVentiX machine groups based on their member packages' content share locations. If packages belong to multiple machine groups, the Connection Group is published to "All Machine Groups".

| | |
|---|---|
| Type: | SwitchParameter |
| Position: | Named |
| Default value: | False |
| Accept pipeline input: | False |
| Accept wildcard characters: | False |

### -GUI

When specified, displays a graphical user interface to select which Connection Groups to import. This allows you to review and choose specific Connection Groups instead of importing all available ones.

| | |
|---|---|
| Type: | SwitchParameter |
| Position: | Named |
| Default value: | False |
| Accept pipeline input: | False |
| Accept wildcard characters: | False |

## Examples

### Example 1: Import all Connection Groups using Windows Authentication

```powershell
Import-AppVManagementConnectionGroup -SQLServer "sql01.domain.local"
```

Imports all enabled Connection Groups from the Management database on the specified SQL Server using Windows Integrated Authentication.

### Example 2: Import using SQL Authentication

```powershell
$cred = Get-Credential
Import-AppVManagementConnectionGroup -SQLServer "sql01.domain.local" -SQLCredential $cred
```

Prompts for SQL Server credentials and imports Connection Groups using SQL Server Authentication.

### Example 3: Import Connection Groups with GUI selection

```powershell
Import-AppVManagementConnectionGroup -SQLServer "sql01.domain.local" -GUI
```

Displays a GUI dialog showing Connection Group details (ID, Name, Enabled status, Priority, Version, Description) to select which Connection Groups to import.

### Example 4: Import with Machine Group matching

```powershell
$params = @{
    SQLServer = "sql01.domain.local"
    MatchConnectionGroupWithMachineGroup = $true
    GUI = $true
}
Import-AppVManagementConnectionGroup @params
```

Displays a GUI for Connection Group selection and automatically assigns Connection Groups to the appropriate machine groups based on their member packages' content share locations.

### Example 5: Import from named SQL instance

```powershell
$params = @{
    SQLServer = "sql01.domain.local"
    SQLInstance = "SQLINSTANCE"
    SQLDatabase = "AppVManagement"
}
Import-AppVManagementConnectionGroup @params
```

Imports Connection Groups from a named SQL Server instance.

### Example 6: Complete migration workflow

```powershell
# Store credentials for reuse
$sqlCred = Get-Credential

# First import all packages
Import-AppVManagementPackage -SQLServer "sql01.domain.local" `
    -SQLCredential $sqlCred `
    -MatchPackageWithMachineGroup

# Then import Connection Groups
Import-AppVManagementConnectionGroup -SQLServer "sql01.domain.local" `
    -SQLCredential $sqlCred `
    -MatchConnectionGroupWithMachineGroup
```

## Output

The function outputs a PSCustomObject for each imported Connection Group with the following properties:

| Property | Description |
|----------|-------------|
| Name | The name of the imported Connection Group |
| Id | The ID of the created publishing task |

## Notes

- Requires a valid AppVentiX license
- Requires the SqlServer PowerShell module
- Disabled Connection Groups are skipped during import
- Only packages with valid UNC paths are included
- Connection Group priority is preserved during import
- AD group assignments are converted to AppVentiX publishing tasks
- If member packages span multiple machine groups, the Connection Group is published to "All Machine Groups"

## Connection Group Package Settings

When importing Connection Groups, the following package settings are preserved:

| Setting | Description |
|---------|-------------|
| Optional | Whether the package is optional in the Connection Group |
| UseAnyVersion | Whether any version of the package can be used |

## Related Links

- [Import-AppVManagementPackage](Import-AppVManagementPackage.html)
- [New-AppVentiXConnectionGroup](New-AppVentiXConnectionGroup.html)
- [New-AppVentiXPublishingTask](New-AppVentiXPublishingTask.html)
- [Get-AppVManagementConnectionGroup](Get-AppVManagementConnectionGroup.html)
