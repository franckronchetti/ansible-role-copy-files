# Ansible Role: Copy Files

Copy files on the remote machine.

## Role Variables

Available variables are listed below, along with default values (see `defaults/main.yml`):

`owner`
If it's a new app deployed with our infra CD, it has to be set to *gitlab-ci*

`group`
If it's a new app deployed with our infra CD, it has to be set to *docker*

`files`
A list of files to copy, each file has to have the following properties
* `source_path` (string)
* `destination_path` (string)
* `owner` (string) - optional
* `group` (string) - optional

`templates`
A list of jinja templates to use and copy the dynamized file to the destination path.
* `source_path` (string)
* `destination_path` (string)
* `owner` (string) - optional
* `group` (string) - optional


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
    files:
      - source_path: "{{ playbook_dir }}/files/MY_ENV/secrets/service-account.json"
        destination_path: /app/MY_APP_DIR/gcloud/application_default_credentials.json
        owner: my_app_user
        group: my_group
      - source_path: "{{ playbook_dir }}/files/MY_ENV/secrets/dotenv"
        destination_path: /app/MY_APP_DIR/.env.secrets

    templates:
      - source_path: "{{ playbook_dir }}/templates/app.yml.j2"
        destination_path: /app/MY_APP_DIR/my_app.yml
        owner: my_app_user
        group: my_group

## Install

*Inside `./roles/requirements.yml`*:

    ---
    roles:
    - name: copy_files
      src: https://github.com/franckronchetti/ansible-role-copy-files
      version: v1.1.0
