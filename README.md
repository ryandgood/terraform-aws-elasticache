<!-- This file was automatically generated by the `geine`. Make all changes to `README.yaml` and run `make readme` to rebuild this file. -->

<p align="center"> <img src="https://user-images.githubusercontent.com/50652676/62349836-882fef80-b51e-11e9-99e3-7b974309c7e3.png" width="100" height="100"></p>


<h1 align="center">
    Terraform AWS Elasticache
</h1>

<p align="center" style="font-size: 1.2rem;"> 
    Terraform module to create Elasticache Cluster and replica for Redis and Memcache.
     </p>

<p align="center">

<a href="https://www.terraform.io">
  <img src="https://img.shields.io/badge/terraform-v1.1.7-green" alt="Terraform">
</a>
<a href="LICENSE.md">
  <img src="https://img.shields.io/badge/License-APACHE-blue.svg" alt="Licence">
</a>
<a href="https://github.com/clouddrove/terraform-aws-elasticache/actions/workflows/tfsec.yml">
  <img src="https://github.com/clouddrove/terraform-aws-elasticache/actions/workflows/tfsec.yml/badge.svg" alt="tfsec">
</a>
<a href="https://github.com/clouddrove/terraform-aws-elasticache/actions/workflows/terraform.yml">
  <img src="https://github.com/clouddrove/terraform-aws-elasticache/actions/workflows/terraform.yml/badge.svg" alt="static-checks">
</a>


</p>
<p align="center">

<a href='https://facebook.com/sharer/sharer.php?u=https://github.com/clouddrove/terraform-aws-elasticache'>
  <img title="Share on Facebook" src="https://user-images.githubusercontent.com/50652676/62817743-4f64cb80-bb59-11e9-90c7-b057252ded50.png" />
</a>
<a href='https://www.linkedin.com/shareArticle?mini=true&title=Terraform+AWS+Elasticache&url=https://github.com/clouddrove/terraform-aws-elasticache'>
  <img title="Share on LinkedIn" src="https://user-images.githubusercontent.com/50652676/62817742-4e339e80-bb59-11e9-87b9-a1f68cae1049.png" />
</a>
<a href='https://twitter.com/intent/tweet/?text=Terraform+AWS+Elasticache&url=https://github.com/clouddrove/terraform-aws-elasticache'>
  <img title="Share on Twitter" src="https://user-images.githubusercontent.com/50652676/62817740-4c69db00-bb59-11e9-8a79-3580fbbf6d5c.png" />
</a>

</p>
<hr>


We eat, drink, sleep and most importantly love **DevOps**. We are working towards strategies for standardizing architecture while ensuring security for the infrastructure. We are strong believer of the philosophy <b>Bigger problems are always solved by breaking them into smaller manageable problems</b>. Resonating with microservices architecture, it is considered best-practice to run database, cluster, storage in smaller <b>connected yet manageable pieces</b> within the infrastructure. 

This module is basically combination of [Terraform open source](https://www.terraform.io/) and includes automatation tests and examples. It also helps to create and improve your infrastructure with minimalistic code instead of maintaining the whole infrastructure code yourself.

We have [*fifty plus terraform modules*][terraform_modules]. A few of them are comepleted and are available for open source usage while a few others are in progress.




## Prerequisites

This module has a few dependencies: 

- [Terraform 1.x.x](https://learn.hashicorp.com/terraform/getting-started/install.html)
- [Go](https://golang.org/doc/install)
- [github.com/stretchr/testify/assert](https://github.com/stretchr/testify)
- [github.com/gruntwork-io/terratest/modules/terraform](https://github.com/gruntwork-io/terratest)







## Examples


**IMPORTANT:** Since the `master` branch used in `source` varies based on new modifications, we suggest that you use the release versions [here](https://github.com/clouddrove/terraform-aws-elasticache/releases).


Here are some examples of how you can use this module in your inventory structure:
### Redis
```hcl
    module "redis" {
    source                       = "clouddrove/elasticache/aws
    version                      = "1.0.1"
    name                         = "redis"
    environment                  = "test"
    label_order                  = ["environment", "name"]
    engine                       = "redis"
    engine_version               = "5.0.0"
    family                       = "redis5.0"
    port                         = 6379
    node_type                    = "cache.t2.micro"
    subnet_ids                   = ["subnet-xxxxxxx","subnet-xxxxxxx","subnet-xxxxxxx"]
    security_group_ids           = ["sg-xxxxxxxxx"]
    availability_zones           = ["eu-west-1a","eu-west-1b" ]
    auto_minor_version_upgrade   = true
    number_cache_clusters        = 2
   }

```
### Redis Cluster
```hcl
    module "redis-cluster" {
    source                      = "clouddrove/elasticache/aws
    version                     = "1.0.1"
    name                        = "cluster"
    environment                 = "test"
    label_order                 = ["environment","name"]
    cluster_replication_enabled = true
    engine                      = "redis"
    engine_version              = "5.0.0"
    family                      = "redis5.0"
    port                        = 6379
    node_type                   = "cache.t2.micro"
    subnet_ids                  = module.subnets.public_subnet_id
    security_group_ids          = [module.redis-sg.security_group_ids]
    availability_zones          = ["eu-west-1a","eu-west-1b" ]
    auto_minor_version_upgrade  = true
    replicas_per_node_group     = 2
    num_node_groups             = 1
    automatic_failover_enabled  = true
  }
```
### Memcache
```hcl
    module "memcached" {
    source                       = "clouddrove/elasticache/aws
    version                      = "1.0.1"
    name                         = "memcached"
    environment                  = "test"
    label_order                  = ["environment", "name"]
    cluster_enabled              = true
    engine                       = "memcached"
    engine_version               = "1.5.10"
    family                       = "memcached1.5"
    parameter_group_name         = "default.memcached1.5"
    az_mode                      = "cross-az"
    port                         = 11211
    node_type                    = "cache.t2.micro"
    num_cache_nodes              = 2
    subnet_ids                   = ["subnet-xxxxxxx","subnet-xxxxxxx","subnet-xxxxxxx"]
    security_group_ids           = ["sg-xxxxxxxxx"]
    availability_zones           = ["eu-west-1a","eu-west-1b" ]
    }
```






## Inputs

| Name | Description | Type | Default | Required |
|------|-------------|------|---------|:--------:|
| apply\_immediately | Specifies whether any modifications are applied immediately, or during the next maintenance window. Default is false. | `bool` | `false` | no |
| at\_rest\_encryption\_enabled | Enable encryption at rest. | `bool` | `true` | no |
| attributes | Additional attributes (e.g. `1`). | `list(any)` | `[]` | no |
| auth\_token | The password used to access a password protected server. Can be specified only if transit\_encryption\_enabled = true. | `string` | `"gihweisdjhewiuei"` | no |
| auto\_minor\_version\_upgrade | Specifies whether a minor engine upgrades will be applied automatically to the underlying Cache Cluster instances during the maintenance window. Defaults to true. | `bool` | `true` | no |
| automatic\_failover\_enabled | Specifies whether a read-only replica will be automatically promoted to read/write primary if the existing primary fails. If true, Multi-AZ is enabled for this replication group. If false, Multi-AZ is disabled for this replication group. Must be enabled for Redis (cluster mode enabled) replication groups. Defaults to false. | `bool` | `false` | no |
| availability\_zones | A list of EC2 availability zones in which the replication group's cache clusters will be created. The order of the availability zones in the list is not important. | `list(string)` | n/a | yes |
| az\_mode | (Memcached only) Specifies whether the nodes in this Memcached node group are created in a single Availability Zone or created across multiple Availability Zones in the cluster's region. Valid values for this parameter are single-az or cross-az, default is single-az. If you want to choose cross-az, num\_cache\_nodes must be greater than 1. | `string` | `"single-az"` | no |
| cluster\_enabled | (Memcache only) Enabled or disabled cluster. | `bool` | `false` | no |
| cluster\_replication\_enabled | (Redis only) Enabled or disabled replication\_group for redis cluster. | `bool` | `false` | no |
| description | Description for the cache subnet group. Defaults to `Managed by Terraform`. | `string` | `"Managed by Terraform"` | no |
| enable | Enable or disable of elasticache | `bool` | `true` | no |
| engine | The name of the cache engine to be used for the clusters in this replication group. e.g. redis. | `string` | `""` | no |
| engine\_version | The version number of the cache engine to be used for the cache clusters in this replication group. | `string` | `""` | no |
| environment | Environment (e.g. `prod`, `dev`, `staging`). | `string` | `""` | no |
| extra\_tags | Additional tags (e.g. map(`BusinessUnit`,`XYZ`). | `map(string)` | `{}` | no |
| family | (Required) The family of the ElastiCache parameter group. | `string` | `""` | no |
| kms\_key\_id | The ARN of the key that you wish to use if encrypting at rest. If not supplied, uses service managed encryption. Can be specified only if at\_rest\_encryption\_enabled = true. | `string` | `""` | no |
| label\_order | Label order, e.g. `name`,`application`. | `list(any)` | `[]` | no |
| log\_delivery\_configuration | The log\_delivery\_configuration block allows the streaming of Redis SLOWLOG or Redis Engine Log to CloudWatch Logs or Kinesis Data Firehose. Max of 2 blocks. | `list(map(any))` | `[]` | no |
| maintenance\_window | Maintenance window. | `string` | `"sun:05:00-sun:06:00"` | no |
| managedby | ManagedBy, eg 'CloudDrove' or 'AnmolNagpal'. | `string` | `"anmol@clouddrove.com"` | no |
| name | Name  (e.g. `app` or `cluster`). | `string` | `""` | no |
| node\_type | The compute and memory capacity of the nodes in the node group. | `string` | `"cache.t2.small"` | no |
| notification\_topic\_arn | An Amazon Resource Name (ARN) of an SNS topic to send ElastiCache notifications to. | `string` | `""` | no |
| num\_cache\_nodes | (Required unless replication\_group\_id is provided) The initial number of cache nodes that the cache cluster will have. For Redis, this value must be 1. For Memcache, this value must be between 1 and 20. If this number is reduced on subsequent runs, the highest numbered nodes will be removed. | `number` | `1` | no |
| num\_node\_groups | Number of Shards (nodes). | `string` | `""` | no |
| number\_cache\_clusters | (Required for Cluster Mode Disabled) The number of cache clusters (primary and replicas) this replication group will have. If Multi-AZ is enabled, the value of this parameter must be at least 2. Updates will occur before other modifications. | `string` | `""` | no |
| parameter\_group\_name | The name of the parameter group to associate with this replication group. If this argument is omitted, the default cache parameter group for the specified engine is used. | `string` | `"default.redis5.0"` | no |
| port | the port number on which each of the cache nodes will accept connections. | `string` | `""` | no |
| replicas\_per\_node\_group | Replicas per Shard. | `string` | `""` | no |
| replication\_enabled | (Redis only) Enabled or disabled replication\_group for redis standalone instance. | `bool` | `false` | no |
| replication\_group\_id | The replication group identifier This parameter is stored as a lowercase string. | `string` | `""` | no |
| repository | Terraform current module repo | `string` | `"https://github.com/clouddrove/terraform-aws-elasticache"` | no |
| retention\_in\_days | Specifies the number of days you want to retain log events in the specified log group. | `number` | `0` | no |
| security\_group\_ids | One or more VPC security groups associated with the cache cluster. | `list` | `[]` | no |
| security\_group\_names | A list of cache security group names to associate with this replication group. | `any` | `null` | no |
| snapshot\_arns | A single-element string list containing an Amazon Resource Name (ARN) of a Redis RDB snapshot file stored in Amazon S3. | `any` | `null` | no |
| snapshot\_name | The name of a snapshot from which to restore data into the new node group. Changing the snapshot\_name forces a new resource. | `string` | `""` | no |
| snapshot\_retention\_limit | (Redis only) The number of days for which ElastiCache will retain automatic cache cluster snapshots before deleting them. For example, if you set SnapshotRetentionLimit to 5, then a snapshot that was taken today will be retained for 5 days before being deleted. If the value of SnapshotRetentionLimit is set to zero (0), backups are turned off. Please note that setting a snapshot\_retention\_limit is not supported on cache.t1.micro or cache.t2.\* cache nodes. | `string` | `"0"` | no |
| snapshot\_window | (Redis only) The daily time range (in UTC) during which ElastiCache will begin taking a daily snapshot of your cache cluster. The minimum snapshot window is a 60 minute period. | `any` | `null` | no |
| subnet\_ids | List of VPC Subnet IDs for the cache subnet group. | `list` | `[]` | no |
| transit\_encryption\_enabled | Whether to enable encryption in transit. | `bool` | `true` | no |

## Outputs

| Name | Description |
|------|-------------|
| id | Redis cluster id. |
| memcached\_arn | Memcached arn |
| memcached\_endpoint | Memcached endpoint address. |
| port | Redis port. |
| redis\_arn | Redis arn |
| redis\_endpoint | Redis endpoint address. |
| tags | A mapping of tags to assign to the resource. |




## Testing
In this module testing is performed with [terratest](https://github.com/gruntwork-io/terratest) and it creates a small piece of infrastructure, matches the output like ARN, ID and Tags name etc and destroy infrastructure in your AWS account. This testing is written in GO, so you need a [GO environment](https://golang.org/doc/install) in your system. 

You need to run the following command in the testing folder:
```hcl
  go test -run Test
```



## Feedback 
If you come accross a bug or have any feedback, please log it in our [issue tracker](https://github.com/clouddrove/terraform-aws-elasticache/issues), or feel free to drop us an email at [hello@clouddrove.com](mailto:hello@clouddrove.com).

If you have found it worth your time, go ahead and give us a ★ on [our GitHub](https://github.com/clouddrove/terraform-aws-elasticache)!

## About us

At [CloudDrove][website], we offer expert guidance, implementation support and services to help organisations accelerate their journey to the cloud. Our services include docker and container orchestration, cloud migration and adoption, infrastructure automation, application modernisation and remediation, and performance engineering.

<p align="center">We are <b> The Cloud Experts!</b></p>
<hr />
<p align="center">We ❤️  <a href="https://github.com/clouddrove">Open Source</a> and you can check out <a href="https://github.com/clouddrove">our other modules</a> to get help with your new Cloud ideas.</p>

  [website]: https://clouddrove.com
  [github]: https://github.com/clouddrove
  [linkedin]: https://cpco.io/linkedin
  [twitter]: https://twitter.com/clouddrove/
  [email]: https://clouddrove.com/contact-us.html
  [terraform_modules]: https://github.com/clouddrove?utf8=%E2%9C%93&q=terraform-&type=&language=
