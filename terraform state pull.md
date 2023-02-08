$ terraform state pull -h

Usage: terraform [global options] state pull [options]

  Pull the state from its location, upgrade the local copy, and output it
  to stdout.

  This command "pulls" the current state and outputs it to stdout.
  As part of this process, Terraform will upgrade the state format of the
  local copy to the current version.

  The primary use of this is for state stored remotely. This command
  will still work with local state but is less useful for this.
  
  
  Usage: terraform state pull

This command downloads the state from its current location, upgrades the local copy to the latest state file version that is compatible with locally-installed Terraform, and outputs the raw format to stdout.

This is useful for reading values out of state (potentially pairing this command with something like jq). It is also useful if you need to make manual modifications to state.

You cannot use this command to inspect the Terraform version of the remote state, as it will always be converted to the current Terraform version before output.

Note: Terraform state files must be in UTF-8 format without a byte order mark (BOM). For PowerShell on Windows, use Set-Content to automatically encode files in UTF-8 format. For example, run terraform state pull | sc terraform.tfstate.
