# Cloud HA Rails – Deploy Automático

## Pré-requisitos

- VirtualBox
- Vagrant
- Ansible (na VM de controle)
- (Opcional) Git para clonar o projeto

## Passo a Passo – Suba Todo o Ambiente
```bash
1. Clone o repositório do projeto 
git clone https://github.com/LucasMachado77/cloud-final-vagrant.git

2. Suba as VMs com Vagrant
vagrant up
Aguarde até todas as VMs subirem (ansible, lb01, lb02, web1, web2, db, db_replica, prometheus, grafana, consul).

3. Acesse a VM de controle (ansible)
vagrant ssh ansible
cd ~/ansible/playbooks   # Ajuste se necessário

4. Gere e copie as chaves SSH para as VMs (só na primeira vez)
ssh-keygen -t rsa -b 4096 -C "ansible@control"
# Se não funcionar siga os passos abaixo
# Pressione Enter para aceitar o padrão de caminho

COPIAR O ARQUIVO DO WIN PARA O ANSIBLE
cp /vagrant/consul_private_key ~/.vagrant.d/consul_private_key

chmod 600 ~/.vagrant.d/consul_private_key

ssh vagrant@192.168.56.18 "mkdir -p ~/.ssh && chmod 700 ~/.ssh"

cat ~/.vagrant.d/consul_private_key.pub | ssh vagrant@192.168.56.18 "cat >> ~/.ssh/authorized_keys && chmod 600 ~/.ssh/authorized_keys"

ssh-keygen -y -f ~/.vagrant.d/consul_private_key > ~/.vagrant.d/consul_private_key.pub


# Copie a chave pública para cada VM (substitua pelo IP de cada máquina):
ssh-copy-id -i ~/.ssh/id_rsa.pub vagrant@192.168.56.11
ssh-copy-id -i ~/.ssh/id_rsa.pub vagrant@192.168.56.12
ssh-copy-id -i ~/.ssh/id_rsa.pub vagrant@192.168.56.10
ssh-copy-id -i ~/.ssh/id_rsa.pub vagrant@192.168.56.20
ssh-copy-id -i ~/.ssh/id_rsa.pub vagrant@192.168.56.14
ssh-copy-id -i ~/.ssh/id_rsa.pub vagrant@192.168.56.15
ssh-copy-id -i ~/.ssh/id_rsa.pub vagrant@192.168.56.16
ssh-copy-id -i ~/.ssh/id_rsa.pub vagrant@192.168.56.17
ssh-copy-id -i ~/.ssh/id_rsa.pub vagrant@192.168.56.18
Senha padrão: vagrant

5. Ajuste o inventário Ansible (hosts.ini)
Exemplo básico:

[ansible]
localhost ansible_connection=local

[web]
192.168.56.11
192.168.56.12

[lb]
192.168.56.10
192.168.56.20

[db]
192.168.56.14
192.168.56.15

[prometheus]
192.168.56.16

[grafana]
192.168.56.17

[consul]
192.168.56.18

[all:vars]
ansible_user=vagrant
ansible_ssh_private_key_file=~/.ssh/id_rsa


6. Execute os playbooks para provisionar o ambiente
cd ~/ansible/playbooks
ansible-playbook web.yml -i hosts.ini         # Provisiona web1 e web2
ansible-playbook lb.yml -i hosts.ini          # Provisiona lb01 e lb02
ansible-playbook db.yml -i hosts.ini          # Configura o banco e réplica
ansible-playbook monitor.yml -i hosts.ini     # Instala Prometheus,  Grafana e Service Discovery Consul
ansible-playbook vegeta.yml -i hosts.ini      # (Opcional) Instala Vegeta para teste de carga

7. Acesse os serviços
App Rails (via VIP):
http://192.168.56.50

App Rails (via web1):
http://192.168.56.11:3000

App Rails (via web2):
http://192.168.56.12:3000

App Rails (via lb):
http://192.168.56.10

App Rails (via lb2):
http://192.168.56.20

Prometheus:
http://192.168.56.16:9090

Grafana:
http://192.168.56.17:3000

Consul UI:
http://192.168.56.18:8500

8. Teste de carga com Vegeta
echo "GET http://192.168.56.50/" > targets.txt
vegeta attack -duration=30s -rate=10 -targets=targets.txt | tee results.bin | vegeta report


9. Desligue e destrua o ambiente quando terminar
exit                 # Sai da VM ansible (se estiver dentro)
vagrant halt         # Para todas as VMs
vagrant destroy      # Remove todas as VMs

```
# Observações
## Sempre cheque o status das VMs com vagrant status.

## Se ocorrer erro, confira logs e ajuste o passo correspondente.

## Atualize os IPs conforme sua configuração real.

## Siga a ordem dos playbooks para evitar conflitos e garantir o deploy automático.
