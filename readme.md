This Terraform workspace demonstrates Terraform stripping off object attributes that do not pass validation without giving any error.

The file main.tfvars has this content:
```
var1 = {
  name = "test",
  extra = "no"
}
```
The variable spec is in variables.tf:
```
variable "var1" {
  type = object({
    name = string
  })
}
```
and finally output.tf just shows a count of object attributes:
```
output "length" {
  value = length(var.var1)
}
```

I perceive proper behavior to be one of
1) Fail the run with a validation error, because the value provided for var1 does not conform to the spec (extra is not part of the definition), or
1) Keep the object.

Neither of these are seen, instead the run silently strips the non-conformant attribute, and notes only 1 attribute

```
terraform apply -var-file=main.tfvars

Changes to Outputs:
  + length = 1

You can apply this plan to save these new output values to the Terraform state, without changing any real infrastructure.

Do you want to perform these actions?
  Terraform will perform the actions described above.
  Only 'yes' will be accepted to approve.

  Enter a value: yes


Apply complete! Resources: 0 added, 0 changed, 0 destroyed.

Outputs:

length = 1
