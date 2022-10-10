# octo-happiness

octo-happiness is a playground and demo for creating pipeline projects on demand. The Ansible playbooks wrap the "odering" process.


## Prerequisites for the Ansible playbook to create pipeline projects

In future versions, the prerequisites will be covered in a custom execution environment.


### Install prerequisites - RHEL 8 example

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



## Deploy a test chart project

By default, the Github user `happy-octo` will be used to fork the template repo. This repo contains a `happy-octo`  Github token in the Ansible vault `vault.yml`


**Clone this Repo:**
```
git clone https://github.com/sa-mw-dach/octo-happiness.git
cd octo-happiness
```

**Configure target Github account:**

- Create a new Github account manually or use own account. 
- Create a Github Personal access token
  - Github -> Settings -> Developer setting -> Personal access tokens -> Generate new token
  - Check only repo
  - Copy token and save it temporarily


**Save the Github token in a Ansible vault:**

Configure a vault-password-file:
```
echo replace-with-your-own-vault-password > ansible/vault-password-file
```

Save the Github token in a new Ansible `ansible/vault.yaml` file as variable `gh_token: 'ghp_********'`:


Create a new, plain, unencrypted vault file with your GitHub token. E.g.,
```
echo "gh_token: 'ghp_********'" > ansible/vault.yaml
```

Create a vault file:
```
ansible-vault encrypt --vault-password-file ansible/vault-password-file ansible/vault.yml
```


Login into your target OpenShift cluster, where the test chart should be deployed:
```
oc login ....
```

Run the test. The playbook is going to create a new target Git repository on GitHub, by forking the template repository.

In this case, the `template_lang=test`, the current repository is used as test template. The playbook installs a helm chart into the OpenShift project for this pipeline project.


```
cd ansible
ansible-playbook --vault-password-file vault-password-file 010_create_project.yml -e "app_name=my-octo template_lang=test"
```

## Create a Java Template based pipeline project

Prerequisites:
- OpenShift with OpenShift Pipelines (Tekton) deployed
- Prerequisites of this GitHub repo
- Target Github account configured
- Ansible vault with Github token configured


Create the project:
The playbook will perform following steps:
- Fork the template repository into the target Github account
- Clone the repository into a local temp directory
- Deploy the template helm chart including a build pipeline
- The pipeline will be started automatically.
    

```
cd ansible
ansible-playbook --vault-password-file vault-password-file 010_create_project.yml -e "app_name=my-octo-java template_lang=java"
```
