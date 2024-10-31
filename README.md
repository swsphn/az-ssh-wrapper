# Az SSH Wrapper

This package provides a wrapper script for the Azure CLI `az ssh`
command. This makes it possible to use `az ssh` with programs which
expect the standard SSH interface, such as Ansible. This is useful for
example if you want to deploy an Ansible role to an Azure VM, and you
want to automatically connect to the VM using `az ssh`, rather than
needing to manage SSH keys, etc.

For example, if you can already connect to a VM using the following
Azure CLI command, this wrapper will enable Ansible to connect in the
same way:

    az ssh --ip 1.2.3.4

After installing this package you can simply specify
`ansible_ssh_executable = az-ssh`, and it will automatically connect to
the VM using your Azure credentials with your existing Azure CLI login.

`az-ssh` accepts all the standard SSH arguments, and passes them on
directly to `az ssh`.

It filters out the `ControlMaster` and `ControlPersist` options, as
there is a known issue using these options with `az ssh`, which causes
`az ssh` to block for the duration of the `ControlPersist` value. See
the following GitHub issue for details:

https://github.com/Azure/azure-cli-extensions/issues/7285

## Install


