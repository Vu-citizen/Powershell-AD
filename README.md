# PowerShell Active Directory Automation  
Automate user creation and management in Active Directory using PowerShell scripts.

---

## Technologies & Software Used  
- **Windows PowerShell**  
- **PowerShell Scripts**

---

## Steps to Execute the Project

### Step 1: Open PowerShell ISE with Administrator Privileges  
Ensure that you launch PowerShell ISE as an administrator to execute the scripts successfully.

---

### Step 2: Run the `1_CREATE_USERS.ps1` Script  
Use the premade script to create users in Active Directory. Below is the content of the script:

```powershell
# ----- Edit these Variables for your own Use Case ----- #
$PASSWORD_FOR_USERS   = "Password1"
$USER_FIRST_LAST_LIST = Get-Content .\names.txt
# ------------------------------------------------------ #

$password = ConvertTo-SecureString $PASSWORD_FOR_USERS -AsPlainText -Force
New-ADOrganizationalUnit -Name _USERS -ProtectedFromAccidentalDeletion $false

foreach ($n in $USER_FIRST_LAST_LIST) {
    $first = $n.Split(" ")[0].ToLower()
    $last = $n.Split(" ")[1].ToLower()
    $username = "$($first.Substring(0,1))$($last)".ToLower()
    Write-Host "Creating user: $($username)" -BackgroundColor Black -ForegroundColor Cyan
    
    New-AdUser -AccountPassword $password `
               -GivenName $first `
               -Surname $last `
               -DisplayName $username `
               -Name $username `
               -EmployeeID $username `
               -PasswordNeverExpires $true `
               -Path "ou=_USERS,$(([ADSI]`"").distinguishedName)" `
               -Enabled $true
}

### Step 3: Optimize and Execute the Script
1. **Set Execution Policy**:  
   Run the following command in PowerShell to allow the script to execute:  
   ```powershell
   Set-ExecutionPolicy Unrestricted
2
