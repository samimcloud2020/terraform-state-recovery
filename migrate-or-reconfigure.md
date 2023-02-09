 A change in the backend configuration has been detected, which may require migrating existing state.
----------------------------------------------------------------------------------------------

 If you wish to attempt automatic migration of the state, use "terraform init -migrate-state".
----------------------------------------------------------------------------------------------------
 If you wish to store the current configuration with no changes to the state, use "terraform init -reconfigure".
 
 ---------------------------------------------------------------------------------------------
 after changing s3 backend to new one, then by migrate you can copy the existing terradorm.tfstate
 to new one.
 
 root@SDOP_Ts:~/terraform-modules/xxx# terraform init -migrate-state
Initializing modules...

Initializing the backend...
Backend configuration changed!

Terraform has detected that the configuration specified for the backend
has changed. Terraform will now check for existing state in the backends.


Acquiring state lock. This may take a few moments...
Acquiring state lock. This may take a few moments...
Do you want to copy existing state to the new backend?
  Pre-existing state was found while migrating the previous "s3" backend to the
  newly configured "s3" backend. No existing state was found in the newly
  configured "s3" backend. Do you want to copy this state to the new "s3"
  backend? Enter "yes" to copy and "no" to start with an empty state.

  Enter a value: yes

Releasing state lock. This may take a few moments...
Releasing state lock. This may take a few moments...

Successfully configured the backend "s3"! Terraform will automatically
use this backend unless the backend configuration changes.

Initializing provider plugins...
- Reusing previous version of hashicorp/aws from the dependency lock file
- Using previously-installed hashicorp/aws v2.70.4

Terraform has been successfully initialized!

-------------------------------------------------------------------------------------------

root@SDOP_Ts:~/terraform-modules/xxx# terraform state pull

---------------------------------------------------------------------------------------
if after s3 backend change and you do
$terraform init -reconfigure  <----store the current configuration with no changes to the state
