---
title: PowerShell
description: How to execute powershell scripts when building code in VSTS and Team Foundation Server TFS
ms.prod: vs-devops-alm
ms.technology: vs-devops-build
ms.assetid: 0D682DFA-9BC7-47A7-B0D3-C59DE1D431B5
ms.manager: douge
ms.author: alewis
ms.date: 08/10/2016
---

# Utility: PowerShell

[!INCLUDE [temp](../../_shared/version-tfs-2015-rtm.md)]

![](_img/powershell.png) Run a PowerShell script from a file or an inline script definition.

## Demands

DotNetFramework

## Arguments

| Argument | Description |
| -------- | ----------- |
| Type | Specify whether to use a file or an inline script |
| Script Path | Specify the path to the script to you want to run. The path must be a fully qualified path or a valid path relative to the default working directory. In Team Foundation Build, this directory is [$(Build.SourcesDirectory)](../../concepts/definitions/build/variables.md).  This can only be specified if `Type` is File Path |
| Inline Script | The script content to execute.  There is a 500 character limit for inline script definitions.  This can only be specified if `Type` is Inline Script |
| Arguments | Specify arguments to pass to the script.  These arguments will be interpreted the same way as they would by a regular powershell script. |
| Advanced - Working folder | Specify the working directory in which you want to run the script. If you leave it empty, the working directory is the folder where the script is located. |
| [!INCLUDE [control-options-arguments-md](../_shared/control-options-arguments-md.md)] | |

## Examples
For all of these examples you need to add a ![](_img/powershell.png) **Utility: PowerShell** build step on the Build tab of a build definition.

### Run a script from a path

Create ```test.ps1``` at the root of your repo:

```ps
Write-Host "Hello World from $Env:AGENT_NAME."
Write-Host "My ID is $Env:AGENT_ID."
Write-Host "AGENT_WORKFOLDER contents:"
gci $Env:AGENT_WORKFOLDER
Write-Host "AGENT_BUILDDIRECTORY contents:"
gci $Env:AGENT_BUILDDIRECTORY
Write-Host "BUILD_SOURCESDIRECTORY contents:"
gci $Env:BUILD_SOURCESDIRECTORY
Write-Host "Over and out."
```
**Arguments**
* **Display Name:** Run script from file.
* **Type:** Script Path
* **Script Path**: `test.ps1`

### Run an inline script

* **Display Name:** Run script from file.
* **Type:** Script Path
* **Inline Script**: ```psWrite-Host "Hello World from $Env:AGENT_NAME."```


### Write a warning

![icon](_img/powershell.png) Set warning message

* Arguments

 ```
"You've been warned by"
```

* Script

 ```ps
Write-Host "$("##vso[task.setvariable variable=WarningMessage]") $($args[0])"
```

![icon](_img/powershell.png) Write warning using task.LogIssue

* Script

 ```ps
# Writes a warning to build summary and to log in yellow text
Write-Host  "$("##vso[task.logissue type=warning;]") $($env:WarningMessage) $("the task.LogIssue Team Build logging command.")"
```

![icon](_img/powershell.png) Write warning using PowerShell command

* Script

 ```ps
# Writes a warning to log preceded by "WARNING: "
Write-Warning "$($env:WarningMessage) $("the Write-Warning PowerShell command.")"
```

### Write an error

![icon](_img/powershell.png) Set error message

* Arguments

 ```
"something went wrong."
```

* Script

 ```ps
Write-Host "$("##vso[task.setvariable variable=ErrorMessage]") $($args[0])"
```

![icon](_img/powershell.png) Write error using task.LogIssue

* Script

 ```ps
# Writes an error to the build summary and to the log in red text
Write-Host  "$("##vso[task.logissue type=error;]") $("the task.LogIssue Team Build logging command reported that") $($env:ErrorMessage)"
```

> [!TIP]
> 
> If you want this error to fail the build, then add this line:
 ```ps
exit 1
``` 

![icon](_img/powershell.png) Write error using PowerShell command

* Script

 ```ps
# Writes an error to the build summary and the log with details about the error
Write-Error "$("the Write-Error PowerShell command reported that") $($env:ErrorMessage)"
```

> [!TIP]
> 
> If you don't want this error to fail the build, then clear the **Advanced: Fail on Standard Error** check box.


### ApplyVersionToAssemblies.ps1

[Use a script to customize your build process](../../actions/scripts/powershell.md)

## Q&A

<!-- BEGINSECTION class="md-qanda" -->

### Where can I learn about PowerShell scripts?

[Scripting with Windows PowerShell](https://technet.microsoft.com/en-us/library/bb978526.aspx)

[Microsoft Script Center (the Scripting Guys)](https://technet.microsoft.com/en-us/scriptcenter/bb410849.aspx)

[Windows PowerShell Tutorial](http://www.computerperformance.co.uk/powershell/index.htm)

[PowerShell.org](http://powershell.org/)

[!INCLUDE [include](../../concepts/definitions/_shared/variable-set-in-script-qa.md)]

[!INCLUDE [temp](../_shared/build-step-common-qa.md)]

[!INCLUDE [temp](../../_shared/qa-agents.md)]

[!INCLUDE [temp](../../_shared/qa-versions.md)]

<!-- ENDSECTION -->
