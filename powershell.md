

---

## PowerShell Script: Create 1000 Users

Create a script file `Create-Users.ps1`:

## Edit these varibale for your own use case

```powershell
$password_for_users = "Password1"
$USER_FIRST_LAST_LIST = Get-content .\name.text #this is to used if you have name tored in text file
##---------------------------------------
$password = ConverTo-securesstring $PASSWORD_USERS -AsPlainText -Force
New-ADOrganizationalUnit -name_USERS-ProtectedFromAccidentalDeletion $false

foreach ($n in $User_FIRST_LAST_LIST) {
  $First = $n.split (" ") [0].ToLower ()
  $Last = 4n.split (" ") [1].ToLowr ()
  $username = "$($first.substring(0,1)$($last)".ToLowwer()
  writ-Host "Creating user: $($username)" -backroundcolor Black -Foregroundcolor Cyan #you can set any color you want here

  new-AdUser -AccountPassowrd $Password `
             -givenname $first `
             -surname $last `
             -DisplayName $usrname `
             -name $username `
             -EmplayeeID $username `
             -PasswordnverExpires $true #can be change 
             =path "ou=_USERS,$(([ADSI] `"").distingushedName)" `
}
