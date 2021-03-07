- [Role Name](#role-name)
  - [Role Variables](#role-variables)
  - [Dependencies](#dependencies)
  - [Example Playbook](#example-playbook)
  - [NOTE: Certificates in Git](#note-certificates-in-git)

Role Name
=========

This role creates a local update repository, which can be used for situations where it is not possible to access the Internet.

Role Variables
--------------

A description of the settable variables for this role should go here, including any variables that are in defaults/main.yml, vars/main.yml, and any variables that can/should be set via parameters to the role. Any variables that are read from other roles and/or the global scope (ie. hostvars, group vars, etc.) should be mentioned here as well.

| Variable name | Type   | Description    
|---|---|---|
| repo_location | String | The repo location is the place where the repository will be stored on the repository box. It is important that you ensure wherever this is, there is adequate space for the repository to be downloaded. |
| repo_repos | String Array | A list of the repos that should be downloaded locally in the repo |
| repo_hostname | String | The hostname for the repository, this is used to configure nginx to expect connections using this hostname. The role will also add in the IP address for the repository automatically so you don't need to include this. |
| config_ssl | Boolean | Whether the repoistory should make the config files available over https |
| ssl_crt_filename | String | The file name that should be used for the crt file in the nginx configuration. |
| ssl_crt_src | String | The location of the crt file. |
| ssl_key_filename | String | The file name that should be used for the key file in the nginx configuration. |
| ssl_key_src | String | The location of the key file. |


Dependencies
------------

This role does not have any dependencies on other roles.

Example Playbook
----------------

Including an example of how to use your role (for instance, with variables passed in as parameters) is always nice for users too:

    - hosts: servers
      roles:
         - role: username.rolename
           repo_location: /repo/
           repo_repos:
             - baseos
             - appstream
             - extras
             - epel
           config_ssl: true
           ssl_crt_filename: repo.crt
           ssl_crt_src: files/repo.crt
           ssl_key_filename: repo.key
           ssl_key_src: files/repo.key

NOTE: Certificates in Git
-------

Storing certificates in Git, especially if it is public is not something you should do. To get around this, it is recommended that you use something like Ansible Vault to encrypt your certificates if you want to store them with your ansible code.

You can achieve by running the following command against your repo.key/crt files.

```ansible-vault encrypt repo.key```

Ansible Vault will prompt you for a password. If you forget the password then you will not be able to recover the files so don't lose it.

When running your ansible playbook you'll need to provide your ansible vault password to allow ansible to unencrypt the file.

``` ansible-playbook -i inventory.yml site.yml --ask-vault-pass ```