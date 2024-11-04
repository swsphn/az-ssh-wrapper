# Az SSH Wrapper

This package provides a wrapper script for the Azure CLI `az ssh`
command. This makes it possible to use `az ssh` with programs which
expect the standard SSH interface, such as Ansible. This is useful for
example if you want to deploy an Ansible role to an Azure VM, and you
want to automatically connect to the VM using `az ssh`, rather than
needing to manage SSH keys, etc.

`az-ssh` accepts all the standard SSH arguments, and passes them on
directly to `az ssh`.

It filters out the `ControlMaster` and `ControlPersist` options, as
there is a known issue using these options with `az ssh`, which causes
`az ssh` to block for the duration of the `ControlPersist` value. See
the following GitHub issue for details:

https://github.com/Azure/azure-cli-extensions/issues/7285

## Requirements

- [Azure CLI][azure-cli]
- An Azure VM with the `AADSSHLoginForLinux` VM extension installed.
- An Entra account with the `Virtual Machine Administrator Login` or
  `Virtual Machine User Login` role configured for the VM.
- Linux or WSL

## Install from PyPI

Install with `pip`, `pipx` or `uv tool`.

```
pip install az-ssh-wrapper
```

```
pipx install az-ssh-wrapper
```

```
uv tool install az-ssh-wrapper
```

## Install latest development version from GitHub

Install with `pip`, `pipx` or `uv tool`

```
pip install az-ssh-wrapper@git+https://github.com/swsphn/az-ssh-wrapper.git
```

```
pipx install az-ssh-wrapper@git+https://github.com/swsphn/az-ssh-wrapper.git
```

```
uv tool install az-ssh-wrapper@git+https://github.com/swsphn/az-ssh-wrapper.git
```

Note: While the package is called `az-ssh-wrapper`, the installed
executable is called `az-ssh`.

## Examples

If you can already connect to a VM using the following Azure CLI
command, this wrapper will enable Ansible to connect in the same way:

    az ssh vm --ip 1.2.3.4

For example, the above command using this wrapper is:

    az-ssh 1.2.3.4

If you need to pass options or arguments to SSH, Azure CLI requires you
to use the following syntax:

    az ssh vm --ip 1.2.3.4 -- <options>

For example, to connect to the server, run the `date` command, then
exit:

    az ssh vm --ip 1.2.3.4 -- date

In contrast, Az SSH Wrapper allows you to use the standard SSH form:

    az-ssh 1.2.3.4 date

## Use with Ansible

After installing this package you can simply specify
`ansible_ssh_executable = az-ssh`, and it will automatically connect to
the VM using your Azure credentials with your existing Azure CLI login.
For example, you could configure this as a variable in your hosts
inventory file.

[azure-cli]: https://learn.microsoft.com/en-gb/cli/azure/install-azure-cli
