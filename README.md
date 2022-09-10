# octo-happiness




## Prerequisites for the Ansible playbook to create pipeline projects

By default, the Github user `happy-octo` will be used to fork the template repo. This repo contains a Github tokne in the Ansible vault `vault.yml`

### RHEL 8 in AWS

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

login into you openshift cluster
```
oc login ....
```

Clone Repo:
```
git clone https://github.com/sa-mw-dach/octo-happiness.git
cd octo-happiness
```

Set Ansible vault password file
```
echo replay-with-my-secret-password >> ansible/vault-password-file
```



### Deploy a test chart

```
cd ansible
ansible-playbook --vault-password-file vault-password-file 010_create_project.yml -e "app_name=my-octo template_lang=test"
```







