# Atelier 1

## Exercice 1 : 

1. **Démarrer la VM Ubuntu**  
```bash
cd ~/formation-ansible/atelier-01
vagrant up ubuntu
``` 

2. **Se connecter à la VM**
```bash
vagrant ssh ubuntu
```

3. **Mettre à jour les informations sur les paquets**
```bash
sudo apt update && sudo apt upgrade -y
```

4. **Rechercher le paquet Ansible**
```bash
apt-cache search --names-only ansible
```

5. **Installer Ansible**
```bash
sudo apt install -y ansible
```

6. **Vérifier l'installation et noter la version**
```bash
ansible --version

ansible 2.10.8
  config file = None
  configured module search path = ['/home/vagrant/.ansible/plugins/modules', '/usr/share/ansible/plugins/modules']
  ansible python module location = /usr/lib/python3/dist-packages/ansible
  executable location = /usr/bin/ansible
  python version = 3.10.12 (main, Jan 17 2025, 14:35:34) [GCC 11.4.0]
```

7. **Quitter la VM et la supprimer**
```bash
exit
vagrant destroy -f ubuntu
```

## Exercice 2 

1. **Démarrer la VM Ubuntu**  
```bash
cd ~/formation-ansible/atelier-01
vagrant up ubuntu
``` 

2. **Se connecter à la VM**
```bash
vagrant ssh ubuntu
```

3. **Mettre à jour le système**
```bash
sudo apt update && sudo apt upgrade -y
```

4. **Ajouter le dépôt PPA pour Ansible**
```bash
sudo apt-add-repository ppa:ansible/ansible
```

5. **Mettre à jour à nouveau les informations sur les paquets**
```bash
sudo apt update
```

6. **Rechercher le paquet Ansible**
```bash
apt-cache search --names-only ansible
```

7. **Installer Ansible**
```bash
sudo apt install -y ansible
```

8. **Vérifier l'installation et noter la version**
```bash
ansible --version

ansible [core 2.17.8]
  config file = /etc/ansible/ansible.cfg
  configured module search path = ['/home/vagrant/.ansible/plugins/modules', '/usr/share/ansible/plugins/modules']
  ansible python module location = /usr/lib/python3/dist-packages/ansible
  ansible collection location = /home/vagrant/.ansible/collections:/usr/share/ansible/collections
  executable location = /usr/bin/ansible
  python version = 3.10.12 (main, Jan 17 2025, 14:35:34) [GCC 11.4.0] (/usr/bin/python3)
  jinja version = 3.0.3
  libyaml = True
```

La version est plus récente en utilisant le depôt PPA que les repos de base de Linux

9. **Quitter la VM et la supprimer**
```bash
exit
vagrant destroy -f ubuntu
```

# Exercice 3

1. **Démarrer la VM Rocky**  
```
cd ~/formation-ansible/atelier-01
vagrant up rocky
```

2. **Se connecter à la VM**
```bash
vagrant ssh rocky
```

3. **Mettre à jour le système**
```bash
sudo dnf update -y
```

4. **Installer Python3 PIP**
```bash
sudo dnf install -y python3-pip
```

5. **Créer un environnement virtuel pour Ansible**
```bash
python3 -m venv ~/.venv/ansible
```

6. **Activer l'environnement virtuel**
```bash
source ~/.venv/ansible/bin/activate
```

7. **Installer Ansible via PIP**
```bash
pip install ansible
```

8. **Vérifier l'installation et noter la version**
```bash
ansible --version

ansible [core 2.15.13]
  config file = None
  configured module search path = ['/home/vagrant/.ansible/plugins/modules', '/usr/share/ansible/plugins/modules']
  ansible python module location = /home/vagrant/.venv/ansible/lib64/python3.9/site-packages/ansible
  ansible collection location = /home/vagrant/.ansible/collections:/usr/share/ansible/collections
  executable location = /home/vagrant/.venv/ansible/bin/ansible
  python version = 3.9.21 (main, Dec  5 2024, 00:00:00) [GCC 11.5.0 20240719 (Red Hat 11.5.0-2)] (/home/vagrant/.venv/ansible/bin/python3)
  jinja version = 3.1.5
  libyaml = True
```

9. **Quitter l'environnement virtuel et la VM**
```bash
deactivate
exit
vagrant destroy -f rocky
```
