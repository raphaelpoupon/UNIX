# **TP2 UNIX**

## **1 Secure Shell : SSH**
### 1.1 Exercice : Connection ssh root(reprise TP1)

L'élément que nous avons modifié est le PermitRootLogin, passant de   ``prohibit-password`` à ``yes`` . Les trois options possibles sont celle-ci :
- ``yes`` : L'utilisateur root peut se connecter
- ``no`` : L'utilisateur root n'est pas autorisé à se connecter
- ``prohibit-password`` : Désactive identifiant et mot de passe

### 1.2 Exercice : Authentification par clef / Génération de clefs
```bash
ssh-keygen
```
Pour créer une clé d'authentification, on utilise la commande ``ssh-keygen``.
Nous ne mettons pas de passphrase pour des raisons de commodité. Nous n'aurons plus besoin de mot de passe pour nous connecter, la clé que nous avons créé s'occupera de l'authentification.

### 1.3 Exercice : Authentification par clef / Connexion serveur
```bash
touch root/.ssh/authorized-keys
```
On créé le fichier `authorized-keys` qui recevra la clé dans la machine virtuelle.
```bash
chmod 700 authorized-keys
```
On modifie les droits du fichier ``authorized-keys`` avec la commande ``chmod``.

### 1.4 Exercice : Authentification par clef : depuis la machine hôte
```bash
ssh-copy-id -i /users/Etu5/1011355/.ssh/id_rsa root@10.21.0.84
```
Pour copier la clé d'authentification depuis la machine hôte, on utilise la commande ``ssh-copy-id`` en mettant le fichier où elle se situe et le destinataire.

### 1.5 Exercice : Sécurisez
```bash
nano /root/ssh/sshd_config
```
On re-modifie la ligne ``PermitRootLogin`` dans le fichier ``sshd_config`` en mettant la valeur ``prohibit-password``.
Les attaques brute-force consistent à tester une multitude de combinaison de mot de passe jusqu'à avoir un bon résultat. Ces attaques sont donc éviter car la connexion par mot de passe est bloquée.

Pour permettre la connexion à d'autres en bloquant ce genre d'attaques, on peut par exemple bloquer une adresse IP après tant de tentatives. Un utilisateur ayant le contrôle d'une multitude de machines pourra encore faire ce genre d'attaque mais ça sécurise tout de même l'ensemble.

Sinon, on peut peut-être donner des clés à tous si le nombre d'utilisateur est vraiment limité.

## Processus
### 2.1 Exercice : Étude es processus UNIX
#### Question 1
```bash
ps
PID TTY         TIME CHD
437 tty1        00:00:00 login
487 tty1        00:00:00 bash
2867 tty1       00:00:00 DS
```
L'indication ``TIME`` indique le temps d'utilisation consacré au processus.
```bash
ps u
USER    PID  %CPU  %MEM     VSZ
```
La commande ``ps`` permet d'avoir plus d'informations comme l'utilisation du CPU ou l'heure de démarrage. Pour l'utilisation du CPU, ils sont tous à ``0.0``. Le premier processus a été start à 14H39, l'heure de démarrage de la machine.
#### Question 2
```bash
ps -l
UID   PID  PPID        F CPU PRI NI       SZ    RSS WCHAN     S             ADDR TTY           TIME CMD
501  8650  8649     4006   0  31  0 408676800   3328 -      S                   0 ttys000    0:00.03 -zsh
```
La commande ``ps -l`` permet d'afficher le ``ppid`` de chaque processus (Bon là j'affiche le résultat de la commande sur une autre machine mais c'est bon).
