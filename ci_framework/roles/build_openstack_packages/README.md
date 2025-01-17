## Role: build_openstack_packages
An Ansible role for generating custom RPMSs of OpenStack Projects using DLRN

### Parameters

* `cifmw_bop_dlrn_deps`: A list of DLRN project system dependencies.
* `cifmw_bop_build_repo_dir`: The directory where the DLRN repo is built. The variable defaults to the home directory of the user.
* `cifmw_bop_dlrn_repo_url`: The URL of the DLRN repository
* `cifmw_bop_dlrn_from_source`: Install DLRN from git. The default value is false
* `cifmw_bop_rdoinfo_repo_url`: The URL of the rdoinfo repository that contains the project definitions for DLRN
* `cifmw_bop_rdoinfo_repo_name`: Name of the rdoinfo repository
* `cifmw_bop_initial_dlrn_config`: Name of the initial DLRN config. The default value is centos
* `cifmw_bop_dlrn_target`: Name of the DLRN target. The default value is centos9-local.
* `cifmw_bop_use_components`: Build the openstack rpm packages under component. The default value is 1. Set 0 to avoid using it.
* `cifmw_bop_yum_repos_dir`: The path to repos generated by repo-setup tool.
* `cifmw_bop_openstack_release`: The default release of OpenStack project. Default value is master.
* `cifmw_bop_openstack_project_path`: The full path of openstack clonned project to be built
* `cifmw_bop_gating_repo`: The path of directory to store the generated rpms.
* `cifmw_bop_dlrn_cleanup`: Clean up the DLRN aftifacts. The default is set to False.

### TODO
- Allow building multiple openstack projects
- Build RDO dist-git changes
- Parse gerrit review and clone the respective project and build it
- Build rpm from Zuul Depends On

### Sample Playbook

```
- hosts: localhost
  vars:
    cifmw_bop_dlrn_target: centos9-stream
    cifmw_bop_initial_dlrn_config: centos9-local
    cifmw_bop_dlrn_baseurl: https://trunk.rdoproject.org/centos9-master
    cifmw_bop_yum_repos_dir: /tmp/repos
  pre_tasks:
    - name: Clone neutron-tempest-plugin
      ansible.builtin.git:
        accept_hostkey: true
        dest: "/tmp/neutron-tempest-plugin"
        repo: "https://github.com/openstack/neutron-tempest-plugin"

    - name: Set cifmw_bop_openstack_project_path
      ansible.builtin.set_fact:
        cifmw_bop_openstack_project_path: "/tmp/neutron-tempest-plugin"

  roles:
    - role: "build_openstack_packages"
```
