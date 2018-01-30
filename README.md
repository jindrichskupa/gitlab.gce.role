Role Name
=========

A brief description of the role goes here.

Requirements
------------

* GCE service account credentials
* Google Cloud DNS
* GCE inventory script with setup

Role Variables
--------------

```
machine_type: n1-standard-1
image: debian-9
instance_name: gitlab-example-org
service_account_email: gitlab@example.org
credentials_file: ~/.gce/credentials.json
project_id: example-org
domain_name: example.org
admin_email: admin@example.org
```

Dependencies
------------

No dependencies

Example Playbook
----------------

**Runs**

```bash
ansible-playbook -i contrib/inventory/gce.py --skip-tags=deploy --tags=gce playbooks/gitlab.yml
ansible-playbook -i contrib/inventory/gce.py --skip-tags=provision --tags=install,config playbooks/gitlab.yml
```

**Playbook**

```yaml
- name: Create virtual server with DNS record for GitLab on GCE
  hosts: localhost
  gather_facts: false
  roles:
    - role: gitlab.gce.role
      machine_type: n1-standard-1
      instance_name: gitlab-example-org
      image: debian-9
      service_account_email: 1081628783844-compute@developer.gserviceaccount.com
      credentials_file: ~/.gce/GCEGitlabCloud-2828f7861962.json
      project_id: gitlab-cloud-193706
      domain_name: example.org
      admin_email: admin@example.org
      tags:
        - provision

- name: Deploy GitLab on virtual server via docker image
  hosts: gitlab-example-org
  become: true
  become_user: root
  remote_user: shorty
  roles:
    - role: gitlab.gce.role
      domain_name: example.org
      project_id: gitlab-example-org
      admin_email: admin@example.org
      tags:
        - deploy
```


License 
-------

MIT

Author Information
------------------

Jindrich Skupa, jindrich.skupa@gmail.com, https://github.com/jindrichskupa
