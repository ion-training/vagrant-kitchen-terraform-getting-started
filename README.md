# vagrant-kitchen-terraform-getting-started
kitchen-terraform on vagrant box

[This is a quick guide to getting started with Kitchen-Terraform](https://newcontext-oss.github.io/kitchen-terraform/getting_started.html)
# How to use this repo
Clone and cd into repo
```
git clone https://github.com/ion-training/vagrant-kitchen-terraform-getting-started.git
```
```
cd vagrant-kitchen-terraform-getting-started
```

Bring up the box and SSH
```
vagrant up
```
```
vagrant ssh
```

# Kitchen Converge
Create terraform resources (see Sample Output 2)
```
bundle exec kitchen converge
```

# To destroy the lab
Disconnect from SSH
```
exit
```

make sure you are in the `vagrant-kitchen-terraform-getting-started` dir

```
vagrant destroy -f
```

```
rm -rf vagrant-kitchen-terraform-getting-started
```


# Test bulbs in the fixture (analogy)
![](./screenshots/2022-02-08-02-51-32.png)

- The fixture is /vagrant/my_terraform_module/test/fixtures/
- Bulb is /vagrant/my_terraform_module/test/fixtures/tf_module/main.tf (which pulls from ../../ the actual main.tf file)

# Sample output 1
Before running `bundle exec kitchen converge`
```
vagrant@kitchen:/vagrant/my_terraform_module$ tree -a
.
├── .kitchen.yml
├── Gemfile
├── Gemfile.lock
├── main.tf
└── test
    ├── fixtures
    │   └── tf_module
    │       └── main.tf
    └── integration
        └── kt_suite
            └── controls

6 directories, 5 files
vagrant@kitchen:/vagrant/my_terraform_module$
```

After running `bundle exec kitchen converge`
```
vagrant@kitchen:/vagrant/my_terraform_module$ tree -a
.
├── .kitchen
│   ├── kt-suite-terraform.yml
│   └── logs
│       ├── kitchen.log
│       └── kt-suite-terraform.log
├── .kitchen.yml
├── Gemfile
├── Gemfile.lock
├── main.tf
└── test
    ├── fixtures
    │   └── tf_module
    │       ├── .terraform
    │       │   ├── environment
    │       │   ├── modules
    │       │   │   └── modules.json
    │       │   └── providers
    │       │       └── registry.terraform.io
    │       │           └── hashicorp
    │       │               └── null
    │       │                   └── 3.1.0
    │       │                       └── linux_amd64
    │       │                           └── terraform-provider-null_v3.1.0_x5
    │       ├── .terraform.lock.hcl
    │       ├── foobar
    │       ├── main.tf
    │       └── terraform.tfstate.d
    │           └── kitchen-terraform-kt-suite-terraform
    │               └── terraform.tfstate
    └── integration
        └── kt_suite
            └── controls

18 directories, 14 files
vagrant@kitchen:/vagrant/my_terraform_module$ 
```

# Sample Output 2
```
vagrant@kitchen:/vagrant/my_terraform_module$ bundle exec kitchen converge
-----> Starting Test Kitchen (v3.2.2)
-----> Creating <kt-suite-terraform>...
$$$$$$ Reading the Terraform client version...
       Terraform v1.1.5
       on linux_amd64
$$$$$$ Finished reading the Terraform client version.
$$$$$$ Verifying the Terraform client version is in the supported interval of >= 0.11.4, < 2.0.0...
$$$$$$ Finished verifying the Terraform client version.
$$$$$$ Initializing the Terraform working directory...
       Upgrading modules...
       - kt_test in ../../..
       
       Initializing the backend...
       
       Initializing provider plugins...
       - Finding latest version of hashicorp/null...
       - Installing hashicorp/null v3.1.0...
       - Installed hashicorp/null v3.1.0 (signed by HashiCorp)
       
       Terraform has created a lock file .terraform.lock.hcl to record the provider
       selections it made above. Include this file in your version control repository
       so that Terraform can guarantee to make the same selections by default when
       you run "terraform init" in the future.
       
       Terraform has been successfully initialized!
$$$$$$ Finished initializing the Terraform working directory.
$$$$$$ Creating the kitchen-terraform-kt-suite-terraform Terraform workspace...
       Created and switched to workspace "kitchen-terraform-kt-suite-terraform"!
       
       You're now on a new, empty workspace. Workspaces isolate their state,
       so if you run "terraform plan" Terraform will not see any existing state
       for this configuration.
$$$$$$ Finished creating the kitchen-terraform-kt-suite-terraform Terraform workspace.
       Finished creating <kt-suite-terraform> (0m1.45s).
-----> Converging <kt-suite-terraform>...
$$$$$$ Reading the Terraform client version...
       Terraform v1.1.5
       on linux_amd64
       + provider registry.terraform.io/hashicorp/null v3.1.0
$$$$$$ Finished reading the Terraform client version.
$$$$$$ Verifying the Terraform client version is in the supported interval of >= 0.11.4, < 2.0.0...
$$$$$$ Finished verifying the Terraform client version.
$$$$$$ Selecting the kitchen-terraform-kt-suite-terraform Terraform workspace...
$$$$$$ Finished selecting the kitchen-terraform-kt-suite-terraform Terraform workspace.
$$$$$$ Downloading the modules needed for the Terraform configuration...
       - kt_test in ../../..
$$$$$$ Finished downloading the modules needed for the Terraform configuration.
$$$$$$ Validating the Terraform configuration files...
       Success! The configuration is valid.
       
$$$$$$ Finished validating the Terraform configuration files.
$$$$$$ Building the infrastructure based on the Terraform configuration...
       
       Terraform used the selected providers to generate the following execution
       plan. Resource actions are indicated with the following symbols:
         + create
       
       Terraform will perform the following actions:
       
         # module.kt_test.null_resource.create_file will be created
         + resource "null_resource" "create_file" {
             + id = (known after apply)
           }
       
       Plan: 1 to add, 0 to change, 0 to destroy.
       module.kt_test.null_resource.create_file: Creating...
       module.kt_test.null_resource.create_file: Provisioning with 'local-exec'...
       module.kt_test.null_resource.create_file (local-exec): Executing: ["/bin/sh" "-c" "echo 'this is my first test' > foobar"]
       module.kt_test.null_resource.create_file: Creation complete after 0s [id=2968974916571745265]
       
       Apply complete! Resources: 1 added, 0 changed, 0 destroyed.
$$$$$$ Finished building the infrastructure based on the Terraform configuration.
$$$$$$ Reading the output variables from the Terraform state...
$$$$$$ Finished reading the output variables from the Terraform state.
$$$$$$ Parsing the Terraform output variables as JSON...
$$$$$$ Finished parsing the Terraform output variables as JSON.
$$$$$$ Writing the output variables to the Kitchen instance state...
$$$$$$ Finished writing the output variables to the Kitchen instance state.
$$$$$$ Writing the input variables to the Kitchen instance state...
$$$$$$ Finished writing the input variables to the Kitchen instance state.
       Finished converging <kt-suite-terraform> (0m1.64s).
-----> Test Kitchen is finished. (0m3.83s)
vagrant@kitchen:/vagrant/my_terraform_module$
```
