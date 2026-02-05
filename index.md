---
layout: default
title: Home
nav_order: 1
---

# AppVentiX PowerShell Module

Welcome to the documentation for the **AppVentiX** PowerShell Module. This module provides tools for managing App-V packages and enabling seamless application publishing.

## Overview

AppVentiX is a comprehensive solution for managing packages like App-V and MSIX and much more:

- Creating publishing tasks for seamless application delivery
- Managing machine groups and content shares
- Configuring deployment and user configurations

## Getting Started

### Prerequisites

- Windows PowerShell 5.1 or later
- Valid AppVentiX license

### Installation

```powershell
# Installing the latest module from the PowerShell Gallery:
Install-Module -Name AppVentiX -Force

# Import the module
Import-Module AppVentiX
```

### Configuration

Before using the module, a connection must be made to the config share.
When the AppVentiX Powershell module is installed on the same machine as the AppVentiX Central management Console it will automatically detect the config share when configured correctly.
You can also configure the share manually if the detection does not succeed or is being run from a different machine. To configure your AppVentiX environment:

```powershell
# Set the configuration share path (Not needed if detected automatically)
Set-AppVentiXConfigShare -Path "\\fileserver.domain.com\config$"

# If credentials are required to connect to the Configuration Share, you can provide these
# E.g. if the current user does not have permissions to access the config share

$Credential = Get-Credential -Message "Enter AppVentiX Config Share credentials"
$ConfigShare = "\\fileserver.domain.local\config$"
Set-AppVentiXConfigShare -ConfigShare $ConfigShare -Credential $Credential

# If Separate Credentials are required for communication with Active Directory, you must specify this beforehand.
# By default it connects on port 389, you can change the port by specifying the -Port parameter valid values are 389 or 636

$ADCredential = Get-Credential -Message "Enter Active Directory credentials"
$ADDomainController = "dc01.domain.local"
Set-AppVentiXADCredential -Credential $ADCredential -Server $ADDomainController [-Port 636]

# To verify you have a valid license and are connected to the config share
Test-AppVentiXIsLicensed
```

## Support

For questions, issues, or feature requests, please contact [AppVentiX Support](../support/).

---

*Copyright (c) AppVentiX*