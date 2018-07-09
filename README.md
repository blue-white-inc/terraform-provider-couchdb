Terraform Provider
==================

- Website: https://www.terraform.io
- [![Gitter chat](https://badges.gitter.im/hashicorp-terraform/Lobby.png)](https://gitter.im/hashicorp-terraform/Lobby)
- Mailing list: [Google Groups](http://groups.google.com/group/terraform-tool)

<img src="https://cdn.rawgit.com/hashicorp/terraform-website/master/content/source/assets/images/logo-hashicorp.svg" width="600px">


Requirements
------------

-	[Terraform](https://www.terraform.io/downloads.html) 0.10.x
-	[Go](https://golang.org/doc/install) 1.8 (to build the provider plugin)

Building The Provider
---------------------
After installing golang, set the $GOPATH and $GOROOT to point to the correct path. (i.e $GOPATH=/root/go, $GOROOT=/usr/local/go). Do so by editing `.bash_profile` in the root directory
```
export GOROOT=/usr/local/go
export PATH=$PATH:$GOROOT/bin

export GOPATH=/root/go
```
Then execute `source ~/.bash_profile`

Clone repository to: `$GOPATH/src/github.com/blue-white-inc/terraform-provider-couchdb`

```sh
$ mkdir -p $GOPATH/src/github.com/blue-white-inc; cd $GOPATH/src/github.com/blue-white-inc
$ git clone git@github.com:blue-white-inc/terraform-provider-couchdb
```

Enter the provider directory and build the provider

```sh
$ cd $GOPATH/src/github.com/blue-white-inc/terraform-provider-couchdb
$ make build
```

Using the provider
----------------------

See also `website/doc` folder

```hcl
provider "couchdb" {
  endpoint = "http://localhost:5984"
}

resource "couchdb_admin" "jenny" {
  name = "jenny"
  password = "secret" 
}

resource "couchdb_database" "db1" {
  name = "example"
}

resource "couchdb_database_replication" "db2db" {
  name = "example"
  source = "${couchdb_database.db1.name}"
  target = "example-clone"
  create_target = true
  continuous = true
}

resource "couchdb_database_design_document" "test" {
	database = "${couchdb_database.db1.name}"
	name = "types"

	view {
		name = "people"
		map = "function(doc) { if (doc.type == 'person') { emit(doc); } }"
	}
}
```

Developing the Provider
---------------------------

If you wish to work on the provider, you'll first need [Go](http://www.golang.org) installed on your machine (version 1.8+ is *required*). You'll also need to correctly setup a [GOPATH](http://golang.org/doc/code.html#GOPATH), as well as adding `$GOPATH/bin` to your `$PATH`.

To compile the provider, run `make build`. This will build the provider and put the provider binary in the `$GOPATH/bin` directory.

```sh
$ make bin
...
$ $GOPATH/bin/terraform-provider-$PROVIDER_NAME
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
