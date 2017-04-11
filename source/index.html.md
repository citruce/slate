---
title: API Reference

language_tabs:
  - shell

toc_footers:
  - <a href='mailto:bassel@citruce.com'>bassel@citruce.com</a>
  - <a href='https://www.rendementlocatif.com'>Rendement Locatif</a>

includes:
  - errors

search: true
---

# Introduction

Welcome to the Rendement Locatif APIs documentation.
This documentation is stricly confidential.

# API Principles

## Dev Authentication

> To authenticate against the api:

```shell
curl "https://www.rendementlocatif.com/api/login"
  -H "X-API-KEY: secretkey"
```

> Replace secretkey with your assigned dev key.

The Rendement locatif API expects an authentication key.

Just add the key to every request as a header like this:

`X-API-KEY: secretkey`

<aside class="notice">
Replace <code>secretkey</code> with your personal key.
</aside>

## Versionning

The endpoint accepts the version like this https://www.rendementlocatif.com/api/gestion/version/.
<aside class="notice">
Last version is 21. So please use the following endpoint:<br/>
https://www.rendementlocatif.com/api/gestion/21/

21 is not backward compatible with 2 because 21 uses timestamps for dates parameters instead of human readable french dates.
</aside>

# Authentificating a user

## Signup

```shell
curl --data "username=username&password=password&email=email"
  "https://www.rendementlocatif.com/api/signup"
  -H "X-API-KEY: secretkey"
```

> Returns the following JSON if success

```json
{
  "signup": true,
  "Code": "0",
  "Message": "Bienvenue, vous avez accès à toutes les fonctions de l'application pour une durée de 30 jours."
}
```
> Returns the following JSON if error

```json
{
  "signup": false,
  "Code": "-1",
  "message": "there are errors, see errors array",
  "errors": [
    {
      "code": -13,
      "message": "Il existe déjà une personne inscrite avec ce nom d'utilisateur. osamu. Ce nom d'utilisateur est trop ressemblant. Il doit différer par au moins un caractère alpha-numérique (a-z ou 0-9). Veuillez essayer un autre nom d'utilisateur."
    },
    {
      "code": -2,
      "message": "L'adresse électronique que vous avez saisie est invalide."
    }
  ]
}
```

This endpoint signs up a new user.

### HTTP Request

`POST https://www.rendementlocatif.com/api/signup`

### Parameters

Parameter | Description
--------- | -------
username | The username
password | The user password
email | The user email address

Should be sent with the request as "application/x-www-form-urlencoded".

### Returned parameters

Parameter | Type | Description
--------- | ------- | -------
signup | boolean | true if the user is signed up, false otherwise
code | string | 0 : user is premium, -1 : there are errors,
message | string | a message that could be displayed to user (optional)
errors | array | array of code and message for every error to display to the user

<aside class="success">
The user should be signed up if signup=true
</aside>

## Login

```shell
curl --data "username=username&password=password"
  "https://www.rendementlocatif.com/api/login"
  -H "X-API-KEY: secretkey"
```

> Returns the following JSON:

```json
{
  "auth": true,
  "userid": "2",
  "Code": "0",
  "Message": "Bienvenue!",
  "premium_end": "2016-06-30T23:06:06+02:00",
  "premium_offer": "PRO",
  "username": "bob",
  "last_name": "Dupont",
  "first_name": "Jacques"
}
```

This endpoint logs in a user and returns if the user is premium or not.

### HTTP Request

`POST https://www.rendementlocatif.com/api/login`

### Parameters

Parameter | Description
--------- | -------
username | The user name or email address
password | The user password

Should be sent with the request as "application/x-www-form-urlencoded".

### Returned parameters

Parameter | Type | Description
--------- | ------- | -------
Auth | boolean | true if the user is authenticated ok and premium
userid | string | the is of the user (use this as the unique and permanent id)
Code | string | 0 : user is premium, -1 : unknown username, -2 : wrong password, -3 : free access granted
Message | string | a message that could be displayed to user (optional)
premium_offer | date | the name of the premium plan to which the user is subscribed to
premium_end | date | end of current premium subscription period
last_name | string | the last name of the user if known (empty string otherwise)
first_name | string | the first name of the user if known (empty string otherwise)

<aside class="success">
The user should be authenticated if auth=true only (when premium access only is allowed).
If non premium access is allowed, check Auth=false and code=-3.
</aside>

## Forgot Password

```shell
curl --data "username=username&password=password"
  "https://www.rendementlocatif.com/api/pass"
  -H "X-API-KEY: secretkey"
```

> Returns the following JSON:

```json
{
  "pass": true,
  "Code": "0",
  "message": "Vous allez recevoir un email qui va vous permettre de réinitialiser votre mot de passe.!"
}
```

Initiate the process of renewing a user password.

### HTTP Request

`POST https://www.rendementlocatif.com/api/pass`

### Parameters

Parameter | Description
--------- | -------
email | The email address of the user

Should be sent with the request as "application/x-www-form-urlencoded".

### Returned parameters

Parameter | Type | Description
--------- | ------- | -------
pass | boolean | true if the password renewal steps were sent by email to the user
Code | string | "0" : ok, other : error
message | string | Error message if error or user message if everything went ok


## User authenticated requests

When requesting the API for user related content the username and password should always be passed as POST parameters with the request.

# Gestion

## Immeubles

Gets all the "immeubles" of a user and their associated "biens".

> Code to get the immeubles of a user

```shell
curl --data "username=username&password=password"
"https://www.rendementlocatif.com/api/gestion/21/immeubles"
  -H "X-API-KEY: secretkey"
```

> Returns the following JSON:

```json
{
  "count": 6,
  "immeubles": [
    {
      "active": true,
      "address": "24, rue d'iena",
      "comagence": 3703.7,
      "comcourtier": 0,
      "country": "FR",
      "dateCreated": {
        "sec": 1453815399,
        "usec": 517000
      },
      "dateLastModified": {
        "sec": 1460968762,
        "usec": 150000
      },
      "fraisnotaire": 4641,
      "investId": "56a76ea148177ef3328b4567",
      "meubles": 0,
      "milliemes": 1000,
      "name": "Studio rue Iena",
      "options": {
        "autoPaiement": true,
        "id": ""
      },
      "prixnet": 46296.3,
      "prixtoday": 45000,
      "robot": false,
      "sourceFavoriteId": "5609c8be48177e1f128b4569",
      "surface": 23,
      "switchNbImmeubles": "1",
      "travauxr": 0,
      "type": "appartement",
      "user_id": "2",
      "ville": "Angers|49000",
      "villevalue": "Angers (49000)",
      "imageURL": "https://maps.googleapis.com/maps/api/streetview?size=640x400&location=24%2C+rue+d%27iena%2C+Angers%2C+France&key=AIzaSyC0bksAgpmpdGIZt4Ed3gj7yEes4RG7TZo",
      "totalEmprunt": 50095.67,
      "totalMensualite": 363.82,
      "date_achat": {
        "sec": 1299452400,
        "usec": 0
      },
      "id": "56a7766748177e22348b4567",
      "emprunts": [
        {
          "index": 0,
          "montant": 30336.5,
          "taux": 3.09,
          "typeEmprunt": "amorti",
          "mensualite": 299.35,
          "mensualiteAssurance": 5.16,
          "dateFirstmensualite": {
            "sec": 1302127200,
            "usec": 0
          },
          "dateLastmensualite": {
            "sec": 1615071600,
            "usec": 0
          }
        },
        {
          "index": 1,
          "montant": 19759.17,
          "taux": 3.45,
          "typeEmprunt": "amorti",
          "mensualite": 64.47,
          "mensualiteAssurance": 3.36,
          "dateFirstmensualite": {
            "sec": 1302127200,
            "usec": 0
          },
          "dateLastmensualite": {
            "sec": 1772838000,
            "usec": 0
          }
        }
      ],
      "biens": [
        {
          "active": true,
          "chambres": 0,
          "dateCreated": {
            "sec": 1453815399,
            "usec": 517000
          },
          "dateLastModified": {
            "sec": 1461096415,
            "usec": 746000
          },
          "energy": "",
          "etage": 2,
          "ges": "",
          "immeubleId": "56a7766748177e22348b4567",
          "milliemes": 1000,
          "nom": "Bien Principal",
          "pieces": "T1",
          "robot": false,
          "surface": 23,
          "type": "studio",
          "user_id": "2",
          "id": "56a7766748177e22348b4568"
        }
      ],
      "countOperationsUnpaid": 1
    },
    {
      "active": true,
      "address": "3, rue Flore",
      "comagence": 3925.93,
      "comcourtier": 0,
      "country": "FR",
      "dateCreated": {
        "sec": 1453850724,
        "usec": 645000
      },
      "dateLastModified": {
        "sec": 1460357112,
        "usec": 335000
      },
      "fraisnotaire": 4834,
      "investId": "56a8003848177ede3c8b4567",
      "meubles": 0,
      "milliemes": 1000,
      "name": "2 pièces rue Flore",
      "prixnet": 83100,
      "prixtoday": 83102,
      "robot": false,
      "sourceFavoriteId": "552a862048177ee125959525",
      "surface": 32,
      "switchNbImmeubles": "1",
      "travauxr": 0,
      "type": "appartement",
      "user_id": "2",
      "ville": "Angers|49000",
      "villevalue": "Angers (49000)",
      "imageURL": "https://maps.googleapis.com/maps/api/streetview?size=640x400&location=3%2C+rue+Flore%2C+Angers%2C+France&key=AIzaSyC0bksAgpmpdGIZt4Ed3gj7yEes4RG7TZo",
      "totalEmprunt": 145100,
      "totalMensualite": 828.09,
      "date_achat": {
        "sec": 1185919200,
        "usec": 0
      },
      "id": "56a8006448177e563d8b4567",
      "emprunts": [
        {
          "index": 0,
          "montant": 62000,
          "taux": 3.35,
          "typeEmprunt": "amorti",
          "mensualite": 166.31,
          "mensualiteAssurance": 10.54,
          "dateFirstmensualite": {
            "sec": 1349560800,
            "usec": 0
          },
          "dateLastmensualite": {
            "sec": 1662501600,
            "usec": 0
          }
        },
        {
          "index": 1,
          "montant": 83100,
          "taux": 3.7,
          "typeEmprunt": "amorti",
          "mensualite": 661.78,
          "mensualiteAssurance": 50.97,
          "dateFirstmensualite": {
            "sec": 1187820000,
            "usec": 0
          },
          "dateLastmensualite": {
            "sec": 1345672800,
            "usec": 0
          }
        }
      ],
      "biens": [
        {
          "active": true,
          "annonceDesc": "Charmant 2 pièces en hyper centre (Place Imbach, en bas de la Rue Lenepveu).\n\n- Parquet au sol\n- 1 Chambre séparée\n- Séjour avec coin cuisine équipé (frigo, plaques, petit bar)\n- Placards muraux\n- Salle de bains\n- WC\n- idéal étudiants / Couple\n- pas de frais d'agence\n\nLoyer: 460€ + 20€ de charges\n\nDisponible de suite.",
          "annonceTitle": "T2 Angers Hyper Centre - Place Imbach",
          "chambres": 1,
          "dateCreated": {
            "sec": 1453850724,
            "usec": 645000
          },
          "dateLastModified": {
            "sec": 1460994811,
            "usec": 322000
          },
          "energy": "f",
          "etage": 2,
          "ges": "d",
          "immeubleId": "56a8006448177e563d8b4567",
          "milliemes": 1000,
          "nom": "T2 Flore",
          "pieces": "T2",
          "robot": false,
          "surface": 32.02,
          "type": "appartement",
          "user_id": "2",
          "id": "56a8006448177e563d8b4568"
        }
      ],
      "countOperationsUnpaid": 1
    },
    {
      "active": true,
      "address": "6 Boulevard Saint-Germain",
      "address_number": "6",
      "codepostal": "75005",
      "comagence": 1876.74,
      "comcourtier": 0,
      "country": "FR",
      "country_long": "France",
      "dateCreated": {
        "sec": 1461964136,
        "usec": 939000
      },
      "dateLastModified": {
        "sec": 1470400120,
        "usec": 373000
      },
      "fraisnotaire": 2505.47,
      "investId": "5713bbfb48177e10128b4568",
      "meubles": 0,
      "milliemes": 538,
      "name": "dsfdsf",
      "options": {
        "autoPaiement": true,
        "id": ""
      },
      "place_id": "ChIJsZg7Nftx5kcRk2Z6iROLZCE",
      "place_url": "https://maps.google.com/?q=6+Boulevard+Saint-Germain,+75005+Paris,+France&ftid=0x47e671fb353b98b1:0x21648b13897a6693",
      "prixnet": 25023.26,
      "prixtoday": 25023.26,
      "robot": false,
      "surface": 23,
      "travauxr": 2690,
      "type": "appartement",
      "user_id": "2",
      "ville": "Paris|75005",
      "ville_niveau1": "Île-de-France",
      "ville_niveau2": "Paris",
      "ville_seule": "Paris",
      "villevalue": "Paris (75005)",
      "imageURL": "https://maps.googleapis.com/maps/api/streetview?size=640x400&location=6+Boulevard+Saint-Germain%2C+Paris%2C+France&key=AIzaSyC0bksAgpmpdGIZt4Ed3gj7yEes4RG7TZo",
      "totalEmprunt": 100000,
      "totalMensualite": 650,
      "date_achat": {
        "sec": 1323126000,
        "usec": 0
      },
      "id": "5723cd6848177e7f298b4567",
      "emprunts": [
        {
          "index": 0,
          "montant": 100000,
          "taux": 3,
          "typeEmprunt": "amorti",
          "mensualite": 650,
          "mensualiteAssurance": 15,
          "dateFirstmensualite": {
            "sec": 1346796000,
            "usec": 0
          },
          "dateLastmensualite": {
            "sec": 1820095200,
            "usec": 0
          }
        }
      ],
      "biens": [
        {
          "nom": "Bien Principal",
          "immeubleId": "5723cd6848177e7f298b4567",
          "surface": 23,
          "milliemes": 1000,
          "pieces": "",
          "type": "appartement",
          "user_id": "2",
          "active": true,
          "dateCreated": {
            "sec": 1461964136,
            "usec": 939000
          },
          "robot": true,
          "id": "5723cd6848177e7f298b4568"
        }
      ],
      "countOperationsUnpaid": 0
    },
    {
      "active": true,
      "address": "100 avenue Pasteur",
      "address_number": "",
      "codepostal": "49000",
      "comagence": 6500,
      "comcourtier": 0,
      "country": "FR",
      "country_long": "France",
      "dateCreated": {
        "sec": 1454879066,
        "usec": 59000
      },
      "dateLastModified": {
        "sec": 1470567646,
        "usec": 725000
      },
      "fraisnotaire": 4350,
      "investId": "56b7b0e348177ee5055932cb",
      "meubles": 0,
      "milliemes": 1000,
      "name": "Pasteur Double Appart",
      "options": {
        "autoPaiement": true,
        "id": ""
      },
      "place_id": "",
      "place_url": "",
      "prixnet": 83500,
      "prixtoday": 83500,
      "robot": false,
      "sourceFavoriteId": "",
      "surface": 55.6,
      "switchNbImmeubles": "1",
      "travauxr": 0,
      "type": "appartement",
      "user_id": "2",
      "ville": "Angers|49000",
      "ville_niveau1": "",
      "ville_niveau2": "",
      "ville_seule": "Angers",
      "villevalue": "Angers (49000)",
      "imageURL": "https://maps.googleapis.com/maps/api/streetview?size=640x400&location=100+avenue+Pasteur%2C+Angers%2C+France&key=AIzaSyC0bksAgpmpdGIZt4Ed3gj7yEes4RG7TZo",
      "totalEmprunt": 90000,
      "totalMensualite": 685.21,
      "date_achat": {
        "sec": 1348783200,
        "usec": 0
      },
      "id": "56b7b15a48177ea7085932cd",
      "emprunts": [
        {
          "index": 0,
          "montant": 90000,
          "taux": 3.09,
          "typeEmprunt": "amorti",
          "mensualite": 685.21,
          "mensualiteAssurance": 6.45,
          "dateFirstmensualite": {
            "sec": 1352070000,
            "usec": 0
          },
          "dateLastmensualite": {
            "sec": 1822687200,
            "usec": 0
          }
        }
      ],
      "biens": [
        {
          "active": true,
          "annonceDesc": "Appartement lumineux à visiter le plus tôt possible.\n\nIl t'appartient de venir pour le voir ;)",
          "annonceTitle": "Beau studio titre",
          "chambres": 0,
          "dateCreated": {
            "sec": 1454879066,
            "usec": 59000
          },
          "dateLastModified": {
            "sec": 1459891643,
            "usec": 26000
          },
          "energy": "b",
          "etage": 1,
          "ges": "c",
          "immeubleId": "56b7b15a48177ea7085932cd",
          "milliemes": 422,
          "nom": "Studio porte droite",
          "pieces": "T1",
          "surface": 23.49,
          "type": "studio",
          "user_id": "2",
          "locataires": [
            {
              "id": "576665fc48177efe118b4567",
              "nom": "Ouriele",
              "prenom": "Braque"
            },
            {
              "id": "56b7b52348177ea2085932cc",
              "nom": "Faral",
              "prenom": "Jordan"
            }
          ],
          "id": "56b7b15a48177ea7085932ce"
        },
        {
          "active": true,
          "chambres": 1,
          "dateCreated": {
            "sec": 1454879205,
            "usec": 55000
          },
          "dateLastModified": {
            "sec": 1459605973,
            "usec": 888000
          },
          "etage": 1,
          "immeubleId": "56b7b15a48177ea7085932cd",
          "milliemes": 578,
          "nom": "2 pièces gauche",
          "pieces": "T2",
          "surface": 32.11,
          "type": "appartement",
          "user_id": "2",
          "id": "56b7b1e548177eab085932c6"
        }
      ],
      "countOperationsUnpaid": 6
    },
    {
      "active": true,
      "address": "",
      "comagence": 3703.7,
      "comcourtier": 0,
      "country": "FR",
      "dateCreated": {
        "sec": 1467620963,
        "usec": 695000
      },
      "date_achat": {
        "sec": 1404165600,
        "usec": 0
      },
      "fraisnotaire": 4621,
      "investId": "577a1e6348177e60128218a4",
      "meubles": 500,
      "milliemes": 1000,
      "name": "App. Angers Iena LMNP",
      "options": {
        "autoPaiement": true,
        "id": ""
      },
      "prixnet": 46296.3,
      "prixtoday": 46296.3,
      "robot": true,
      "sourceFavoriteId": "575140e048177e9937d0f7d5",
      "surface": 23,
      "travauxr": 0,
      "type": "appartement",
      "user_id": "2",
      "ville": "Angers|49000",
      "villevalue": "Angers (49000)",
      "imageURL": "https://maps.googleapis.com/maps/api/staticmap?zoom=6&size=640x400&maptype=roadmap&markers=color:red%7CAngers%2C+49000%2C+France&key=AIzaSyC0bksAgpmpdGIZt4Ed3gj7yEes4RG7TZo",
      "totalEmprunt": 50683,
      "totalMensualite": 346.75,
      "id": "577a1e6348177e60128218a5",
      "emprunts": [
        {
          "index": 0,
          "montant": 50683,
          "taux": 2.23,
          "typeEmprunt": "amorti",
          "mensualite": 346.75,
          "mensualiteAssurance": 15.2,
          "dateFirstmensualite": {
            "sec": 1404165600,
            "usec": 0
          },
          "dateLastmensualite": {
            "sec": 1874959200,
            "usec": 0
          }
        }
      ],
      "biens": [
        {
          "active": true,
          "dateCreated": {
            "sec": 1467620963,
            "usec": 698000
          },
          "dateLastModified": {
            "sec": 1473101374,
            "usec": 567000
          },
          "energy": "",
          "ges": "",
          "immeubleId": "577a1e6348177e60128218a5",
          "milliemes": 1000,
          "nom": "Bien Principal",
          "pieces": "",
          "robot": false,
          "surface": 23,
          "type": "appartement",
          "user_id": "2",
          "locataires": [
            {
              "id": "577a1e6348177e60128218a7",
              "nom": "Dupont",
              "prenom": "Jean"
            }
          ],
          "id": "577a1e6348177e60128218a6"
        }
      ],
      "countOperationsUnpaid": 0
    },
    {
      "active": true,
      "address": "56 rue de la place",
      "comagence": 1611.63,
      "comcourtier": 0,
      "country": "FR",
      "dateCreated": {
        "sec": 1460911099,
        "usec": 738000
      },
      "dateLastModified": {
        "sec": 1461964032,
        "usec": 225000
      },
      "date_achat": {
        "sec": 1323126000,
        "usec": 0
      },
      "fraisnotaire": 2151.53,
      "investId": "5713bbfb48177e10128b4568",
      "meubles": 0,
      "milliemes": 462,
      "name": "App. Test sans emprunt",
      "options": {
        "autoPaiement": true,
        "id": ""
      },
      "prixnet": 21488.37,
      "prixtoday": 46511.63,
      "robot": false,
      "sourceFavoriteId": "55f5beee48177e7f60676b04",
      "surface": 23,
      "travauxr": 2310,
      "type": "appartement",
      "user_id": "2",
      "ville": "Angers|49000",
      "villevalue": "Angers (49000)",
      "imageURL": "https://maps.googleapis.com/maps/api/streetview?size=640x400&location=56+rue+de+la+place%2C+Angers%2C+France&key=AIzaSyC0bksAgpmpdGIZt4Ed3gj7yEes4RG7TZo",
      "totalEmprunt": 100000,
      "totalMensualite": 650,
      "id": "5713bbfb48177e10128b4569",
      "emprunts": [
        {
          "index": 0,
          "montant": 100000,
          "taux": 3,
          "typeEmprunt": "amorti",
          "mensualite": 650,
          "mensualiteAssurance": 15,
          "dateFirstmensualite": {
            "sec": 1346796000,
            "usec": 0
          },
          "dateLastmensualite": {
            "sec": 1820095200,
            "usec": 0
          }
        }
      ],
      "biens": [
        {
          "active": true,
          "dateCreated": {
            "sec": 1460911099,
            "usec": 738000
          },
          "dateLastModified": {
            "sec": 1462348335,
            "usec": 845000
          },
          "energy": "",
          "ges": "",
          "immeubleId": "5713bbfb48177e10128b4569",
          "milliemes": 548,
          "nom": "test 1er",
          "pieces": "",
          "robot": false,
          "surface": 23,
          "type": "appartement",
          "user_id": "2",
          "locataires": [
            {
              "id": "574d448b48177e1928d0f7d5",
              "nom": "2ème dupont",
              "prenom": "georges"
            },
            {
              "id": "5713bbfb48177e10128b456b",
              "nom": "Dupont (à modifier)",
              "prenom": "Jean (à modifier)"
            }
          ],
          "id": "5713bbfb48177e10128b456a"
        },
        {
          "nom": "test 2ème",
          "surface": 10.4,
          "type": "appartement",
          "milliemes": 452,
          "pieces": "",
          "etage": 0,
          "chambres": 0,
          "energy": "",
          "ges": "",
          "user_id": "2",
          "immeubleId": "5713bbfb48177e10128b4569",
          "active": true,
          "dateCreated": {
            "sec": 1462348325,
            "usec": 462000
          },
          "robot": false,
          "locataires": [
            {
              "id": "57323dbc48177e31558b4567",
              "nom": "deuxieme",
              "prenom": "locataire"
            }
          ],
          "id": "5729aa2548177e08408b4567"
        }
      ],
      "countOperationsUnpaid": 0
    }
  ],
  "result": true,
  "code": 0
}
```

### HTTP Request

`POST https://www.rendementlocatif.com/api/gestion/21/immeubles`

### Parameters

Should be sent with the request as "application/x-www-form-urlencoded".

Parameter | Description
--------- | -------
username | The user name or email address
password | The user password


## Immeuble

Gets the details of a "immeuble" of a user.

> Code to get the bien of a user

```shell
curl --data "username=username&password=password"
"https://www.rendementlocatif.com/api/gestion/21/immeuble/<immeubleId>"
  -H "X-API-KEY: secretkey"
```

> Returns the following JSON:

```json
{
  "immeuble": {
    "active": true,
    "address": "24, rue d'iena",
    "comagence": 3703.7,
    "comcourtier": 0,
    "country": "FR",
    "dateCreated": {
      "sec": 1453815399,
      "usec": 517000
    },
    "dateLastModified": {
      "sec": 1460968762,
      "usec": 150000
    },
    "fraisnotaire": 4641,
    "investId": "56a76ea148177ef3328b4567",
    "meubles": 0,
    "milliemes": 1000,
    "name": "Studio rue Iena",
    "options": {
      "autoPaiement": true
    },
    "prixnet": 46296.3,
    "prixtoday": 45000,
    "robot": false,
    "sourceFavoriteId": "5609c8be48177e1f128b4569",
    "surface": 23,
    "switchNbImmeubles": "1",
    "travauxr": 0,
    "type": "appartement",
    "user_id": "2",
    "ville": "Angers|49000",
    "villevalue": "Angers (49000)",
    "imageURL": "https://maps.googleapis.com/maps/api/streetview?size=640x400&location=24%2C+rue+d%27iena%2C+Angers%2C+France&key=AIzaSyC0bksAgpmpdGIZt4Ed3gj7yEes4RG7TZo",
    "totalEmprunt": 50095.67,
    "totalMensualite": 363.82,
    "date_achat": {
      "sec": 1299452400,
      "usec": 0
    },
    "id": "56a7766748177e22348b4567",
    "emprunts": [
      {
        "index": 0,
        "montant": 30336.5,
        "taux": 3.09,
        "typeEmprunt": "amorti",
        "mensualite": 299.35,
        "mensualiteAssurance": 5.16,
        "dateFirstmensualite": {
          "sec": 1302127200,
          "usec": 0
        },
        "dateLastmensualite": {
          "sec": 1615071600,
          "usec": 0
        }
      },
      {
        "index": 1,
        "montant": 19759.17,
        "taux": 3.45,
        "typeEmprunt": "amorti",
        "mensualite": 64.47,
        "mensualiteAssurance": 3.36,
        "dateFirstmensualite": {
          "sec": 1302127200,
          "usec": 0
        },
        "dateLastmensualite": {
          "sec": 1772838000,
          "usec": 0
        }
      }
    ]
  },
  "result": true,
  "code": 0
}
```

### HTTP Request

`POST https://www.rendementlocatif.com/api/gestion/21/immeuble/<immeubleId>`

### Parameters

Should be sent with the request as "application/x-www-form-urlencoded".

Parameter | Description
--------- | -------
username | The user name or email address
password | The user password

## Bien

Gets the details of a "bien" of a user and its current present locataires (current tenants).

> Code to get the bien of a user

```shell
curl --data "username=username&password=password"
"https://www.rendementlocatif.com/api/gestion/21/bien/<bienId>"
  -H "X-API-KEY: secretkey"
```

> Returns the following JSON:

```json
{
    "bien": {
        "active": true,
        "annonceDesc": "Appartement lumineux à visiter le plus tôt possible.\n\nIl t'appartient de venir pour le voir ;)",
        "annonceTitle": "Beau studio titre",
        "chambres": 0,
        "dateCreated": {
            "sec": 1454879066,
            "usec": 59000
        },
        "dateLastModified": {
            "sec": 1459891643,
            "usec": 26000
        },
        "energy": "b",
        "etage": 1,
        "ges": "c",
        "immeubleId": "56b7b15a48177ea7085932cd",
        "milliemes": 422,
        "nom": "Studio porte droite",
        "pieces": "T1",
        "surface": 23.49,
        "type": "studio",
        "user_id": "2",
        "id": "56b7b15a48177ea7085932ce",
        "locataires": [
            {
                "nom": "Ouriele",
                "prenom": "Braque",
                "precision": "",
                "email": "",
                "loyer": 350,
                "provision": 25,
                "indiceRevision": 3,
                "bienId": "56b7b15a48177ea7085932ce",
                "signatureDate": {
                    "sec": 1465250400,
                    "usec": 0
                },
                "entryDate": {
                    "sec": 1466805600,
                    "usec": 0
                },
                "quitDate": null,
                "dayLoyer": 1,
                "phone": "",
                "typeLocation": 0,
                "address1": "",
                "address2": "",
                "ville": "",
                "codepostal": "",
                "country": "",
                "bornDate": null,
                "bornLocation": "",
                "nationality": "",
                "user_id": "2",
                "immeubleId": "56b7b15a48177ea7085932cd",
                "dateCreated": {
                    "sec": 1466328572,
                    "usec": 724000
                },
                "robot": false,
                "id": "576665fc48177efe118b4567",
                "loyerLast": 350,
                "provisionLast": 25
            },
            {
                "address1": "",
                "address2": "",
                "bienId": "56b7b15a48177ea7085932ce",
                "bornDate": {
                    "sec": 1460412000,
                    "usec": 0
                },
                "bornLocation": "",
                "codepostal": "",
                "country": "",
                "dateCreated": {
                    "sec": 1454880035,
                    "usec": 690000
                },
                "dateLastModified": {
                    "sec": 1469194115,
                    "usec": 191000
                },
                "dayLoyer": 28,
                "depotGarantie": 1,
                "email": "bassel@citruce.com",
                "entryDate": {
                    "sec": 1348783200,
                    "usec": 0
                },
                "immeubleId": "56b7b15a48177ea7085932cd",
                "indiceRevision": 0,
                "loyer": 300,
                "nationality": "",
                "nom": "Faral",
                "phone": "",
                "precision": "Jeune qui fait le brin",
                "prenom": "Jordan",
                "provision": 15,
                "quitDate": null,
                "revisions": {
                    "id": ""
                },
                "revisionsPC": {
                    "2015": {
                        "dateDebutCalcul": {
                            "sec": 1420066800,
                            "usec": 0
                        },
                        "dateFinCalcul": {
                            "sec": 1451516400,
                            "usec": 0
                        },
                        "provisionNew": 15,
                        "provisionOld": 15,
                        "nextStart": {
                            "sec": 1446591600,
                            "usec": 0
                        },
                        "revisionChargesId": "57303aac48177e3b4d8b4567",
                        "_id": {
                            "$id": "57303af248177ef84a8b4567"
                        },
                        "dateCreated": {
                            "sec": 1462778610,
                            "usec": 507000
                        }
                    },
                    "id": ""
                },
                "robot": false,
                "signatureDate": {
                    "sec": 1348783200,
                    "usec": 0
                },
                "typeLocation": 1,
                "user_id": "2",
                "ville": "",
                "id": "56b7b52348177ea2085932cc",
                "loyerLast": 300,
                "provisionLast": 15
            }
        ]
    },
    "immeuble": {
        "active": true,
        "address": "100 avenue Pasteur",
        "address_number": "",
        "codepostal": "49000",
        "comagence": 6500,
        "comcourtier": 0,
        "country": "FR",
        "country_long": "France",
        "dateCreated": {
            "sec": 1454879066,
            "usec": 59000
        },
        "dateFirstmensualite": null,
        "dateLastModified": {
            "sec": 1470567646,
            "usec": 725000
        },
        "dateLastmensualite": null,
        "declarations": [
            {
                "year": 2014,
                "regime": "rr",
                "type": "foncier",
                "id": "56e1dc8a48177e404a8b4568"
            },
            {
                "year": 2015,
                "regime": "rr",
                "type": "foncier",
                "id": "56f5a6de48177e801c8b456f"
            },
            {
                "year": 2016,
                "regime": "rr",
                "type": "foncier",
                "id": "5710c0e948177e22438b456a"
            },
            {
                "ensembleId": "574949d848177e39198b4567",
                "year": 2016,
                "regime": "mer",
                "type": "bic",
                "id": "578f97ce48177e9918d071bf"
            }
        ],
        "fraisnotaire": 4350,
        "investId": "56b7b0e348177ee5055932cb",
        "meubles": 0,
        "milliemes": 1000,
        "name": "Pasteur Double Appart",
        "options": {
            "autoPaiement": true
        },
        "place_id": "",
        "place_url": "",
        "prixnet": 83500,
        "prixtoday": 83500,
        "robot": false,
        "sourceFavoriteId": "",
        "surface": 55.6,
        "switchNbImmeubles": "1",
        "travauxr": 0,
        "type": "appartement",
        "user_id": "2",
        "ville": "Angers|49000",
        "ville_niveau1": "",
        "ville_niveau2": "",
        "ville_seule": "Angers",
        "villevalue": "Angers (49000)",
        "imageURL": "https://maps.googleapis.com/maps/api/streetview?size=640x400&location=100+avenue+Pasteur%2C+Angers%2C+France&key=AIzaSyC0bksAgpmpdGIZt4Ed3gj7yEes4RG7TZo",
        "id": "56b7b15a48177ea7085932cd"
    },
    "result": true,
    "code": 0
}
```

### HTTP Request

`POST https://www.rendementlocatif.com/api/gestion/21/bien/<bienId>`

### Parameters

Should be sent with the request as "application/x-www-form-urlencoded".

Parameter | Description
--------- | -------
username | The user name or email address
password | The user password

## Locataires

Gets all the "locataires" of a bien.

> Code to get the bien of a user

```shell
curl --data "username=username&password=password"
"https://www.rendementlocatif.com/api/gestion/21/locataires/<immeubleIdOrbienId>/<limit>"
  -H "X-API-KEY: secretkey"
```

> Returns the following JSON:

```json
{
    "countAll": 2,
    "count": 2,
    "locataires": [
        {
            "nom": "Ouriele",
            "prenom": "Braque",
            "precision": "",
            "email": "",
            "loyer": 350,
            "provision": 25,
            "indiceRevision": 3,
            "bienId": "56b7b15a48177ea7085932ce",
            "signatureDate": {
                "sec": 1465250400,
                "usec": 0
            },
            "entryDate": {
                "sec": 1466805600,
                "usec": 0
            },
            "quitDate": null,
            "dayLoyer": 1,
            "phone": "",
            "typeLocation": 0,
            "address1": "",
            "address2": "",
            "ville": "",
            "codepostal": "",
            "country": "",
            "bornDate": null,
            "bornLocation": "",
            "nationality": "",
            "user_id": "2",
            "immeubleId": "56b7b15a48177ea7085932cd",
            "dateCreated": {
                "sec": 1466328572,
                "usec": 724000
            },
            "robot": false,
            "id": "576665fc48177efe118b4567",
            "loyerLast": 350,
            "provisionLast": 25
        },
        {
            "address1": "",
            "address2": "",
            "bienId": "56b7b15a48177ea7085932ce",
            "bornDate": {
                "sec": 1460412000,
                "usec": 0
            },
            "bornLocation": "",
            "codepostal": "",
            "country": "",
            "dateCreated": {
                "sec": 1454880035,
                "usec": 690000
            },
            "dateLastModified": {
                "sec": 1469194115,
                "usec": 191000
            },
            "dayLoyer": 28,
            "depotGarantie": 1,
            "email": "bassel@citruce.com",
            "entryDate": {
                "sec": 1348783200,
                "usec": 0
            },
            "immeubleId": "56b7b15a48177ea7085932cd",
            "indiceRevision": 0,
            "loyer": 300,
            "nationality": "",
            "nom": "Faral",
            "phone": "",
            "precision": "Jeune qui fait le brin",
            "prenom": "Jordan",
            "provision": 15,
            "quitDate": null,
            "revisions": {
                "id": ""
            },
            "revisionsPC": {
                "2015": {
                    "dateDebutCalcul": {
                        "sec": 1420066800,
                        "usec": 0
                    },
                    "dateFinCalcul": {
                        "sec": 1451516400,
                        "usec": 0
                    },
                    "provisionNew": 15,
                    "provisionOld": 15,
                    "nextStart": {
                        "sec": 1446591600,
                        "usec": 0
                    },
                    "revisionChargesId": "57303aac48177e3b4d8b4567",
                    "_id": {
                        "$id": "57303af248177ef84a8b4567"
                    },
                    "dateCreated": {
                        "sec": 1462778610,
                        "usec": 507000
                    }
                },
                "id": ""
            },
            "robot": false,
            "signatureDate": {
                "sec": 1348783200,
                "usec": 0
            },
            "typeLocation": 1,
            "user_id": "2",
            "ville": "",
            "id": "56b7b52348177ea2085932cc",
            "loyerLast": 300,
            "provisionLast": 15
        }
    ],
    "bien": {
        "active": true,
        "annonceDesc": "Appartement lumineux à visiter le plus tôt possible.\n\nIl t'appartient de venir pour le voir ;)",
        "annonceTitle": "Beau studio titre",
        "chambres": 0,
        "dateCreated": {
            "sec": 1454879066,
            "usec": 59000
        },
        "dateLastModified": {
            "sec": 1459891643,
            "usec": 26000
        },
        "energy": "b",
        "etage": 1,
        "ges": "c",
        "immeubleId": "56b7b15a48177ea7085932cd",
        "milliemes": 422,
        "nom": "Studio porte droite",
        "pieces": "T1",
        "surface": 23.49,
        "type": "studio",
        "user_id": "2",
        "id": "56b7b15a48177ea7085932ce"
    },
    "immeuble": {
        "active": true,
        "address": "100 avenue Pasteur",
        "address_number": "",
        "codepostal": "49000",
        "comagence": 6500,
        "comcourtier": 0,
        "country": "FR",
        "country_long": "France",
        "dateCreated": {
            "sec": 1454879066,
            "usec": 59000
        },
        "dateFirstmensualite": null,
        "dateLastModified": {
            "sec": 1470567646,
            "usec": 725000
        },
        "dateLastmensualite": null,
        "declarations": [
            {
                "year": 2014,
                "regime": "rr",
                "type": "foncier",
                "id": "56e1dc8a48177e404a8b4568"
            },
            {
                "year": 2015,
                "regime": "rr",
                "type": "foncier",
                "id": "56f5a6de48177e801c8b456f"
            },
            {
                "year": 2016,
                "regime": "rr",
                "type": "foncier",
                "id": "5710c0e948177e22438b456a"
            },
            {
                "ensembleId": "574949d848177e39198b4567",
                "year": 2016,
                "regime": "mer",
                "type": "bic",
                "id": "578f97ce48177e9918d071bf"
            }
        ],
        "fraisnotaire": 4350,
        "investId": "56b7b0e348177ee5055932cb",
        "meubles": 0,
        "milliemes": 1000,
        "name": "Pasteur Double Appart",
        "options": {
            "autoPaiement": true
        },
        "place_id": "",
        "place_url": "",
        "prixnet": 83500,
        "prixtoday": 83500,
        "robot": false,
        "sourceFavoriteId": "",
        "surface": 55.6,
        "switchNbImmeubles": "1",
        "travauxr": 0,
        "type": "appartement",
        "user_id": "2",
        "ville": "Angers|49000",
        "ville_niveau1": "",
        "ville_niveau2": "",
        "ville_seule": "Angers",
        "villevalue": "Angers (49000)",
        "imageURL": "https://maps.googleapis.com/maps/api/streetview?size=640x400&location=100+avenue+Pasteur%2C+Angers%2C+France&key=AIzaSyC0bksAgpmpdGIZt4Ed3gj7yEes4RG7TZo",
        "id": "56b7b15a48177ea7085932cd"
    },
    "result": true,
    "code": 0
}
```

### HTTP Request

`POST https://www.rendementlocatif.com/api/gestion/21/locataires/<immeubleIdOrbienId>/<limit>`

"immeubleIdOrbienId" can be either the ID of a bien or the ID of an immeuble. The API will figure out which kinf it is.

The "limit" parameter is the max number of locataires to return. By default (if not present), it returns 10 locataires.

### Parameters

Should be sent with the request as "application/x-www-form-urlencoded".

Parameter | Description
--------- | -------
username | The user name or email address
password | The user password


## Locataire

Gets the details of a "locataire" of a user.

> Code to get the bien of a user

```shell
curl --data "username=username&password=password"
"https://www.rendementlocatif.com/api/gestion/21/locataire/<locataireId>"
  -H "X-API-KEY: secretkey"
```

> Returns the following JSON:

```json
{
    "locataire": {
        "nom": "Ouriele",
        "prenom": "Braque",
        "precision": "",
        "email": "",
        "loyer": 350,
        "provision": 25,
        "indiceRevision": 3,
        "bienId": "56b7b15a48177ea7085932ce",
        "signatureDate": {
            "sec": 1465250400,
            "usec": 0
        },
        "entryDate": {
            "sec": 1466805600,
            "usec": 0
        },
        "quitDate": null,
        "dayLoyer": 1,
        "phone": "",
        "typeLocation": 0,
        "address1": "",
        "address2": "",
        "ville": "",
        "codepostal": "",
        "country": "",
        "bornDate": null,
        "bornLocation": "",
        "nationality": "",
        "user_id": "2",
        "immeubleId": "56b7b15a48177ea7085932cd",
        "dateCreated": {
            "sec": 1466328572,
            "usec": 724000
        },
        "robot": false,
        "id": "576665fc48177efe118b4567",
        "loyerLast": 350,
        "provisionLast": 25
    },
    "result": true,
    "code": 0
}
```

### HTTP Request

`POST https://www.rendementlocatif.com/api/gestion/21/locataire/<locataireId>`

### Parameters

Should be sent with the request as "application/x-www-form-urlencoded".

Parameter | Description
--------- | -------
username | The user name or email address
password | The user password


## Operations

Gets the last n operations of a "immeuble" of a user.

> Code to get the bien of a user

```shell
curl --data "username=username&password=password"
"https://www.rendementlocatif.com/api/gestion/21/operations/<immeubleId>/<limit>"
  -H "X-API-KEY: secretkey"
```

> Returns the following JSON:

```json
{
  "countAll": 3,
  "count": 3,
  "operations": [
    {
      "dateCreated": {
        "sec": 1473612496,
        "usec": 372000
      },
      "immeubleId": "56b7b15a48177ea7085932cd",
      "locataireId": "56b7b52348177ea2085932cc",
      "operationAuto": false,
      "operationDate": {
        "sec": 1473026400,
        "usec": 0
      },
      "operationDesc": "Créée du compte bancaire BOURSORAMA ESSENTIEL",
      "operationMontant": 135,
      "operationPaid": true,
      "robot": true,
      "user_id": "2",
      "id": "57d58ad048177e4f1b8b4567",
      "operationType": {
        "id": "recetteLoyer",
        "name": "Recette Loyers Hors Charges",
        "positive": true
      },
      "locataire": {
        "id": "56b7b52348177ea2085932cc",
        "nom": "Faral",
        "prenom": "Jordan"
      }
    },
    {
      "dateCreated": {
        "sec": 1473612496,
        "usec": 373000
      },
      "immeubleId": "56b7b15a48177ea7085932cd",
      "locataireId": "56b7b52348177ea2085932cc",
      "operationAuto": false,
      "operationDate": {
        "sec": 1473026400,
        "usec": 0
      },
      "operationDesc": "Créée du compte bancaire BOURSORAMA ESSENTIEL",
      "operationMontant": 15,
      "operationPaid": true,
      "robot": true,
      "user_id": "2",
      "id": "57d58ad048177e4f1b8b4568",
      "operationType": {
        "id": "recetteLoyerProvision",
        "name": "Recette Provision Charges Locataire",
        "positive": true
      },
      "locataire": {
        "id": "56b7b52348177ea2085932cc",
        "nom": "Faral",
        "prenom": "Jordan"
      }
    },
    {
      "dateCreated": {
        "sec": 1473612496,
        "usec": 375000
      },
      "immeubleId": "56b7b15a48177ea7085932cd",
      "locataireId": "56b7b52348177ea2085932cc",
      "operationAuto": false,
      "operationDate": {
        "sec": 1473026400,
        "usec": 0
      },
      "operationDesc": "Créée du compte bancaire BOURSORAMA ESSENTIEL",
      "operationMontant": 165,
      "operationPaid": true,
      "robot": true,
      "user_id": "2",
      "id": "57d58ad048177e4f1b8b4569",
      "operationType": {
        "id": "recetteCaf",
        "name": "Recette CAF (APL/ACL)",
        "positive": true
      },
      "locataire": {
        "id": "56b7b52348177ea2085932cc",
        "nom": "Faral",
        "prenom": "Jordan"
      }
    }
  ]
}
```

### HTTP Request

`POST https://www.rendementlocatif.com/api/gestion/21/operations/<immeubleId>/<limit>`

The "limit" parameter is the max number of operations to return. By default (if not present), it returns 1000 operations.

### Parameters


Should be sent with the request as "application/x-www-form-urlencoded".

Parameter | Description
--------- | -------
username | The user name or email address
password | The user password


## Save an Operation

Creates a new operation or update an existing operation (if operationId is given in the path).

> Code to get the bien of a user

```shell
curl --data "username=username&password=password"
"https://www.rendementlocatif.com/api/gestion/21/saveOperation/<immeubleId>/<operationId>"
  -H "X-API-KEY: secretkey"
```

> Returns the following JSON:

```json
{
  "message": "Operation ajoutée",
  "result": true,
  "operationId": "5809db0b48177ed5098b4568",
  "code": 0
}
```

### HTTP Request

`POST https://www.rendementlocatif.com/api/gestion/21/saveOperation/<immeubleId>/<operationId>`

"immeubleId" is mandatory and should correspond to the id of the immeuble to which the operation will be attached.

"operationId" is optional. If given, it tells the API to update the operation with id <operationId> instead of adding a new operation.
If "operationId" is incorrect or empty, the operation will be added as a new one by default.

### Parameters

Should be sent with the request in the body as "application/x-www-form-urlencoded".

Parameter | Description | Type
--------- | ------- | -------
username | The user name or email address | string
password | The user password | string
operationName | The type of the operation from a predefined list | string
operationMontant | The operation amount (always positive) | float
locataireId | The Id of the locataire if required (depend on the operation type) | string
operationDate | The date of the operation (format should be unix timestamp) | integer (unix timestamp)
operationPaid | Operations was paid or not ( "on" to set as paid, anything else would mark it not paid) | string
operationAuto | Operations has to be repeated automatically in the future ( "on" to set as enabled, anything else would mark it not enabled) | string
operationDesc | A description the user can freely add to the operation | string
operationMontantPC | In the case the type of the operation (operationName) is 'recetteLoyer' then this can be added so to create an operation of type 'recetteLoyerProvision' at the same time. The reason is that a "locataire" will usually pay both recetteLoyer and recetteLoyerProvision at the same time. The UI of the client app should allow the user to fill in both things at the same time and help the user split the amount reveived from the "locataire" in both 'recetteLoyer' and 'recetteLoyerProvision' | string
operationMontantTEOM | The amount of "Taxe ordure menagère" (TEOM) to be added as new field in the case the operation is of type "chargeTF" (charge taxe foncière) | float
empruntId | the index of the emprunt of the immeuble (retreived from /immeuble or /immeubles in "emprunt" field ) | integer
operationRepeatDate | A max date for repeating the operation creation several time until that date. This date should be at the minimum equal to (operationDate + 1 month) and has no maximum except current date. This field should be displayed only if operationDate is set and at the maximum 1 month in the past compared to current dateTime | integer (unix timestamp)
dateDebut | start date for the regularisation of charges. Should be displayed and used only if (operationName (type) = recetteReguLocataire) or (operationName (type) = chargeReguLocataire) | integer (unix timestamp)
dateFin | end date for the regularisation of charges. Should be displayed and used only if (operationName (type) = recetteReguLocataire) or (operationName (type) = chargeReguLocataire) | integer (unix timestamp)



Example of body with parameters:

username:bob<br/>
password:bobpassword<br/>
operationName:recetteLoyer<br/>
operationMontant:9999<br/>
locataireId:56a7b72548177e22348b4569<br/>
operationDate:1482447600<br/>
operationPaid:on<br/>
operationAuto:off<br/>


## Set Locataire quit Date

Set the the locataire departure date.

> Code to get the bien of a user

```shell
curl --data "username=username&password=password"
"https://www.rendementlocatif.com/api/gestion/21/setLocataireGone/<locataireId>"
  -H "X-API-KEY: secretkey"
```

> <locataireId> is mandatory and is the id of the locataire for which the quit date needs to be updated
> Returns the following JSON:

>success

```json
{
  "result": true,
  "locataireId": "5729f23b48177eba408b456a",
  "code": 0
}
```

>error

```json
{
  "result": false,
  "code": -23,
  "message": "La date de départ doit être postérieure à la date d'entrée"
}
```

```json
{
  "result": false,
  "code": -21,
  "message": "La date de départ du locataire est invalide."
}
```

### HTTP Request

`POST https://www.rendementlocatif.com/api/gestion/21/setLocataireGone/<locataireId>`

"locataireId" is mandatory and should correspond to the id of the lcoataire.

### Parameters

Should be sent with the request in the body as "application/x-www-form-urlencoded".

Parameter | Mandatory | Description | Type
--------- | ------- | ------- | -------
username | yes | The user name or email address | string
password | yes | The user password | string
quitDate | no | Timestamp of leaving the bien | integer (1422745200)

Example of body with parameters:

username:bob<br/>
password:bobpassword<br/>
quitDate:1422745200<br/>

```shell
curl --request POST \
  --url https://www.rendementlocatif.com/api/gestion/21/setLocataireGone/580a81d348177eb0128b4568 \
  --header 'cache-control: no-cache' \
  --header 'content-type: application/x-www-form-urlencoded' \
  --header 'x-api-key: secretkey' \
  --data 'username=boss&password=password&quitDate=1422745200'
```

## Split Loyer

Splits a loyer paid by a "locataire" in loyer Hors Charges and Provision pour charge for a locataire.

> Code to split a loyer in 2 operations for a locataire

```shell
curl --data "username=username&password=password"
"https://www.rendementlocatif.com/api/gestion/21/ventilationLoyer/<locataireId>/<montantTotal>"
  -H "X-API-KEY: secretkey"
```

> Returns the following JSON:

```json
{
  "currentLoyer": 300,
  "currentProvision": 15,
  "restantAPayer": 150,
  "caf": 165,
  "montantTotal": 400,
  "loyerHC": 385,
  "provision": 15,
  "result": true,
  "code": 0
}
```

### HTTP Request

`POST https://www.rendementlocatif.com/api/gestion/21/ventilationLoyer/<locataireId>/<montantTotal>`

"locataireId" is mandatory and is the id of the locataire from which the "montantTotal" was received.

"montantTotal" is the amount of money received from a locataire for a loyer. This API method will split that amount into loyer hors charges and provision pour charges for this locataire depending on his contract (bail).

### Parameters

Should be sent with the request in the body as "application/x-www-form-urlencoded".

Parameter | Description | Type
--------- | ------- | -------
username | The user name or email address | string
password | The user password | string

### Returned parameters

Parameter | Type | Description
--------- | ------- | -------
result | boolean | true if operation was marked successfully otherwise false
code | integer | 0 if ok otherwise error code of error
loyerHC | float | <b>result of the split:</b> the loyer HC to be used for the operationMontant field
provision | float | <b>result of the split:</b> the provision to be used for the operationMontant field
currentLoyer | float | the current loyer owned by the locataire (hors charge)
currentProvision | float | the current provision pour charges owned by the locataire
caf | float | the amount of caf received by the "proprietaire" that should not be paid by the locataire
montantTotal | float | the total amount paid by the locataire as provided the API call url path
message | string | error message if error


## Save a Locataire

Creates a new locataire or update an existing locataire (if locataireId is given in the path).

> Code to get the bien of a user

```shell
curl --data "username=username&password=password"
"https://www.rendementlocatif.com/api/gestion/21/saveLocataire/<bienId>/<locataireId>"
  -H "X-API-KEY: secretkey"
```

> <bienId> is mandatory and is the id of the bien to which the locataire will be attached
> <locataireId> is optional and is the id of the locataire if
> Returns the following JSON:

>success

```json
{
  "result": true,
  "locataireId": "5846e1e948177e30128b4567",
  "code": 0
}
```

>error

```json
{
  "result": false,
  "code": -21,
  "message": "Some fields are incorrect or missing (see error for more details).",
  "error": "\"Date d'entreé\" est obligatoire.\n"
}
```

### HTTP Request

`POST https://www.rendementlocatif.com/api/gestion/21/saveLocataire/<bienId>/<locataireId>`

"bienId" is mandatory and should correspond to the id of the bien to which the locataire will be attached.

"locataireId" is optional. If given, it tells the API to update the locataire with id <locataireId> instead of adding a new locataire.
If "locataireId" is incorrect or empty, the operation will be added as a new one by default.

### Parameters

Should be sent with the request in the body as "application/x-www-form-urlencoded".

Parameter | Mandatory | Description | Type
--------- | ------- | ------- | -------
username | yes | The user name or email address | string
password | yes | The user password | string
nom | yes | Last name of locataire | string
prenom | yes | first name of locataire | string
precision| no | More details about the locataire | string
email | no | Email of the locataire | string (email format)
loyer | yes | Loyer to be paid by the locataire | float
provision | yes | Provision pour charges to be paid by the locataire | float
indiceRevision | yes | Numéro du trimestre de révision pour charges | enum (0=>'Non précisé',1=>'1er trimestre', 2=>'2ème trimestre', 3=>'3ème trimestre', 4=>'4ème trimestre')
signatureDate | yes | Timestamp of contract signature (signature bail) | integer (1422745200)
entryDate | yes| Timestamp of entry of the locataire | integer (1422745200)
quitDate | no | Timestamp of leaving the bien | integer (1422745200)
dayLoyer | yes | Month day at which the loyer should be paid | integer (1 to 28)
phone | no | phone number of the locataire | string (+33xxxxxxxxx)
typeLocation | yes | type of the location (nu ou meublé) | integer (0=nu, 1=meublé)
depotGarantie | yes | Durée en mois du dépot de garantie | integer (1="1 mois", 2="2 mois (possible en meublé)")
address1 | no | First line of locataire adresse | string
address2 | no | Second line of locataire adresse | string
ville | no | Ville of the locataire | string
codepostal | no | Code postal of the locataire | string
country | no | Country of the locataire | string
bornDate | no | born Timestamp of the locataire | integer (1422745200)
bornLocation | no | born location of the locataire | string
nationality | no | Nationality of the locataire | string


Example of body with parameters:

username:bob<br/>
password:bobpassword<br/>
nom:Dupont<br/>
prenom:Jean<br/>
loyer:666<br/>
provision:20<br/>
indiceRevision:0<br/>
signatureDate:1422745200<br/>
entryDate:1422745200<br/>
dayLoyer:3<br/>
typeLocation:0<br/>
depotGarantie:1<br/>

```shell
curl --request POST \
  --url https://www.rendementlocatif.com/api/gestion/21/saveLocataire/580a81d348177eb0128b4568 \
  --header 'cache-control: no-cache' \
  --header 'content-type: application/x-www-form-urlencoded' \
  --header 'x-api-key: secretkey' \
  --data 'username=boss&password=password&nom=Dupont&prenom=jean&loyer=666&provision=20&indiceRevision=0&signatureDate=1422745200&entryDate=1422745200&dayLoyer=3&typeLocation=0&depotGarantie=1'
```

## Split Loyer

Splits a loyer paid by a "locataire" in loyer Hors Charges and Provision pour charge for a locataire.

> Code to split a loyer in 2 operations for a locataire

```shell
curl --data "username=username&password=password"
"https://www.rendementlocatif.com/api/gestion/21/ventilationLoyer/<locataireId>/<montantTotal>"
  -H "X-API-KEY: secretkey"
```

> Returns the following JSON:

```json
{
  "currentLoyer": 300,
  "currentProvision": 15,
  "restantAPayer": 150,
  "caf": 165,
  "montantTotal": 400,
  "loyerHC": 385,
  "provision": 15,
  "result": true,
  "code": 0
}
```

### HTTP Request

`POST https://www.rendementlocatif.com/api/gestion/21/ventilationLoyer/<locataireId>/<montantTotal>`

"locataireId" is mandatory and is the id of the locataire from which the "montantTotal" was received.

"montantTotal" is the amount of money received from a locataire for a loyer. This API method will split that amount into loyer hors charges and provision pour charges for this locataire depending on his contract (bail).

### Parameters

Should be sent with the request in the body as "application/x-www-form-urlencoded".

Parameter | Description | Type
--------- | ------- | -------
username | The user name or email address | string
password | The user password | string

### Returned parameters

Parameter | Type | Description
--------- | ------- | -------
result | boolean | true if operation was marked successfully otherwise false
code | integer | 0 if ok otherwise error code of error
loyerHC | float | <b>result of the split:</b> the loyer HC to be used for the operationMontant field
provision | float | <b>result of the split:</b> the provision to be used for the operationMontant field
currentLoyer | float | the current loyer owned by the locataire (hors charge)
currentProvision | float | the current provision pour charges owned by the locataire
caf | float | the amount of caf received by the "proprietaire" that should not be paid by the locataire
montantTotal | float | the total amount paid by the locataire as provided the API call url path
message | string | error message if error

## Mark Operation Paid

Marks an operation as paid

> Code to get the bien of a user

```shell
curl --data "username=username&password=password"
"https://www.rendementlocatif.com/api/gestion/21/markOperationPaid/<operationId>"
  -H "X-API-KEY: secretkey"
```

> Returns the following JSON:

```json
{
  "result": true,
  "code": 0
}
```

### HTTP Request

`POST https://www.rendementlocatif.com/api/gestion/21/markOperationPaid/<operationId>`

"operationId" is mandatory and should correspond to the id of the operation to mark as paid.

### Parameters

Should be sent with the request in the body as "application/x-www-form-urlencoded".

Parameter | Description | Type
--------- | ------- | -------
username | The user name or email address | string
password | The user password | string

### Returned parameters

Parameter | Type | Description
--------- | ------- | -------
result | boolean | true if operation was marked successfully otherwise false
code | integer | 0 if ok otherwise error code of error
message | string | error message if error

## Mark Operation not Paid

Marks an operation as paid

> Code to get the bien of a user

```shell
curl --data "username=username&password=password"
"https://www.rendementlocatif.com/api/gestion/21/markOperationUnPaid/<operationId>"
  -H "X-API-KEY: secretkey"
```

> Returns the following JSON:

```json
{
  "result": true,
  "code": 0
}
```

### HTTP Request

`POST https://www.rendementlocatif.com/api/gestion/21/markOperationUnPaid/<operationId>`

"operationId" is mandatory and should correspond to the id of the operation to mark as unpaid.

### Parameters

Should be sent with the request in the body as "application/x-www-form-urlencoded".

Parameter | Description | Type
--------- | ------- | -------
username | The user name or email address | string
password | The user password | string

### Returned parameters

Parameter | Type | Description
--------- | ------- | -------
result | boolean | true if operation was marked successfully otherwise false
code | integer | 0 if ok otherwise error code of error
message | string | error message if error


## Delete an Operation

Deletes an operation

> Code to get the bien of a user

```shell
curl --data "username=username&password=password"
"https://www.rendementlocatif.com/api/gestion/21/deleteOperation/<operationId>"
  -H "X-API-KEY: secretkey"
```

> Returns the following JSON:

```json
{
  "result": true,
  "code": 0
}
```

### HTTP Request

`POST https://www.rendementlocatif.com/api/gestion/21/deleteOperation/<operationId>`

"operationId" is mandatory and should correspond to the id of the operation to delete.

### Parameters

Should be sent with the request in the body as "application/x-www-form-urlencoded".

Parameter | Description | Type
--------- | ------- | -------
username | The user name or email address | string
password | The user password | string

### Returned parameters

Parameter | Type | Description
--------- | ------- | -------
result | boolean | true if operation was deleted successfully otherwise false
code | integer | 0 if ok otherwise error code of error
message | string | error message if error

## Quittance

Gets a "quittance de loyer"

> Code to get the bien of a user

```shell
curl --data "username=username&password=password"
"https://www.rendementlocatif.com/api/gestion/21/quittance/<locataireId>/<anythingOnlyToSendEmail>"
  -H "X-API-KEY: secretkey"
```

> If success, it returns the PDF file (streamed over HTTP)
> Otherwise it returns an error as a json

```json
{
  "result": false,
  "code": -10,
  "message": "Missing or invalid locataireId (in url path)"
}
```

> If success and email send mode, it always returns a json

```json
{
  "result": true,
  "code": 0,
  "message": "Un email avec le document a été envoyé à votre locataire."
}
```

> if error when seding email


```json
{
  "result": false,
  "code": -15,
  "message": "Message non envoyé. Vous devez renseigner une adresse email pour votre locataire."
}
```

### HTTP Request

`POST https://www.rendementlocatif.com/api/gestion/21/quittance/<locataireId>/<anythingToSendEmail>`

"locataireId" is mandatory and should correspond to the id of the locataire for which we want to edit a quittance.

"anythingOnlyToSendEmail" is optional.  Can be any non empty string. If given, the server will send an email to the locataire with the quittance PDF attached.

### Parameters

Should be sent with the request in the body as "application/x-www-form-urlencoded".

Parameter | Mandatory | Description | Type | default
--------- | ------- | ------- | ------- | -------
username | yes | The user name or email address | string |
password | yes | The user password | string |
startDate | no | The start date (timestamp) for the quittance | integer | first day of current month
endDate | no | The end date (timestamp) for the quittance | integer | last day of current month
startSolde | no | The start date (timestamp) for the solde| integer | first day of month of locataire entry
ignoreSolde | no | Tells to ignore the solde calculation if equals "on" or take it into account if equals off | string | 'on'

### Returned parameters

Parameter | Type | Description
--------- | ------- | -------
result | boolean | true if doc is generated ok or if email send ok, otherwise false
code | integer | 0 if ok otherwise error code of error
message | string | error message if error, ok message of code=0 (success)


## Bail

Gets a "contrat de bail"

> Code to get the bien of a user

```shell
curl --data "username=username&password=password"
"https://www.rendementlocatif.com/api/gestion/21/bail/<locataireId>/<type>/<empty>/<collocation>/<anythingToSendEmail>"
  -H "X-API-KEY: secretkey"
```

> If success, it returns the PDF file (streamed over HTTP)
> Otherwise it returns an error as a json

```json
{
  "result": false,
  "code": -10,
  "message": "Missing or invalid locataireId (in url path)"
}
```

> If success and email send mode, it always returns a json

```json
{
  "result": true,
  "code": 0,
  "message": "Un email avec le document a été envoyé à votre locataire."
}
```

> if error when seding email


```json
{
  "result": false,
  "code": -15,
  "message": "Message non envoyé. Vous devez renseigner une adresse email pour votre locataire."
}
```

### HTTP Request

`POST https://www.rendementlocatif.com/api/gestion/21/bail/<locataireId>/<type>/<empty>/<collocation>/<anythingToSendEmail>`

"locataireId" is mandatory and should correspond to the id of the locataire for which we want to edit the bail.

"type" (optional) is either "m" (bail meublé) or "nu" (bail nu, default)

"empty" (optional) can be anything to produce an empty bail document (not pre-filled with owner and locataire personal details). Set to "skip" to keep the document pre-filled and set another next parameter in the URL (collocation and send_email parameters).

"collocation" (optional) can be anything and will generate a bail with all currently active locataires (pour collocation). Set to "skip" to keep the document not empty and set another next parameter in the URL.

"anythingOnlyToSendEmail" is optional.  Can be any non empty string. If given, the server will send an email to the locataire with the bail PDF attached.

### POST Parameters

Should be sent with the request in the body as "application/x-www-form-urlencoded".

Parameter | Mandatory | Description | Type | default
--------- | ------- | ------- | ------- | -------
username | yes | The user name or email address | string |
password | yes | The user password | string |


### Returned parameters

Parameter | Type | Description
--------- | ------- | -------
result | boolean | true if doc is generated ok or if email send ok, otherwise false
code | integer | 0 if ok otherwise error code of error
message | string | error message if error, ok message of code=0 (success)


Examples:

Send a meublé pre-filled bail document to locataire "5845a17f48177ea2098b4567" (no collocation)

```shell
curl --request POST \
  --url https://www.rendementlocatif.com/api/gestion/21/bail/5845a17f48177ea2098b4567/m/skip/skip/send \
  --header 'cache-control: no-cache' \
  --header 'content-type: application/x-www-form-urlencoded' \
  --header 'x-api-key: secretkey' \
  --data 'username=boss&password=password'
```


## Etat des lieux

Gets a "etat des lieux" document.

> Code to get the bien of a user

```shell
curl --data "username=username&password=password"
"https://www.rendementlocatif.com/api/gestion/21/etatDesLieux/<locataireId>/<type>/<empty>/<collocation>/<anythingToSendEmail>"
  -H "X-API-KEY: secretkey"
```

> If success, it returns the PDF file (streamed over HTTP)
> Otherwise it returns an error as a json

```json
{
  "result": false,
  "code": -10,
  "message": "Missing or invalid locataireId (in url path)"
}
```

> If success and email send mode, it always returns a json

```json
{
  "result": true,
  "code": 0,
  "message": "Un email avec le document a été envoyé à votre locataire."
}
```

> if error when seding email


```json
{
  "result": false,
  "code": -15,
  "message": "Message non envoyé. Vous devez renseigner une adresse email pour votre locataire."
}
```

### HTTP Request

`POST https://www.rendementlocatif.com/api/gestion/21/etatDesLieux/<locataireId>/<type>/<empty>/<collocation>/<anythingToSendEmail>`

"locataireId" is mandatory and should correspond to the id of the locataire for which we want to edit the document.

"type" (optional) is either "e" (Etat des lieux d'entrée, default) or "s" (état des lieux de sortie)

"empty" (optional) can be anything to produce an empty document (not pre-filled with owner and locataire personal details). Set this to "skip" to keep the document pre-filled and set another next parameter in the URL.

"collocation" (optional) can be anything and will generate a document with all currently active locataires (pour collocation). Set to "skip" to keep the document not empty and set another next parameter in the URL.

"anythingOnlyToSendEmail" is optional.  Can be any non empty string. If given, the server will send an email to the locataire with the document PDF attached.

### POST Parameters

Should be sent with the request in the body as "application/x-www-form-urlencoded".

Parameter | Mandatory | Description | Type | default
--------- | ------- | ------- | ------- | -------
username | yes | The user name or email address | string |
password | yes | The user password | string |


### Returned parameters

Parameter | Type | Description
--------- | ------- | -------
result | boolean | true if doc is generated ok or if email send ok, otherwise false
code | integer | 0 if ok otherwise error code of error
message | string | error message if error, ok message of code=0 (success)


Examples:

Send a pre-filled document "etat de lieux de sortie" to locataire "5845a17f48177ea2098b4567"

```shell
curl --request POST \
  --url https://www.rendementlocatif.com/api/gestion/21/etatDesLieux/5845a17f48177ea2098b4567/s/skip/skip/send \
  --header 'cache-control: no-cache' \
  --header 'content-type: application/x-www-form-urlencoded' \
  --header 'x-api-key: secretkey' \
  --data 'username=boss&password=password'
```

## Liste avant Départ

Gets a "liste choses à faire avant départ" document.

> Code to get the bien of a user

```shell
curl --data "username=username&password=password"
"https://www.rendementlocatif.com/api/gestion/21/avantDepart/<locataireId>/<empty>/<anythingToSendEmail>"
  -H "X-API-KEY: secretkey"
```

> If success, it returns the PDF file (streamed over HTTP)
> Otherwise it returns an error as a json

```json
{
  "result": false,
  "code": -10,
  "message": "Missing or invalid locataireId (in url path)"
}
```

> If success and email send mode, it always returns a json

```json
{
  "result": true,
  "code": 0,
  "message": "Un email avec le document a été envoyé à votre locataire."
}
```

> if error when seding email


```json
{
  "result": false,
  "code": -15,
  "message": "Message non envoyé. Vous devez renseigner une adresse email pour votre locataire."
}
```

### HTTP Request

`POST https://www.rendementlocatif.com/api/gestion/21/avantDepart/<locataireId>/<empty>/<anythingToSendEmail>`

"locataireId" is mandatory and should correspond to the id of the locataire for which we want to edit the document.

"empty" (optional) can be anything to produce an empty document (not pre-filled with owner and locataire personal details). Set this to "skip" to keep the document pre-filled and set another next parameter in the URL.

"anythingOnlyToSendEmail" is optional.  Can be any non empty string. If given, the server will send an email to the locataire with the document PDF attached.

### POST Parameters

Should be sent with the request in the body as "application/x-www-form-urlencoded".

Parameter | Mandatory | Description | Type | default
--------- | ------- | ------- | ------- | -------
username | yes | The user name or email address | string |
password | yes | The user password | string |


### Returned parameters

Parameter | Type | Description
--------- | ------- | -------
result | boolean | true if doc is generated ok or if email send ok, otherwise false
code | integer | 0 if ok otherwise error code of error
message | string | error message if error, ok message of code=0 (success)


Examples:

Send a pre-filled document to locataire "5845a17f48177ea2098b4567"

```shell
curl --request POST \
  --url https://www.rendementlocatif.com/api/gestion/21/avantDepart/5845a17f48177ea2098b4567/skip/send \
  --header 'cache-control: no-cache' \
  --header 'content-type: application/x-www-form-urlencoded' \
  --header 'x-api-key: secretkey' \
  --data 'username=boss&password=password'
```


## Caution Solidaire

Gets a "acte de cautionnement solidaire" document.

> Code to get the bien of a user

```shell
curl --data "username=username&password=password"
"https://www.rendementlocatif.com/api/gestion/21/cautionSolidaire/<locataireId>/<empty>/<collocation>/<anythingToSendEmail>"
  -H "X-API-KEY: secretkey"
```

> If success, it returns the PDF file (streamed over HTTP)
> Otherwise it returns an error as a json

```json
{
  "result": false,
  "code": -10,
  "message": "Missing or invalid locataireId (in url path)"
}
```

> If success and email send mode, it always returns a json

```json
{
  "result": true,
  "code": 0,
  "message": "Un email avec le document a été envoyé à votre locataire."
}
```

> if error when seding email


```json
{
  "result": false,
  "code": -15,
  "message": "Message non envoyé. Vous devez renseigner une adresse email pour votre locataire."
}
```

### HTTP Request

`POST https://www.rendementlocatif.com/api/gestion/21/etatDesLieux/<locataireId>/<empty>/<collocation>/<anythingToSendEmail>`

"locataireId" is mandatory and should correspond to the id of the locataire for which we want to edit the document.


"empty" (optional) can be anything to produce an empty document (not pre-filled with owner and locataire personal details). Set this to "skip" to keep the document pre-filled and set another next parameter in the URL.

"collocation" (optional) can be anything and will generate a document with all currently active locataires (pour collocation). Set to "skip" to keep the document not empty and set another next parameter in the URL.

"anythingOnlyToSendEmail" is optional.  Can be any non empty string. If given, the server will send an email to the locataire with the document PDF attached.

### POST Parameters

Should be sent with the request in the body as "application/x-www-form-urlencoded".

Parameter | Mandatory | Description | Type | default
--------- | ------- | ------- | ------- | -------
username | yes | The user name or email address | string |
password | yes | The user password | string |


### Returned parameters

Parameter | Type | Description
--------- | ------- | -------
result | boolean | true if doc is generated ok or if email send ok, otherwise false
code | integer | 0 if ok otherwise error code of error
message | string | error message if error, ok message of code=0 (success)


Examples:

Send a pre-filled document "caution solidaire" to locataire "5845a17f48177ea2098b4567"

```shell
curl --request POST \
  --url https://www.rendementlocatif.com/api/gestion/21/cautionSolidaire/5845a17f48177ea2098b4567/skip/skip/send \
  --header 'cache-control: no-cache' \
  --header 'content-type: application/x-www-form-urlencoded' \
  --header 'x-api-key: secretkey' \
  --data 'username=boss&password=password'
```


## Taxe ordures ménagères

Gets a "rappel paiement taxe enlèvement des ordures ménagères" document.

> Code to get the bien of a user

```shell
curl --data "username=username&password=password"
"https://www.rendementlocatif.com/api/gestion/21/taxeOrduresMenageres/<locataireId>/<total>/<anythingToSendEmail>"
  -H "X-API-KEY: secretkey"
```

> If success, it returns the PDF file (streamed over HTTP)
> Otherwise it returns an error as a json or success if sent by email

```json
{
  "result": false,
  "code": -10,
  "message": "Missing or invalid locataireId (in url path)"
}
```

> If success and email send mode, it always returns a json

```json
{
  "result": true,
  "code": 0,
  "message": "Un email avec le document a été envoyé à votre locataire."
}
```

> if error when seding email


```json
{
  "result": false,
  "code": -15,
  "message": "Message non envoyé. Vous devez renseigner une adresse email pour votre locataire."
}
```

### HTTP Request

`POST https://www.rendementlocatif.com/api/gestion/21/taxeOrduresMenageres/<locataireId>/<total>/<anythingToSendEmail>`

"locataireId" is mandatory and should correspond to the id of the locataire for which we want to edit the document.

"total" (optional) is a number to set the amount due for the tax in the document. By default it is set to 0.

"anythingOnlyToSendEmail" is optional.  Can be any non empty string. If given, the server will send an email to the locataire with the document PDF attached.

### POST Parameters

Should be sent with the request in the body as "application/x-www-form-urlencoded".

Parameter | Mandatory | Description | Type | default
--------- | ------- | ------- | ------- | -------
username | yes | The user name or email address | string |
password | yes | The user password | string |


### Returned parameters

Parameter | Type | Description
--------- | ------- | -------
result | boolean | true if doc is generated ok or if email send ok, otherwise false
code | integer | 0 if ok otherwise error code of error
message | string | error message if error, ok message of code=0 (success)


Examples:

Send a pre-filled document "taxe ordures ménagères" to locataire "5845a17f48177ea2098b4567" with amount due 34€ (set in the document).

```shell
curl --request POST \
  --url https://www.rendementlocatif.com/api/gestion/21/taxeOrduresMenageres/5845a17f48177ea2098b4567/34/send \
  --header 'cache-control: no-cache' \
  --header 'content-type: application/x-www-form-urlencoded' \
  --header 'x-api-key: secretkey' \
  --data 'username=boss&password=password'
```


## Relance Loyer impayé

Gets a "relance loyers impayés" document.

> Code to get the bien of a user

```shell
curl --data "username=username&password=password"
"https://www.rendementlocatif.com/api/gestion/21/loyerImpaye/<locataireId>/<total>/<anythingToSendEmail>"
  -H "X-API-KEY: secretkey"
```

> If success, it returns the PDF file (streamed over HTTP)
> Otherwise it returns an error as a json or success if sent by email

```json
{
  "result": false,
  "code": -10,
  "message": "Missing or invalid locataireId (in url path)"
}
```

> If success and email send mode, it always returns a json

```json
{
  "result": true,
  "code": 0,
  "message": "Un email avec le document a été envoyé à votre locataire."
}
```

> if error when seding email


```json
{
  "result": false,
  "code": -15,
  "message": "Message non envoyé. Vous devez renseigner une adresse email pour votre locataire."
}
```

### HTTP Request

`POST https://www.rendementlocatif.com/api/gestion/21/loyerImpaye/<locataireId>/<total>/<anythingToSendEmail>`

"locataireId" is mandatory and should correspond to the id of the locataire for which we want to edit the document.

"total" (optional) is a number to set the amount due in the document. By default it is set to 0.

"anythingOnlyToSendEmail" is optional.  Can be any non empty string. If given, the server will send an email to the locataire with the document PDF attached.

### POST Parameters

Should be sent with the request in the body as "application/x-www-form-urlencoded".

Parameter | Mandatory | Description | Type | default
--------- | ------- | ------- | ------- | -------
username | yes | The user name or email address | string |
password | yes | The user password | string |


### Returned parameters

Parameter | Type | Description
--------- | ------- | -------
result | boolean | true if doc is generated ok or if email send ok, otherwise false
code | integer | 0 if ok otherwise error code of error
message | string | error message if error, ok message of code=0 (success)


Examples:

Send a pre-filled document "relance loyers impayés" to locataire "5845a17f48177ea2098b4567" with amount due 34€ (set in the document).

```shell
curl --request POST \
  --url https://www.rendementlocatif.com/api/gestion/21/loyerImpaye/5845a17f48177ea2098b4567/34/send \
  --header 'cache-control: no-cache' \
  --header 'content-type: application/x-www-form-urlencoded' \
  --header 'x-api-key: secretkey' \
  --data 'username=boss&password=/21/'
```

## Compte Locataire

Gets the detailled status of a locataire account. Can be returned as data (full data of account and lines) or as a PDF. Can also be sent by email to the locataire (when pdf).

> Code to get the bien of a user

```shell
curl --data "username=username&password=password"
"https://www.rendementlocatif.com/api/gestion/21/compteLocataire/<locataireId>/<mode>/<anythingToSendEmail>"
  -H "X-API-KEY: secretkey"
```

> If success, it returns the PDF file (streamed over HTTP)
> Otherwise it returns an error as a json or success if sent by email

```json
{
  "result": false,
  "code": -10,
  "message": "Missing or invalid locataireId (in url path)"
}
```

> If success and data return (not pdf), it always returns a json like this

```json
{
  "result": true,
  "locataire": {
    "address1": "",
    "address2": "",
    "bienId": "580a81d348177eb0128b4568",
    "bornDate": null,
    "bornLocation": "",
    "codepostal": "",
    "country": "",
    "dateCreated": {
      "sec": 1480958335,
      "usec": 313000
    },
    "dateLastModified": {
      "sec": 1491916791,
      "usec": 704000
    },
    "dayLoyer": 3,
    "depotGarantie": 1,
    "email": "bassouli@gmail.com",
    "entryDate": {
      "sec": 1422745200,
      "usec": 0
    },
    "immeubleId": "580a81d348177eb0128b4567",
    "indiceRevision": 0,
    "loyer": 300,
    "nationality": "",
    "nom": "testAPINom",
    "phone": "",
    "precision": "",
    "prenom": "testAPIPrénom",
    "provision": 30,
    "quitDate": null,
    "robot": false,
    "signatureDate": {
      "sec": 1422745200,
      "usec": 0
    },
    "typeLocation": 0,
    "user_id": "23",
    "ville": "",
    "id": "5845a17f48177ea2098b4567"
  },
  "immeuble": {
    "active": true,
    "address": "5 Rue d'Alençon",
    "address2": "",
    "address_number": "5",
    "codepostal": "75015",
    "comagence": 3816.45,
    "comcourtier": 0,
    "country": "FR",
    "country_long": "France",
    "dateCreated": {
      "sec": 1477083603,
      "usec": 168000
    },
    "dateLastModified": {
      "sec": 1482254485,
      "usec": 553000
    },
    "fraisnotaire": 1688.59,
    "investId": "5729f23b48177eba408b4567",
    "meubles": 277,
    "milliemes": 554,
    "name": "test immeuble 2",
    "options": {
      "autoPaiement": true
    },
    "place_id": "ChIJMR_W1DJw5kcRHxOw3-Nr5DE",
    "place_url": "https://maps.google.com/?q=5+Rue+d'Alençon,+75015+Paris,+France&ftid=0x47e67032d4d61f31:0x31e46be3dfb0131f",
    "prixnet": 47705.55,
    "prixtoday": 47705.55,
    "robot": false,
    "surface": 23,
    "travauxr": 0,
    "type": "immeuble",
    "user_id": "23",
    "ville": "Paris|75015",
    "ville_niveau1": "Île-de-France",
    "ville_niveau2": "Paris",
    "ville_seule": "Paris",
    "villevalue": "Paris (75015)",
    "imageURL": "https://maps.googleapis.com/maps/api/streetview?size=640x400&location=5+Rue+d%27Alen%C3%A7on%2C+Paris%2C+France&key=AIzaSyC0bksAgpmpdGIZt4Ed3gj7yEes4RG7TZo",
    "totalEmprunt": 91915,
    "totalMensualite": 649.15,
    "date_achat": {
      "sec": 1398895200,
      "usec": 0
    },
    "id": "580a81d348177eb0128b4567"
  },
  "amount": -8910,
  "lines": [
    {
      "time": 1422745200,
      "montant": 330,
      "description": "Loyer + Provisions charges",
      "positive": false
    },
    {
      "time": 1425337200,
      "montant": 330,
      "description": "Loyer + Provisions charges",
      "positive": false
    },
    {
      "time": 1428012000,
      "montant": 330,
      "description": "Loyer + Provisions charges",
      "positive": false
    },
    {
      "time": 1430604000,
      "montant": 330,
      "description": "Loyer + Provisions charges",
      "positive": false
    },
    {
      "time": 1433282400,
      "montant": 330,
      "description": "Loyer + Provisions charges",
      "positive": false
    },
    {
      "time": 1435874400,
      "montant": 330,
      "description": "Loyer + Provisions charges",
      "positive": false
    },
    {
      "time": 1438552800,
      "montant": 330,
      "description": "Loyer + Provisions charges",
      "positive": false
    },
    {
      "time": 1441231200,
      "montant": 330,
      "description": "Loyer + Provisions charges",
      "positive": false
    },
    {
      "time": 1443823200,
      "montant": 330,
      "description": "Loyer + Provisions charges",
      "positive": false
    },
    {
      "time": 1446505200,
      "montant": 330,
      "description": "Loyer + Provisions charges",
      "positive": false
    },
    {
      "time": 1449097200,
      "montant": 330,
      "description": "Loyer + Provisions charges",
      "positive": false
    },
    {
      "time": 1451775600,
      "montant": 330,
      "description": "Loyer + Provisions charges",
      "positive": false
    },
    {
      "time": 1454454000,
      "montant": 330,
      "description": "Loyer + Provisions charges",
      "positive": false
    },
    {
      "time": 1456959600,
      "montant": 330,
      "description": "Loyer + Provisions charges",
      "positive": false
    },
    {
      "time": 1459634400,
      "montant": 330,
      "description": "Loyer + Provisions charges",
      "positive": false
    },
    {
      "time": 1462226400,
      "montant": 330,
      "description": "Loyer + Provisions charges",
      "positive": false
    },
    {
      "time": 1464904800,
      "montant": 330,
      "description": "Loyer + Provisions charges",
      "positive": false
    },
    {
      "time": 1467496800,
      "montant": 330,
      "description": "Loyer + Provisions charges",
      "positive": false
    },
    {
      "time": 1470175200,
      "montant": 330,
      "description": "Loyer + Provisions charges",
      "positive": false
    },
    {
      "time": 1472853600,
      "montant": 330,
      "description": "Loyer + Provisions charges",
      "positive": false
    },
    {
      "time": 1475445600,
      "montant": 330,
      "description": "Loyer + Provisions charges",
      "positive": false
    },
    {
      "time": 1478127600,
      "montant": 330,
      "description": "Loyer + Provisions charges",
      "positive": false
    },
    {
      "time": 1480719600,
      "montant": 330,
      "description": "Loyer + Provisions charges",
      "positive": false
    },
    {
      "time": 1483398000,
      "montant": 330,
      "description": "Loyer + Provisions charges",
      "positive": false
    },
    {
      "time": 1486076400,
      "montant": 330,
      "description": "Loyer + Provisions charges",
      "positive": false
    },
    {
      "time": 1488495600,
      "montant": 330,
      "description": "Loyer + Provisions charges",
      "positive": false
    },
    {
      "time": 1491170400,
      "montant": 330,
      "description": "Loyer + Provisions charges",
      "positive": false
    }
  ]
}
```

> If success and email send mode, it always returns a json

```json
{
  "result": true,
  "code": 0,
  "message": "Un email avec le document a été envoyé à votre locataire."
}
```

> if error when seding email


```json
{
  "result": false,
  "code": -15,
  "message": "Message non envoyé. Vous devez renseigner une adresse email pour votre locataire."
}
```

### HTTP Request

`POST https://www.rendementlocatif.com/api/gestion/21/compteLocataire/<locataireId>/<mode>/<anythingToSendEmail>`

"locataireId" is mandatory and should correspond to the id of the locataire for which we want to edit the account status.

"mode" (optional) is a string to set wether or not we want the API to return a PDF document (<mode> = 'pdf' , by default) or data as json (<mode> = 'data').

"anythingOnlyToSendEmail" is optional.  Can be any non empty string. If given, the server will send an email to the locataire with the document PDF attached.

### POST Parameters

Should be sent with the request in the body as "application/x-www-form-urlencoded".

Parameter | Mandatory | Description | Type | default
--------- | ------- | ------- | ------- | -------
username | yes | The user name or email address | string |
password | yes | The user password | string |


### Returned parameters

Parameter | Type | Description
--------- | ------- | -------
result | boolean | true if doc is generated ok or if email send ok, otherwise false
code | integer | 0 if ok otherwise error code of error
locataire | object | details of the locataire for display
immeuble | object | details of the immeuble (to get the adress and everything in case needed for display)
amount | float | total amount due by the locataire (is negative) or due by the landlord (if positive)
lines | array | an array of all the lines of the account to be usually displayed as a table. Every line is an object with "time" = timestamp of operation, "montant" = float, "positive" = false or true depending on whether it is a du amount (dette) or paid amount (by the locataire)
message | string | error message if error, ok message of code=0 (success)


Examples:

Returns the status of the account of locataire 5845a17f48177ea2098b4567 as json data.

```shell
curl --request POST \
  --url https://www.rendementlocatif.com/api/gestion/21/lcompteLocataire/5845a17f48177ea2098b4567/data \
  --header 'cache-control: no-cache' \
  --header 'content-type: application/x-www-form-urlencoded' \
  --header 'x-api-key: secretkey' \
  --data 'username=boss&password=/21/'
```

## Fiche candidature

Gets a "fiche de candidature" document for a new locataire.

> Code to get the bien of a user

```shell
curl --data "username=username&password=password"
"https://www.rendementlocatif.com/api/gestion/21/ficheCandidature/<bienId>/<empty>/<anythingToSendEmail>"
  -H "X-API-KEY: secretkey"
```

> If success, it returns the PDF file (streamed over HTTP)
> Otherwise it returns an error as a json

```json
{
  "result": false,
  "code": -10,
  "message": "Missing or invalid locataireId (in url path)"
}
```

> If success and email send mode, it always returns a json

```json
{
  "result": true,
  "code": 0,
  "message": "Un email avec le document a été envoyé à votre locataire."
}
```

> if error when seding email


```json
{
  "result": false,
  "code": -15,
  "message": "Message non envoyé. Vous devez renseigner une adresse email pour votre locataire."
}
```

### HTTP Request

`POST https://www.rendementlocatif.com/api/gestion/21/etatDesLieux/<bienId>/<empty>/<anythingToSendEmail>`

"bienId" is mandatory and should correspond to the bien id for which we want to edit the document.


"empty" (optional) can be anything to produce an empty document (not pre-filled with owner and locataire personal details). Set this to "skip" to keep the document pre-filled and set another next parameter in the URL.

"anythingOnlyToSendEmail" is optional.  Can be any non empty string. If given, the server will send an email to the locataire with the document PDF attached.

### POST Parameters

Should be sent with the request in the body as "application/x-www-form-urlencoded".

Parameter | Mandatory | Description | Type | default
--------- | ------- | ------- | ------- | -------
username | yes | The user name or email address | string |
password | yes | The user password | string |


### Returned parameters

Parameter | Type | Description
--------- | ------- | -------
result | boolean | true if doc is generated ok or if email send ok, otherwise false
code | integer | 0 if ok otherwise error code of error
message | string | error message if error, ok message of code=0 (success)


Examples:

Send a pre-filled document "fiche candidature" to locataire "580a81d348177eb0128b4568"

```shell
curl --request POST \
  --url https://www.rendementlocatif.com/api/gestion/21/ficheCandidature/580a81d348177eb0128b4568/skip/send \
  --header 'cache-control: no-cache' \
  --header 'content-type: application/x-www-form-urlencoded' \
  --header 'x-api-key: secretkey' \
  --data 'username=boss&password=password'
```



## Any document

Gets other documents as PDF, can also be sent to a locataire by email.
It can be used for documents that already have a specific endpoint (bail, ficheCandidature, etc) but it is not recommanded. Otherwise using the specific endpoint will pre-fill much more detailsand give more options.

> Code to get the bien of a user

```shell
curl --data "username=username&password=password"
"https://www.rendementlocatif.com/api/gestion/21/doc/<docType>/<locataireId>/<anythingToSendEmail>"
  -H "X-API-KEY: secretkey"
```

> If success, it returns the PDF file (streamed over HTTP)
> Otherwise it returns an error as a json

```json
{
  "result": false,
  "code": -10,
  "message": "Missing or invalid docType (in url path, 1st parameter)"
}
```

> If success and email send mode, it always returns a json

```json
{
  "result": true,
  "code": 0,
  "message": "Un email avec le document a été envoyé à votre locataire."
}
```

> if error when seding email


```json
{
  "result": false,
  "code": -15,
  "message": "Message non envoyé. Vous devez renseigner une adresse email pour votre locataire."
}
```

### HTTP Request

`POST https://www.rendementlocatif.com/api/gestion/21/doc/<docType>/<locataireId>/<anythingToSendEmail>`

"docType" is the id of the document to return (string). DocType can be one of :

* listeChargesRecuperables
* listeReparationsLocatives

"locataireId" is mandatory and should correspond to the id of the locataire for which we want to edit the document.

"anythingOnlyToSendEmail" is optional.  Can be any non empty string. If given, the server will send an email to the locataire with the document PDF attached.

### POST Parameters

Should be sent with the request in the body as "application/x-www-form-urlencoded".

Parameter | Mandatory | Description | Type | default
--------- | ------- | ------- | ------- | -------
username | yes | The user name or email address | string |
password | yes | The user password | string |


### Returned parameters

Parameter | Type | Description
--------- | ------- | -------
result | boolean | true if doc is generated ok or if email send ok, otherwise false
code | integer | 0 if ok otherwise error code of error
message | string | error message if error, ok message of code=0 (success)


Examples:

Gets a "liste des charges récupérables" document as PDF.

```shell
curl --request POST \
  --url https://www.rendementlocatif.com/api/gestion/21/doc/listeChargesRecuperables \
  --header 'cache-control: no-cache' \
  --header 'content-type: application/x-www-form-urlencoded' \
  --header 'x-api-key: secretkey' \
  --data 'username=boss&password=password'
```


## Rapport Performance

Retreives all the data to generate a rapport of the user patrimoine immobilier.

> Code to get the bien of a user

```shell
curl --data "username=username&password=password"
"https://www.rendementlocatif.com/api/gestion/21/raport/"
  -H "X-API-KEY: secretkey"
```

> error example

```json
{
  "result": false,
  "code": 25,
  "message": "Aucun rapport disponible pour le moment."
}
```

> If success

```json
{
  "year": 2016,
  "years": [
    2017,
    2016,
    2015,
    2014,
    2013,
    2012,
    2011,
    2010
  ],
  "yearsEmprunt": [],
  "monthsEmprunt": {
    "0": 1401487200,
    "1": 1404079200,
    "2": 1406757600,
    "3": 1409436000,
    "4": 1412028000,
    "5": 1414710000,
    "6": 1417302000,
    "7": 1419980400,
    "8": 1422658800,
    "9": 1425078000,
    "10": 1427752800,
    "11": 1430344800,
    "12": 1433023200,
    "13": 1435615200,
    "14": 1438293600,
    "15": 1440972000,
    "16": 1443564000,
    "17": 1446246000,
    "18": 1448838000,
    "19": 1451516400,
    "20": 1454194800,
    "21": 1456700400,
    "22": 1459375200,
    "23": 1461967200,
    "24": 1464645600,
    "25": 1467237600,
    "26": 1469916000,
    "27": 1472594400,
    "28": 1475186400,
    "29": 1477868400,
    "30": 1480460400,
    "31": 1483138800,
    "32": 1485817200,
    "33": 1488236400,
    "34": 1490911200,
    "35": 1493503200,
    "36": 1496181600,
    "37": 1498773600,
    "38": 1501452000,
    "39": 1504130400,
    "40": 1506722400,
    "41": 1509404400,
    "42": 1511996400,
    "43": 1514674800,
    "44": 1517353200,
    "45": 1519772400,
    "46": 1522447200,
    "47": 1525039200,
    "48": 1527717600,
    "49": 1530309600,
    "50": 1532988000,
    "51": 1535666400,
    "52": 1538258400,
    "53": 1540940400,
    "54": 1543532400,
    "55": 1546210800,
    "56": 1548889200,
    "57": 1551308400,
    "58": 1553986800,
    "59": 1556575200,
    "60": 1559253600,
    "61": 1561845600,
    "62": 1564524000,
    "63": 1567202400,
    "64": 1569794400,
    "65": 1572476400,
    "66": 1575068400,
    "67": 1577746800,
    "68": 1580425200,
    "69": 1582930800,
    "70": 1585605600,
    "71": 1588197600,
    "72": 1590876000,
    "73": 1593468000,
    "74": 1596146400,
    "75": 1598824800,
    "76": 1601416800,
    "77": 1604098800,
    "78": 1606690800,
    "79": 1609369200,
    "80": 1612047600,
    "81": 1614466800,
    "82": 1617141600,
    "83": 1619733600,
    "84": 1622412000,
    "85": 1625004000,
    "86": 1627682400,
    "87": 1630360800,
    "88": 1632952800,
    "89": 1635631200,
    "90": 1638226800,
    "91": 1640905200,
    "92": 1643583600,
    "93": 1646002800,
    "94": 1648677600,
    "95": 1651269600,
    "96": 1653948000,
    "97": 1656540000,
    "98": 1659218400,
    "99": 1661896800,
    "100": 1664488800,
    "101": 1667170800,
    "102": 1669762800,
    "103": 1672441200,
    "104": 1675119600,
    "105": 1677538800,
    "106": 1680213600,
    "107": 1682805600,
    "108": 1685484000,
    "109": 1688076000,
    "110": 1690754400,
    "111": 1693432800,
    "112": 1696024800,
    "113": 1698706800,
    "114": 1701298800,
    "115": 1703977200,
    "116": 1706655600,
    "117": 1709161200,
    "118": 1711839600,
    "119": 1714428000,
    "120": 1717106400,
    "121": 1719698400,
    "122": 1722376800,
    "123": 1725055200,
    "124": 1727647200,
    "125": 1730329200,
    "126": 1732921200,
    "127": 1735599600,
    "128": 1738278000,
    "129": 1740697200,
    "130": 1743372000,
    "131": 1745964000,
    "132": 1748642400,
    "133": 1751234400,
    "134": 1753912800,
    "135": 1756591200,
    "136": 1759183200,
    "137": 1761865200,
    "138": 1764457200,
    "139": 1767135600,
    "140": 1769814000,
    "141": 1772233200,
    "142": 1774908000,
    "143": 1777500000,
    "144": 1780178400,
    "145": 1782770400,
    "146": 1785448800,
    "147": 1788127200,
    "148": 1790719200,
    "149": 1793401200,
    "150": 1795993200,
    "151": 1798671600,
    "152": 1801350000,
    "153": 1803769200,
    "154": 1806444000,
    "155": 1809036000,
    "156": 1811714400,
    "157": 1814306400,
    "158": 1816984800,
    "159": 1819663200,
    "160": 1822255200,
    "161": 1824933600,
    "162": 1827529200,
    "163": 1830207600,
    "164": 1832886000,
    "165": 1835391600,
    "166": 1838066400,
    "167": 1840658400,
    "168": 1843336800,
    "169": 1845928800,
    "170": 1848607200,
    "171": 1851285600,
    "172": 1853877600,
    "173": 1856559600,
    "174": 1859151600,
    "175": 1861830000,
    "176": 1864508400,
    "177": 1866927600,
    "178": 1869602400,
    "179": 1872194400,
    "180": 972946800,
    "181": 975538800,
    "182": 978217200,
    "183": 980895600,
    "184": 983314800,
    "185": 985989600,
    "186": 988581600,
    "187": 991260000,
    "188": 993852000,
    "189": 996530400,
    "190": 999208800,
    "191": 1001800800,
    "192": 1004482800,
    "193": 1007074800,
    "194": 1009753200,
    "195": 1012431600,
    "196": 1014850800,
    "197": 1017529200,
    "198": 1020117600,
    "199": 1022796000,
    "200": 1025388000,
    "201": 1028066400,
    "202": 1030744800,
    "203": 1033336800,
    "204": 1036018800,
    "205": 1038610800,
    "206": 1041289200,
    "207": 1043967600,
    "208": 1046386800,
    "209": 1049061600,
    "210": 1051653600,
    "211": 1054332000,
    "212": 1056924000,
    "213": 1059602400,
    "214": 1062280800,
    "215": 1064872800,
    "216": 1067554800,
    "217": 1070146800,
    "218": 1072825200,
    "219": 1075503600,
    "220": 1078009200,
    "221": 1080684000,
    "222": 1083276000,
    "223": 1085954400,
    "224": 1088546400,
    "225": 1091224800,
    "226": 1093903200,
    "227": 1096495200,
    "228": 1099173600,
    "229": 1101769200,
    "230": 1104447600,
    "231": 1107126000,
    "232": 1109545200,
    "233": 1112220000,
    "234": 1114812000,
    "235": 1117490400,
    "236": 1120082400,
    "237": 1122760800,
    "238": 1125439200,
    "239": 1128031200,
    "240": 1130713200,
    "241": 1133305200,
    "242": 1135983600,
    "243": 1138662000,
    "244": 1141081200,
    "245": 1143756000,
    "246": 1146348000,
    "247": 1149026400,
    "248": 1151618400,
    "249": 1154296800,
    "250": 1156975200,
    "251": 1159567200,
    "252": 1162249200,
    "253": 1164841200,
    "254": 1167519600,
    "255": 1170198000,
    "256": 1172617200,
    "257": 1175292000,
    "258": 1177884000,
    "259": 1180562400,
    "260": 1183154400,
    "261": 1185832800,
    "262": 1188511200,
    "263": 1191103200,
    "264": 1193785200,
    "265": 1196377200,
    "266": 1199055600,
    "267": 1201734000,
    "268": 1204239600,
    "269": 1206914400,
    "270": 1209506400,
    "271": 1212184800,
    "272": 1214776800,
    "273": 1217455200,
    "274": 1220133600,
    "275": 1222725600,
    "276": 1225407600,
    "277": 1227999600,
    "278": 1230678000,
    "279": 1233356400,
    "280": 1235775600,
    "281": 1238450400,
    "282": 1241042400,
    "283": 1243720800,
    "284": 1246312800,
    "285": 1248991200,
    "286": 1251669600,
    "287": 1254261600,
    "288": 1256943600,
    "289": 1259535600,
    "290": 1262214000,
    "291": 1264892400,
    "292": 1267311600,
    "293": 1269986400,
    "294": 1272578400,
    "295": 1275256800,
    "296": 1277848800,
    "297": 1280527200,
    "298": 1283205600,
    "299": 1285797600,
    "300": 1288476000,
    "301": 1291071600,
    "302": 1293750000,
    "303": 1296428400,
    "304": 1298847600,
    "305": 1301522400,
    "306": 1304114400,
    "307": 1306792800,
    "308": 1309384800,
    "309": 1312063200,
    "310": 1314741600,
    "311": 1317333600,
    "312": 1320015600,
    "313": 1322607600,
    "314": 1325286000,
    "315": 1327964400,
    "316": 1330470000,
    "317": 1333144800,
    "318": 1335736800,
    "319": 1338415200,
    "320": 1341007200,
    "321": 1343685600,
    "322": 1346364000,
    "323": 1348956000,
    "324": 1351638000,
    "325": 1354230000,
    "326": 1356908400,
    "327": 1359586800,
    "328": 1362006000,
    "329": 1364684400,
    "330": 1367272800,
    "331": 1369951200,
    "332": 1372543200,
    "333": 1375221600,
    "334": 1377900000,
    "335": 1380492000,
    "336": 1383174000,
    "337": 1385766000,
    "338": 1388444400,
    "339": 1391122800,
    "340": 1393542000,
    "341": 1396216800,
    "342": 1398808800,
    "343": 1401487200,
    "344": 1404079200,
    "345": 1406757600,
    "346": 1409436000,
    "347": 1412028000,
    "348": 1414710000,
    "349": 1417302000,
    "350": 1419980400,
    "351": 1422658800,
    "352": 1425078000,
    "353": 1427752800,
    "354": 1430344800,
    "355": 1433023200,
    "356": 1435615200,
    "357": 1438293600,
    "358": 1440972000,
    "359": 1443564000,
    "360": 1446246000,
    "361": 1448838000,
    "362": 1451516400,
    "363": 1454194800,
    "364": 1456700400,
    "365": 1459375200,
    "366": 1461967200,
    "367": 1464645600,
    "368": 1467237600,
    "369": 1469916000,
    "370": 1472594400,
    "371": 1475186400,
    "372": 1477868400,
    "373": 1480460400,
    "374": 1483138800,
    "375": 1485817200,
    "376": 1488236400,
    "377": 1490911200,
    "378": 1493503200,
    "379": 1496181600,
    "380": 1498773600,
    "381": 1501452000,
    "382": 1504130400,
    "383": 1506722400,
    "384": 1509404400,
    "385": 1511996400,
    "386": 1514674800,
    "387": 1517353200,
    "388": 1519772400,
    "389": 1522447200,
    "390": 1525039200,
    "391": 1527717600,
    "392": 1530309600,
    "393": 1532988000,
    "394": 1535666400,
    "395": 1538258400,
    "396": 1540940400,
    "397": 1543532400,
    "398": 1546210800,
    "399": 1548889200,
    "400": 1551308400,
    "401": 1553986800,
    "402": 1556575200,
    "403": 1559253600,
    "404": 1561845600,
    "405": 1564524000,
    "406": 1567202400,
    "407": 1569794400,
    "408": 1572476400,
    "409": 1575068400,
    "410": 1577746800,
    "411": 1580425200,
    "412": 1582930800,
    "413": 1585605600,
    "414": 1588197600,
    "415": 1590876000,
    "416": 1593468000,
    "417": 1596146400,
    "418": 1598824800,
    "419": 1601416800,
    "420": 1604098800,
    "421": 1606690800,
    "422": 1609369200,
    "423": 1612047600,
    "424": 1614466800,
    "425": 1617141600,
    "426": 1619733600,
    "427": 1622412000,
    "428": 1625004000,
    "429": 1627682400,
    "430": 1630360800,
    "431": 1632952800,
    "432": 1635631200,
    "433": 1638226800,
    "434": 1640905200,
    "435": 1643583600,
    "436": 1646002800,
    "437": 1648677600,
    "438": 1651269600,
    "439": 1653948000,
    "440": 1656540000,
    "441": 1659218400,
    "442": 1661896800,
    "443": 1664488800,
    "444": 1667170800,
    "445": 1669762800,
    "446": 1672441200,
    "447": 1675119600,
    "448": 1677538800,
    "449": 1680213600,
    "450": 1682805600,
    "451": 1685484000,
    "452": 1688076000,
    "453": 1690754400,
    "454": 1693432800,
    "455": 1696024800,
    "456": 1698706800,
    "457": 1701298800,
    "458": 1703977200,
    "459": 1706655600,
    "460": 1709161200,
    "461": 1711839600,
    "462": 1714428000,
    "463": 1717106400,
    "464": 1719698400,
    "465": 1722376800,
    "466": 1725055200,
    "467": 1727647200,
    "468": 1730329200,
    "469": 1732921200,
    "470": 1735599600,
    "471": 1738278000,
    "472": 1740697200,
    "473": 1743372000,
    "474": 1745964000,
    "475": 1748642400,
    "476": 1751234400,
    "477": 1753912800,
    "478": 1756591200,
    "479": 1759183200,
    "480": 1761865200,
    "481": 1764457200,
    "482": 1767135600,
    "483": 1769814000,
    "484": 1772233200,
    "485": 1774908000,
    "486": 1777500000,
    "487": 1780178400,
    "488": 1782770400,
    "489": 1785448800,
    "490": 1788127200,
    "491": 1790719200,
    "492": 1793401200,
    "493": 1795993200,
    "494": 1798671600,
    "495": 1801350000,
    "496": 1803769200,
    "497": 1806444000,
    "498": 1809036000,
    "499": 1811714400,
    "500": 1814306400,
    "501": 1816984800,
    "502": 1819663200,
    "503": 1822255200,
    "504": 1824933600,
    "505": 1827529200,
    "506": 1830207600,
    "507": 1832886000,
    "508": 1835391600,
    "509": 1838066400,
    "510": 1840658400,
    "511": 1843336800,
    "512": 1845928800,
    "513": 1848607200,
    "514": 1851285600,
    "515": 1853877600,
    "516": 1856559600,
    "517": 1859151600,
    "518": 1861830000,
    "519": 1864508400,
    "520": 1866927600,
    "521": 1869602400,
    "522": 1872194400,
    "523": 1874872800,
    "524": 1877464800,
    "525": 1880143200,
    "526": 1882821600,
    "527": 1885413600,
    "528": 1888095600,
    "529": 1890687600,
    "530": 1893366000,
    "531": 1896044400,
    "532": 1898463600,
    "533": 1901142000,
    "534": 1903730400,
    "535": 1906408800,
    "536": 1909000800,
    "537": 1911679200,
    "538": 1914357600,
    "539": 1916949600,
    "540": 1919631600,
    "541": 1922223600,
    "542": 1924902000,
    "543": 1927580400,
    "544": 1929999600,
    "545": 1932674400,
    "546": 1935266400,
    "547": 1937944800,
    "548": 1940536800,
    "549": 1943215200,
    "550": 1945893600,
    "551": 1948485600
  },
  "yearsLocataires": {
    "1": 2016,
    "3": 2014,
    "4": 1983,
    "5": 1984
  },
  "rapport": {
    "2016": {
      "nbImmeubles": 3,
      "psAvecFoncier": 1506.76,
      "psSansFoncier": 0,
      "psFoncierSeul": 1506.76,
      "impotFoncierSeul": 1543.18,
      "impotAvecFoncier": 5127.18,
      "impotSansFoncier": 3584,
      "foncier2044RevenusE": 12180,
      "foncier2044InteretsG": 1959,
      "foncier2044ChargesF": 500,
      "foncier2044": 9721,
      "microfoncier": null,
      "microfoncierI": 0,
      "bic": 0,
      "bicF": 0,
      "microbic": 0,
      "microbicF": 0,
      "bicRecettes": 0,
      "bicCharges": 0,
      "bicAmortissements": 0,
      "reductionImpot": 0,
      "abattements1": 5000,
      "abattements2": null,
      "immeubles": {
        "580a81d348177eb0128b4567": {
          "name": "test immeuble 2",
          "investissementTotal": 53488,
          "rendementNet": -0.037391564463057,
          "rendementNetInterets": -4.1373766078373,
          "cashflow": -7478.96,
          "actifBrut": 47705.55,
          "passif": 77276.43
        },
        "57fb365448177efb16d17f10": {
          "name": "test delete bien inventaire",
          "investissementTotal": 100615,
          "rendementNet": 8.152521989763,
          "rendementNetInterets": 7.9597077970482,
          "cashflow": 413.7,
          "actifBrut": 89814.81,
          "passif": -11236.76
        },
        "5729f23b48177eba408b4568": {
          "name": "App. Angers, 93 000 euros",
          "investissementTotal": 44500,
          "rendementNet": 2.257797752809,
          "rendementNetInterets": -1.7084943820225,
          "cashflow": -6454.24,
          "actifBrut": 86111.11,
          "passif": 77276.43
        }
      },
      "cashflow": -13519.5,
      "actifBrut": 223631.47,
      "passif": 66039.67,
      "investissements": {
        "5729f23b48177eba408b4567": {
          "passif": 77276.43,
          "capitalAcquis": 5271
        },
        "57fb35de48177eee16d17f10": {
          "passif": -11236.76,
          "capitalAcquis": 7939.48
        }
      },
      "capitalAcquis": 13210.48,
      "biens": {
        "580a81d348177eb0128b4567": {
          "580a81d348177eb0128b4568": {
            "allNbJours": 366,
            "vacanceNbJours": 0,
            "occupationNbJours": 366,
            "vacancePourcent": 0,
            "occupationPourcent": 100
          }
        },
        "57fb365448177efb16d17f10": {
          "57fb365448177efb16d17f11": {
            "allNbJours": 366,
            "vacanceNbJours": 0,
            "occupationNbJours": 366,
            "vacancePourcent": 0,
            "occupationPourcent": 100
          },
          "58af694048177ea2118b456c": {
            "allNbJours": 366,
            "vacanceNbJours": 0,
            "occupationNbJours": 366,
            "vacancePourcent": 0,
            "occupationPourcent": 100
          }
        },
        "5729f23b48177eba408b4568": {
          "5729f23b48177eba408b4569": {
            "allNbJours": 366,
            "vacanceNbJours": 263,
            "occupationNbJours": 103,
            "vacancePourcent": 72,
            "occupationPourcent": 28
          }
        }
      }
    },
    "2017": {
      "nbImmeubles": 3,
      "psAvecFoncier": 191.12,
      "psSansFoncier": 0,
      "psFoncierSeul": 191.12,
      "impotFoncierSeul": 172.62,
      "impotAvecFoncier": 4313.82,
      "impotSansFoncier": 4141.2,
      "foncier2044RevenusE": 1250,
      "foncier2044InteretsG": 0,
      "foncier2044ChargesF": 40,
      "foncier2044": 1233,
      "microfoncier": null,
      "microfoncierI": 0,
      "bic": 0,
      "bicF": 0,
      "microbic": 0,
      "microbicF": 0,
      "bicRecettes": 0,
      "bicCharges": 0,
      "bicAmortissements": 0,
      "reductionImpot": 0,
      "abattements1": 1000,
      "abattements2": null,
      "immeubles": {
        "580a81d348177eb0128b4567": {
          "name": "test immeuble 2",
          "investissementTotal": 53488,
          "rendementNet": -0.037391564463057,
          "rendementNetInterets": -2.4958869279091,
          "cashflow": -7478.96,
          "actifBrut": 47705.55,
          "passif": 77276.43
        },
        "57fb365448177efb16d17f10": {
          "name": "test delete bien inventaire",
          "investissementTotal": 100615,
          "rendementNet": 0.8410873130249,
          "rendementNetInterets": 0.86394672762511,
          "cashflow": -6942.7,
          "actifBrut": 89814.81,
          "passif": -11236.76
        },
        "5729f23b48177eba408b4568": {
          "name": "App. Angers, 93 000 euros",
          "investissementTotal": 44500,
          "rendementNet": -0.044943820224719,
          "rendementNetInterets": -2.4247191011236,
          "cashflow": -7478.96,
          "actifBrut": 86111.11,
          "passif": 77276.43
        }
      },
      "cashflow": -21900.62,
      "actifBrut": 223631.47,
      "passif": 66039.67,
      "investissements": {
        "5729f23b48177eba408b4567": {
          "passif": 77276.43,
          "capitalAcquis": 5415.1
        },
        "57fb35de48177eee16d17f10": {
          "passif": -11236.76,
          "capitalAcquis": 8156.5
        }
      },
      "capitalAcquis": 13571.6,
      "biens": {
        "580a81d348177eb0128b4567": {
          "580a81d348177eb0128b4568": {
            "allNbJours": 365,
            "vacanceNbJours": 0,
            "occupationNbJours": 365,
            "vacancePourcent": 0,
            "occupationPourcent": 100
          }
        },
        "57fb365448177efb16d17f10": {
          "57fb365448177efb16d17f11": {
            "allNbJours": 365,
            "vacanceNbJours": 0,
            "occupationNbJours": 365,
            "vacancePourcent": 0,
            "occupationPourcent": 100
          },
          "58af694048177ea2118b456c": {
            "allNbJours": 365,
            "vacanceNbJours": 0,
            "occupationNbJours": 365,
            "vacancePourcent": 0,
            "occupationPourcent": 100
          }
        },
        "5729f23b48177eba408b4568": {
          "5729f23b48177eba408b4569": {
            "allNbJours": 365,
            "vacanceNbJours": 0,
            "occupationNbJours": 365,
            "vacancePourcent": 0,
            "occupationPourcent": 100
          }
        }
      }
    },
    "chargesEmpruntGlobalPerYear": {
      "2000": 731,
      "2001": 2837,
      "2002": 2692,
      "2003": 2543,
      "2004": 2390,
      "2005": 2233,
      "2006": 2072,
      "2007": 1906,
      "2008": 1736,
      "2009": 1561,
      "2010": 1381,
      "2011": 1197,
      "2012": 1007,
      "2013": 812,
      "2014": 2461,
      "2015": 3065,
      "2016": 4154,
      "2017": 2353,
      "2018": 1982,
      "2019": 1601,
      "2020": 1209,
      "2021": 807,
      "2022": 393,
      "2023": -31,
      "2024": -467,
      "2025": -915,
      "2026": -1376,
      "2027": -1849,
      "2028": -2334,
      "2029": -3015,
      "2030": -3447,
      "2031": -2793
    },
    "explications": {
      "2015": {
        "immeubles": {
          "580a81d348177eb0128b4567": {
            "investissementTotal": "[ Net vendeur (47706) + Comission agence (3816) + Travaux (0) + Frais dossier (0) + Crédit logement(0) + Meubles (277) + Frais de notaire (1689) - Primes Anah (0) ]",
            "rendementNet": "[ Recettes (0) - Charges (20) - Prél. Sociaux (0) - Impôt (0) ] * 100 / Investissement (53488)",
            "rendementNetInterets": "[ Recettes (0) - Charges (20) - Intérêts (1473) - Prél. Sociaux (0) - Impôt (0) ] * 100 / Investissement (53488)",
            "cashflow": "Loyers (0) - MensualitésSansAssurance (7459) - Charges (20) - Prél. Sociaux (0) - Impôt (0)"
          },
          "57fb365448177efb16d17f10": {
            "investissementTotal": "[ Net vendeur (89815) + Comission agence (7185) + Travaux (0) + Frais dossier (0) + Crédit logement(0) + Meubles (500) + Frais de notaire (3115) - Primes Anah (0) ]",
            "rendementNet": "[ Recettes (11400) - Charges (40) - Prél. Sociaux (1698) - Impôt (2191) ] * 100 / Investissement (100615)",
            "rendementNetInterets": "[ Recettes (11400) - Charges (40) - Intérêts (406) - Prél. Sociaux (1698) - Impôt (2191) ] * 100 / Investissement (100615)",
            "cashflow": "Loyers (11400) - MensualitésSansAssurance (7789) - Charges (40) - Prél. Sociaux (1698) - Impôt (2191)"
          },
          "5729f23b48177eba408b4568": {
            "investissementTotal": "[ Net vendeur (38406) + Comission agence (3072) + Travaux (0) + Frais dossier (750) + Crédit logement(690) + Meubles (223) + Frais de notaire (1359) - Primes Anah (0) ]",
            "rendementNet": "[ Recettes (12624) - Charges (200) - Prél. Sociaux (0) - Impôt (0) ] * 100 / Investissement (44500)",
            "rendementNetInterets": "[ Recettes (12624) - Charges (200) - Intérêts (3568) - Prél. Sociaux (0) - Impôt (0) ] * 100 / Investissement (44500)",
            "cashflow": "Loyers (12624) - MensualitésSansAssurance (7459) - Charges (200) - Prél. Sociaux (0) - Impôt (0)"
          }
        }
      },
      "2016": {
        "immeubles": {
          "580a81d348177eb0128b4567": {
            "investissementTotal": "[ Net vendeur (47706) + Comission agence (3816) + Travaux (0) + Frais dossier (0) + Crédit logement(0) + Meubles (277) + Frais de notaire (1689) - Primes Anah (0) ]",
            "rendementNet": "[ Recettes (0) - Charges (20) - Prél. Sociaux (0) - Impôt (0) ] * 100 / Investissement (53488)",
            "rendementNetInterets": "[ Recettes (0) - Charges (20) - Intérêts (2193) - Prél. Sociaux (0) - Impôt (0) ] * 100 / Investissement (53488)",
            "cashflow": "Loyers (0) - MensualitésSansAssurance (7459) - Charges (20) - Prél. Sociaux (0) - Impôt (0)"
          },
          "57fb365448177efb16d17f10": {
            "investissementTotal": "[ Net vendeur (89815) + Comission agence (7185) + Travaux (0) + Frais dossier (0) + Crédit logement(0) + Meubles (500) + Frais de notaire (3115) - Primes Anah (0) ]",
            "rendementNet": "[ Recettes (11400) - Charges (90) - Prél. Sociaux (1507) - Impôt (1601) ] * 100 / Investissement (100615)",
            "rendementNetInterets": "[ Recettes (11400) - Charges (90) - Intérêts (194) - Prél. Sociaux (1507) - Impôt (1601) ] * 100 / Investissement (100615)",
            "cashflow": "Loyers (11400) - MensualitésSansAssurance (7789) - Charges (90) - Prél. Sociaux (1507) - Impôt (1601)"
          },
          "5729f23b48177eba408b4568": {
            "investissementTotal": "[ Net vendeur (38406) + Comission agence (3072) + Travaux (0) + Frais dossier (750) + Crédit logement(690) + Meubles (223) + Frais de notaire (1359) - Primes Anah (0) ]",
            "rendementNet": "[ Recettes (780) - Charges (410) - Prél. Sociaux (-216) - Impôt (-419) ] * 100 / Investissement (44500)",
            "rendementNetInterets": "[ Recettes (780) - Charges (410) - Intérêts (1765) - Prél. Sociaux (-216) - Impôt (-419) ] * 100 / Investissement (44500)",
            "cashflow": "Loyers (780) - MensualitésSansAssurance (7459) - Charges (410) - Prél. Sociaux (-216) - Impôt (-419)"
          }
        }
      },
      "2017": {
        "immeubles": {
          "580a81d348177eb0128b4567": {
            "investissementTotal": "[ Net vendeur (47706) + Comission agence (3816) + Travaux (0) + Frais dossier (0) + Crédit logement(0) + Meubles (277) + Frais de notaire (1689) - Primes Anah (0) ]",
            "rendementNet": "[ Recettes (0) - Charges (20) - Prél. Sociaux (0) - Impôt (0) ] * 100 / Investissement (53488)",
            "rendementNetInterets": "[ Recettes (0) - Charges (20) - Intérêts (1315) - Prél. Sociaux (0) - Impôt (0) ] * 100 / Investissement (53488)",
            "cashflow": "Loyers (0) - MensualitésSansAssurance (7459) - Charges (20) - Prél. Sociaux (0) - Impôt (0)"
          },
          "57fb365448177efb16d17f10": {
            "investissementTotal": "[ Net vendeur (89815) + Comission agence (7185) + Travaux (0) + Frais dossier (0) + Crédit logement(0) + Meubles (500) + Frais de notaire (3115) - Primes Anah (0) ]",
            "rendementNet": "[ Recettes (1250) - Charges (40) - Prél. Sociaux (191) - Impôt (173) ] * 100 / Investissement (100615)",
            "rendementNetInterets": "[ Recettes (1250) - Charges (40) - Intérêts (-23) - Prél. Sociaux (191) - Impôt (173) ] * 100 / Investissement (100615)",
            "cashflow": "Loyers (1250) - MensualitésSansAssurance (7789) - Charges (40) - Prél. Sociaux (191) - Impôt (173)"
          },
          "5729f23b48177eba408b4568": {
            "investissementTotal": "[ Net vendeur (38406) + Comission agence (3072) + Travaux (0) + Frais dossier (750) + Crédit logement(690) + Meubles (223) + Frais de notaire (1359) - Primes Anah (0) ]",
            "rendementNet": "[ Recettes (0) - Charges (20) - Prél. Sociaux (0) - Impôt (0) ] * 100 / Investissement (44500)",
            "rendementNetInterets": "[ Recettes (0) - Charges (20) - Intérêts (1059) - Prél. Sociaux (0) - Impôt (0) ] * 100 / Investissement (44500)",
            "cashflow": "Loyers (0) - MensualitésSansAssurance (7459) - Charges (20) - Prél. Sociaux (0) - Impôt (0)"
          }
        }
      }
    }
  },
  "rapportEmprunt": {
    "972946800": {
      "57fb35de48177eee16d17f10": {
        "passif": 95548.88,
        "actifNet": -5734.07
      }
    },
    "975538800": {
      "57fb35de48177eee16d17f10": {
        "passif": 95114.79,
        "actifNet": -5299.98
      }
    },
    "978217200": {
      "57fb35de48177eee16d17f10": {
        "passif": 94679.72,
        "actifNet": -4864.91
      }
    },
    "980895600": {
      "57fb35de48177eee16d17f10": {
        "passif": 94243.67,
        "actifNet": -4428.86
      }
    },
    "983314800": {
      "57fb35de48177eee16d17f10": {
        "passif": 93806.64,
        "actifNet": -3991.83
      }
    }
  },
  "countImmeubles": null,
  "time_to_compute_report": 13.190853118896,
  "result": true,
  "code": 0,
  "cacheLastUpdateTime": 1488359129
}
```


### HTTP Request

`POST https://www.rendementlocatif.com/api/gestion/21/raport/`


### POST Parameters

Should be sent with the request in the body as "application/x-www-form-urlencoded".

Parameter | Mandatory | Description | Type | default
--------- | ------- | ------- | ------- | -------
username | yes | The user name or email address | string |
password | yes | The user password | string |


### Returned parameters

TBD
