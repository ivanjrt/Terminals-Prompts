# Install and load the Req:
```js
Install-Module -Name PowerShellAI
Import-Module  -Name PowerShellAI
```

Create a new StartUp Profile: <br/>
```js
New-item –type file –force $profile
```
This will create a file structure like: ```C:\Users\user\Documents\PowerShell\Microsoft.PowerShell_profile.ps1```<br/>

The key for ChatGPT could be more plain, if you follow this steps below, although no recommend it<br/>
```js
#$ApikeyText = "sk-keygoeshere"
#$APIsecureIt = ConvertTo-SecureString $ApikeyText -AsPlainText -Force
```

Add the below in the content of this file:```C:\Users\user\Documents\PowerShell\Microsoft.PowerShell_profile.ps1``` <br/>
```js
#Auth for API
$key = "cwBrAC0AZwBGADgAMQBNAGoATwBMADMAVQBoAFMAUQBNAGIARQBnAGoAZgBSAFQAMwBCAGwAYgBrAEYASgBuAFQASQBnAHoAeQAyAE8AdQB2AGEAeABvAHUANgB5AGgAbgByAFcA"
$ApikeyText = [System.Text.Encoding]::Unicode.GetString([System.Convert]::FromBase64String($key))
$APIsecureIt = ConvertTo-SecureString $ApikeyText -AsPlainText -Force
Set-OpenAIKey -Key ($APIsecureIt)

# Example to make the calls:
#ai "list of planets only names as json" | ai 'convert to  xml' | ai 'convert to  powershell'


#Adding more flare on the looks
function prompt {

    #Assign Windows Title Text
    $ip                        = (Get-NetIPAddress | Where-Object {$_.IpAddress -like "10.*"}).IpAddress
    $host.ui.RawUI.WindowTitle = "INT IP: $($ip)"

    #Configure current user, current folder and date outputs
    $CmdPromptCurrentFolder = Split-Path -Path $pwd -Leaf
    $CmdPromptUser          = [Security.Principal.WindowsIdentity]::GetCurrent()
    $Date                   = Get-Date -Format 'dddd MM/dd hh:mm:ss tt'

    # Test for Admin / Elevated
    $IsAdmin                = (New-Object Security.Principal.WindowsPrincipal (
                              [Security.Principal.WindowsIdentity]::GetCurrent())).IsInRole(
                              [Security.Principal.WindowsBuiltinRole]::Administrator)

    #Calculate execution time of last cmd and convert to milliseconds, seconds or minutes
    $LastCommand = Get-History -Count 1
    if ($lastCommand) { $RunTime = ($lastCommand.EndExecutionTime - $lastCommand.StartExecutionTime).TotalSeconds}

    if ($RunTime -ge 60) {
        $ts          = [timespan]::fromseconds($RunTime)
        $min, $sec   = ($ts.ToString("mm\:ss")).Split(":")
        $ElapsedTime = -join ($min, " min ", $sec, " sec")
    }else{
        $ElapsedTime = [math]::Round(($RunTime), 2)
        $ElapsedTime = -join (($ElapsedTime.ToString()), " sec")
    }

    #Decorate the CMD Prompt
    Write-Host ""
    Write-host ($(if ($IsAdmin) { 'Elevated ' } else { '' })) -BackgroundColor DarkRed -ForegroundColor White -NoNewline
    Write-Host " USER:$($CmdPromptUser.Name.split("\")[1]) "  -BackgroundColor DarkBlue -ForegroundColor White -NoNewline
    If ($CmdPromptCurrentFolder -like "*:*")
        {Write-Host "$(Get-Location)\µ "  -ForegroundColor black -BackgroundColor DarkYellow -NoNewline}
    else {Write-Host "$(Get-Location)\µ " -ForegroundColor black -BackgroundColor DarkYellow -NoNewline}

    Write-Host " $date "         -ForegroundColor White
    Write-Host "[$elapsedTime] " -NoNewline -ForegroundColor Green
    return "> "
} #end prompt function
```
