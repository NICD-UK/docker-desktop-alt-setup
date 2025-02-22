# docker-desktop-alt-setup

> WARNING: This guide is work in progress!! Please read carefully and trust nothing/no-one! :D

## Prerequisites

- Some knowledge of command line, SSH, and Docker.
- Have `brew` installed

## Uninstall Docker Desktop

We need to remove all of the current desktop setup. 

Uninstalling Docker Desktop destroys Docker containers, images, volumes, and other Docker related data local to the machine, and removes the files generated by the application. Refer to the [back up and restore data](https://docs.docker.com/desktop/backup-and-restore/) section to learn how to preserve important data before uninstalling.


 1. From the Docker menu, select Troubleshoot and then select Uninstall.
 2. Click Uninstall to confirm your selection.


## Install Multipass

https://multipass.run/

### Quick install Mac

```
brew install --cask multipass
```

## Create Multipass VM

You'll first need to make a copy of `cloud-init-example.yaml` and name the copy `cloud-init.yaml`

In `cloud-init.yaml` now add your public SSH key so that it will be added when you create the VM.

This command will setup a VM with an allocation of 2 CPUS, 8G of RAM, and 50G disk. Change these to something that works for you.

```bash
multipass launch ubuntu -name docker-machine -cpus 2 -mem 8G -disk 50G --cloud-init cloud-init.yaml
```

## Install Docker CLI

The next command will install the docker CLI that we'lll use to talk to the multipass VM.

```
brew install docker
```

## Install Docker Compose

The next command will install the Docker Compose CLI that we'lll use to talk to the multipass VM.

```
brew install docker-compose
```


## Mount local home directory to vm

This next step will mount your user directory in the VM so it can you can access it in docker. 

You can alternatively be more specific with the mount if you like. So rather than mounting to `$HOME`, you could mount to `$HOME/code` or similar.

```
multipass mount $HOME docker-machine
```

## List available VMs and details

```
multipass list
```

## Test that you can log into the VM via SSH

```
ssh ubuntu@<your-vm-ip-address>
```
## Now exit the VM

```
exit
```

## Create context so that the docker cli connects to the VM via SSH

```
docker context create docker-vm --docker "host=ssh://ubuntu@docker-machine.local:22"
docker context use docker-vm 
```

## If you need to change the host to an IP or other details of the context use:
```
docker context update docker-vm --docker "host=ssh://ubuntu@192.168.64.4:22"
```


## Want to completely remove the VM we made with Multipass?
```
multipass delete docker-machine
multipass purge
```
