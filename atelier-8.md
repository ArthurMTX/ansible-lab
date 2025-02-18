# Atelier 8

## Exercice 1 : 

1. **Démarrer les VMs**  
```bash
cd ~/formation-ansible/atelier-07
vagrant up
```

2. **Connexion au Control Host**
```bash
vagrant ssh ansible
```

3. **Se rendre dans le repertoire du projet**
```bash
cd ansible/projets/ema/
ls

ansible.cfg  inventory
```

4. **Créer un utilisateur `greg` sur la VM Rocky**
```bash
ansible rocky -m user -a "name=greg shell=/bin/bash"

rocky | CHANGED => {
    "changed": true,
    "comment": "",
    "create_home": true,
    "group": 1001,
    "home": "/home/greg",
    "name": "greg",
    "shell": "/bin/bash",
    "state": "present",
    "system": false,
    "uid": 1001
}
```

5. **Refaire la même commande**
```bash
ansible rocky -m user -a "name=greg shell=/bin/bash"

rocky | SUCCESS => {
    "append": false,
    "changed": false,
    "comment": "",
    "group": 1001,
    "home": "/home/greg",
    "move_home": false,
    "name": "greg",
    "shell": "/bin/bash",
    "state": "present",
    "uid": 1001
}
```

6. **Installer `tree`**
```bash
ansible all -m package -a "name=tree state=present"
ansible all -m package -a "name=tree state=present"
```

7. **Installer `git`**
```bash
ansible all -m package -a "name=git state=present"
ansible all -m package -a "name=git state=present"
```

8. **Installer `nmap`**
```bash
ansible all -m package -a "name=nmap state=present"
ansible all -m package -a "name=nmap state=present"
```

9. **Désinstaller `tree`**
```bash
ansible all -m package -a "name=tree state=absent"
ansible all -m package -a "name=tree state=absent"
```

10. **Désinstaller `git`**
```bash
ansible all -m package -a "name=git state=absent"
ansible all -m package -a "name=git state=absent"
```

11. **Désinstaller `nmap`**
```bash
ansible all -m package -a "name=nmap state=absent"
ansible all -m package -a "name=nmap state=absent"
```

A chaque fois, la première exécution indique `CHANGED` et la seconde indique `SUCCESS`

12. **Copier le fichier `/etc/fstab`**
```bash
ansible all -m copy -a "src=/etc/fstab dest=/tmp/test3.txt" -o

debian | CHANGED => {"changed": true,"checksum": "d39263691e31170df199aae3d93f7c556fbb3446","dest": "/tmp/test3.txt","gid": 0,"group": "root","md5sum": "b6f1fe0d790a8d2f9b74b95df1c889dc","mode": "0644","owner": "root","size": 743,"src": "/home/vagrant/.ansible/tmp/ansible-tmp-1739357651.6854994-6778-155654359764998/source","state": "file","uid": 0}
rocky | CHANGED => {"changed": true,"checksum": "d39263691e31170df199aae3d93f7c556fbb3446","dest": "/tmp/test3.txt","gid": 0,"group": "root","md5sum": "b6f1fe0d790a8d2f9b74b95df1c889dc","mode": "0644","owner": "root","secontext": "unconfined_u:object_r:user_home_t:s0","size": 743,"src": "/home/vagrant/.ansible/tmp/ansible-tmp-1739357651.7552931-6777-170601953450545/source","state": "file","uid": 0}
suse | CHANGED => {"changed": true,"checksum": "d39263691e31170df199aae3d93f7c556fbb3446","dest": "/tmp/test3.txt","gid": 0,"group": "root","md5sum": "b6f1fe0d790a8d2f9b74b95df1c889dc","mode": "0644","owner": "root","size": 743,"src": "/home/vagrant/.ansible/tmp/ansible-tmp-1739357651.6653867-6779-63839757506707/source","state": "file","uid": 0}

ansible all -m copy -a "src=/etc/fstab dest=/tmp/test3.txt" -o

debian | SUCCESS => {"changed": false,"checksum": "d39263691e31170df199aae3d93f7c556fbb3446","dest": "/tmp/test3.txt","gid": 0,"group": "root","mode": "0644","owner": "root","path": "/tmp/test3.txt","size": 743,"state": "file","uid": 0}
suse | SUCCESS => {"changed": false,"checksum": "d39263691e31170df199aae3d93f7c556fbb3446","dest": "/tmp/test3.txt","gid": 0,"group": "root","mode": "0644","owner": "root","path": "/tmp/test3.txt","size": 743,"state": "file","uid": 0}
rocky | SUCCESS => {"changed": false,"checksum": "d39263691e31170df199aae3d93f7c556fbb3446","dest": "/tmp/test3.txt","gid": 0,"group": "root","mode": "0644","owner": "root","path": "/tmp/test3.txt","secontext": "unconfined_u:object_r:user_home_t:s0","size": 743,"state": "file","uid": 0}
```

13. **Supprimer le fichier `/etc/fstab`**
```bash
ansible all -m file -a "path=/tmp/test3.txt state=absent" -o

debian | CHANGED => {"changed": true,"path": "/tmp/test3.txt","state": "absent"}
rocky | CHANGED => {"changed": true,"path": "/tmp/test3.txt","state": "absent"}
suse | CHANGED => {"changed": true,"path": "/tmp/test3.txt","state": "absent"}

ansible all -m file -a "path=/tmp/test3.txt state=absent" -o

debian | SUCCESS => {"changed": false,"path": "/tmp/test3.txt","state": "absent"}
suse | SUCCESS => {"changed": false,"path": "/tmp/test3.txt","state": "absent"}
rocky | SUCCESS => {"changed": false,"path": "/tmp/test3.txt","state": "absent"}
```

14. **Afficher l'espace utilisé**
```bash
ansible all -a "df -h /" -o

debian | CHANGED | rc=0 | (stdout) Filesystem      Size  Used Avail Use% Mounted on\n/dev/sda3       124G  2.3G  115G   2% /
rocky | CHANGED | rc=0 | (stdout) Filesystem                  Size  Used Avail Use% Mounted on\n/dev/mapper/rl_rocky9-root   70G  2.4G   68G   4% /
suse | CHANGED | rc=0 | (stdout) Filesystem      Size  Used Avail Use% Mounted on\n/dev/sda3       124G  2.8G  118G   3% /
``` 

> Ici on remarque que chaque exécution retourne exactement le même résultat pour chaque Target Host. Cela confirme que la commande n’altère pas l’état du système (elle est purement informative)

15. **Quitter et supprimer**
```bash
exit
vagrant destroy -f
```
