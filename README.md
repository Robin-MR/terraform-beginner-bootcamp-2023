# Terraform Beginner Bootcamp 2023

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

### Gitpod lifecycle (Before, Init, Command)

We need to be careful when using the init because it will ***not rerun*** if we restart an existing workspace.


[Gitpod lifecycle](https://www.gitpod.io/docs/configure/workspaces/workspace-lifecycle)

## External Reference

- [Shebang Unix](https://en.wikipedia.org/wiki/Shebang_(Unix))
- [Install Terraform CLI](https://developer.hashicorp.com/terraform/tutorials/aws-get-started/install-cli)
- [Linux Os Version Command](https://www.cyberciti.biz/faq/how-to-check-os-version-in-linux-command-line/)
- [Chmod](https://en.wikipedia.org/wiki/Chmod)
- [Gitpod lifecycle](https://www.gitpod.io/docs/configure/workspaces/workspace-lifecycle)