# terraform-state-recovery

Terraform State Restoration Overview
Avatar Adedayo Samuel
2 years ago Updated
Not yet followed by anyone
Introduction
If, in the process of using Terraform, you find yourself in situations where you've backed yourself into a corner with your configuration - either with irreconcilable errors or with a corrupted state, and want to "go back" to your last working configuration. Given that terraform state is the source of truth of your infrastructure, i.e what contains your resource mappings to the real world, it often is where we need to fix things to get back to a working state.

 

Restoring your state file falls generally into these 3 approaches/options:

 

Terraform State Restore Options
1). Restore from backup
This is the easiest route to restore operations. To do that, you restore the last working state backup file you had before you ran into this issue. If you have frequent state backups in place, you can sort by the date and time before you ran into the issue.

Alternatively, if you're running Terraform locally, a terraform.tfstate.backup file is generated before a new state file is created. You can use that as your new state file and see if that works for you.

To make an old backup state file your new one, all you need do is to move your current one to a different (safe) folder/directory (in case anything goes wrong), then rename the backup file as your new terraform.tfstate file, and run terraform plan again.

Please note that your old state file might be a couple of versions behind your current infra setup so you might need to recreate or re-import the additional resources/config.

 

2). Removing the bad resource from state and re-import the resource
If you don't have a suitable state file, then your other choice would be to remove the bad resource from your current state file using the terraform state rm command [a]. Here, you can specify the bad resource address (example below), and then re-import it. Please check the provider documentation for the specific resource for its import command. Link [b] talks about terraform import from a general standpoint.

 

3). (Remote backends only) Terraform state Push/Pull - ADVANCED Users Only.
A distant 3rd option (if you're using a remote backend) is to replicate your setup locally, then perform a state push to override your remote backend's state file. Link [c] talks about how to use the terraform state push/pull commands.

 

Link References
[a] https://www.terraform.io/docs/cli/commands/state/rm.html

[b] https://www.terraform.io/docs/cli/commands/import.html

[c] https://www.terraform.io/docs/cli/state/recover.html

