---
layout: default
title: Import-AppVManagementPackage
nav_exclude: true
---

# Import-AppVManagementPackage

Imports App-V packages from the App-V Management Database into AppVentiX.

## Syntax

```powershell
Import-AppVManagementPackage
    [-SQLServer <String>]
    [-SQLInstance <String>]
    [-SQLDatabase <String>]
    [-SQLCredential <PSCredential>]
    [-MatchPackageWithMachineGroup]
    [-GUI]
    [<CommonParameters>]
```

## Description

The `Import-AppVManagementPackage` function retrieves App-V packages from the App-V Management Database and imports them into AppVentiX. This function allows you to migrate packages from the Microsoft App-V Management Server to AppVentiX for seamless publishing.

For each package imported, the function:
1. Connects to the App-V Management database
2. Retrieves package details including AD group assignments
3. Matches packages to AppVentiX content shares
4. Creates publishing tasks in AppVentiX

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

### -MatchPackageWithMachineGroup

When specified, automatically matches packages to AppVentiX machine groups based on their content share location. Without this switch, packages are published to "All Machine Groups".

| | |
|---|---|
| Type: | SwitchParameter |
| Position: | Named |
| Default value: | False |
| Accept pipeline input: | False |
| Accept wildcard characters: | False |

### -GUI

When specified, displays a graphical user interface to select which packages to import. This allows you to review and choose specific packages instead of importing all available packages.

| | |
|---|---|
| Type: | SwitchParameter |
| Position: | Named |
| Default value: | False |
| Accept pipeline input: | False |
| Accept wildcard characters: | False |

## Examples

### Example 1: Import all packages using (current) Windows Authentication

```powershell
Import-AppVManagementPackage -SQLServer "sql01.domain.local"
```

Imports all enabled App-V packages from the Management database on the specified SQL Server using Windows Integrated Authentication.

### Example 2: Import using SQL Authentication

```powershell
$cred = Get-Credential
Import-AppVManagementPackage -SQLServer "sql01.domain.local" -SQLCredential $cred
```

### Example 3: Import packages with GUI selection

```powershell
Import-AppVManagementPackage -SQLServer "sql01.domain.local" -GUI
```

Displays a GUI dialog to select which packages to import from the Management database.

Prompts for SQL Server credentials and imports packages using SQL Server Authentication.

### Example 4: Import with Machine Group matching and showing the GUI

```powershell
$params = @{
    SQLServer = "sql01.domain.local"
    MatchPackageWithMachineGroup = $true
    GUI = $true
}
Import-AppVManagementPackage @params
```

Displays a GUI for package selection and automatically assigns packages to the appropriate machine groups based on their content share location.

### Example 5: Import from named SQL instance

```powershell
$params = @{
    SQLServer = "sql01.domain.local"
    SQLInstance = "SQLINSTANCE"
    SQLDatabase = "AppVManagement"
}
Import-AppVManagementPackage @params"
```

Imports packages from a named SQL Server instance.

## Output

The function outputs a PSCustomObject for each imported package with the following properties:

| Property | Description |
|----------|-------------|
| Name | The name of the imported package |
| Id | The ID of the created publishing task |

## Notes

- Requires a valid AppVentiX license
- Requires the SqlServer PowerShell module
- Disabled packages are skipped during import
- Only packages with valid UNC paths are imported

## Related Links

- [Import-AppVManagementConnectionGroup](Import-AppVManagementConnectionGroup.html)
- [New-AppVentiXPublishingTask](New-AppVentiXPublishingTask.html)
- [Get-AppVManagementPackage](Get-AppVManagementPackage.html)
