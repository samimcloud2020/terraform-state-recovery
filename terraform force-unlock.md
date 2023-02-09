Usage: terraform force-unlock [options] LOCK_ID

Manually unlock the state for the defined configuration.

This will not modify your infrastructure.

This command removes the lock on the state for the current configuration. 

The behavior of this lock is dependent on the backend being used.

Local state files cannot be unlocked by another process.
------------------------------------------------------------------------------------------------
Options:

-force - Don't ask for input for unlock confirmation.

---------------------------------------------------------------------------------------

You can manually unlock the state using the force-unlock command :

$terraform force-unlock LOCK_ID

The lock ID is generally shown in the error message.

It may not work if your state is local and locked by a local process. 

If it is the case, try killing that process and retry.

-------------------------------------------------------------------------------------
Be very careful with this command. If you unlock the state when someone else is holding the lock it could cause multiple writers. 

Force unlock should only be used to unlock your own lock in the situation where automatic unlocking failed.
----------------------------------------------------------------------------------------

I ran into a Terraform Error Locking State error recently after experiencing a temporary loss of network connectivity
whilst in the middle of running a terraform plan to deploy some Azure resources. 
When I tried to re-run the terraform apply command to attempt to deploy the resources again, this is what was returned:

Error: Error locking state: Error acquiring the state lock: state blob is already locked
 Lock Info:
   ID:        a77xxxxxx-cxxc-70f1-xxx-xxxxxxxx
   Path:      terraform-storage/terraform.tfstate
   Operation: OperationTypeApply
   Who:       domain\user@host
   Version:   0.14.5
   Created:   2022-02-21 15:51:13.5132763 +0000 UTC
   Info:
   
   The reason for this is that I had configured the terraform backend to use an Azure storage account to store the terraform state file.
   When I tried to run the terraform apply command after my network connectivity was restored, the state file had a lock due to my previous 
   failed attempt to run the plan.

When using a backend configuration that supports the functionality, state locking helps safeguard your terraform deployments 
by ensuring only one user has write access to the state file, which guards against corruption when you have multiple users potentially
interacting with and running the terraform plan.

In my case, I knew I was the only user as this was in my own test environment. Therefore I could safely remove the lock.
This is done using the terraform force-unlock command. To run this successfully you need to have the ID: 
from the lock info in the error message. For example:

$terraform force-unlock a77xxxxxx-cxxc-70f1-xxx-xxxxxxxx
 Do you really want to force-unlock?
   Terraform will remove the lock on the remote state.
   This will allow local Terraform commands to modify this state, even though it
   may be still be in use. Only 'yes' will be accepted to confirm.
   
   As you can see, you will be prompted to type yes to continue.

Terraform state has been successfully unlocked!
The state has been unlocked, and Terraform commands should now be able to
 obtain a new lock on the remote state.
Once unlocked I was able to run the 

$terraform apply 
command successfully to deploy my Azure resources.



   
