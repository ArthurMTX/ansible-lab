# Atelier 2/3

## Exercice 2 : 

1. **Démarrer les VMs**  
```bash
cd ~/formation-ansible/atelier-03
vagrant up
```

2. **Connexion au Control Host et vérification d'Ansible**
```bash
vagrant ssh control
type ansible

ansible is /usr/bin/ansible
```

3. **Configuration du fichier `/etc/hosts`**
```bash
192.168.56.20   target01.sandbox.lan     target01
192.168.56.30   target02.sandbox.lan     target02
192.168.56.40   target03.sandbox.lan     target03
```

4. **Test de la connectivité**
```bash
for HOST in target01 target02 target03; do ping -c 1 -q $HOST; done

PING target01.sandbox.lan (192.168.56.20) 56(84) bytes of data.

--- target01.sandbox.lan ping statistics ---
1 packets transmitted, 1 received, 0% packet loss, time 0ms
rtt min/avg/max/mdev = 0.268/0.268/0.268/0.000 ms
PING target02.sandbox.lan (192.168.56.30) 56(84) bytes of data.

--- target02.sandbox.lan ping statistics ---
1 packets transmitted, 1 received, 0% packet loss, time 0ms
rtt min/avg/max/mdev = 0.215/0.215/0.215/0.000 ms
PING target03.sandbox.lan (192.168.56.40) 56(84) bytes of data.

--- target03.sandbox.lan ping statistics ---
1 packets transmitted, 1 received, 0% packet loss, time 0ms
rtt min/avg/max/mdev = 0.286/0.286/0.286/0.000 ms
```

5. **Collecte des clés SSH publiques**
```bash
ssh-keyscan -t rsa target01 target02 target03  >> .ssh/known_hosts

# target02:22 SSH-2.0-OpenSSH_8.4
# target01:22 SSH-2.0-OpenSSH_8.9p1 Ubuntu-3ubuntu0.10
# target03:22 SSH-2.0-OpenSSH_8.9p1 Ubuntu-3ubuntu0.10
```

6. **Génération d'une paire de clés SSH**
```
ssh-keygen
```

7. **Distribution de la clé publique sur les target hosts**
```bash
ssh-copy-id vagrant@target01
ssh-copy-id vagrant@target02
ssh-copy-id vagrant@target03
```


4. **Test de la connectivité avec les Target Hosts**
```bash
ansible all -i target01,target02,target03 -m ping

target03 | SUCCESS => {
    "ansible_facts": {
        "discovered_interpreter_python": "/usr/bin/python3"
    },
    "changed": false,
    "ping": "pong"
}
target01 | SUCCESS => {
    "ansible_facts": {
        "discovered_interpreter_python": "/usr/bin/python3"
    },
    "changed": false,
    "ping": "pong"
}
[WARNING]: Platform linux on host target02 is using the discovered Python interpreter at /usr/bin/python3.6, but future installation of another Python interpreter could change the meaning of that path. See
https://docs.ansible.com/ansible/2.10/reference_appendices/interpreter_discovery.html for more information.
target02 | SUCCESS => {
    "ansible_facts": {
        "discovered_interpreter_python": "/usr/bin/python3.6"
    },
    "changed": false,
    "ping": "pong"
}
```