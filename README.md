# octo-happiness

octo-happiness is a playground and demo for creating pipeline projects on demand. The Ansible playbooks wrap the "odering" process. 


## Prerequisites for the Ansible playbook to create pipeline projects

By default, the Github user `happy-octo` will be used to fork the template repo. This repo contains a `happy-octo`  Github token in the Ansible vault `vault.yml`

In future versions, the prerequisites will be covered in a custom execution environment.


### RHEL 8 (on AWS)

Git:
```
sudo dnf update -y
sudo dnf -y install git python3 ansible
```

[Github CLI](https://cloudaffaire.com/how-to-install-and-configure-github-cli-gh/):

```
curl -LO https://github.com/cli/cli/releases/download/v2.15.0/gh_2.15.0_linux_amd64.rpm
sudo rpm -i gh_2.15.0_linux_amd64.rpm
```


Ansible Collections:
```
ansible-galaxy collection install kubernetes.core
```

Clients:
```
curl -O https://mirror.openshift.com/pub/openshift-v4/clients/ocp/latest/openshift-client-linux.tar.gz
tar zxvf openshift-client-linux.tar.gz
sudo mv oc kubectl /usr/local/bin/
```

```
curl -LO https://mirror.openshift.com/pub/openshift-v4/clients/helm/latest/helm-linux-amd64
sudo mv helm-linux-amd64 /usr/local/bin/helm
sudo chmod +x /usr/local/bin/helm
```



## Deploy a test chart


First, login into your OpenShift cluster, where the test chart should be deployed:
```
oc login ....
```

Clone this Repo:
```
git clone https://github.com/sa-mw-dach/octo-happiness.git
cd octo-happiness
```

Set Ansible vault password file
```
echo replace-with-the-vault-password >> ansible/vault-password-file
```

Run a test. The playbook is going to create a new target Git repository on GitHub, by forking the template repository.
In this case, the `template_lang=test`, there current repository is used as test template. The playbooks install a helm chart into the OpenShift project for this pipeline project.


```
cd ansible
ansible-playbook --vault-password-file vault-password-file 010_create_project.yml -e "app_name=my-octo template_lang=test"
```



