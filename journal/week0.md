# Terraform Beginner Bootcamp 2023 - Week 0

- [Semantic Versioning :mage:](#semantic-versioning--mage-)
- [Install Terraform cli](#install-terraform-cli)
  * [consideration with the Terraform CLI changes](#consideration-with-the-terraform-cli-changes)
  * [Consideration for linux distribution](#consideration-for-linux-distribution)
  * [Refactoring into Bash Scripts](#refactoring-into-bash-scripts)
    + [Shebang](#shebang)
    + [Execution considerations](#execution-considerations)
    + [Linux permissions considerations](#linux-permissions-considerations)
  * [Gitpod lifecycle](#gitpod-lifecycle)
  * [Working with env vars](#working-with-env-vars)
    + [Setting and Unsetting Env Vars](#setting-and-unsetting-env-vars)
- [Print vars](#print-vars)
    + [Scoping of Env Vars](#scoping-of-env-vars)
    + [Persisting env vars in git pod](#persisting-env-vars-in-git-pod)
  * [AWS CLI Install](#aws-cli-install)
- [Terraform basiscs](#terraform-basiscs)
- [Terraform registry](#terraform-registry)
- [Terraform Console](#terraform-console)
    + [Terraform init](#terraform-init)
    + [Terraform Plan](#terraform-plan)
    + [Terraform Apply](#terraform-apply)
  * [Terraform lock Files](#terraform-lock-files)
  * [Terraform  state files](#terraform--state-files)
- [Terraform directory](#terraform-directory)
    + [Terraform destroy](#terraform-destroy)
- [Issues with terraform cloud login and gitpod workspace](#issues-with-terraform-cloud-login-and-gitpod-workspace)

## Semantic Versioning :mage:

This project is going utilize semantic versioning for its tagging
[semver.org](https://semver.org/)

The general format:

**MAJOR.MINOR.PATCH**, eg. `1.0.1`

- **MAJOR** version when you make incompatible API changes
- **MINOR** version when you add functionality in a backward compatible manner
- **PATCH** version when you make backward compatible bug fixes

## Install Terraform cli


### consideration with the Terraform CLI changes
The Terraform installation instructions have change due to gpg keyring changes. So the original gitpod.yml bash. So we needed refer to the latest install CLI instruction via terraform documentation and change
the scripting for install.

[Install Terraform CLI](https://developer.hashicorp.com/terraform/tutorials/aws-get-started/install-cli)

### Consideration for linux distribution

This project is built with Ubuntu.
Please consider checking your linux distribution and change accordingly to distribution needs

[Linux Os Version Command](https://www.cyberciti.biz/faq/how-to-check-os-version-in-linux-command-line/)


Example of checking OS version
```
$ cat /etc/os-release
PRETTY_NAME="Ubuntu 22.04.3 LTS"
NAME="Ubuntu"
VERSION_ID="22.04"
VERSION="22.04.3 LTS (Jammy Jellyfish)"
VERSION_CODENAME=jammy
ID=ubuntu
ID_LIKE=debian
HOME_URL="https://www.ubuntu.com/"
SUPPORT_URL="https://help.ubuntu.com/"
BUG_REPORT_URL="https://bugs.launchpad.net/ubuntu/"
PRIVACY_POLICY_URL="https://www.ubuntu.com/legal/terms-and-policies/privacy-policy"
UBUNTU_CODENAME=jammy
```
### Refactoring into Bash Scripts

While fixing the Terraform CLI gpg depreciation issues we created a new terraform cli bash script. We notices that the bash script steps were a amount of more code. So we decided to create a bash script to install the terraform CLI. 

This bash script is located here: [./bin/install_terraform_cli](./bin/install_terraform_cli)

- This will keep the Gitpod task file ([.gitpod.yml](.gitpod.yml)) tidy.
- This allow us an easier to debug and executes manually a terraform cli install
- This will allow better portability for other projects that need to install the terraform CLI


#### Shebang

A Shebang (pronounced Sha-bang) tells the bash script what program that will interpet the script. eg. `#!/bin/bash`

- for portability for different os distributions 
- utility to locate bash in the system's PATH
ChatGPT recommend this format for bash: `#!/usr/bin/env bash` 

[Shebang Unix](https://en.wikipedia.org/wiki/Shebang_(Unix))

#### Execution considerations
When executing the bash script we can use the `/./` shorthand notation to execute the bash script

eg. `./bin/install_terraform_cli` 

if we are using a script in gitpod.ymp we need to point the script to a program to interpert it.

eg. `source ./bin/install_terraform_cli` 

#### Linux permissions considerations

Linux permissions works as follow.
In order to make our bash script executable we need to change linux permissions for the fix the executable at the user mode. 

```sh
chmod u+x ./bin/install_terraform_cli
```

alternatively 
```sh
chmod 744 ./bin/install_terraform_cli
```

### Gitpod lifecycle

We need to be careful when using the init because it will ***not rerun*** if we restart an existing workspace.


[Gitpod lifecycle](https://www.gitpod.io/docs/configure/workspaces/workspace-lifecycle)


### Working with env vars

We can list out all environment variables (env vars) using the `env` command

We can filter specific env vars using grep eg. `env | grep AWS_`

####   Setting and Unsetting Env Vars

In the terminal we can set using `export HELLO='world'` 

In the terminal we can unset using `unset HELLO` 

We can set an env var temporarily when just running a command


```sh
HELLO='world' .bin/print_message
```

Within a bash script we can set env var without writing export eg.

```sh
#!/usr/bin/env bash

HELLO='world'

echo $HELLO
```

## Print vars

We can print an env var using echo eg. `echo $HELLO`

#### Scoping of Env Vars

When you open a new bash terminals in VSCode it will not be aware of env vars that you set in another window.

If you want to env vars to persist in all future terminals you need to set env vars in your bash profile. eg. `.bash_profile`

#### Persisting env vars in git pod

We can persist env vars into gitpod by storing them in Gitpod secrets Storage.

```
gp env HELLO='world'

```

All future workspace launched will set the env vars for all bash terminals opened in those workspaces

You can also set envars in the `.gitpod.yml` but this can only contain non-sensitive en vars

### AWS CLI Install

AWS CLI is installed for this project

You need to set specific envars via the bash script. 

['./bin/install_aws_cli'](./bin/install_aws_cli)

We can check if our aws credentials are configured correctly by running this command:
```sh
aws sts get-caller-identity
```

if it is succesfull you should see a json payload return that looks like this:

```json
    "UserId": "XXXXXXXXXXXXXX",
    "Account": "XXXXXXXXXXXXXX",
    "Arn": "arn:aws:iam::XXXXXXXXXXXXXXXXX:user/name.example"
```

We need to generate AWS CLI credentials from iam user in order to user the aws CLI.

[Getting starte AWS CLI](https://docs.aws.amazon.com/cli/latest/userguide/cli-chap-getting-started.html)
[AWS CLI Envars](https://docs.aws.amazon.com/cli/latest/userguide/cli-configure-envvars.html)

## Terraform basiscs

## Terraform registry

Terraform soursces their are proiders and modeles form the terraform registory which locatted at the registery at terraform.io
[Terraform registry](https://registry.terraform.io/)

- **Provides** Is an interface to APIs that allow you to create resources in terraform.
[Random Terraform provider](https://registry.terraform.io/providers/hashicorp/random/)
- **Modules** are a way to make large amounts of terraform modular, portable and sharable. 

## Terraform Console 

We can see al list of all the terraform commands by typing 
`terraform`

#### Terraform init

At the start of a new terraform project we will run 
`terraform init` to download the binaries for the terraform providers that we will use for the project.

#### Terraform Plan

`terraform plan`
This will generate a changeset about the state of our infrastructure and what will be changed

We can output this changeset ie. "plan" to be passed to an apply, but often you can just ignore outputting.

#### Terraform Apply

`terraform apply`
This will run a plan and pass the changeset to be executed by terraform. apply should prompt us yes or no. 

If we want to be automated a apply we can use `terraform auto --approve`

### Terraform lock Files

`.terraform.lock.hcl` contains the locked versioning for the providers or modules that should be used with this project

The terraform lock files **should be commited** to your Version Control System eg. Github

### Terraform  state files

`terraform.tfstate` contains info about the current state your infrastructure.
**should not be commited** to your Version Control System eg. Github.

This file can contain sensitive data. If you lose this file you lose knwing the state of your infrasturcture.

`terraform.tfstate.backup` is the previous state file. files state,

## Terraform directory

``.terraform`` directory contains binaries of the terraform providers. 

#### Terraform destroy

`terraform destroy`
This will destroy resources.

`terraform apply auto-approve`
To **skip** the approve prompt where you need to type yes

## Issues with terraform cloud login and gitpod workspace

When attempting to run `terraform login` it launch a bash wiswig to view the token. It does sometimes not work in Gitpod with VS Code.

Create the file mannually here: 

```sh
touch /home/gitpod/.terraform.d/credentials.tfrc.json
open /home/gitpod/.terraform.d/credentials.tfrc.json
```
provide the following code (replace your token in the file)

```json
{
  "credentials": {
    "app.terraform.io": {
      "token": "our token code"
    }
  }
}
```

We have automated this workaround with the following bash script [/bin/generate_tfrc_credentials] (./bin/generate_tfrc_credentials)
