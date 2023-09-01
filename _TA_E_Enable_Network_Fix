# Function to retrieve network adapter information using WMI
Function Get-NetworkAdapters {
    $networkAdapters = Get-WmiObject -Class Win32_NetworkAdapterConfiguration
    return $networkAdapters | Select-Object Description, IPAddress, DefaultIPGateway, DNSServerSearchOrder
}

# Check if running with administrator privileges
$adminRights = ([Security.Principal.WindowsPrincipal] [Security.Principal.WindowsIdentity]::GetCurrent()).IsInRole([Security.Principal.WindowsBuiltInRole]::Administrator)

if (-not $adminRights) {
    Write-Host "This script requires administrator privileges to run."

    if (Read-Host "Do you want to run the script with administrator privileges? (y/n)" -eq "y") {
        Start-Process -FilePath "powershell.exe" -ArgumentList "-NoProfile -ExecutionPolicy Bypass -File $($MyInvocation.MyCommand.Path)" -Verb RunAs
        Exit
    } else {
        Write-Host "Script execution aborted."
        Exit
    }
}

# Function to save network adapter information to a text file
Function Save-InfoToFile {
    param (
        [string]$FilePath,
        [string]$Info
    )
    
    $Info | Set-Content -Path $FilePath
    Write-Host "Network adapter information saved to $FilePath"
}

# Retrieve network adapter information using WMI
$adapterInfo = Get-NetworkAdapters

# Display network adapter information
Write-Host "Network Adapters on this Computer:"
$adapterInfo | Format-Table -AutoSize

# Prompt the user for the file path and name to save the information
$filePath = Read-Host "Enter the file path and name (e.g., C:\Path\To\FileName.txt) to save network adapter information:"
if (-not (Test-Path (Split-Path $filePath))) {
    Write-Host "The specified path does not exist."
    Exit
}

# Save network adapter information to the specified text file
Save-InfoToFile -FilePath $filePath -Info ($adapterInfo | Format-Table -AutoSize | Out-String)

# All steps are completed
Write-Host "All steps are completed."

# Pause to keep the PowerShell window open
Read-Host "Press Enter to exit..."
