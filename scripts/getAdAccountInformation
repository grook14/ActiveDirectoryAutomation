### Gets passed from a CSV file! 
function getSVCAcctTags ($acct_data) {
    
    $groupCheck = @() # Array for looping through an accounts groups that it's a part of
    $accountData = @() # Array for storing all specified AD info / custom fields from a custom object 
    $activeAccount = @() # Array to specify working on active accounts, not disabled accounts
    $disabledAccount = @() # Array to contain disabled accounts
    $catchAllAccount = @() # Catch all array to store any other issues that come up with an account
    $final = @() # Array to store all other account array in one.

        $csvCounter = 0
        foreach ($u in acct_data) {
           # Get display name from CSV
           $displayName = $svc_acct_data[$csvCounter].Name
           # Write-Host "Working on $($displayName)"

           # Logic to get their ACTUAL logon name. 
        
           $logonName = Get-ADUser -Filter {displayname -like $displayName} | select samaccountname

           # Logic to store their ACTUAL logon name as string.
           $logonString = $logonName.samaccountname

               # Logic to check if the account is enabled or not
               # If not, it will store the values in a catchall array. 
               try {$preEnabled = Get-ADUser $logonString | Select-Object enabled} 
               catch {
                    # Fatal_Error is a field to list the display name of an account as the only attribute of an object. 
                    $catchAllAccount += [PSCustomObject]@{
                                SAMAccountName = "N/A"
                                Status = "N/A"
                                GroupCheckVerify1 = "N/A"
                                GroupCheckVerify2 = "N/A"
                                Fatal_Error = $displayName}
               }     
           
           $enabled = $preEnabled.enabled
               if ($enabled -eq "enabled") {$activeAccount += $logonString}
               elseif ($enabled -eq $null) {$activeAccount += "NULL"}
               Else {
                    $disabledAccount += [PSCustomObject]@{
                            SAMAccountName = $logonString
                            Status = "Disabled"
                            GroupCheckVerify1 = "N/A"
                            GroupCheckVerify2 = "N/A"
                            Fatal_Error = "FALSE" 
                    }
               }
           $csvCounter += 1
        }

        #Eliminate blank values (All issues with blank values should be in the 'catchAll' array.
        $activeAccount = $activeAccount | Where-Object { $_ } | Select-Object -Unique
        
        # Loop through the $activeAccount array, get the groups that the account is a part of
        # and then store in the 'accountData' array.
        $paramCount = 0
        $groupCheckCounter = 0
        foreach ($p in $activeAccount) {
            # If statements to check to see if the SVC account is named properly
            # You can use this code to verify that an AD account is using a specific naming convention.
            if ($p -match "CHANGE_HERE") {$name = "TRUE"}
            else {$name = "FALSE"}

            # Gets all groups that the account is a 'Member Of'
            # you can get rid of this loop entirely if you don't want to check for specific groups an account is a part of.
            try {$groupCheck += Get-ADPrincipalGroupMembership $activeAccount[$paramCount] | select name}
            catch {
                if ($activeAccount[$paramCount] -eq $null) {
                    Write-Host "Found no groups for the value $($paramCount) stored in the 'activeAccount' array because it was null."
                }
                else {Write-Host "Looks like there was an issue with $($activeAccount[$paramCount])"} 
                }
               foreach ($g in $groupCheck) {
                # Checks for specific naming convention
                if ($groupCheck[$groupCheckCounter].name -match "CHANGE_HERE") {$check1 = $groupCheck[$groupCheckCounter].name}
                elseif ($groupCheck[$groupCheckCounter].name -match "CHANGE_HERE") {$check2 = $groupCheck[$groupCheckCounter].name}

                $groupCheckCounter += 1
               
                    if ($groupCheck.Count -eq $groupCheckCounter) {
                        $accountData += [PSCustomObject]@{
                            SAMAccountName = $activeAccount[$paramCount]
                            Status = "Active"
                            GroupCheckVerify1 = $check1
                            GroupCheckVerify2 = $check2
                            Naming = $name 
                            Fatal_Error = "FALSE"
                        }
                        break
                    }
               }
            $paramCount += 1
        }
   $final += $accountData
   $final += $disabledAccount
   $final += $catchAllAccount
   return $final
}
