# image_builder

A role that allows you to build VM images using the Image Builder. At the time
of this writting it supports: 

1. Create custom partitioning schema with LVM
2. Add users and packages
3. Supports only content from CDN and the same OS version as the building host
4. `image-installer` image types. It is tested with `qcow` images but it should 
work with other image types as well. 
5. The role assumes that the target image should run `cloud-init` is it is 
intented to be used as a "golden image". However, it is possible to generate
specific images with custom hostnames, packages, fully registered to either
RH CDN or to a Red Hat Satellite infrastructure. 

## Requirements

A fully registered RHEL server. You need to have sufficient space to store the 
built image, ideally, either a NAS or SAN volume. 

## Role Variables

| Variable                   | Default Value                                       | Description                                                          |
|----------------------------|---------------------------------------------------|------------------------------------------------------------------------|
| `ib_name`                  | `custom-image`                                    | Name of the image blueprint                                            |
| `ib_description`           | `A custom image built with Red Hat Image Builder` | Description of the image blueprint                                     |
| `ib_date_based_version`    | `false`                                           | Use date-based versioning for the image                                |
| `ib_image_version`         | `0.0.1`                                           | Version string of the image                                            |
| `ib_vm_distro`             | `rhel-9`                                          | Target OS distribution and version                                     |
| `ib_groups`                | `[core]`                                          | DNF groups to include                                                  |
| `ib_target_image_format`   | `qcow2`                                           | Format of the output image (e.g., qcow2, raw)                          |
| `ib_content_provider`      | `cdn`                                             | Source for packages: `cdn` or `satellite`                              |
| `ib_satellite_base_url`    | `''`                                              | Base URL of Satellite server (if used)                                 |
| `ib_custom_repositories`   | `[]`                                              | List of custom repositories to add                                     |
| `ib_add_packages`          | List of common utilities (bash-completion, vim, etc.) | Additional packages to install alongside core                      |
| `ib_user_packages`         | `[]`                                              | User-defined packages to install                                       |
| `ib_packages_rm_default`   | List of firmware, insights, cockpit, NetworkManager-tui | Packages to remove by default after installation                 |
| `ib_packages_rm_user`      | `[]`                                              | Additional packages to remove specified by user                        |
| `ib_packages_rm`           | `ib_packages_rm_default + ib_packages_rm_user`   | Combined list of packages to remove after installation                  |
| `ib_users`                 | `[]`                                              | List of users to create (with details like username, ssh keys, groups) |
| `ib_timezone`              | `UTC`                                             | Timezone for the image                                                 |
| `ib_ntpservers`            | `[]`                                              | List of NTP servers                                                    |
| `ib_languages`             | `['en_US.UTF-8']`                                 | Languages/locales for the image                                        |
| `ib_keyboard`              | `us`                                              | Keyboard layout                                                        |
| `ib_hostname`              | `localhost.localdomain`                           | Hostname for the image                                                 |
| `ib_kernel_append`         | `''`                                              | Additional kernel command line arguments                               |
| `ib_service_enable`        | `[sshd]`                                          | List of systemd services to enable                                     |
| `ib_service_disable`       | `[cockpit.socket]`                                | List of systemd services to disable                                    |
| `ib_service_masked`        | `[]`                                              | List of systemd services to mask                                       |
| `ib_firewall_services_enabled_ext` | `[]`                                      | User-defined additional firewalld services to enable                   |
| `ib_firewall_services_enabled` | `ib_default_firewall_services + ib_firewall_services_enabled_ext` | Combined firewalld services to enable              |
| `ib_firewall_services_disabled` | `[]`                                         | Firewalld services to disable                                          |
| `ib_firewall_ports`        | `[]`                                              | List of firewall ports to open                                         |
| `ib_directories`           | `[]`                                              | Custom directories to create                                           |
| `ib_files`                 | `[]`                                              | Custom files to add (not recommended except for special cases)         |
| `ib_ca_file`               | `""`                                              | Path to a custom CA certificate file                                   |
| `ib_ca_file_content`       | `""`                                              | Content of the custom CA certificate                                   |
| `ib_partitioning_mode`     | `lvm`                                             | Partitioning mode: `lvm`, `autolvm`, or `raw`                          |
| `ib_lvm_vg_name`           | `RENAME_ME_AT_FIRST_BOOT`                         | Default LVM volume group name                                          |
| `ib_new_vg_name`           | `""`                                              | Desired new VG name for renaming on first boot                         |
| `ib_rename_vg_at_boot`     | `true` (if default VG name is unchanged or new VG name is set) | Whether to rename VG on first boot                        |
| `ib_default_vg`            | Sanitized version of `ib_vm_distro`               | Default VG name derived from the distro                                |
| `ib_partitions`            | See default partition scheme (BOOT + LVM VG)      | Partition layout including LVM logical volumes                         |
| `ib_mount_points`          | `[]`                                              | Additional mount points for `autolvm` mode                             |
| `ib_first_boot`            | Service definitions for first boot customization  | Systemd service configuration for first boot scripts                   |
| `ib_configuration_base_dir`| `/srv/osbuild`                                    | Base directory for configuration                                       |
| `ib_official_overrides_dir`| `/etc/osbuild-composer/repositories/`             | Directory for official repository overrides                            |
| `ib_official_repos_dir`    | `/usr/share/osbuild-composer/repositories/`       | Directory containing official repository definitions                   |
| `ib_default_fs`            | `xfs`                                             | Default filesystem type for custom partitions                          |
| `ib_user`                  | `{{ ansible_ssh_user }}`                          | User running the image build process                                   |
| `ib_blueprint_dir`         | `ib_blueprints`                                   | Directory to store image blueprints                                    |
| `ib_dl_dir`                | `images`                                          | Directory where downloaded images are stored                           |
| `ib_dl_become`             | `false`                                           | Whether to use privilege escalation when storing downloaded images     |
| `ib_clean_build_image`     | `false`                                           | Delete the built image after download                                  |
| `ib_clean_blueprint`       | `false`                                           | Delete blueprint after image build                                     |


> The full list of pre-configured packages that will be installed can be found in [this 
page](https://docs.redhat.com/en/documentation/red_hat_enterprise_linux/9/html/composing_a_customized_rhel_system_image/creating-system-images-with-composer-command-line-interface_composing-a-customized-rhel-system-image)

## Dependencies

TODO

## How can I use this? 

The goal of this role is to evolve into an easy-to-use tool to help you build 
"Golden Images", or VM images that can be used in a VM template. The role 
supports building RHEL, or RHEL derivatives, images only using RH's Image Builder.

You may build one or multiple images. If you want to build one image, you 
need to provide the role with all the `ib_` variables required to build it as 
you wish. 

In case you want to build multiple images at once, you still need to provide
those `ib_` variables but as a list of dictionaries inside the variable 
`ib_images`. 

One thing you need to take into account is that Image Builder can build images
only up to the RHEL version of the host. Thus, on a RHEL 9 host you can build 
RHEL 8 and RHEL 9 images, but not RHEL 10. If you want to build RHEL 10 images, 
you need to use a RHEL 10 host. 

### Notable characteristics of the role

The role was built because I had the need to produce my own RHEL Images for 
different use cases. My target was to replace VM build from kickstarts and 
Red Hat Satellite server. I needed a method to generate RHEL images with
specific packages installed and specific partition scheme. 

#### Default packages
My preference is to use as little packages as possible. Thus, the default 
DNF group used during the image creation is `core`. However, there are some 
packages that I prefer to be installed, like `vim` or `bash-completion`. You 
can define the the additional packages to the `core` packages you would like to 
install using the `ib_add_packages` variable, or the `ib_user_packages`.  
There are 2 variables involved here because during development it was clear that
some users wanted to maintain the list of additional packages, but also to add
more other packages too. Using 2 variables make your life easier since you don't
need to handle that list, but only the second one. Of course, if you don't want 
to install the default additional packages you may override this variable via
`group_vars`, `host_vars` or even extra vars. 

Image Builder doesn't have a mechanism to avoid the installation of specific 
packages, at least a mechanism that I am aware of. At the same time I find very
annoying (to say the least) that on a server (and especially a VM!) the default
installation adds drivers for WiFi cards. I don't even need them. Since we cannot
exclude those packages, the role adds a script that is executed once at the
first boot, typically, after `cloud-init`. The script will remove all packages
listed in the `ib_packages_rm` and `ib_packages_rm_user`, which follow the same
logic as the "add" packages.

#### Partition tables
The default partition table of the role uses LVM of course and has 3 logical 
volumes for `/`, `/var`, `/home`. Also, there is an 8GB swap partition and the 
standard EFI partition which is generated by Image Builder.

In that case you don't want to handle partitions, you also have the 2 more 
options provided by Image Builder, the default auto-LVM functionality and "raw". 

In brief, the "autolvm" means that Image Builder will create an LVM on top of 
the physical volume if you define **at least** one custom mount point. In the 
role this is controlled from the variable `ib_partitioning_mode` as follows: 

| Value    | Meaning                                                       |
|----------|---------------------------------------------------------------|
| lvm      | Full control on the partition table                           |
| autolvm  | You only mount points, not full details for the mount points  |
| raw/any value| Fully managed by Image Builder, no LVM at all, every other storage-related variable is ignored |
**Table: `ib_partitioning_mode` Options and Their Meanings**

To enable the `autolvm` mode, you also need to define at least one mount point 
in the `ib_mount_points` list of dictionaries. For example: 

```yaml
ib_mount_points: 
  - mp: /var
    minsize: 2Gb
```

> Please note that defining `autolvm` without specifying any mount point under the
> `ib_mount_points` has the same effect as `raw`.

Mount points are ignored if you define `ib_partitioning_mode` to `lvm`.



In case you want to have full control, withing the limitations of Image Builder,
for the LVM, you can define the partion scheme in the variable named `ib_partitions`.

To define an LVM setup in Image Builder, we must create a volume group and 
specify the logical volumes that resides on top of it. Each Logical volume 
requires a name, a mount point, the **minimum** size of the LV, and its 
filesystem.

The default partition scheme is the following: 

```yaml

ib_partitions:
  - type: lvm
    name: vg_{{ ib_lvm_vg_name | default(ib_default_vg) }}
    minsize: 15 GiB
    logical_volumes:
      - name: root
        mountpoint: /
        minsize: 10 GiB
        label: root
        fs_type: xfs
      - name: var
        minsize: 4 GiB
        mountpoint: /var
        label: var
        fs_type: xfs
      - name: home
        minsize: 4 GiB
        label: home
        fs_type: xfs
        mountpoint: /home
      - name: swap
        fs_type: swap
        minsize: 8GiB
```

#### Volume group name
My personal preference for LVM VGs is to use a unique, per server, Volume Group. 
However, this is not what everyone wants. The role, by default, generates a VG
name that is easy to change afterwards. You may choose a name that will help
administrators to change it afterwards, for example "CHANGE_ME" or you may omit 
this variable and use the default VG name which is a sanitized version of the
distribution. Thus, if the distribution is set to "rhel-9.5" the VG name will
be `vg_rhel-9_5`.

The following table summarizes the variables controlling the behavior or VG.

| Value    | Meaning                                                       |
|----------|---------------------------------------------------------------|
| ib_lvm_vg_name       | The VG name defined by the role |
| ib_new_vg_name       | The VG name we wish for our new servers |
| ib_rename_vg_at_boot | Control if we will change the name of the VG during fist boot |
| ib_default_vg        | Default value, set to a sanitized version of `ib_vm_distro` |

**Table: Volume group name management**

> It is clear that the VG name variables have an effect only if you have 
> specified `lvm` as the partitioning scheme and 

If you wish to use a unique name per server, you might find useful the option 
to rename the default volume group to a truly unique VG name. The role will
add the option to change the VG name during first boot using a sanitized
hostname. For example for a server named `dev-server.1.example.com` the first 
boot script will rename the VG to `dev-server_1`. 

### First boot script
If you request to change the VG namd and/or to remove packages after first boot
the role will add a `systemd` service for the first boot of the image. The
service is scheduled to run only at the first boot and only once, after the 
initialization of the new VM from `cloud-init`. 

## What is tested and confirmed to work

The following functionality is tested to work: 

* `qcow2` image format
* Custom partion scheme
* Auto-lvm 
* Users and groups
* Custom directories
* CA file. If you add a custom CA certificate, the first boot script will run
* Removal of RPM packages at first boot
* Rename of VG name when we use custom partition table

## Not tested and not confirmed functionality

* Usage a Satellite server as the packages source 
* Automatic registration of the image
* Custom repositories
* Any other than qcow2 image format 
* Kernel settings
* Firewald services and ports
* Masked services
* `ext4` filesystems

### Untested functionality that should work out of the box

* Any other than qcow2 image format 
* Kernel settings
* Firewald services and ports
* Masked services
* `ext4` filesystems

Please feel free to report any issue or even a success story in the "Issues" 
section.

## References


* [OSBuild User Guide - Introduction](https://osbuild.org/docs/user-guide/introduction/)
* [Red Hat Enterprise Linux 9 - Composing a Customized RHEL System Image](https://docs.redhat.com/en/documentation/red_hat_enterprise_linux/9/html/composing_a_customized_rhel_system_image/index)
* [Cloud-init Official Documentation](https://cloud-init.io/)

## Example Playbookσ

### Single image management
```yaml

  - hosts: build_server
    become: false
    gather_facts: false
    roles:
      image_builder
```

### Multiple images

```yaml

  - hosts: build_server
    become: false
    gather_facts: false
    tasks:
      - name: Build multiple images
        ansible.builtin.include_role:
          name: itcultus.golden_image.image_builder
        loop: "{{ ib_images | default([]) }}"
        loop_control:
          label: "{{ __image_data.name }}"
          loop_var: __image_data
```


## License

GPLv3

## Author Information
Peter Tselios  
ITCultus LTD
