Le but de ce challenge est de découvrir le mot de passe de l'utilisateur "uta" en utilisant ces logs. 

Il s’agit de log de serveur web. En consultant les logs on s’aperçoit que les mots de passe des utilisateurs n’y sont bien sûr pas consignés directement. Nous nous penchons donc vers les informations d’authentifications des utilisateurs du serveur en lui même. Nous savons que ces informations sont stockés dans les fichiers /etc/shadow et /etc/passwd pour les environnements linux. Il suffit donc de trouver une requête réussie dans nos logs vers ces endpoints pour en découvrir le contenu.
Nous utilisons l’outil "jq" qui permet d’effectuer facilement des traitements sur des json:

<img width="692" alt="Capture d’écran 2024-11-03 à 23 22 59" src="https://github.com/user-attachments/assets/14019357-81ec-4741-939c-3c6f4f211023">

Avec cette commande ci-dessus nous cherchons les requêtes réussies vers tout les endpoints contenant le mot "shadow". Nous optenons la requête suivante:

<img width="705" alt="Capture d’écran 2024-11-03 à 23 23 34" src="https://github.com/user-attachments/assets/16da74c8-2849-48a4-ac7d-2fd6a8505a1b">

Dans le champ "response" de la requête nous avons la réponse du serveur qui est ici une chaîne encodée en base 64. Utilisons cyberchef pour la décoder: 

<img width="1182" alt="Capture d’écran 2024-11-03 à 23 24 23" src="https://github.com/user-attachments/assets/bcbedf99-48b4-499f-8155-4269bb341cdb">

Bingo! nous avons maintenant le "hash" du mot de passe de l’utilisateur Uta. Nous le stockons dans un fichier ("uta"). 
Le hash commence par les caractères $y$ indiquant que le mot de passe est hashé avec yescrypt. Plus qu’à utiliser john the ripper pour tenter une attaque par dictionnaire sur le hash avec le paramètre —format=crypt.

<img width="704" alt="Using default input encoding UTF-8" src="https://github.com/user-attachments/assets/03efd6e4-8e63-49e4-8c4f-3183e289272f">

En quelques secondes nous obtenons le mot de passe en claire de uta: *pasaway*.
