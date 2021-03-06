Terraform module which creating VPC and bastion host on Alibaba Cloud

terraform-alicloud-vpc-bastionhost
=====================================================================

English | [简体中文](README-CN.md)

This module is used to create VPC and bastion host on Alibaba Cloud under Alibaba Cloud.

These types of resources are supported:

* [alicloud_bastionhost_host](https://registry.terraform.io/providers/aliyun/alicloud/latest/docs/resources/bastionhost_host)
* [alicloud_bastionhost_instance](https://registry.terraform.io/providers/aliyun/alicloud/latest/docs/resources/bastionhost_instance)

## Usage

```hcl
data "alicloud_zones" "default" {
  available_resource_creation = "VSwitch"
}

resource "alicloud_vpc" "default" {
  vpc_name   = "TerraformTest"
  cidr_block = "172.16.0.0/12"
}

resource "alicloud_security_group" "default" {
  vpc_id = alicloud_vpc.default.id
  name   = "TerraformTest"
}

resource "alicloud_vswitch" "default" {
  vpc_id     = alicloud_vpc.default.id
  cidr_block = cidrsubnet(alicloud_vpc.default.cidr_block, 8, 4)
  zone_id    = data.alicloud_zones.default.zones[0].id
}

locals {
  zone_id = data.alicloud_zones.default.ids[length(data.alicloud_zones.default.ids) - 1]
}

module "example" {
  source               = "terraform-alicloud-modules/vpc-bastionhost/alicloud"
  vswtich_id           = alicloud_vswitch.default.id
  security_group_ids   = [alicloud_security_group.default.id]
  bastion_license_code = "bhah_ent_50_asset"
}
```

## Notes

* This module using AccessKey and SecretKey are from `profile` and `shared_credentials_file`. If you have not set them
  yet, please install [aliyun-cli](https://github.com/aliyun/aliyun-cli#installation) and configure it.

## Requirements

| Name | Version |
|------|---------|
| <a name="requirement_terraform"></a> [terraform](#requirement\_terraform) | > = 0.13.0 |
| <a name="requirement_alicloud"></a> [alicloud](#requirement\_alicloud) | > = 1.110.0 |

## Providers

| Name | Version |
|------|---------|
| <a name="provider_alicloud"></a> [alicloud](#provider\_alicloud) | > = 1.110.0 |

## Submit Issues

If you have any problems when using this module, please opening
a [provider issue](https://github.com/aliyun/terraform-provider-alicloud/issues/new) and let us know.

**Note:** There does not recommend opening an issue on this repo.

## Authors

Created and maintained by Alibaba Cloud Terraform Team(terraform@alibabacloud.com)

## License

MIT Licensed. See LICENSE for full details.

## Reference

* [Terraform-Provider-Alicloud Github](https://github.com/aliyun/terraform-provider-alicloud)
* [Terraform-Provider-Alicloud Release](https://releases.hashicorp.com/terraform-provider-alicloud/)
* [Terraform-Provider-Alicloud Docs](https://registry.terraform.io/providers/aliyun/alicloud/latest/docs)