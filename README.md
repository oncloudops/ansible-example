# ansible example

This is an example of ansible playbook.

For the generic single binary application.

Such as [etcd](https://etcd.io/).

## playbook structure

```
├── README.md
├── inventory                 # inventory
│   └── dev.yml               # environment
├── roles
│   └── etcd                  # ansible role
│       ├── README.md
│       ├── defaults
│       ├── files
│       ├── handlers
│       ├── meta
│       ├── tasks
│       ├── templates
│       ├── tests
│       └── vars
└── site.yml                  # entry
```

The all variables are defined in _roles/etcd/vars/main.yml_

```
service_name: etcd
service_user: etcd
service_home: /opt/etcd
service_data: /opt/etcd/data
service_version: v3.4.27
service_os_arch: linux-amd64
service_tar_gz_url: |
  https://github.com/etcd-io/etcd/releases/download/{{ service_version }}/etcd-{{ service_version }}-{{ service_os_arch }}.tar.gz
download_timeout: 120
```

## The application structure

```
/opt/etcd                                 # service_home
├── current -> /opt/etcd/etcd-v3.4.27-linux-amd64 # softlink for current version
├── data                                  # service_data
├── etcd-v3.4.27-linux-amd64              # service binaries
├── etcd-v3.4.27-linux-amd64.tar.gz       # service binaries tarball
└── ssl                                   # extract dependencies

/etc/default/etcd                         # default environment file
/lib/systemd/system/etcd.service          # systemdctl service
```

for sensitive data, such as _password_ and _private keys_, please using _[ansible-vault](https://docs.ansible.com/ansible/latest/cli/ansible-vault.html)_ to encrypt the string or file.

## how to start

```
# install
ansible-playbook -i inventory/dev.yml site.yml

# upgrade
ansible-playbook -i inventory/dev.yml site.yml -e upgrade=true
```
