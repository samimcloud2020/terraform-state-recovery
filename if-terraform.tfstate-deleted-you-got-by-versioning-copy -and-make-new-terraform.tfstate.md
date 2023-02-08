As s3 versioning is enabled in s3.
if terraform.tfstate is deleted then copy the versioning file which pointed with delete marker.
make anothe file called terraform.tfstate and upload it.

if dynamodb lock set , delete it as its having digest and lockid.(as no one at a same time update it)

$terraform state pull   <-----you see in stdout the terraform.tfstate file

$terraform destroy <----if you can

-----------------------------------------------------------------------------------------


You really are in a mess. You need to restore the S3 bucket or make a new one and point your code at that. You then need to recreate the state you lost, that or delete every object you created via Terraform and start again. Most objects have the ability to import existing objects via the Terraform import command.

This could be a significantly large task.

And you'd be needing write access to the bucket? Terraform refresh is only going to help if you still had the state file. You don't. If you haven't got permission to do that, then maybe give up that or persist in getting sufficient privilege.

If you can't run Terraform locally then you are also wasting your time. Good luck.

However....

You don't want to be here again. How did you delete/lose the bucket? You really need that never to happen again as @ydaetskcoR said some MFA protection on the bucket - definitely do that and adding versioning to it is a REALLY good idea. Also if you haven't added DynamoDB locking to the bucket do so, its really worth it.

-------------------------------------------------------------------------------------------------
also you can import resource

$terraform import aws-resource.resource-name
  
$terraform plan
  
check upto its upto set all variable as per real infra.  
