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
Currently only version = 2 is supported. So please use the following endpoint:<br/>
https://www.rendementlocatif.com/api/gestion/2/immeubles
</aside>

# Authentificating a user

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
"https://www.rendementlocatif.com/api/gestion/2/immeubles"
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

`POST https://www.rendementlocatif.com/api/gestion/2/immeubles`

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
"https://www.rendementlocatif.com/api/gestion/2/immeuble/<immeubleId>"
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

`POST https://www.rendementlocatif.com/api/gestion/2/immeuble/<immeubleId>`

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
"https://www.rendementlocatif.com/api/gestion/2/bien/<bienId>"
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

`POST https://www.rendementlocatif.com/api/gestion/2/bien/<bienId>`

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
"https://www.rendementlocatif.com/api/gestion/2/locataires/<immeubleIdOrbienId>/<limit>"
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

`POST https://www.rendementlocatif.com/api/gestion/2/locataires/<immeubleIdOrbienId>/<limit>`

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
"https://www.rendementlocatif.com/api/gestion/2/locataire/<locataireId>"
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

`POST https://www.rendementlocatif.com/api/gestion/2/locataire/<locataireId>`

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
"https://www.rendementlocatif.com/api/gestion/2/operations/<immeubleId>/<limit>"
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

`POST https://www.rendementlocatif.com/api/gestion/2/operations/<immeubleId>/<limit>`

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
"https://www.rendementlocatif.com/api/gestion/2/saveOperation/<immeubleId>/<operationId>"
  -H "X-API-KEY: secretkey"
```

> Returns the following JSON:

```json
{
  "message": "Operation added",
  "result": true,
  "operationId": "5809db0b48177ed5098b4568",
  "code": 0
}
```

### HTTP Request

`POST https://www.rendementlocatif.com/api/gestion/2/saveOperation/<immeubleId>/<operationId>`

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
operationDate | The date of the operation (format should be the french one: dd/mm/YYYY) | string (dd/mm/YYYY)
operationPaid | Operations was paid or not ( "on" to set as paid, anything else would mark it not paid) | string
operationAuto | Operations has to be repeated automatically in the future ( "on" to set as enabled, anything else would mark it not enabled) | string
operationDesc | A description the user can freely add to the operation | string
operationMontantPC | In the case the type of the operation (operationName) is 'recetteLoyer' then this can be added so to create an operation of type 'recetteLoyerProvision' at the same time. The reason is that a "locataire" will usually pay both recetteLoyer and recetteLoyerProvision at the same time. The UI of the client app should allow the user to fill in both things at the same time and help the user split the amount reveived from the "locataire" in both 'recetteLoyer' and 'recetteLoyerProvision' | string
operationMontantTEOM | The amount of "Taxe ordure menagère" (TEOM) to be added as new field in the case the operation is of type "chargeTF" (charge taxe foncière) | float
empruntId | the index of the emprunt of the immeuble (retreived from /immeuble or /immeubles in "emprunt" field ) | integer
operationRepeatDate | A max date for repeating the operation creation several time until that date. This date should be at the minimum equal to (operationDate + 1 month) and has no maximum. This field should be displayed only if operationDate is set and at the maximum 1 month in the past compared to current dateTime | string (dd/mm/YYYY)
dateDebut | start date for the regularisation of charges. Should be displayed and used only if (operationName (type) = recetteReguLocataire) or (operationName (type) = chargeReguLocataire) | string (dd/mm/YYYY)
dateFin | end date for the regularisation of charges. Should be displayed and used only if (operationName (type) = recetteReguLocataire) or (operationName (type) = chargeReguLocataire) | string (dd/mm/YYYY)



Example of body with parameters:

username:bob<br/>
password:bobpassword<br/>
operationName:recetteLoyer<br/>
operationMontant:9999<br/>
locataireId:56a7b72548177e22348b4569<br/>
operationDate:17/08/2016<br/>
operationPaid:on<br/>
operationAuto:off<br/>


## Set Locataire quit Date

Set the the locataire departure date.

> Code to get the bien of a user

```shell
curl --data "username=username&password=password"
"https://www.rendementlocatif.com/api/gestion/2/setLocataireGone/<locataireId>"
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

`POST https://www.rendementlocatif.com/api/gestion/2/setLocataireGone/<locataireId>`

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
quitDate:12/08/2016<br/>

```shell
curl --request POST \
  --url https://www.rendementlocatif.com/api/gestion/2/setLocataireGone/580a81d348177eb0128b4568 \
  --header 'cache-control: no-cache' \
  --header 'content-type: application/x-www-form-urlencoded' \
  --header 'x-api-key: secretkey' \
  --data 'username=boss&password=6407&quitDate=12%2F08%2F2014'
```

## Split Loyer

Splits a loyer paid by a "locataire" in loyer Hors Charges and Provision pour charge for a locataire.

> Code to split a loyer in 2 operations for a locataire

```shell
curl --data "username=username&password=password"
"https://www.rendementlocatif.com/api/gestion/2/ventilationLoyer/<locataireId>/<montantTotal>"
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

`POST https://www.rendementlocatif.com/api/gestion/2/ventilationLoyer/<locataireId>/<montantTotal>`

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
"https://www.rendementlocatif.com/api/gestion/2/saveLocataire/<bienId>/<locataireId>"
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

`POST https://www.rendementlocatif.com/api/gestion/2/saveLocataire/<bienId>/<locataireId>`

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
signatureDate:12/08/2014<br/>
entryDate:12/08/2014<br/>
dayLoyer:3<br/>
typeLocation:0<br/>
depotGarantie:1<br/>

```shell
curl --request POST \
  --url https://www.rendementlocatif.com/api/gestion/2/saveLocataire/580a81d348177eb0128b4568 \
  --header 'cache-control: no-cache' \
  --header 'content-type: application/x-www-form-urlencoded' \
  --header 'x-api-key: secretkey' \
  --data 'username=boss&password=6407&nom=Dupont&prenom=jean&loyer=666&provision=20&indiceRevision=0&signatureDate=12%2F08%2F2014&entryDate=12%2F08%2F2014&dayLoyer=3&typeLocation=0&depotGarantie=1'
```

## Split Loyer

Splits a loyer paid by a "locataire" in loyer Hors Charges and Provision pour charge for a locataire.

> Code to split a loyer in 2 operations for a locataire

```shell
curl --data "username=username&password=password"
"https://www.rendementlocatif.com/api/gestion/2/ventilationLoyer/<locataireId>/<montantTotal>"
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

`POST https://www.rendementlocatif.com/api/gestion/2/ventilationLoyer/<locataireId>/<montantTotal>`

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
"https://www.rendementlocatif.com/api/gestion/2/markOperationPaid/<operationId>"
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

`POST https://www.rendementlocatif.com/api/gestion/2/markOperationPaid/<operationId>`

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
"https://www.rendementlocatif.com/api/gestion/2/markOperationUnPaid/<operationId>"
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

`POST https://www.rendementlocatif.com/api/gestion/2/markOperationUnPaid/<operationId>`

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
"https://www.rendementlocatif.com/api/gestion/2/deleteOperation/<operationId>"
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

`POST https://www.rendementlocatif.com/api/gestion/2/deleteOperation/<operationId>`

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
"https://www.rendementlocatif.com/api/gestion/2/quittance/<locataireId>/<anythingOnlyToSendEmail>"
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
  "message": "Un email avec la quittance a été envoyé à votre locataire."
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

`POST https://www.rendementlocatif.com/api/gestion/2/quittance/<locataireId>/<anythingToSendEmail>`

"locataireId" is mandatory and should correspond to the id of the operation to delete.

"anythingOnlyToSendEmail" is optional.  Can be any non empty string. If given, the server will send an email to the locataire with the quittance PDF attached.

### Parameters

Should be sent with the request in the body as "application/x-www-form-urlencoded".

Parameter | Mandatory | Description | Type | default
--------- | ------- | ------- | ------- | -------
username | yes | The user name or email address | string |
password | yes | The user password | string |
startDate | no | The start date for the quittance (dd/mm/yyyy) | string | first day of current month
endDate | no | The end date for the quittance (dd/mm/yyyy) | string | last day of current month
startSolde | no | The start date for the solde(dd/mm/yyyy) | string | first day of month of locataire entry
ignoreSolde | no | Tells to ignore the solde calculation if equals "on" or take it into account if equals off | string | 'on'

### Returned parameters

Parameter | Type | Description
--------- | ------- | -------
result | boolean | true if quittance is generated ok or if email send ok, otherwise false
code | integer | 0 if ok otherwise error code of error
message | string | error message if error, ok message of code=0 (success)
