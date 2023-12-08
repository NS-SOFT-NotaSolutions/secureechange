
---
# <img src="/Documentation/images/SecureEchange-165.png" width="40" height="40">SecureEchange &reg; 
### Envoyez et recevez vos documents en toute sécurité.

***[Présentation commerciale](https://www.notasolutions.fr/secure-echange/)***

---
# Présentation
L'objectif de SecureEchange est de permettre, l'envoi et la réception de documents sensibles en assurant l'origine, l'intégrité et le suivi des actions des documents échangés.  
Pour atteindre cet objectif, SecureEchange met en oeuvre plusieurs stratégies :
- Authentification forte de l'office ou l'entreprise qui initie le transfère.
- Authentification à double facteurs du client final qui recoit et expédie des documents.
- Signatures Avancée qualifiée eIDAS des documents demandés.
- Signature Qualifiée eIDAS des documents envoyés par l'office ou l'entreprise qui initie le transfert.
- Chiffrement de tous les documents échangés. 
- ZeroTrust
  >Clé de chiffrement uniquement partagée entre l'office ou l'entreprise initiateur et le client final. **NS SOFT ne possède pas la clé de chiffrement, les fichiers sont illisibles dans les serveurs de NS SOFT.**
- Date de péremption des échanges.
- Intéropérabilité avec les logiciels métiers des notaires.
- Assistance et contrôle d'une IA pour la gestion des IBANs.

SecureEchange se compose des tiers suivants:
- Poste de travail de l'office ou l'entreprise
  - Navigateur + client javascript + Html (Angular) : https://secure-echange.notasolutions.fr
  - Interopérabilité Notariale (Office notarial)
  - Outlook

-Poste de Travail du client final
  - PC:  Edge ou Chrome ou Firefox
  - Mobile: Safari, Chrome

- Back-End
  - APIs (Restfull paradigm)
  - Base de données (NoSQL - Mongo)
  - Cache mémoire partagé (Redis)

- Partenaires
  - LexIA by Lexfluent
  - SMS MODE
  - Oodrive 

## Architecture 
<figure>
<img src="/Documentation/images/macro-architecture.png"></img>
<figcaption>Vue d'ensemble SecureEchange</figcaption>
</figure>
<figure>
<img src="/Documentation/images//Secure-Echange-process.png"></img>
<figcaption>Processus d'échanges bidirectionnel</figcaption>
</figure>

Comme on peut le comprendre depuis les schémas ci-dessus, SecureEchange doit interagir avec plusieurs acteurs:
- Les utilisateurs de l'office ou de l'entreprise qui initient les échanges.
- Le logiciel métier (LRA) dans lequel on trouve les fiches clients et les documents à envoyer mais aussi dans lequel on rangera les documents à recevoir
- SMS Mode pour l'envoi de SMS dans le processus d'authentification à double facteur.
- OODRIVE pour les demandes de signatures électroniques avancées eIDAS.
- LexIA pour la reconnaissance et la vérification des RIBs.
- La clé REAL qui permet une authentification forte grâce à un dispositif physique (clé USB) et code PIN. Et qui permet aussi la signature qualifiée eIDAS des documents PDF.
- La clé AVOCAT qui permet une authentification forte grâce à une dispositif physique (clé USB) et code PIN. Et qui permet aussi la signature qualifiée eIDAS des documents PDF.
- La clé Certinomis qui permet une authentitication forte grâce à un dispositif physique (clé USB) et code PIN. Et qui permet aussi la signature qualifiée eIDAS des documents PDF.
- Le client final qui reçoit les documents envoyés et envoit ceux requis.

Toutes ces interactions doivent s'effectuer de manière à **garantir l'intégrité, l'origine et le suivi des échanges**.  

Afin d'atteindre ces objectifs, SecureEchange a été découpé en plusieurs parties :
- Services d'authentification 
- API privées réservées aux seuls utilisateurs de l'office ou l'entreprise.
- API publiques réservés aux clients destinataires de l'échange.
- L'interopérabilité notariale pour les informations des fiches clients, l'import et le rangement des documents échangés.
- Une application Web réservée aux utilisateurs de l'Office ou l'entreprise
- Une application Web réservée aux clients finaux.

# Sécurisé par Design
## Stratégie sécurité sur les rôles pour les Endpoints
## Cycle de vie d'un jeton d'authentification
## Liste de contrôles d'accès sur les entités
### Denied
### Granted

# Les services Authentification

## Authentification par clé REAL
La clé REAL&reg; est un dispositif physique, sécurisée avec un code PIN et contenant un certificat de signature, élaboré par l'ADSN et sous l'autorité du Conseil Supérieur du Notariat qui permet de signer les actes électroniques et s'authentifier pour diverses formalités.  Ce dispositif est disponible pour les notaires et pour les clercs bien que les rôles et permissions soient spécifiques à chaque acteur. Ce dispositif dépend de l'autorité de certification Racine Notaires 20XX, cette autorité est qualifiée eIDAS. 
Son utilisation dans le cadre de l'authentification des notaires et clercs permet une authentification forte des utilisateurs.  
Cependant la profession, préfère que cette clé soit utilisée uniquement dans le cadre des signatures électroniques des actes et des formalités réglementaires. Son utilisation en tant que dispositif d'authentification pour les applications autres que celles réglementaires n'est pas préconisé ou encouragé.  

## Authentification par Identifiant et mot de passe (Auth0)
Pour tous les acteurs qui ne possèdent pas de dispositif sécurisé tel que la clé REAL&reg;, nous proposons une authentification basée sur un identifiant et mot de passe avec vérification de l'adresse email. Pour cela, nous mettons en oeuvre une authentification hébergée par [Auth0](https://auth0.com/docs). Cette authentification se base sur OAuth et délivre des Bearer tokens propres à chaque application, utilisateur. Elle intégre les permissions, les rôles par application ainsi que des metadata supplémentaires propre à l'utilisateur (SIRET de la société, identifiant unique, etc... )

## Authentification SSO GenApi&copy; 
GenApi &copy; (aka Septeo Notaires &copy;) est l'éditeur leader français du logiciel de rédaction d'actes pour des notaires. A ce jour, c'est le seul éditeur proposant un SSO d'authentification. Grâce à son SSO, l'utilisateur peut s'authentifier avec son identifant et mot de passe de son outil métier et permet de nous fournir un jeton d'authentification sur lequel nous nous baseons pour autoriser ou non l'accès à l'application SecureEchange. 

## Authentification par clé AVOCAT&reg; ou Certinomis&reg;
A l'instar de la clé REAL&reg;, il existe d'autres dispositifs physiques de sécurité disponibles pour les autres professions. 
En particulier, les avocats disposent d'une [clé d'authentification AVOCAT (anciennement e-barreau ou RPVA )](https://dl.avocatparis.org/ebarreau/doc/cle_avocat_V6_2021.pdf) qui peut être utilisée pour s'authentifier et signer électroniquement des documents avec une certification eIDAS. 
Pour tous les professionnels qui ne sont ni Notaire ni Avocat, il est possible d'acquérir un dispositif physique de sécurité équivalent à la clé REAL&reg; ou AVOCAT&reg;, par exemple chez [Certinomis](https://www.certinomis.fr/produit/certinomis-decideur).
L'usage de ces dispositifs de sécurité par l'utilisateur permet d'offrir un moyen fort d'authentification forte pour l'accès à l'application SecureEchange. 

# Les API
[Les interfaces de programmation ou Application Programming Interfaces (API)](https://fr.wikipedia.org/wiki/Interface_de_programmation) définissent un ensemble de fonctionnalités indépendantes ou liées qui servent de façade par laquelle un système ou logiciel offre des services à d'autres logiciels.  Les services exposés font abstraction de la complexité de leur fonctionnement et des ressources qu'ils utilisent. Cela permet aux logiciels qui les consomment de se concentrer uniquement sur la fonctionnalité proposée tout en laissant traiter la compléxité aux API. 
Exemple d'API :
[Le modèle Document Object Model (DOM)](https://fr.wikipedia.org/wiki/Document_Object_Model) qui permet la manipulation des objets du document d'une page HTML contenue dans un navigateur. 
Exemple: [height (hauteur) et width (largeur)](https://developer.mozilla.org/fr/docs/Web/API/Document_Object_Model/Examples#exemple_1_height_hauteur_et_width_largeur)

SecureEchange définit des API réparties en plusieurs domaines :
- Authentification 
  - Authentification à partir d'une clé REAL, AVOCAT ou Certinomis permet de générer un Bearer token SecureEchange
  - Authentification à partir d'un token Auth0 permet de générer un Bearer Token SecureEchange
  - Authentification à partir d'un token SSO GenApi permet de générer un Bearer Token SecureEchange
- Fonctionnel
  - Permet de gérer la création, la modification, le traitement et l'archivage des échanges. 
  - la collecte et la restitution des suivi des traitements
  - la gestion des crédits de Signature avancées 
- Administration
  - Permet de gérer la création, la modification, les commandes, la consommation des Offices ou entreprises clientes de SecureEchange

A ce découplage par domaine, il faut appliquer une répartition par acteur. 
- Utilisateur de l'office ou l'entreprise : un jeu d'**API privés**  donne accès à toutes les fonctionnalités pour gérer le cycle de vie des échanges.
- Le client final: un jeu d'**API public**  donne accès à un nombre limité de fonctionnalités qui permettront la récupération des documents et la transmission des documents requis. 
- Les opérateurs du site SecureEchange: un jeu d'**API d'administration** permet la création des offices ou entreprises, la prise en compte des commandes et crédits  de Signature, la gestion des utilisateurs et la remontés de statistiques comptables pour le suivi dans notre ERP. 

Pour la publication de toutes nos API, nous avons choisi d'utiliser le modèle [Swagger RestFull respectant OpenApi V3](https://swagger.io/specification/). 

---
## API privées
Gestion des accès à toutes les fonctionnalités des SecureEchange pour les utilisateurs, de l'office ou l'entreprise, authentifiés. 
Chaque utilisateur authentifié, ne peut gérer que les SecureEchange de l'office ou l'entreprise dont il est un membre reconnu à la suite de son authentification.

### Authentification
Pour une authentification clé REAL, AVOCAT ou Certinomis
 Méthode | URI | Paramètres | Retour |
 ----- | ---- | -------| ---- |
  <font color="blue">GET |  **/api/clientcertificate** |(query) service, (valeur par défaut) *SecureEchange*<br>**Client Certificate validé par code pin** | **(200) Success:<br> accessToken (SecureEchange Bearer Token),<br>  description,<br>  expireAt,<br>  notBefore,<br>  issuedAt**<br> <BR>(400) Bad Request:<BR>(401) Unauthorized| 

Pour une authentification par identifiant et mot de passe (Auth0)
 Méthode | URI | Paramètres | Retour |
 ----- | ---- | -------| ---- |
  <font color="blue">GET |  **/api/Auth0** |(query) service, (valeur par defaut) *SecureEchange* <br> (headers):exclamation: Authorization: **Bearer Auth0_Access_Token** | **(200) Success:<br>accessToken, (SecureEchange Bearer Token)<br>  description,<br>  expireAt, <br>  notBefore,<br>  issuedAt**<br><br>(400) Bad Request<br> (401) Unauthorized |

Pour une authentification avec un jeton SSO GenApi
 | Méthode | URI | Paramètres | Retour |
 | ----- | ---- | -------| ---- |
|  <font color="blue">GET | **/api/iNotCloudAuth** | (query) service, (valeur par defaut) *SecureEchange*<br> (headers):exclamation: Authorization: **Bearer Genapi_Access_Token** | **(200) Success:<br>accessToken, <br>SecureEchange Bearer Token), <br>  description,<br>  expireAt,<br>  notBefore,<br>  issuedAt**<br><br>(400) Bad Request<br> (401) Unauthorized

### Fonctionnel
**Toutes les API doivent obligatoirement respecter les régles par défaut suivantes :**
| Régle | Description | valeur |
| ----- | -----| ---- |
| Role obligatoire | Role défini dans le jeton SecureEchange  |  **member** |
| Permission obligatoire | Permission défini dans le jeton SecureEchange | **read:secure_echange** |
| Headers obligatoires | Headers de la requête RestFull| **Authorization: Bearer [Access_Token]** |

Les roles, permissions sont contenues dans le jeton Access_Token SecureEchange généré par les API d'authentification. 
>
#### Liste des permissions 
| Méthode | Description | valeur |
 ----- | -----| ---- |
 <font color="blue">GET | Permission défini dans le jeton SecureEchange | **read:secure_echange** |
 <font color="green">POST | Permission défini dans le jeton SecureEchange | **create:secure_echange** |
<font color="orange"> PUT | Permission défini dans le jeton SecureEchange | **write:secure_echange** |
 <font color="red">DELETE</font> | Permission défini dans le jeton SecureEchange | **delete:secure_echange** |

#### Share
Cette API permet la création, la modification, la suppression et l'archivage des échanges par les utilisateurs authentifiés en tant membre d'une office ou entreprise existante et active. 
Tous les échanges créés, sont automatiquement attachés à l'office ou l'entreprise. Tous les membres d'un office ou d'une entreprise ont accès aux échanges. Pas forcément à la clé de chiffrement des documents qui réside uniquement sur le poste de l'utilisateur qui a créé l'échange et dans le lien adressé au client final. 

 | Méthode | URI | Paramètres | Retour | Payload |
 | ----- | ---- | -------| ---- | --- |
| <font color="blue">GET</font> | /api/share | | **(200) Success**:<br><br> (400) Bad Request<br> (401) Unauthorized|
| <font color="blue">GET</font> | /api/share/{id}/audits | | **(200) Success**:<br><br> (400) Bad Request<br> (401) Unauthorized|
| <font color="blue">GET</font> | /api/share/{id}/file/{fileId} ||**(200) Success**:<br><br> (400) Bad Request<br> (401) Unauthorized|
| <font color="blue">GET</font> | /api/share/{id} ||**(200) Success**:<br><br> (400) Bad Request<br> (401) Unauthorized|
| <font color="green">POST</font> | /api/share ||**(200) Success**:<br><br> (400) Bad Request<br> (401) Unauthorized|
| <font color="green">POST</font> | /api/share/{id}/duplicate ||**(200) Success**:<br><br> (400) Bad Request<br> (401) Unauthorized|
| <font color="green">POST</font> | /api/share/{id}/file ||**(200) Success**:<br><br> (400) Bad Request<br> (401) Unauthorized|
| <font color="green">POST</font> | /api/share/{id}/file/required/file/{index} ||**(200) Success**:<br><br> (400) Bad Request<br> (401) Unauthorized|
| <font color="green">POST</font> | /api/share/{id}/audits||**(200) Success**:<br><br> (400) Bad Request<br> (401) Unauthorized|
| <font color="red">DELETE</font> | /api/share/{id}||**(200) Success**:<br><br> (400) Bad Request<br> (401) Unauthorized|
| <font color="red">DELETE</font> | /api/share/{id}/file/{fileId}||**(200) Success**:<br><br> (400) Bad Request<br> (401) Unauthorized|
| <font color="red">DELETE</font> | /api/share/{id}/requiredfiles/{fileId} ||**(200) Success**:<br><br> (400) Bad Request<br> (401) Unauthorized|

#### Vérification de la connexion
 | Méthode | URI | Paramètres | Retour | Payload |
 | ----- | ---- | -------| ---- | --- |
| <font color="blue">GET</font> | /api/Auth | |**(200) Success**:<br><br> (400) Bad Request<br> (401) Unauthorized|Propriétés principale de l'utilisateur connecté|

#### Companies

 | Méthode | URI | Paramètres | Retour | Payload |
 | ----- | ---- | -------| ---- | --- |
| <font color="blue">GET</font> | /api/companies/info | |**(200) Success**:<br><br> (400) Bad Request<br> (401) Unauthorized| Propriétés principales de l'office ou l'entreprise |
| <font color="blue">GET</font> | /api/companies/Accounting/Credit | |**(200) Success**:<br><br> (400) Bad Request<br> (401) Unauthorized|Nb Crédits Signature |
| <font color="orange">PUT</font> | /api/companies/info |(body) Propriétés à mettre à jour |**(200) Success**:<br><br> (400) Bad Request<br> (401) Unauthorized|Propriétés de l'office ou l'entreprise mis à jour. <br>*:memo: Uniquemenent le champs Propriétes peut-être mis à jour*|

#### Members
 Méthode | URI | Paramètres | Retour | Payload |
 ----- | ---- | -------| ---- | --- |
 <font color="blue">GET</font> | /api/Members/self | |**(200) Success**:<br><br> (400) Bad Request<br> (401) Unauthorized| Les propriétés principales de l'utilisateur connecté. |
 <font color="orange">PUT</font> | /api/Members |(body) Propriétés à mettre à jour |**(200) Success**:<br><br> (400) Bad Request<br> (401) Unauthorized|Propriétés de l'office ou l'entreprise mis à jour. <br>*:memo: Uniquemenent les champs:<br>- Propriétes<br>- Firstname<br>- Lastname<br>- Email<br> Peuvent-être mis à jour.*|

#### SecureEchange
API de test du jeton Access Token SecureEchange
 | Méthode | URI | Paramètres | Retour | Payload |
 | ----- | ---- | -------| ---- | --- |
 | <font color="blue">GET</font> | /api/SecureEchange | |**(200) Success**:<br><br> (400) Bad Request<br> (401) Unauthorized| Les propriétés principales du jeton de l'utilisateur connecté.<br>- Siret/CRPCEN<br>- UniqueID<br>- Name<br>- Email<br>- Liste des rôles<br>- Liste des permissions<br>- IsSecureEchangeAdmin(V/F)<br>- Tenant |

---
## API publics
Gére les fonctionnalités d'un SecureEchange pour un client final. Le client finale ne peut que télécharger les documents transmis et téléverser et signer les documents requis.
### Régles et Liste des permissions 
**Toutes les appels à cette API (sauf ceux de PublicAuth) doivent obligatoirement respecter les régles suivantes :**
| Régle | Description | valeur |
| ----- | -----| ---- |
| Permission obligatoire | Permission défini dans le jeton SecureEchange | **read:secure_echange** |
| Headers obligatoires | Headers de la requête RestFull| **Authorization: Bearer [Access_Token]** |

| Méthode | Description | valeur |
| ----- | -----| ---- |
| <font color="blue">GET</font> | Permission défini dans le jeton SecureEchange | **read:secure_echange** |
| <font color="green">POST<font> | Permission défini dans le jeton SecureEchange | **create:secure_echange** |
| <font color="orange">PUT</font> | Permission défini dans le jeton SecureEchange | **write:secure_echange** |
| <font color="red">DELETE</font> | Permission défini dans le jeton SecureEchange | **delete:secure_echange** |
### Authentification à double facteur 
Lorsque l'utilisateur de l'office ou l'entreprise a créé le SecureEchange, il a inscrit le client final en précisant l'adresse email, le numéro de téléphone et le type de canal de transmission du code de vérification :
1. SMS
1. Message vocal

Processus d'authentification du client final
1. Le client clique sur le lien qu'il a reçu dans sa boite à lettre
2. Dans la page d'authentification, il doit saisir son adresse email
3. Vérification de l'adresse email saisie :
   1. L'adresse email ne fait pas partie des destinataires, on retourne un Unauthorized
4. L'adresse email a été validé, le client peut recevoir un code en fonction du canal choisi et du numéro de téléphone saisis par l'utilisateur de l'office ou l'entreprise
5. Le client doit saisir le code envoyé sur le numéro de téléphone soit par SMS soit en message vocal.
   1. Le code n'est pas le bon, on retourne Unauthorized
6. Le code est validé, le client final accède à son SecureEchange.

Les API qui permettent de prendre en charge ce processus sont PublicAuth.

### PublicAuth

| Méthode | URI | Paramètres | Retour | Payload | Action |
| ----- | ---- | -------| ---- | --- | --- |
| <font color="blue">GET</font> | /publicAuth/{id}/SenderCompanyName |(route) id: idenfiant du SecureEchange |**(200) Success**:<br><br>(400) Bad Request<br> | Le nom de l'office ou l'entreprise|
| <font color="blue">GET</font> | /publicAuth/{shareId} | (route) shareId: identifiant du SecureEchange |**(204) No content**:<br><br> (400) Bad Request<br> (404) Not Found<br>| | Vérifie que le SecureEchange existe et n'est pas expiré. |
| <font color="blue">GET</font> | /publicAuth/{shareId}/{email} | (route) shareId: identifiant du SecureEchange <br>(route) email: email à tester pour ce destinataire |**(200) Success**:<br><br> (400) Bad Request<br> (404) Not Found<br> |Retourne les éléments pour passer à l'étape 2  | Vérification de l'adresse email |
| <font color="blue">GET</font> | /publicAuth/{shareId}/email/sendotp/{method} | (route) shareID: identifiant SecureEchange<br>(route) email: Email validé<br>(route) Method: canal envoi du code SMS ou Vocal |**(204) No Content**:<br><br> (400) Bad Request<br>  (404) Not Found<br>||Envoi le code de vérification par la méthode choisie| 
| <font color="blue">GET</font> |PublicAuth/{shareID}/{email}/verify/{otpPassword}|(route) shareId: identifiant SecureEchange<br>(route) email: email validé<br>(route) otpPassword: code à vérifier |**(200) Success**:<br><br> (400) Bad Request<br>(401) Unauthorized <br>(404) Not Found| Retourne le jeton d'auhtentification  |Controle le code de vérification et si ok génére le jeton d'authentification  |

### Share
| Méthode | URI | Paramètres | Retour | Payload | Action |
| ----- | ---- | -------| ---- | --- | --- |
| <font color="blue">GET</font> | /share/{id} |(route) id: identifiant du SecureEchange |**(200) Success**:<br><br> (400) Bad Request<br>(401) Unauthorized <br>(404) Not Found |les propriétés du SecureEchange| |
| <font color="blue">GET</font> | /share/{id}/file/{fileId} |(route)id: identifiant du SecureEchange<br>(route) fileId: identifiant du fichier à télécharger| **(200) Success**:<br><br> (400) Bad Request<br>(401) Unauthorized <br>(404) Not Found|Le fichier encrypté||
| <font color="green">POST</font> | /share/{id} |(route) id: identifiant du SecureEchange |**(200) Success**:<br><br> (400) Bad Request<br>(401) Unauthorized <br>(404) Not Found|Modifie unique le status du SecureEchange  | |
| <font color="green">POST<font> | /share/{id}/file/{name}|(route) id: identifiant du SecureEchange<br>(route) name: catégorie du fichier à téléverser<br>(multipart) File |**(200) Success**:<br><br> (400) Bad Request<br>(401) Unauthorized <br>(404) Not Found |Téléverse le fichier joint| |
| <font color="red">DELETE</font> | /share/{id}/file/{fileId} |(route) id: identifant du SecureEchange<br>(route) fileID: Identifiant du fichier requis à supprimer |**(200) Success**:<br><br> (400) Bad Request<br>(401) Unauthorized <br>(404) Not Found | |

---
## API Admin

---
## Site public et privée

---
## Partenaires

---
## Interopérabilité Notariale

---
# RGPD

# Architecture Matériel
## Kubernetes
## Devops YAMLs

# Construction des images
## AUTH image
```
docker build -f Dockerfile_auth -t xxxxxxx/auth_secure_echange .   
docker push xxxxxxx/auth_secure_echange      
```
## REAL Image
```
docker build -f Dockerfile_real -t xxxxxxx/real_secure_echange .    
docker push xxxxxxx/real_secure_echange   
```

## PUBLIC API & Web Site image (Production)
```
docker build -f Dockerfile_public -t xxxxxxx/public_secure_echange  .
docker push xxxxxxx/public_secure_echange
```

## PUBLIC API & Web Site image (Preproduction)
```
docker build -f Dockerfile_preprod -t xxxxxxx/preprod_public_secure_echange .
docker push xxxxxxx/preprod_public_secure_echange
```

## INTERNAL APIs image
```
docker build -f Dockerfile_internal -t xxxxxxx/internal_secure_echange  .
docker push xxxxxxx/internal_secure_echange
```

## ADMIN image 
```
docker build -f Dockerfile_admin -t xxxxxxx/admin_secure_echange  .
docker push xxxxxxx/admin_secure_echange
```


