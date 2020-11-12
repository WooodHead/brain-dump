# Terraform

- [Terraform](https://learn.hashicorp.com/tutorials/terraform/install-cli)

Make a ~/.terraform.d/credentials.tfrc.json file

should contain:

```
{
  "credentials": {
    "app.terraform.io": {
      "token": "xxx"
    }
  }
}
```

generate token here: https://app.terraform.io/app/settings/tokens
