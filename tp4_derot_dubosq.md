Robin DÉROT William DUBOSQ 4ETI

# Administration Système
# TP4 - Utilisateurs, groupes et permissions

## Exercice 1. Gestion des utilisateurs et des groupes

### Question 1.
#### Commencez par créer deux groupes groupe1 et groupe2

  On crée les deux groupes à partir de la commande suivante :
  ```bash
  sudo addgroup groupe1
  sudo addgroup groupe2
  ```

### Question 2.
#### Créez ensuite 4 utilisateurs u1, u2, u3, u4 avec la commande useradd, en demandant la création de leur dossier personnel et avec bash pour shell

  On crée ces utilisateurs avec la commande suivante :
  ```bash
  sudo useradd -m u1
  sudo useradd -m u2
  sudo useradd -m u3
  sudo useradd -m u4
  ```
  
  L'option -m permet de créer le dossier personnel en même temps que les utilisateurs.
  
  ### Question 3.
  #### Ajoutez les utilisateurs dans les groupes créés :
  #### - u1, u2, u4 dans groupe1
  #### - u2, u3, u4 dans groupe2
  
  On ajoute les utlisateurs dans les différents groupes avec la commande suivante :
  ```bash
  sudo usermod -a -G groupe1 u1
  sudo usermod -a -G groupe1 u2
  sudo usermod -a -G groupe1 u4
  sudo usermod -a -G groupe2 u2
  sudo usermod -a -G groupe2 u3
  sudo usermod -a -G groupe2 u4
  ```
  
  L'option -a permet de dire qu'on ajoute l'utilisateur à un groupe secondaire, et -G donne la liste des groupes secondaires auxquels il va être ajouté.
  
  ### Question 4.
  #### Donnez deux moyens d’afficher les membres de groupe2
  
  On peut utiliser la commande suivante :
  ```bash
  cat /etc/group | grep groupe1
  cat /etc/group | grep groupe1
  ```
  Ou alors :
  ```bash
  members groupe1
  members groupe2
  ```
  
  ### Question 5.
  #### Faites de groupe1 le groupe propriétaire de /home/u1 et /home/u2 et de groupe2 le groupe propriétaire de /home/u3 et /home/u4

  On réalise cela à l'aide de la commande suivante :
  ```bash
  sudo chown :groupe1 /home/u1 /home/u2
  sudo chown :groupe2 /home/u3 /home/u4
  ls -l
  
  total 20
  drwxr-xr-x 8 derot derot  4096 févr. 28 16:48 derot
  drwxr-xr-x 2 u1     groupe1 4096 mars 13 13:16 u1
  drwxr-xr-x 2 u2     groupe1 4096 mars 13 13:16 u2
  drwxr-xr-x 2 u3     groupe2 4096 mars 13 13:16 u3
  drwxr-xr-x 2 u4     groupe2 4096 mars 13 13:16 u4
  ```
  
  ### Question 6.
  #### Remplacez le groupe primaire des utilisateurs :
  #### - groupe1 pour u1 et u2
  #### - groupe2 pour u3 et u4
  
  On se sert de la commande suivante :
  ```bash
  sudo usermod u1 -g groupe1
  sudo usermod u2 -g groupe1
  sudo usermod u3 -g groupe2
  sudo usermod u4 -g groupe2
  ```

  ### Question 7.
  #### Créez deux répertoires /home/groupe1 et /home/groupe2 pour le contenu commun aux groupes, et mettez en place les permissions permettant aux membres de chaque groupe d’écrire dans le dossier associé.
  
   On se sert de la commande suivante :
  ```bash
  sudo chown :groupe1 /home/groupe1
  sudo chown :groupe2 /home/groupe2
  
  sudo chmod g+w /home/groupe1
  sudo chmod g+w /home/groupe2
  ```
  
  On donne le droit au groupe d'écrire, c'est pourquoi on utilise *g+w*.
  
  ### Question 8.
  #### Comment faire pour que, dans ces dossiers, seul le propriétaire d’un fichier ait le droit de renommer ou supprimer ce fichier ?
  
  On veut que seul le *user* puisse écrire et donc supprimer et renommer le fichier.
  On utilise donc la commande suivante :
  ```bash
  sudo chmod g-w /home/groupe1
  sudo chmod g-w /home/groupe2
  ```
  
  ### Question 9.
  #### Pouvez-vous vous connecter en tant que u1 ? Pourquoi ?
  
  Il n'est pas possible de se connecter en tant que u1. En effet, son mot de pase n'a pas été défini.
  Il est inactif jusqu'à ce qu'on en définisse un.
  
  ### Question 10.
  #### Activez le compte de l’utilisateur u1 et vérifiez que vous pouvez désormais vous connecter avec son compte.
  
  Pour mettre à jour le mot de passe de u1, on utilise la commande suivante :
  ```bash
  sudo passwd u1
  New password: u1
  Retype new password: u1
  ```
  Et pour se connecter en tant que u1 :
  ```bash
  su u1
  cd
  echo $PWD
  /home/u1
  ```
  
  #### Dans la suite du TP, les id des groupes sont décalés de 1 puisque nous avons crée un groupe test en début de TP.
  
  ### Question 11.
  #### Quels sont l’uid et le gid de u1 ?
  
  On utilise la commande suivante :
  ```bash
  id u1
  uid=1001(u1) gid=1002(groupe1) groups=1002(groupe1)
  ```
  
  ### Question 12.
  #### Quel utilisateur a pour uid 1003 ?
  
  On utilise la commande suivante :
  ```bash
  cat /etc/passwd | grep 1003
  u3:x:1003:1003::/home/u3:/bin/sh
  ```
  
  ### Question 13.
  #### Quel est l’id du groupe groupe1 ?
  
  On peut utiliser la commande *getent*:
  ```bash
  getent group groupe1
  groupe1:x:1002:u4,u1,u2
  ```
  L'id de groupe1 est donc 1002.
  
  ### Question 14.
  #### Quel groupe a pour guid 1002 ?
  
  On utilise la commande *getent*:
  ```bash
  getent group 1002
  groupe1:x:1002:u4,u1,u2
  ```
  
  ### Question 15.
  #### Retirez l’utilisateur u3 du groupe groupe2. Que se passe-t-il ? Expliquez.
  
  On peut utiliser la commande *gpasswd*. L'option -d doit permettre de supprimer un utilisateur (*delete*).
  ```bash
  sudo gpasswd -d u3 groupe2
  cat /etc/group | grep groupe2
  
  groupe2:x:1003:u2,u4
  ```
  L'utilisateur *u3* a bien été supprimé.
  
  ### Question 16.
  #### Modifiez le compte de u4 de sorte que :
  #### — il expire au 1er juin 2020
  #### — il faut changer de mot de passe avant 90 jours
  #### — il faut attendre 5 jours pour modifier un mot de passe
  #### — l’utilisateur est averti 14 jours avant l’expiration de son mot de passe
  #### — le compte sera bloqué 30 jours après expiration du mot de passe.
  
  *Expiration :*
  ```bash
  sudo chage -E 2020/06/01 u4
  [sudo] password for kz:
  ```
  
  *Changement de mot de passe :*
  ```bash
  sudo chage -M 90 u4
  ```
  
  *Attente de 5 jours :*
  ```bash
  sudo chage -m 5 u4
  ```
  
  *14 jours avant expiration :*
  ```bash
  sudo chage -W 14 u4
  ```
  
  *Compte bloqué après 30 jours :*
  ```bash
  sudo chage -I 30 u4
  ```
  
  ### Question 17.
  #### Quel est l’interpréteur de commandes (Shell) de l’utilisateur root ?
  
  ```bash
  sudo echo $SHELL
  /bin/bash
  ```
  
  ### Question 18.
  #### À quoi correspond l’utilisateur nobody ?

  C'est un utilisateur qui ne possède aucun fichier, ni de privilèges spécifiques. Il a juste les possibilités de base des utilisateurs.
  
  ### Question 19.
  #### Par défaut, combien de temps la commande sudo conserve-t-elle votre mot de passe en mémoire ?
  #### Quelle commande permet de forcer sudo à oublier votre mot de passe ?
  
  Le mot de passe est conservé 15 minutes. 
  On peut utiliser la commande *exit* ou *ctrl+d* pour faire oublier le mot de passe.
  
  ## Exercice 2. Gestion des permissions
  
  ### Question 1.
  #### Dans votre $HOME, créez un dossier test, et dans ce dossier un fichier fichier contenant quelques lignes de texte. Quels sont les droits sur test et fichier ?
  
  On utilise les commandes suivantes :
  
  ```bash
  mkdir test
  cd test
  touch fichier
  ```
  
  Et pour voir les droits :
  ```bash
  ls -l
  -rw-rwr-- 1 derot derot 60 mars  18 14:34 fichier
  ```
  
  ### Question 2.
  ### Retirez tous les droits sur ce fichier (même pour vous), puis essayez de le modifier et de l’afficher en tant que root. Conclusion ?

  On utilise les commandes suivantes :
  
  ```bash
  chmod 000 fichier
  ls -l
  --------- 1 derot derot 60 mars  18 14:34 fichier
  ```
  
  ```bash
  sudo nano fichier
  sudo cat fichier
  ```
  
  Cela fonctionne avec *root* malgré tout.
  
  ### Question 3.
  #### Redonnez vous les droits en écriture et exécution sur fichier puis exécutez la commande echo "echo Hello" > fichier. On a vu lors des TP précédents que cette commande remplace le contenu d’un fichier s’il existe déjà. Que peut-on dire au sujet des droits ?
  
  ```bash
  chmod 330 fichier
  ls -l
  --wx-wx--- 1 derot derot 60 mars  18 14:34 fichier
  
  echo "echo Hello" > fichier
  ```
  
  *echo Hello* est bien écrit.
  
  ### Question 4.
  #### Essayez d’exécuter le fichier. Est-ce que cela fonctionne ? Et avec sudo ? Expliquez.

  Sans *sudo*, cela ne fonctionne pas. En revanche, cela fonctionne lorsque l'on utilise *sudo*, car nous obtenons les droits requis.
  
  ### Question 5.
  #### Placez-vous dans le répertoire test, et retirez-vous le droit en lecture pour ce répertoire. Listez le contenu du répertoire, puis exécutez ou affichez le contenu du fichier fichier. Qu’en déduisez-vous ? Rétablissez le droit en lecture sur test.
  
  On utilise les commandes suivantes :
  
  ```bash
  chmod 330 test
  cd test
  ls
  cat fichier
  ```
  On retourne ensuite dans le dossier parent pour rétablir les droits :
  ```bash
  cd ..
  chmod 770 test
  ```
  
  ### Question 6.
  #### Créez dans test un fichier nouveau ainsi qu’un répertoire sstest. Retirez au fichier nouveau et au répertoire test le droit en écriture. Tentez de modifier le fichier nouveau. Rétablissez ensuite le droit en écriture au répertoire test. Tentez de modifier le fichier nouveau, puis de le supprimer. Que pouvezvous déduire de toutes ces manipulations ?
  
  Nous utilisons les commandes suivantes :
  
  ```bash
  touch nouveau
  mkdir sstest
  ls
  ```
  Cela donne :
  ```bash
  fichier  nouveau  sstest
  ```
  
  On retire le droit en écriture : 
  ```bash
  chmod u-w nouveau
  cd ..
  chmod u-w test
  ```
  
  On utilise les commandes suivantes afin d'essayer de modifier le fichier :
  
  ```bash
  echo "test" > nouveau
  -bash: nouveau: Permission denied
  chmod u+w  test
  echo "test" > nouveau
  -bash: nouveau: Permission denied
  ```
  
  Même en rétablissant les droits d'écriture à *test*, *nouveau* n'a toujours pas les droits d'écriture.
  
  ### Question 7.
  #### Positionnez vous dans votre répertoire personnel, puis retirez le droit en exécution du répertoire test. Tentez de créer, supprimer, ou modifier un fichier dans le répertoire test, de vous y déplacer, d’en lister le contenu, etc… Qu’en déduisez vous quant au sens du droit en exécution pour les répertoires ?
  
  On utilise les commandes suivantes :
  
  ```bash
  chmod u-x test
  cd test
  Permission denied
  ```
  
  On ne peut plus accéder à ce répertoire.
  
  ### Question 8.
  #### Rétablissez le droit en exécution du répertoire test. Positionnez vous dans ce répertoire et retirez lui à nouveau le droit d’exécution. Essayez de créer, supprimer et modifier un fichier dans le répertoire test, de vous déplacer dans ssrep, de lister son contenu. Qu’en concluez-vous quant à l’influence des droits que l’on possède sur le répertoire courant ? Peut-on retourner dans le répertoire parent avec ”cd ..” ? Pouvez-vous donner une explication ?
  
  On rétablit les droits :
   ```bash
  chmod u+x test
  ``` 

```bash
  chmod u-x test ~/test
  ```

  Le droit a été retiré dans tous les sous-dossiers.
  Il est possible de sortir du répertoire avec la commande *cd ..*, en revanche il est impossible de rentrer à nouveau puisqu'on a retiré ce droit.
  
  ### Question 9.
  #### Rétablissez le droit en exécution du répertoire test. Attribuez au fichier fichier les droits suffisants pour qu’une autre personne de votre groupe puisse y accéder en lecture, mais pas en écriture.
  
  On utilise le code suivant :
  ```bash
  chmod u+x test
  cd test
  chmod g+r fichier
  chmod g-w fichier
  ```

  ### Question 10. Définissez un umask très restrictif qui interdit à quiconque à part vous l’accès en lecture ou en écriture, ainsi que la traversée de vos répertoires. Testez sur un nouveau fichier et un nouveau répertoire.

  On utilise les commandes suivantes :
  ```bash
  umask 077
  cd test
  mkdir testDir
  touch testFichier
  ls -l
  drwx------ 2 derot derot 4096 mars 18 15:38 testDir
  -rw------- 1 derot derot 0 mars 18 15:38 testFichier
  ```

  ### Question 11.
  #### Définissez un umask très permissif qui autorise tout le monde à lire vos fichiers et traverser vos répertoires, mais n’autorise que vous à écrire. Testez sur un nouveau fichier et un nouveau répertoire.
  
  On utilise les commandes suivantes :
  
  ```bash
  umask 022
  cd test
  mkdir testDir2
  touch testFichier2
  ls -l
  drwxr-xr-x 2 derot derot 4096 mars 18 15:38 testDir2
  -rw-r--r-- 1 derot derot 0 mars 18 15:38 testFichier2
  ```

  ### Question 12.
  #### Définissez un umask équilibré qui vous autorise un accès complet et autorise un accès en lecture aux membres de votre groupe. Testez sur un nouveau fichier et un nouveau répertoire.
  
  ```bash
  umask 037
  cd test
  mkdir testDir3
  touch testFichier3
  ls -l
  drwxr----- 2 derot derot 4096 mars 18 15:38 testDir3
  -rw-r----- 1 derot derot 0 mars 18 15:38 testFichier3
  ```

  ### Question 13.
  #### Transcrivez les commandes suivantes de la notation classique à la notation octale ou vice-versa (vous pourrez vous aider de la commande stat pour valider vos réponses) :
  #### - chmod u=rx,g=wx,o=r fic
  Cette notation devient :
  ```bash
  touch fic
  chmod 534 fic
  -r-x-wxr-- fic
  ```
  #### - chmod uo+w,g-rx fic en sachant que les droits initiaux de fic sont r--r-x---
  ```bash
  touch fic
  chmod 602 fic
  rw-----w- fic
  ```
  #### - chmod 653 fic en sachant que les droits initiaux de fic sont 711
  ```bash
  touch fic
  chmod 653 fic
  rw-r-x-wx fic
  ```
  #### - chmod u+x,g=w,o-r fic en sachant que les droits initiaux de fic sont r--r-x---
  ```bash
  touch fic
  chmod 520 fic
  r-x-w---- fic
  ```
  
  ### Question 14.
  #### Affichez les droits sur le programme passwd. Que remarquez-vous ? En affichant les droits du fichier /etc/passwd, pouvez-vous justifier les permissions sur le programme passwd ?

   ```bash
  ls -l /etc/passwd
  
  -rw-r--r-- root root 2328 mars 13 14:12 /etc/passwd
  ```

  On voit que *root* peut lire et écrire, en revanche les groupes et *others* ne peuvent que lire. On ne peut donc pas modifier le programme.






