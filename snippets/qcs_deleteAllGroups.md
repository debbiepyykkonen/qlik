
This is Powershell script
In Powershell run QLik context init
Enter the URL of tenant with https://
Create API in tenant
Paste API in powerhell

confirm you are in correct tenant
qlik context ls
the one you are in will have asterisk

Then paste the script in below and enter

Might not work with this if: if ($groups.count > 0) {" it failed for me
So run from line 38 thrugh 47 instead and will only 100 at a time so keep running till all groups are gone
Dave fix this if line 37 is working :-)






# Delete all groups from a QCS tenant

This script selects 100 groups at a time from the tenant, and sends a delete request per group to remove them from the tenant. 

Notes:
* This uses a deprecated API, which may stop working at any time
* Groups will be repopulated the next time a user logs into the tenant (their groups)
* If the script prints "No groups returned." then the initial request either returned no groups or an error

```
# Assign result of groups to a groups variable
$groups=$(qlik raw get v1/qlik-groups --query limit=100) | ConvertFrom-Json

# If there are groups
if ($groups.count > 0) {
    
    # Print out the number of groups returned by the query
    Write-Host $groups.count "groups returned."
    
    # Now iterate over all groups with a delete command
    $groups | ForEach {

        $group = $_.id
        Write-Host "Deleting group" $_.displayName "("$group")"
        $deleteResponse=$(qlik raw delete v1/qlik-groups/$group)
    }
    
} else {
    Write-Host "No groups returned."
}
```
