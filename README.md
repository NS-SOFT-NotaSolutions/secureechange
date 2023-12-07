---
#<img src="/Documentation/images/SecureEchange-165.png" width="40" height="40">SecureEchange &reg; 

> Envoyez et recevez vos documents en toute sécurité.

***[Présentation commerciale](https://www.notasolutions.fr/secure-echange/)***


# Sommaire
>Vue globale
>
>Architecture applicative
>>Services Authentification Endpoints
>>Private Endpoints
>>Public Endpoints
>>Admin Endpoints
>>Web Site
>>Partenaires Endpoints
>
>Interopérabilité Notariale
>
>RGPD
>
>Architecture Matériel
>>Kubernetes
>>Devops YAMLs
>
>Sécurisé par Design
>>Stratégie sécurité sur les rôles pour les Endpoints
>>Liste de contrôles d'accès sur les entités
>>>Permission Denied
>>>Permission Granted
>
>Construction des images
>>Services d'authentification
>>>Auth Image
>>>REAL Image
>>
>>APIs
>>>Public API & Web Site Image (Production)
>>>Public API & Web Site Image (Preproduction)
>>>Internal APIs image
>>
>>Services d'administration
>>>Admin image
>

#Vue Globale
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
>- Navigateur + client javascript + Html (Angular) : https://secure-echange.notasolutions.fr
>- Interopérabilité Notariale (Office notarial)
>- Outlook

-Poste de Travail du client final
>- PC:  Edge ou Chrome ou Firefox
>- Mobile: Safari, Chrome

- Back-End
>- APIs (Restfull paradigm)
>- Base de données (NoSQL - Mongo)
>- Cache mémoire partagé (Redis)

- Partenaires
>- LexIA by Lexfluent
>- SMS MODE
>- Oodrive 

##Architecture applicative
###Services Authentification Endpoints
###Private Endpoints
###Public Endpoints
###Admin Endpoints
###Web Site
###Partenaires Endpoints
###Interopérabilité Notariale

#RGPD

#Architecture Matériel
##Kubernetes
##Devops YAMLs

#Sécurisé par Design

##Stratégie sécurité sur les rôles pour les Endpoints
##Liste de contrôles d'accès sur les entités
###Permission Denied
###Permission Granted
##Cycle de vie d'un jeton d'authentification

# Construction des images
## AUTH image
```
docker build --no-cache -f Dockerfile_auth -t jacquesmassa/auth_secure_echange:1_0_1_20 .   
docker push jacquesmassa/auth_secure_echange:1_0_1_20      
```
## REAL Image
```
docker build --no-cache -f Dockerfile_real -t jacquesmassa/real_secure_echange:1_0_0_6 .    
docker push jacquesmassa/real_secure_echange:1_0_0_6   
```

## PUBLIC API & Web Site image (Production)
```
docker build -f Dockerfile_public -t jacquesmassa/public_secure_echange:1_0_1_14  .
docker push jacquesmassa/public_secure_echange:1_0_1_14
```

## PUBLIC API & Web Site image (Preproduction)
```
docker build -f Dockerfile_preprod -t jacquesmassa/preprod_public_secure_echange:1_0_1_52 .
docker push jacquesmassa/preprod_public_secure_echange:1_0_1_52
```

## INTERNAL APIs image
```
docker build -f Dockerfile_internal -t jacquesmassa/internal_secure_echange:1_0_1_16  .
docker push jacquesmassa/internal_secure_echange:1_0_1_16
```


## ADMIN image 
```
docker build -f Dockerfile_admin -t jacquesmassa/admin_secure_echange:1_0_1_18  .
docker push jacquesmassa/admin_secure_echange:1_0_1_18
```



