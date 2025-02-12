# Atelier 6

## Exercice 1 : 

1. **Démarrer les VMs**  
```bash
cd ~/formation-ansible/atelier-06
vagrant up
```

2. **Connexion au Control Host**
```bash
vagrant ssh control
```

3. **Edition du fichier `/etc/hosts`**
Ajout des hosts :
```bash
192.168.56.20  target01
192.168.56.30  target02
192.168.56.40  target03
```

4. **Création d'une paire de clés SSH**
```bash
ssh-keygen -t rsa
```

5. **Distribution des clés**
```bash
ssh-copy-id vagrant@target01
ssh-copy-id vagrant@target02
ssh-copy-id vagrant@target03
```

6. **Mise à jour et installation d'Ansible**
```bash
sudo apt update && sudo apt install -y ansible
```

7. **Ping sans configuration**
```bash
ansible all -i target01,target02,target03 -m ping

target01 | SUCCESS => {
    "ansible_facts": {
        "discovered_interpreter_python": "/usr/bin/python3"
    },
    "changed": false,
    "ping": "pong"
}
target03 | SUCCESS => {
    "ansible_facts": {
        "discovered_interpreter_python": "/usr/bin/python3"
    },
    "changed": false,
    "ping": "pong"
}
target02 | SUCCESS => {
    "ansible_facts": {
        "discovered_interpreter_python": "/usr/bin/python3"
    },
    "changed": false,
    "ping": "pong"
}
```

8. **Création du dossier projet et du fichier de config**
```bash
mkdir -p ~/monprojet
cd ~/monprojet
touch ansible.cfg
```

9. **Vérifier le fichier**
```bash
ansible --version

ansible 2.10.8
  config file = /home/vagrant/monprojet/ansible.cfg
  configured module search path = ['/home/vagrant/.ansible/plugins/modules', '/usr/share/ansible/plugins/modules']
  ansible python module location = /usr/lib/python3/dist-packages/ansible
  executable location = /usr/bin/ansible
  python version = 3.10.12 (main, Mar 22 2024, 16:50:05) [GCC 11.4.0]
```

10. **Créer l'inventaire**
```bash
cat <<EOF > hosts
[testlab]
target01
target02
target03
EOF
```

11. **Activer la journalisation**
```bash
mkdir -p ~/journal
cat <<EOF >> ansible.cfg
[defaults]
inventory = hosts
log_path = ~/journal/ansible.log
EOF
```

12. **Tester la journalisation**
Pour tester la journalisation nous pouvons lancer un ping
```bash
ansible all -i hosts -m ping

target03 | SUCCESS => {
    "changed": false,
    "ping": "pong"
}
target02 | SUCCESS => {
    "changed": false,
    "ping": "pong"
}
target01 | SUCCESS => {
    "changed": false,
    "ping": "pong"
}
```
Et cat le fichier de log
```bash
cat ~/journal/ansible.log

2025-02-12 10:04:40,343 p=3650 u=vagrant n=ansible | target03 | SUCCESS => {
    "changed": false,
    "ping": "pong"
}
2025-02-12 10:04:40,346 p=3650 u=vagrant n=ansible | target02 | SUCCESS => {
    "changed": false,
    "ping": "pong"
}
2025-02-12 10:04:40,347 p=3650 u=vagrant n=ansible | target01 | SUCCESS => {
    "changed": false,
    "ping": "pong"
}
```

13. **Définir l'utilisateur `vagrant`**
```bash
cat <<EOF >> hosts
[testlab:vars]
ansible_user=vagrant
EOF
```

14. **Envoyer un ping Ansible**
```bash
ansible all -i hosts -m ping

target01 | SUCCESS => {
    "changed": false,
    "ping": "pong"
}
target02 | SUCCESS => {
    "changed": false,
    "ping": "pong"
}
target03 | SUCCESS => {
    "changed": false,
    "ping": "pong"
}
```

15. **Elévation des droits pour `vagrant`**
```bash
cat <<EOF >> hosts
ansible_become=true
EOF
```

16. **Afficher la première ligne du fichier `/etc/shadow` sur les Target Hosts**
```bash
ansible all -i hosts -m command -a "head -n 1 /etc/shadow"

target02 | CHANGED | rc=0 >>
root:*:19769:0:99999:7:::
target03 | CHANGED | rc=0 >>
root:*:19769:0:99999:7:::
target01 | CHANGED | rc=0 >>
root:*:19769:0:99999:7:::
```

17. **Quitter et supprimer**
```bash
exit
vagrant destroy -f
```