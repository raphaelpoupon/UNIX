# **TP1 UNIX**

## **1 Installation Machine Virtuelle**
Des difficultés communes ont été rencontrées lors de l'installation de la machine virtuelle. Nous avons enfin réussi avec une base commune déjà créée pour Nous

## **2 Post-installation**
### 2.1 Configuration SSH
```
apt updtate
apt install ssh
nano /etc/ssh/sshd_config
```
Comme montré ci-dessus, pour installer SSH, il faut d'abord s'assurer que la base des packages est bien à jour avec `apt update` puis ensuite installer le package `ssh`.

Une fois installé, il faut autoriser les connexions root dans le fichier , ``/etc/ssh/sshd_config``. Pour cela, on utilise la commande `nano` et on modifie/ajoute la ligne `PermitRootLogin yes`.

### 2.2 Connection
Pour se connecter en SSH à la machine virtuelle depuis l'hôte, il faut d'abord connaitre l'adresse IP de la machine. Pour cela, on utilise la commande `ip addr` qui nous renvoie plusieurs informations sur notre connexion dont l'adresse IP. Je n'ai plus à ma disposition cette adresse, imaginons qu'il s'agit de `86.247.46.83`.

Nous devons maintenant utiliser la commande `ssh` depuis la machine hôte pour nous connecter :
```bash
ssh root@86.247.46.83
```
Nous entrons le mot de passe et voilà, nous sommes connectés à la machine virtuelle depuis la machine hôte via SSH.

### 2.3 Nombre de paquets
```bash
dpkg -l | wc -l
353
```
Il y a 353 paquets.

### 2.4 Nombre de paquets
Oui c'est vrai. (je n'ai plus la machine virtuelle à disposition pour afficher la réponse de la commande `df -h`)

### 2.5 Informations à indiquer et expliquer
#### Locales
```bash
$ echo $LANG
fr_FR.UTF-8
```
Cette commande retourne la variable contenant la langue du système. Ici, elle retourne "fr_FR.UTF-8" car notre système est en français de la France, et l'encodage utilisé est l'UDT-8.

#### Nom machine
```bash
$ hostname
debian
```
Cette commande retourne le nom de la machine, qui est "debian".

#### Domaine
```bash
$ hostname --domain
ufr-info-p6.jussieu.fr
```
Cette commande retourne le nom de domaine. Il est dans mon cas existant suite à une demande d'IP fait au DNS. Il s'agit donc de "ufr-info-p6.jussieu.fr".

#### Vérification emplacement dépôts
```bash
$ cat /etc/apt/sources.list | grep -v -E '^#|^$'
deb http://deb.debian.org/debian/ bullseye main
deb http://security.debian.org/debian-security bullseye-security main
deb http://deb.debian.org/debian/ bullseye-updates main
```
Cette commande retourne les lignes présentes dans le fichier `/etc/apt/sources.list` qui ne commencent pas par `#` ou qui ne sont pas vides. Le -E indique que nous utilisons une expression régulière (`'^#|^$'`) et le -v inverse le résultat.

#### Password shadow
```bash
$ cat /etc/shadow | grep -vE ':*:|:!*:'
root:$y$j9T$y90rcIl80vyxY3IRiZvdM.$kncavvkt1G5w6qIpAgLtCt/ZqQfPX3fMBbRg.ExcxK4:19274:0:99999:7:::
```
Cette commande retourne les lignes présentes dans le fichier `/etc/shadow` qui ne contiennent pas la chaîne `:*:` ni la chaîne `:!*:`.

#### Comptes utilisateurs
```bash
$ cat /etc/passwd | grep -vE ’nologin|sync’
root:x:0:0:root:/root:/bin/bash
```
Cette commande retourne les lignes présentes dans le fichier `/etc/passwd` qui ne contiennent pas la chaîne `nologin` ni la chaîne `sync`.

#### Commande `fdiskd`
```bash
fdisk -l
Disque /dev/sda : 22 GiB, 23622320128 octets, 46137344 secteurs
Modèle de disque : HARDDISK
Unités : secteur de 1 × 512 = 512 octets
Taille de secteur (logique / physique) : 512 octets / 512 octets
taille d'E/S (minimale / optimale) : 512 octets / 512 octets
Type d'étiquette de disque : dos
Identifiant de disque : 0xe1c8a978

Périphérique Amorçage    Début      Fin Secteurs Taille Id Type
/dev/sda1    *            2048 44136447 44134400    21G 83 Linux
/dev/sda2             44138494 46135295  1996802   975M  5 Étendue
/dev/sda5             44138496 46135295  1996800   975M 82 partition d'échange Linux / Solaris
```

```bash
fdisk -x
Disque /dev/sda : 22 GiB, 23622320128 octets, 46137344 secteurs
Modèle de disque : HARDDISK
Unités : secteur de 1 × 512 = 512 octets
Taille de secteur (logique / physique) : 512 octets / 512 octets
taille d'E/S (minimale / optimale) : 512 octets / 512 octets
Type d'étiquette de disque : dos
Identifiant de disque: 0xe1c8a978

Périphérique Amorçage    Début      Fin Secteurs Id Type                                Début-C/T/S  Fin-C/T/S Attr.
/dev/sda1    *            2048 44136447 44134400 83 Linux                                     4/4/1 1023/254/2    80
/dev/sda2             44138494 46135295  1996802  5 Étendue                              1023/254/2 1023/254/2
/dev/sda5             44138496 46135295  1996800 82 partition d'échange Linux / Solaris  1023/254/2 1023/254/2
```

Cette commande retourne grâce au -l les informations du disque et les différentes partitions de celui-ci. Le -x permet pareil mais avec plus de détails.

#### La commande `df -h`
```bash
$ cat /etc/passwd | grep -vE ’nologin|sync’
root:x:0:0:root:/root:/bin/bash
```
Cette commande indique l'espace occupé par chaque système de fichiers sur le disque. Le `-h` permet d'afficher les tailles en puissance de 1024.
