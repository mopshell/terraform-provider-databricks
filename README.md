Terraform Provider
==================

- Website: https://www.terraform.io
- [![Gitter chat](https://badges.gitter.im/hashicorp-terraform/Lobby.png)](https://gitter.im/hashicorp-terraform/Lobby)
- Mailing list: [Google Groups](http://groups.google.com/group/terraform-tool)

<img src="https://cdn.rawgit.com/hashicorp/terraform-website/master/content/source/assets/images/logo-hashicorp.svg" width="600px">

Requirements
------------

-	[Terraform](https://www.terraform.io/downloads.html) 0.10.x
-	[Go](https://golang.org/doc/install) 1.9 (to build the provider plugin)

Usage
---------------------

```
# For example, restrict provider version in 0.1.x
provider "databricks" {
  version = "~> 0.1"
}
```

Building The Provider
---------------------

Clone repository to: `$GOPATH/src/github.com/betabandido/terraform-provider-databricks`

```sh
$ mkdir -p $GOPATH/src/github.com/betabandido; cd $GOPATH/src/github.com/betabandido
$ git clone git@github.com:betabandido/terraform-provider-databricks
```

Enter the provider directory and build the provider

```sh
$ cd $GOPATH/src/github.com/betabandido/terraform-provider-databricks
$ make build
```

Using the provider
----------------------

```hcl
provider "databricks" {
    domain = "<your-account>.cloud.databricks.com"
    token = "<your token>"
}

resource "databricks_notebook" "notebook" {
    path = "/Users/<username>/tf-test"
    language = "PYTHON"
    content = "${base64encode("print('generated by terraform')")}"
}

resource "databricks_cluster" "cluster" {
    name                    = "tf-test"
    spark_version           = "4.1.x-scala2.11"
    node_type_id            = "m4.large"
    num_workers             = 1
    autotermination_minutes = 10

    aws_attributes = {
        zone_id          = "eu-west-1c"
        ebs_volume_type  = "GENERAL_PURPOSE_SSD"
        ebs_volume_count = 1
        ebs_volume_size  = 100
    }    
}
```

Developing the Provider
---------------------------

If you wish to work on the provider, you'll first need [Go](http://www.golang.org) installed on your machine (version 1.9+ is *required*). You'll also need to correctly setup a [GOPATH](http://golang.org/doc/code.html#GOPATH), as well as adding `$GOPATH/bin` to your `$PATH`.

To compile the provider, run `make build`. This will build the provider and put the provider binary in the `$GOPATH/bin` directory.

```sh
$ make bin
...
$ $GOPATH/bin/terraform-provider-databricks
...
```

In order to test the provider, you can simply run `make test`.

```sh
$ make test
```

In order to run the full suite of Acceptance tests, run `make testacc`.

*Note:* Acceptance tests create real resources, and often cost money to run.

```sh
$ make testacc
```