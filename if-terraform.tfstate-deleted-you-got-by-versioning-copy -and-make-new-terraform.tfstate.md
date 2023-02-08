As s3 versioning is enabled in s3.
if terraform.tfstate is deleted then copy the versioning file which pointed with delete marker.
make anothe file called terraform.tfstate and upload it.

if dynamodb lock set , delete it as its having digest and lockid.(as no one at a same time update it)

$terraform state pull   <-----you see in stdout the terraform.tfstate file

$terraform destroy <----if you can

