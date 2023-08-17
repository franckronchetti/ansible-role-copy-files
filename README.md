# Ansible Role: Copy Files

Copy files on the target machine.

## Role Variables

Available variables are listed below, along with default values (see `defaults/main.yml`):

`owner`
If it's a new app deployed with our infra CD, set it to *gitlab-ci* ; *github-ci* otherwise

`files`     
A list of files to copy, each file has to have the following properties
* `source_path` (string)
* `destination_path` (string)


## Example Playbook

    - hosts: my_host
      become: true
      gather_facts: true
      vars_files:
        - "{{ playbook_dir }}/env_vars/{{ env }}/copy_files.yml"
      roles:
        - copy_files

*Inside `./env_vars/MY_ENV/copy_files.yml`*:

    ---
    owner: gitlab-ci
    files:
    - source_path: "{{ playbook_dir }}/files/MY_ENV/secrets/service-account.json"
      destination_path: /app/MY_APP_DIR/gcloud/application_default_credentials.json
    - source_path: "{{ playbook_dir }}/files/MY_ENV/secrets/dotenv"
      destination_path: /app/MY_APP_DIR/.env.secrets

## Install

*Inside `./roles/requirements.yml`*:

    ---
    roles:
      - name: copy_files
        src: git@github.com:adeo/kazaplan-ansible-role-copy-files.git
        version: main
        scm: git
