---
title: "Terraform on Proxmox"
date: 2025-04-13
categories: homelab
tags: homelab proxmox terraform
---

## Using Terraform on Proxmox

This is from [Learn Linux TV](https://https://www.google.com/url?sa=t&source=web&rct=j&opi=89978449&url=https://www.youtube.com/watch%3Fv%3D1kFBk0ePtxo&ved=2ahUKEwj_y8_skNWMAxVcwckDHXT2D0cQtwJ6BAgJEAI&usg=AOvVaw0kve9BpSvtpz6HfwEZmr-e)

And [here](https://https://www.youtube.com/redirect?event=video_description&redir_token=QUFFLUhqbmxVb0lHMHBRLUprdFpRU0ozSjZqRzgzaUZiZ3xBQ3Jtc0tuRjlNNkxqV00zNHE5eDFSdGhPbjNMb3lwcm9FU0lZNnViLUJHLXAtQTFNbVBjTHVvZ0FsdHFDVTlEN0JUNnV3bVA1akIzblpRNzVXQ09MTVNlV3hoeHRLOUcyTHl3czBXWmNhUXdnT2g5eGw2dkl3RQ&q=https%3A%2F%2Flearnlinux.link%2Fproxmox-tf&v=1kFBk0ePtxo) is the blog post that has the information needed.

The main.tf file is as follows:

```hcl
terraform {
    required_providers {
        proxmox = {
            source = "telmate/proxmox"
        }
    }
}

provider "proxmox" {
    pm_api_url          = "https://url-to-proxmox-server:8006/api2/json"
    pm_api_token_id     = "your-token-id"
    pm_api_token_secret = "your-secret"
    pm_tls_insecure     = true
}

resource "proxmox_vm_qemu" "vm-instance" {
    name                = "vm-instance"
    target_node         = "pve-1"
    clone               = "your-template-name"
    full_clone          = true
    cores               = 2
    memory              = 2048

    disk {
        size            = "32G"
        type            = "scsi"
        storage         = "your-storage-volume"
        discard         = "on"
    }

    network {
        model     = "virtio"
        bridge    = "vmbr1"
        firewall  = false
        link_down = false
    }

}
```
List of permissions in Proxmox:

- Datastore.AllocateSpace
- Datastore.Audit
- Pool.Allocate
- SDN.Use
- Sys.Audit
- Sys.Console
- Sys.Modify
- Sys.PowerMgmt
- VM.Allocate
- VM.Audit
- VM.Clone
- VM.Config.CDROM
- VM.Config.CPU
- VM.Config.Cloudinit
- VM.Config.Disk
- VM.Config.HWType
- VM.Config.Memory
- VM.Config.Network
- VM.Config.Options
- VM.Migrate
- VM.Monitor
- VM.PowerMgmt