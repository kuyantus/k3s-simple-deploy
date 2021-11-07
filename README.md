# What is it?

[![GitHub Super-Linter](https://github.com/kuyantus/k3s-simple-deploy/workflows/Lint%20Code%20Base/badge.svg)](https://github.com/marketplace/actions/super-linter)


This repo contains code, which deploy k3s cluster (1 master and 2 worker nodes, but you can only deploy master node) and after that will deploy simple php application from kubernetes tutorials: <https://kubernetes.io/docs/tutorials/stateless-application/guestbook/>.

Given playbooks and roles will install all needed stuff to be able to run k3s cluster and php application, but we need some prerequisites before deploy.

## Install prerequisites (venv, roles and collections)

Firstly, we need create python venv to run our deploy.

```bash
python3 -m venv .venv
source .venv/bin/activate
python3 -m pip install -r requirements.txt
```

Afterwards verify that ansible is installed by running `ansible --version`.

* `ansible-galaxy install -r requirements.yml`
* `ansible-galaxy collection install -r requirements.yml`

Secondly, we need some clean installation of Ubuntu Server 20.04 (code was tested only on this OS version).

Thirdly, we need to add entry with our deployment to system host file to further management. By default it is nginx_host: nginx.local and ip address of master node k3s_master: 192.168.2.100, so you will definetely change it depends on your setup.

## Explanation of playbooks

Let's examine main.yaml file in root directory. It contains all playbooks which will deploy our setup.

`"playbooks/users.yaml"` — deploy user for deployment. It's common best practice to deploy such things by separate user. Edit `environments/shared/users.yaml` file to enter your user.

`"playbooks/common.yaml"` — update all packages and install needed python dependencies.

`"playbooks/docker.yaml"` — install latest docker engine to hosts.

`"playbooks/k3s.yaml"` — install k3s cluster to provided hosts. Before installation you can change master node ip and choose k3s version.

`"playbooks/helm.yaml"` — install helm binary to master node.

`"playbooks/nginx.yaml"` — deploy nginx from helm chart.

`"playbooks/deploy.yaml"` — deploy our php application.

`"playbooks/check.yaml"` — just check from localhost, if our deployment success or not. You need to add entry to local hosts file to make the check work.

I hope, this code will help you to make your own application deployments to kubernetes much easier.
### Links
* [Ansible documentation](https://docs.ansible.com/ansible/index.html "Ansible documentation")
* [Ansible Playbook Best Practices](https://docs.ansible.com/ansible/latest/user_guide/playbooks_best_practices.html "Ansible Playbook Best Practices")
* [Ansible example structure](https://github.com/express42/ansible-repertory "Ansible example structure")
* [Python3 venv module documentation](https://docs.python.org/3/library/venv.html "Python3 venv module documentation")

### License

MIT
