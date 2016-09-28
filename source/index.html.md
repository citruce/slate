---
title: API Referencee

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
    "countAll": 157,
    "count": 10,
    "operations": [
        {
            "dateCreated": {
                "sec": 1470305101,
                "usec": 828000
            },
            "dateLastModified": {
                "sec": 1462378891,
                "usec": 107000
            },
            "immeubleId": "56b7b15a48177ea7085932cd",
            "locataireId": "56b7b6e048177e9e085932cc",
            "operationAuto": true,
            "operationDate": {
                "sec": 1470261600,
                "usec": 0
            },
            "operationDesc": "",
            "operationMontant": 224,
            "operationPaid": false,
            "robot": true,
            "user_id": "2",
            "markPaidCode": "57a3134dc9eb3",
            "markNonAutoCode": "57a3134dc9f34",
            "id": "57a3134d48177e8e1875c3cf",
            "operationType": {
                "id": "recetteCaf",
                "name": "Recette CAF (APL/ACL)",
                "positive": true
            }
        },
        {
            "dateCreated": {
                "sec": 1470035102,
                "usec": 20000
            },
            "dateLastModified": {
                "sec": 1462378891,
                "usec": 636000
            },
            "immeubleId": "56b7b15a48177ea7085932cd",
            "locataireId": "56b7b6e048177e9e085932cc",
            "operationAuto": true,
            "operationDate": {
                "sec": 1470002400,
                "usec": 0
            },
            "operationDesc": "",
            "operationMontant": 126.16,
            "operationMontantPC": 25,
            "operationPaid": false,
            "robot": true,
            "user_id": "2",
            "markPaidCode": "579ef49e04a3b",
            "markNonAutoCode": "579ef49e04bf6",
            "id": "579ef49e48177e37118b4567",
            "operationType": {
                "id": "recetteLoyer",
                "name": "Recette Loyers Hors Charges",
                "positive": true
            }
        },
        {
            "dateCreated": {
                "sec": 1470035102,
                "usec": 90000
            },
            "dateLastModified": {
                "sec": 1459873779,
                "usec": 533000
            },
            "immeubleId": "56b7b15a48177ea7085932cd",
            "locataireId": "56b7b6e048177e9e085932cc",
            "operationAuto": true,
            "operationDate": {
                "sec": 1470002400,
                "usec": 0
            },
            "operationDesc": "Créé avec loyer 126.16€ (total: 151.16€).",
            "operationMontant": 25,
            "operationPaid": false,
            "robot": true,
            "user_id": "2",
            "markPaidCode": "579ef49e15799",
            "markNonAutoCode": "579ef49e15832",
            "id": "579ef49e48177e37118b4568",
            "operationType": {
                "id": "recetteLoyerProvision",
                "name": "Recette Provision Charges Locataire",
                "positive": true
            }
        },
        {
            "operationDate": {
                "sec": 1468360800,
                "usec": 0
            },
            "operationMontant": 163,
            "operationAuto": false,
            "operationPaid": true,
            "locataireId": "56b7b52348177ea2085932cc",
            "operationDesc": "Créée du compte bancaire BOURSORAMA ESSENTIEL",
            "sourceBankAccountId": 28,
            "sourceTransactionId": 732,
            "sourceTransactionMongoId": "57a1c10548177e741775c3d0",
            "user_id": "2",
            "immeubleId": "56b7b15a48177ea7085932cd",
            "dateCreated": {
                "sec": 1470218513,
                "usec": 870000
            },
            "robot": true,
            "id": "57a1c11148177ec21675c3ce",
            "operationType": {
                "id": "recetteCaf",
                "name": "Recette CAF (APL/ACL)",
                "positive": true
            }
        },
        {
            "dateCreated": {
                "sec": 1467878701,
                "usec": 882000
            },
            "dateLastModified": {
                "sec": 1470139384,
                "usec": 886000
            },
            "immeubleId": "56b7b15a48177ea7085932cd",
            "locataireId": "56b7b52348177ea2085932cc",
            "operationAuto": true,
            "operationDate": {
                "sec": 1467842400,
                "usec": 0
            },
            "operationDesc": "",
            "operationMontant": 57,
            "operationMontantPC": 15,
            "operationMontantTotal": "72",
            "operationPaid": false,
            "robot": true,
            "user_id": "2",
            "markPaidCode": "577e0d2dd7251",
            "markNonAutoCode": "577e0d2dd730b",
            "id": "577e0d2d48177e070a8b4567",
            "operationType": {
                "id": "recetteLoyer",
                "name": "Recette Loyers Hors Charges",
                "positive": true
            }
        },
        {
            "dateCreated": {
                "sec": 1467878701,
                "usec": 908000
            },
            "dateLastModified": {
                "sec": 1460013926,
                "usec": 343000
            },
            "immeubleId": "56b7b15a48177ea7085932cd",
            "locataireId": "56b7b52348177ea2085932cc",
            "operationAuto": true,
            "operationDate": {
                "sec": 1467842400,
                "usec": 0
            },
            "operationDesc": "Créé avec loyer 57€ (total: 72€).",
            "operationMontant": 15,
            "operationMontantTotal": "72",
            "operationPaid": false,
            "robot": true,
            "user_id": "2",
            "markPaidCode": "577e0d2ddd764",
            "markNonAutoCode": "577e0d2ddd7e9",
            "id": "577e0d2d48177e070a8b4568",
            "operationType": {
                "id": "recetteLoyerProvision",
                "name": "Recette Provision Charges Locataire",
                "positive": true
            }
        },
        {
            "operationDate": {
                "sec": 1467669600,
                "usec": 0
            },
            "operationMontant": 165,
            "operationAuto": false,
            "operationPaid": true,
            "locataireId": "56b7b52348177ea2085932cc",
            "operationDesc": "Créée du compte bancaire BOURSORAMA ESSENTIEL",
            "sourceBankAccountId": 28,
            "sourceTransactionId": 741,
            "sourceTransactionMongoId": "57a1c10548177e741775c3d3",
            "user_id": "2",
            "immeubleId": "56b7b15a48177ea7085932cd",
            "dateCreated": {
                "sec": 1470218513,
                "usec": 873000
            },
            "robot": true,
            "id": "57a1c11148177ec21675c3cf",
            "operationType": {
                "id": "recetteCaf",
                "name": "Recette CAF (APL/ACL)",
                "positive": true
            }
        },
        {
            "dateCreated": {
                "sec": 1467612301,
                "usec": 788000
            },
            "dateLastModified": {
                "sec": 1470305101,
                "usec": 828000
            },
            "immeubleId": "56b7b15a48177ea7085932cd",
            "locataireId": "56b7b6e048177e9e085932cc",
            "operationAuto": false,
            "operationDate": {
                "sec": 1467583200,
                "usec": 0
            },
            "operationDesc": "",
            "operationMontant": 224,
            "operationPaid": false,
            "robot": true,
            "user_id": "2",
            "markPaidCode": "5779fc8dc0589",
            "markNonAutoCode": "5779fc8dc05d4",
            "id": "5779fc8d48177edc158b4568",
            "operationType": {
                "id": "recetteCaf",
                "name": "Recette CAF (APL/ACL)",
                "positive": true
            }
        },
        {
            "dateCreated": {
                "sec": 1467324301,
                "usec": 774000
            },
            "dateLastModified": {
                "sec": 1470035102,
                "usec": 87000
            },
            "immeubleId": "56b7b15a48177ea7085932cd",
            "locataireId": "56b7b6e048177e9e085932cc",
            "operationAuto": false,
            "operationDate": {
                "sec": 1467324000,
                "usec": 0
            },
            "operationDesc": "",
            "operationMontant": 126.16,
            "operationMontantPC": 25,
            "operationPaid": false,
            "robot": true,
            "user_id": "2",
            "markPaidCode": "5775978dbc93d",
            "markNonAutoCode": "5775978dbcda3",
            "id": "5775978d48177e961d8b4567",
            "operationType": {
                "id": "recetteLoyer",
                "name": "Recette Loyers Hors Charges",
                "positive": true
            }
        },
        {
            "dateCreated": {
                "sec": 1467324301,
                "usec": 785000
            },
            "dateLastModified": {
                "sec": 1470035102,
                "usec": 90000
            },
            "immeubleId": "56b7b15a48177ea7085932cd",
            "locataireId": "56b7b6e048177e9e085932cc",
            "operationAuto": false,
            "operationDate": {
                "sec": 1467324000,
                "usec": 0
            },
            "operationDesc": "Créé avec loyer 126.16€ (total: 151.16€).",
            "operationMontant": 25,
            "operationPaid": false,
            "robot": true,
            "user_id": "2",
            "markPaidCode": "5775978dbf63d",
            "markNonAutoCode": "5775978dbf7d1",
            "id": "5775978d48177e961d8b4568",
            "operationType": {
                "id": "recetteLoyerProvision",
                "name": "Recette Provision Charges Locataire",
                "positive": true
            }
        }
    ],
    "result": true,
    "code": 0
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

# Calculation

## Calculate

Calculate rendement

> Code to calculate rendement

```shell
curl --data "username=username&password=password"
"https://www.rendementlocatif.com/apiV2"
  -H "X-API-KEY: secretkey"
```

> Returns the following JSON:

```json
{
  "status": "OK",
  "resume": "Achat d'un appartement de 150m2 , financé par un emprunt sur 15 ans (3 213€/mois),  au prix FAI de 500 000€ à Paris 1 loué en direct 1000€ par mois.  Les revenus sont déclarés en Régime Réel de déficit foncier.  ",
  "neuf": false,
  "travauxa": "0",
  "regime": "rr",
  "duree": 20,
  "tauxemprunt": "1.32",
  "assurance": 150,
  "fraisg": "0",
  "assuranceli": "0",
  "tauxmois": "0",
  "tauxan": "1",
  "pno": "0",
  "valorisation": "1",
  "valorisationc": "2",
  "tauxmi": "30",
  "tauxps": "15.5",
  "origin": "android",
  "creditlogement": "6900",
  "chargest": "3750",
  "chargesr": "78",
  "type": "appartement",
  "loyer": "1000",
  "emprunt": "on",
  "enfants": "0",
  "mbles": "0",
  "revenus": "35000",
  "parts": "1",
  "emprunt110": "",
  "prix": "500000",
  "fraisnotaire": "",
  "regServer": "true",
  "reventeprix": "0",
  "revente": false,
  "typeemprunt": "amorti",
  "fonciers2": "",
  "surface": "150",
  "agence": "5",
  "fraisrecherche": "0",
  "appVersion": "53",
  "ville": "Paris 1",
  "registration_id": "APA91bHYlI4FLWkxWFxNOZQSIhgqWjzoI2IIM6l8OlaDHDQsxTi3CuFaCckaRTXdugg-tkWSB7j0kuiJatgSevNke-cPpHOLw3uhFttY90n_jwtD-kG9PYyg_uj8Wx1gPDZh7_ecY4F9prKT1_fRHPLULCV8pVMr9g",
  "travauxr": "0",
  "tf": "2700",
  "fraisdossier": "5000",
  "valorisations": "0.5",
  "revenus2": "",
  "firstTime": "true",
  "prime": 0,
  "dureeanah": 6,
  "apport": "34186",
  "loyer_plafond": false,
  "prixm2": 3333,
  "net_vendeur": 476190.48,
  "frais_agence": 23809.52,
  "notaire": {
    "emoluments": 5137.92060864,
    "frais_formalites": 1000,
    "frais_publication": 476.19048,
    "droits_enregistrement": 27571.428792,
    "frais": 34186,
    "pourcentage": 7
  },
  "prix_total_investissement": 546086,
  "prix_revient": 534186,
  "vacance_mois": "0",
  "vacance_an": "1",
  "duree_totale": 20,
  "duree_emprunt": "15",
  "frais_dossier": "5000",
  "nb_mensualites": 180,
  "capitalaemprunter": 500000,
  "mensualite": 3213.3693737045,
  "mensualite_annuelle": [
    38560.432484454,
    38560.432484454,
    38560.432484454,
    38560.432484454,
    38560.432484454,
    38560.432484454,
    38560.432484454,
    38560.432484454,
    38560.432484454,
    38560.432484454,
    38560.432484454,
    38560.432484454,
    38560.432484454,
    38560.432484454,
    38560.432484454,
    0,
    0,
    0,
    0,
    0
  ],
  "teg": 2.35,
  "interets_mensuels": [
    550,
    547.23529368893,
    544.46754620091,
    541.69675419065,
    538.92291430919,
    536.14602320385,
    533.3660775183,
    530.5830738925,
    527.79700896271,
    525.00787936149,
    522.21568171771,
    519.42041265653,
    516.62206879937,
    513.82064676398,
    511.01614316434,
    508.20855461075,
    505.39787770975,
    502.58410906415,
    499.76724527305,
    496.94728293177,
    494.12421863192,
    491.29804896134,
    488.46877050413,
    485.63637984061,
    482.80087354736,
    479.96224819718,
    477.12050035913,
    474.27562659845,
    471.42762347663,
    468.57648755138,
    465.72221537661,
    462.86480350245,
    460.00424847523,
    457.14054683747,
    454.27369512792,
    451.40368988149,
    448.53052762928,
    445.6542048986,
    442.77471821291,
    439.89206409187,
    437.0062390513,
    434.11723960318,
    431.22506225567,
    428.32970351307,
    425.43115987586,
    422.52942784065,
    419.6245039002,
    416.71638454342,
    413.80506625534,
    410.89054551715,
    407.97281880614,
    405.05188259575,
    402.12773335553,
    399.20036755115,
    396.26978164438,
    393.33597209311,
    390.39893535134,
    387.45866786915,
    384.51516609273,
    381.56842646436,
    378.6184454224,
    375.66521940129,
    372.70874483155,
    369.74901813979,
    366.78603574867,
    363.81979407692,
    360.85028953933,
    357.87751854675,
    354.90147750608,
    351.92216282026,
    348.93957088828,
    345.95369810519,
    342.96454086203,
    339.9720955459,
    336.97635853993,
    333.97732622325,
    330.97499497102,
    327.96936115441,
    324.9604211406,
    321.94817129278,
    318.93260797013,
    315.91372752782,
    312.89152631703,
    309.8660006849,
    306.83714697458,
    303.80496152518,
    300.76944067178,
    297.73058074544,
    294.68837807319,
    291.642828978,
    288.5939297788,
    285.54167679048,
    282.48606632387,
    279.42709468575,
    276.36475817883,
    273.29905310176,
    270.22997574909,
    267.15752241134,
    264.08168937492,
    261.00247292216,
    257.9198693313,
    254.83387487649,
    251.74448582777,
    248.65169845111,
    245.55550900833,
    242.45591375717,
    239.35290895122,
    236.24649084,
    233.13665566884,
    230.02339967901,
    226.90671910758,
    223.78661018752,
    220.66306914765,
    217.53609221264,
    214.405675603,
    211.27181553509,
    208.1345082211,
    204.99374986907,
    201.84953668285,
    198.70186486213,
    195.5507306024,
    192.39613009499,
    189.23805952702,
    186.07651508142,
    182.91149293694,
    179.74298926809,
    176.57100024521,
    173.39552203441,
    170.21655079757,
    167.03408269237,
    163.84811387226,
    160.65864048644,
    157.4656586799,
    154.26916459338,
    151.06915436335,
    147.86562412208,
    144.65856999754,
    141.44798811346,
    138.23387458931,
    135.01622554028,
    131.7950370773,
    128.57030530701,
    125.34202633178,
    122.11019624967,
    118.87481115447,
    115.63586713566,
    112.39336027844,
    109.14728666367,
    105.89764236792,
    102.64442346345,
    99.387626018187,
    96.127246095732,
    92.863279755362,
    89.595723052018,
    86.324572036301,
    83.049822754466,
    79.771471248421,
    76.489513555719,
    73.203945709555,
    69.914763738761,
    66.621963667799,
    63.325541516758,
    60.025493301352,
    56.721815032909,
    53.41450271837,
    50.103552360285,
    46.788959956807,
    43.470721501684,
    40.148832984261,
    36.823290389469,
    33.494089697822,
    30.161226885415,
    26.824697923914,
    23.484498780555,
    20.140625418139,
    16.793073795024,
    13.441839865124,
    10.086919577901,
    6.7283088783613,
    3.3660037070526
  ],
  "interets_annuels": [
    6416.8586657028,
    6013.8913462552,
    5605.5725589313,
    5191.831235416,
    4772.5953635961,
    4347.7919750265,
    3917.3471322298,
    3481.1859158277,
    3039.2324115009,
    2591.4096967765,
    2137.6398276391,
    1677.8438249651,
    1211.9416607757,
    739.85224430842,
    261.49340790304
  ],
  "interets_total": 51406.487266854,
  "interets_total_assurance": 78406.487266854,
  "capital_restant_du_annuel": [
    469656.42618125,
    438909.88504304,
    407755.02511752,
    376186.42386848,
    344198.58674762,
    311785.94623819,
    278942.86088596,
    245663.61431733,
    211942.41424438,
    177773.3914567,
    143150.59879988,
    108068.01014039,
    72519.519316703,
    36498.939076554,
    0,
    0,
    0,
    0,
    0,
    0
  ],
  "code_postal": "75001",
  "prix_moyen": {
    "moyen": 1606500,
    "bas": 1445850,
    "haut": 2088450,
    "type": "lavieimmo"
  },
  "tableau_amortissement": [
    {
      "date": "05.01.2017",
      "dateStd": "2017-01-05",
      "mensualite": 3063.3693737045,
      "echeanceglobale": 3213.3693737045,
      "taux": "1.32",
      "interets": 550,
      "assurance": 150,
      "capitalamorti": 2513.3693737045,
      "capitalrestantdu": 497486.6306
    },
    {
      "date": "05.02.2017",
      "dateStd": "2017-02-05",
      "mensualite": 3063.3693737045,
      "echeanceglobale": 3213.3693737045,
      "taux": "1.32",
      "interets": 547.2353,
      "assurance": 150,
      "capitalamorti": 2516.1293737045,
      "capitalrestantdu": 494970.5013
    },
    {
      "date": "05.03.2017",
      "dateStd": "2017-03-05",
      "mensualite": 3063.3693737045,
      "echeanceglobale": 3213.3693737045,
      "taux": "1.32",
      "interets": 544.4676,
      "assurance": 150,
      "capitalamorti": 2518.8993737045,
      "capitalrestantdu": 492451.6019
    },
    {
      "date": "05.04.2017",
      "dateStd": "2017-04-05",
      "mensualite": 3063.3693737045,
      "echeanceglobale": 3213.3693737045,
      "taux": "1.32",
      "interets": 541.6968,
      "assurance": 150,
      "capitalamorti": 2521.6693737045,
      "capitalrestantdu": 489929.9325
    },
    {
      "date": "05.05.2017",
      "dateStd": "2017-05-05",
      "mensualite": 3063.3693737045,
      "echeanceglobale": 3213.3693737045,
      "taux": "1.32",
      "interets": 538.9229,
      "assurance": 150,
      "capitalamorti": 2524.4493737045,
      "capitalrestantdu": 487405.4831
    },
    {
      "date": "05.06.2017",
      "dateStd": "2017-06-05",
      "mensualite": 3063.3693737045,
      "echeanceglobale": 3213.3693737045,
      "taux": "1.32",
      "interets": 536.146,
      "assurance": 150,
      "capitalamorti": 2527.2193737045,
      "capitalrestantdu": 484878.2638
    },
    {
      "date": "05.07.2017",
      "dateStd": "2017-07-05",
      "mensualite": 3063.3693737045,
      "echeanceglobale": 3213.3693737045,
      "taux": "1.32",
      "interets": 533.3661,
      "assurance": 150,
      "capitalamorti": 2529.9993737045,
      "capitalrestantdu": 482348.2644
    },
    {
      "date": "05.08.2017",
      "dateStd": "2017-08-05",
      "mensualite": 3063.3693737045,
      "echeanceglobale": 3213.3693737045,
      "taux": "1.32",
      "interets": 530.5831,
      "assurance": 150,
      "capitalamorti": 2532.7893737045,
      "capitalrestantdu": 479815.475
    },
    {
      "date": "05.09.2017",
      "dateStd": "2017-09-05",
      "mensualite": 3063.3693737045,
      "echeanceglobale": 3213.3693737045,
      "taux": "1.32",
      "interets": 527.797,
      "assurance": 150,
      "capitalamorti": 2535.5693737045,
      "capitalrestantdu": 477279.9056
    },
    {
      "date": "05.10.2017",
      "dateStd": "2017-10-05",
      "mensualite": 3063.3693737045,
      "echeanceglobale": 3213.3693737045,
      "taux": "1.32",
      "interets": 525.0079,
      "assurance": 150,
      "capitalamorti": 2538.3593737045,
      "capitalrestantdu": 474741.5463
    },
    {
      "date": "05.11.2017",
      "dateStd": "2017-11-05",
      "mensualite": 3063.3693737045,
      "echeanceglobale": 3213.3693737045,
      "taux": "1.32",
      "interets": 522.2157,
      "assurance": 150,
      "capitalamorti": 2541.1493737045,
      "capitalrestantdu": 472200.3969
    },
    {
      "date": "05.12.2017",
      "dateStd": "2017-12-05",
      "mensualite": 3063.3693737045,
      "echeanceglobale": 3213.3693737045,
      "taux": "1.32",
      "interets": 519.4204,
      "assurance": 150,
      "capitalamorti": 2543.9493737045,
      "capitalrestantdu": 469656.4475
    },
    {
      "date": "05.01.2018",
      "dateStd": "2018-01-05",
      "mensualite": 3063.3693737045,
      "echeanceglobale": 3213.3693737045,
      "taux": "1.32",
      "interets": 516.6221,
      "assurance": 150,
      "capitalamorti": 2546.7493737045,
      "capitalrestantdu": 467109.6981
    },
    {
      "date": "05.02.2018",
      "dateStd": "2018-02-05",
      "mensualite": 3063.3693737045,
      "echeanceglobale": 3213.3693737045,
      "taux": "1.32",
      "interets": 513.8207,
      "assurance": 150,
      "capitalamorti": 2549.5493737045,
      "capitalrestantdu": 464560.1488
    },
    {
      "date": "05.03.2018",
      "dateStd": "2018-03-05",
      "mensualite": 3063.3693737045,
      "echeanceglobale": 3213.3693737045,
      "taux": "1.32",
      "interets": 511.0162,
      "assurance": 150,
      "capitalamorti": 2552.3493737045,
      "capitalrestantdu": 462007.7994
    },
    {
      "date": "05.04.2018",
      "dateStd": "2018-04-05",
      "mensualite": 3063.3693737045,
      "echeanceglobale": 3213.3693737045,
      "taux": "1.32",
      "interets": 508.2086,
      "assurance": 150,
      "capitalamorti": 2555.1593737045,
      "capitalrestantdu": 459452.64
    },
    {
      "date": "05.05.2018",
      "dateStd": "2018-05-05",
      "mensualite": 3063.3693737045,
      "echeanceglobale": 3213.3693737045,
      "taux": "1.32",
      "interets": 505.3979,
      "assurance": 150,
      "capitalamorti": 2557.9693737045,
      "capitalrestantdu": 456894.6706
    },
    {
      "date": "05.06.2018",
      "dateStd": "2018-06-05",
      "mensualite": 3063.3693737045,
      "echeanceglobale": 3213.3693737045,
      "taux": "1.32",
      "interets": 502.5841,
      "assurance": 150,
      "capitalamorti": 2560.7893737045,
      "capitalrestantdu": 454333.8813
    },
    {
      "date": "05.07.2018",
      "dateStd": "2018-07-05",
      "mensualite": 3063.3693737045,
      "echeanceglobale": 3213.3693737045,
      "taux": "1.32",
      "interets": 499.7673,
      "assurance": 150,
      "capitalamorti": 2563.5993737045,
      "capitalrestantdu": 451770.2819
    },
    {
      "date": "05.08.2018",
      "dateStd": "2018-08-05",
      "mensualite": 3063.3693737045,
      "echeanceglobale": 3213.3693737045,
      "taux": "1.32",
      "interets": 496.9473,
      "assurance": 150,
      "capitalamorti": 2566.4193737045,
      "capitalrestantdu": 449203.8625
    },
    {
      "date": "05.09.2018",
      "dateStd": "2018-09-05",
      "mensualite": 3063.3693737045,
      "echeanceglobale": 3213.3693737045,
      "taux": "1.32",
      "interets": 494.1242,
      "assurance": 150,
      "capitalamorti": 2569.2493737045,
      "capitalrestantdu": 446634.6132
    },
    {
      "date": "05.10.2018",
      "dateStd": "2018-10-05",
      "mensualite": 3063.3693737045,
      "echeanceglobale": 3213.3693737045,
      "taux": "1.32",
      "interets": 491.2981,
      "assurance": 150,
      "capitalamorti": 2572.0693737045,
      "capitalrestantdu": 444062.5438
    },
    {
      "date": "05.11.2018",
      "dateStd": "2018-11-05",
      "mensualite": 3063.3693737045,
      "echeanceglobale": 3213.3693737045,
      "taux": "1.32",
      "interets": 488.4688,
      "assurance": 150,
      "capitalamorti": 2574.8993737045,
      "capitalrestantdu": 441487.6444
    },
    {
      "date": "05.12.2018",
      "dateStd": "2018-12-05",
      "mensualite": 3063.3693737045,
      "echeanceglobale": 3213.3693737045,
      "taux": "1.32",
      "interets": 485.6364,
      "assurance": 150,
      "capitalamorti": 2577.7293737045,
      "capitalrestantdu": 438909.915
    },
    {
      "date": "05.01.2019",
      "dateStd": "2019-01-05",
      "mensualite": 3063.3693737045,
      "echeanceglobale": 3213.3693737045,
      "taux": "1.32",
      "interets": 482.8009,
      "assurance": 150,
      "capitalamorti": 2580.5693737045,
      "capitalrestantdu": 436329.3457
    },
    {
      "date": "05.02.2019",
      "dateStd": "2019-02-05",
      "mensualite": 3063.3693737045,
      "echeanceglobale": 3213.3693737045,
      "taux": "1.32",
      "interets": 479.9623,
      "assurance": 150,
      "capitalamorti": 2583.4093737045,
      "capitalrestantdu": 433745.9363
    },
    {
      "date": "05.03.2019",
      "dateStd": "2019-03-05",
      "mensualite": 3063.3693737045,
      "echeanceglobale": 3213.3693737045,
      "taux": "1.32",
      "interets": 477.1205,
      "assurance": 150,
      "capitalamorti": 2586.2493737045,
      "capitalrestantdu": 431159.6869
    },
    {
      "date": "05.04.2019",
      "dateStd": "2019-04-05",
      "mensualite": 3063.3693737045,
      "echeanceglobale": 3213.3693737045,
      "taux": "1.32",
      "interets": 474.2757,
      "assurance": 150,
      "capitalamorti": 2589.0893737045,
      "capitalrestantdu": 428570.5975
    },
    {
      "date": "05.05.2019",
      "dateStd": "2019-05-05",
      "mensualite": 3063.3693737045,
      "echeanceglobale": 3213.3693737045,
      "taux": "1.32",
      "interets": 471.4277,
      "assurance": 150,
      "capitalamorti": 2591.9393737045,
      "capitalrestantdu": 425978.6582
    },
    {
      "date": "05.06.2019",
      "dateStd": "2019-06-05",
      "mensualite": 3063.3693737045,
      "echeanceglobale": 3213.3693737045,
      "taux": "1.32",
      "interets": 468.5765,
      "assurance": 150,
      "capitalamorti": 2594.7893737045,
      "capitalrestantdu": 423383.8688
    },
    {
      "date": "05.07.2019",
      "dateStd": "2019-07-05",
      "mensualite": 3063.3693737045,
      "echeanceglobale": 3213.3693737045,
      "taux": "1.32",
      "interets": 465.7223,
      "assurance": 150,
      "capitalamorti": 2597.6493737045,
      "capitalrestantdu": 420786.2194
    },
    {
      "date": "05.08.2019",
      "dateStd": "2019-08-05",
      "mensualite": 3063.3693737045,
      "echeanceglobale": 3213.3693737045,
      "taux": "1.32",
      "interets": 462.8648,
      "assurance": 150,
      "capitalamorti": 2600.5093737045,
      "capitalrestantdu": 418185.71
    },
    {
      "date": "05.09.2019",
      "dateStd": "2019-09-05",
      "mensualite": 3063.3693737045,
      "echeanceglobale": 3213.3693737045,
      "taux": "1.32",
      "interets": 460.0043,
      "assurance": 150,
      "capitalamorti": 2603.3693737045,
      "capitalrestantdu": 415582.3407
    },
    {
      "date": "05.10.2019",
      "dateStd": "2019-10-05",
      "mensualite": 3063.3693737045,
      "echeanceglobale": 3213.3693737045,
      "taux": "1.32",
      "interets": 457.1406,
      "assurance": 150,
      "capitalamorti": 2606.2293737045,
      "capitalrestantdu": 412976.1113
    },
    {
      "date": "05.11.2019",
      "dateStd": "2019-11-05",
      "mensualite": 3063.3693737045,
      "echeanceglobale": 3213.3693737045,
      "taux": "1.32",
      "interets": 454.2737,
      "assurance": 150,
      "capitalamorti": 2609.0993737045,
      "capitalrestantdu": 410367.0119
    },
    {
      "date": "05.12.2019",
      "dateStd": "2019-12-05",
      "mensualite": 3063.3693737045,
      "echeanceglobale": 3213.3693737045,
      "taux": "1.32",
      "interets": 451.4037,
      "assurance": 150,
      "capitalamorti": 2611.9693737045,
      "capitalrestantdu": 407755.0425
    },
    {
      "date": "05.01.2020",
      "dateStd": "2020-01-05",
      "mensualite": 3063.3693737045,
      "echeanceglobale": 3213.3693737045,
      "taux": "1.32",
      "interets": 448.5305,
      "assurance": 150,
      "capitalamorti": 2614.8393737045,
      "capitalrestantdu": 405140.2032
    },
    {
      "date": "05.02.2020",
      "dateStd": "2020-02-05",
      "mensualite": 3063.3693737045,
      "echeanceglobale": 3213.3693737045,
      "taux": "1.32",
      "interets": 445.6542,
      "assurance": 150,
      "capitalamorti": 2617.7193737045,
      "capitalrestantdu": 402522.4838
    },
    {
      "date": "05.03.2020",
      "dateStd": "2020-03-05",
      "mensualite": 3063.3693737045,
      "echeanceglobale": 3213.3693737045,
      "taux": "1.32",
      "interets": 442.7747,
      "assurance": 150,
      "capitalamorti": 2620.5993737045,
      "capitalrestantdu": 399901.8844
    },
    {
      "date": "05.04.2020",
      "dateStd": "2020-04-05",
      "mensualite": 3063.3693737045,
      "echeanceglobale": 3213.3693737045,
      "taux": "1.32",
      "interets": 439.8921,
      "assurance": 150,
      "capitalamorti": 2623.4793737045,
      "capitalrestantdu": 397278.4051
    },
    {
      "date": "05.05.2020",
      "dateStd": "2020-05-05",
      "mensualite": 3063.3693737045,
      "echeanceglobale": 3213.3693737045,
      "taux": "1.32",
      "interets": 437.0062,
      "assurance": 150,
      "capitalamorti": 2626.3593737045,
      "capitalrestantdu": 394652.0457
    },
    {
      "date": "05.06.2020",
      "dateStd": "2020-06-05",
      "mensualite": 3063.3693737045,
      "echeanceglobale": 3213.3693737045,
      "taux": "1.32",
      "interets": 434.1173,
      "assurance": 150,
      "capitalamorti": 2629.2493737045,
      "capitalrestantdu": 392022.7963
    },
    {
      "date": "05.07.2020",
      "dateStd": "2020-07-05",
      "mensualite": 3063.3693737045,
      "echeanceglobale": 3213.3693737045,
      "taux": "1.32",
      "interets": 431.2251,
      "assurance": 150,
      "capitalamorti": 2632.1393737045,
      "capitalrestantdu": 389390.6569
    },
    {
      "date": "05.08.2020",
      "dateStd": "2020-08-05",
      "mensualite": 3063.3693737045,
      "echeanceglobale": 3213.3693737045,
      "taux": "1.32",
      "interets": 428.3297,
      "assurance": 150,
      "capitalamorti": 2635.0393737045,
      "capitalrestantdu": 386755.6176
    },
    {
      "date": "05.09.2020",
      "dateStd": "2020-09-05",
      "mensualite": 3063.3693737045,
      "echeanceglobale": 3213.3693737045,
      "taux": "1.32",
      "interets": 425.4312,
      "assurance": 150,
      "capitalamorti": 2637.9393737045,
      "capitalrestantdu": 384117.6782
    },
    {
      "date": "05.10.2020",
      "dateStd": "2020-10-05",
      "mensualite": 3063.3693737045,
      "echeanceglobale": 3213.3693737045,
      "taux": "1.32",
      "interets": 422.5294,
      "assurance": 150,
      "capitalamorti": 2640.8393737045,
      "capitalrestantdu": 381476.8388
    },
    {
      "date": "05.11.2020",
      "dateStd": "2020-11-05",
      "mensualite": 3063.3693737045,
      "echeanceglobale": 3213.3693737045,
      "taux": "1.32",
      "interets": 419.6245,
      "assurance": 150,
      "capitalamorti": 2643.7493737045,
      "capitalrestantdu": 378833.0894
    },
    {
      "date": "05.12.2020",
      "dateStd": "2020-12-05",
      "mensualite": 3063.3693737045,
      "echeanceglobale": 3213.3693737045,
      "taux": "1.32",
      "interets": 416.7164,
      "assurance": 150,
      "capitalamorti": 2646.6493737045,
      "capitalrestantdu": 376186.4401
    },
    {
      "date": "05.01.2021",
      "dateStd": "2021-01-05",
      "mensualite": 3063.3693737045,
      "echeanceglobale": 3213.3693737045,
      "taux": "1.32",
      "interets": 413.8051,
      "assurance": 150,
      "capitalamorti": 2649.5593737045,
      "capitalrestantdu": 373536.8807
    },
    {
      "date": "05.02.2021",
      "dateStd": "2021-02-05",
      "mensualite": 3063.3693737045,
      "echeanceglobale": 3213.3693737045,
      "taux": "1.32",
      "interets": 410.8906,
      "assurance": 150,
      "capitalamorti": 2652.4793737045,
      "capitalrestantdu": 370884.4013
    },
    {
      "date": "05.03.2021",
      "dateStd": "2021-03-05",
      "mensualite": 3063.3693737045,
      "echeanceglobale": 3213.3693737045,
      "taux": "1.32",
      "interets": 407.9728,
      "assurance": 150,
      "capitalamorti": 2655.3993737045,
      "capitalrestantdu": 368229.0019
    },
    {
      "date": "05.04.2021",
      "dateStd": "2021-04-05",
      "mensualite": 3063.3693737045,
      "echeanceglobale": 3213.3693737045,
      "taux": "1.32",
      "interets": 405.0519,
      "assurance": 150,
      "capitalamorti": 2658.3193737045,
      "capitalrestantdu": 365570.6826
    },
    {
      "date": "05.05.2021",
      "dateStd": "2021-05-05",
      "mensualite": 3063.3693737045,
      "echeanceglobale": 3213.3693737045,
      "taux": "1.32",
      "interets": 402.1278,
      "assurance": 150,
      "capitalamorti": 2661.2393737045,
      "capitalrestantdu": 362909.4432
    },
    {
      "date": "05.06.2021",
      "dateStd": "2021-06-05",
      "mensualite": 3063.3693737045,
      "echeanceglobale": 3213.3693737045,
      "taux": "1.32",
      "interets": 399.2004,
      "assurance": 150,
      "capitalamorti": 2664.1693737045,
      "capitalrestantdu": 360245.2738
    },
    {
      "date": "05.07.2021",
      "dateStd": "2021-07-05",
      "mensualite": 3063.3693737045,
      "echeanceglobale": 3213.3693737045,
      "taux": "1.32",
      "interets": 396.2698,
      "assurance": 150,
      "capitalamorti": 2667.0993737045,
      "capitalrestantdu": 357578.1744
    },
    {
      "date": "05.08.2021",
      "dateStd": "2021-08-05",
      "mensualite": 3063.3693737045,
      "echeanceglobale": 3213.3693737045,
      "taux": "1.32",
      "interets": 393.336,
      "assurance": 150,
      "capitalamorti": 2670.0293737045,
      "capitalrestantdu": 354908.1451
    },
    {
      "date": "05.09.2021",
      "dateStd": "2021-09-05",
      "mensualite": 3063.3693737045,
      "echeanceglobale": 3213.3693737045,
      "taux": "1.32",
      "interets": 390.399,
      "assurance": 150,
      "capitalamorti": 2672.9693737045,
      "capitalrestantdu": 352235.1757
    },
    {
      "date": "05.10.2021",
      "dateStd": "2021-10-05",
      "mensualite": 3063.3693737045,
      "echeanceglobale": 3213.3693737045,
      "taux": "1.32",
      "interets": 387.4587,
      "assurance": 150,
      "capitalamorti": 2675.9093737045,
      "capitalrestantdu": 349559.2663
    },
    {
      "date": "05.11.2021",
      "dateStd": "2021-11-05",
      "mensualite": 3063.3693737045,
      "echeanceglobale": 3213.3693737045,
      "taux": "1.32",
      "interets": 384.5152,
      "assurance": 150,
      "capitalamorti": 2678.8493737045,
      "capitalrestantdu": 346880.417
    },
    {
      "date": "05.12.2021",
      "dateStd": "2021-12-05",
      "mensualite": 3063.3693737045,
      "echeanceglobale": 3213.3693737045,
      "taux": "1.32",
      "interets": 381.5685,
      "assurance": 150,
      "capitalamorti": 2681.7993737045,
      "capitalrestantdu": 344198.6176
    },
    {
      "date": "05.01.2022",
      "dateStd": "2022-01-05",
      "mensualite": 3063.3693737045,
      "echeanceglobale": 3213.3693737045,
      "taux": "1.32",
      "interets": 378.6185,
      "assurance": 150,
      "capitalamorti": 2684.7493737045,
      "capitalrestantdu": 341513.8682
    },
    {
      "date": "05.02.2022",
      "dateStd": "2022-02-05",
      "mensualite": 3063.3693737045,
      "echeanceglobale": 3213.3693737045,
      "taux": "1.32",
      "interets": 375.6653,
      "assurance": 150,
      "capitalamorti": 2687.6993737045,
      "capitalrestantdu": 338826.1688
    },
    {
      "date": "05.03.2022",
      "dateStd": "2022-03-05",
      "mensualite": 3063.3693737045,
      "echeanceglobale": 3213.3693737045,
      "taux": "1.32",
      "interets": 372.7088,
      "assurance": 150,
      "capitalamorti": 2690.6593737045,
      "capitalrestantdu": 336135.5095
    },
    {
      "date": "05.04.2022",
      "dateStd": "2022-04-05",
      "mensualite": 3063.3693737045,
      "echeanceglobale": 3213.3693737045,
      "taux": "1.32",
      "interets": 369.7491,
      "assurance": 150,
      "capitalamorti": 2693.6193737045,
      "capitalrestantdu": 333441.8901
    },
    {
      "date": "05.05.2022",
      "dateStd": "2022-05-05",
      "mensualite": 3063.3693737045,
      "echeanceglobale": 3213.3693737045,
      "taux": "1.32",
      "interets": 366.7861,
      "assurance": 150,
      "capitalamorti": 2696.5793737045,
      "capitalrestantdu": 330745.3107
    },
    {
      "date": "05.06.2022",
      "dateStd": "2022-06-05",
      "mensualite": 3063.3693737045,
      "echeanceglobale": 3213.3693737045,
      "taux": "1.32",
      "interets": 363.8198,
      "assurance": 150,
      "capitalamorti": 2699.5493737045,
      "capitalrestantdu": 328045.7613
    },
    {
      "date": "05.07.2022",
      "dateStd": "2022-07-05",
      "mensualite": 3063.3693737045,
      "echeanceglobale": 3213.3693737045,
      "taux": "1.32",
      "interets": 360.8503,
      "assurance": 150,
      "capitalamorti": 2702.5193737045,
      "capitalrestantdu": 325343.242
    },
    {
      "date": "05.08.2022",
      "dateStd": "2022-08-05",
      "mensualite": 3063.3693737045,
      "echeanceglobale": 3213.3693737045,
      "taux": "1.32",
      "interets": 357.8776,
      "assurance": 150,
      "capitalamorti": 2705.4893737045,
      "capitalrestantdu": 322637.7526
    },
    {
      "date": "05.09.2022",
      "dateStd": "2022-09-05",
      "mensualite": 3063.3693737045,
      "echeanceglobale": 3213.3693737045,
      "taux": "1.32",
      "interets": 354.9015,
      "assurance": 150,
      "capitalamorti": 2708.4693737045,
      "capitalrestantdu": 319929.2832
    },
    {
      "date": "05.10.2022",
      "dateStd": "2022-10-05",
      "mensualite": 3063.3693737045,
      "echeanceglobale": 3213.3693737045,
      "taux": "1.32",
      "interets": 351.9222,
      "assurance": 150,
      "capitalamorti": 2711.4493737045,
      "capitalrestantdu": 317217.8338
    },
    {
      "date": "05.11.2022",
      "dateStd": "2022-11-05",
      "mensualite": 3063.3693737045,
      "echeanceglobale": 3213.3693737045,
      "taux": "1.32",
      "interets": 348.9396,
      "assurance": 150,
      "capitalamorti": 2714.4293737045,
      "capitalrestantdu": 314503.4045
    },
    {
      "date": "05.12.2022",
      "dateStd": "2022-12-05",
      "mensualite": 3063.3693737045,
      "echeanceglobale": 3213.3693737045,
      "taux": "1.32",
      "interets": 345.9537,
      "assurance": 150,
      "capitalamorti": 2717.4193737045,
      "capitalrestantdu": 311785.9851
    },
    {
      "date": "05.01.2023",
      "dateStd": "2023-01-05",
      "mensualite": 3063.3693737045,
      "echeanceglobale": 3213.3693737045,
      "taux": "1.32",
      "interets": 342.9646,
      "assurance": 150,
      "capitalamorti": 2720.4093737045,
      "capitalrestantdu": 309065.5757
    },
    {
      "date": "05.02.2023",
      "dateStd": "2023-02-05",
      "mensualite": 3063.3693737045,
      "echeanceglobale": 3213.3693737045,
      "taux": "1.32",
      "interets": 339.9721,
      "assurance": 150,
      "capitalamorti": 2723.3993737045,
      "capitalrestantdu": 306342.1763
    },
    {
      "date": "05.03.2023",
      "dateStd": "2023-03-05",
      "mensualite": 3063.3693737045,
      "echeanceglobale": 3213.3693737045,
      "taux": "1.32",
      "interets": 336.9764,
      "assurance": 150,
      "capitalamorti": 2726.3893737045,
      "capitalrestantdu": 303615.787
    },
    {
      "date": "05.04.2023",
      "dateStd": "2023-04-05",
      "mensualite": 3063.3693737045,
      "echeanceglobale": 3213.3693737045,
      "taux": "1.32",
      "interets": 333.9774,
      "assurance": 150,
      "capitalamorti": 2729.3893737045,
      "capitalrestantdu": 300886.3976
    },
    {
      "date": "05.05.2023",
      "dateStd": "2023-05-05",
      "mensualite": 3063.3693737045,
      "echeanceglobale": 3213.3693737045,
      "taux": "1.32",
      "interets": 330.975,
      "assurance": 150,
      "capitalamorti": 2732.3893737045,
      "capitalrestantdu": 298154.0082
    },
    {
      "date": "05.06.2023",
      "dateStd": "2023-06-05",
      "mensualite": 3063.3693737045,
      "echeanceglobale": 3213.3693737045,
      "taux": "1.32",
      "interets": 327.9694,
      "assurance": 150,
      "capitalamorti": 2735.3993737045,
      "capitalrestantdu": 295418.6089
    },
    {
      "date": "05.07.2023",
      "dateStd": "2023-07-05",
      "mensualite": 3063.3693737045,
      "echeanceglobale": 3213.3693737045,
      "taux": "1.32",
      "interets": 324.9605,
      "assurance": 150,
      "capitalamorti": 2738.4093737045,
      "capitalrestantdu": 292680.1995
    },
    {
      "date": "05.08.2023",
      "dateStd": "2023-08-05",
      "mensualite": 3063.3693737045,
      "echeanceglobale": 3213.3693737045,
      "taux": "1.32",
      "interets": 321.9482,
      "assurance": 150,
      "capitalamorti": 2741.4193737045,
      "capitalrestantdu": 289938.7801
    },
    {
      "date": "05.09.2023",
      "dateStd": "2023-09-05",
      "mensualite": 3063.3693737045,
      "echeanceglobale": 3213.3693737045,
      "taux": "1.32",
      "interets": 318.9327,
      "assurance": 150,
      "capitalamorti": 2744.4393737045,
      "capitalrestantdu": 287194.3407
    },
    {
      "date": "05.10.2023",
      "dateStd": "2023-10-05",
      "mensualite": 3063.3693737045,
      "echeanceglobale": 3213.3693737045,
      "taux": "1.32",
      "interets": 315.9138,
      "assurance": 150,
      "capitalamorti": 2747.4593737045,
      "capitalrestantdu": 284446.8814
    },
    {
      "date": "05.11.2023",
      "dateStd": "2023-11-05",
      "mensualite": 3063.3693737045,
      "echeanceglobale": 3213.3693737045,
      "taux": "1.32",
      "interets": 312.8916,
      "assurance": 150,
      "capitalamorti": 2750.4793737045,
      "capitalrestantdu": 281696.402
    },
    {
      "date": "05.12.2023",
      "dateStd": "2023-12-05",
      "mensualite": 3063.3693737045,
      "echeanceglobale": 3213.3693737045,
      "taux": "1.32",
      "interets": 309.866,
      "assurance": 150,
      "capitalamorti": 2753.4993737045,
      "capitalrestantdu": 278942.9026
    },
    {
      "date": "05.01.2024",
      "dateStd": "2024-01-05",
      "mensualite": 3063.3693737045,
      "echeanceglobale": 3213.3693737045,
      "taux": "1.32",
      "interets": 306.8372,
      "assurance": 150,
      "capitalamorti": 2756.5293737045,
      "capitalrestantdu": 276186.3732
    },
    {
      "date": "05.02.2024",
      "dateStd": "2024-02-05",
      "mensualite": 3063.3693737045,
      "echeanceglobale": 3213.3693737045,
      "taux": "1.32",
      "interets": 303.805,
      "assurance": 150,
      "capitalamorti": 2759.5593737045,
      "capitalrestantdu": 273426.8139
    },
    {
      "date": "05.03.2024",
      "dateStd": "2024-03-05",
      "mensualite": 3063.3693737045,
      "echeanceglobale": 3213.3693737045,
      "taux": "1.32",
      "interets": 300.7695,
      "assurance": 150,
      "capitalamorti": 2762.5993737045,
      "capitalrestantdu": 270664.2145
    },
    {
      "date": "05.04.2024",
      "dateStd": "2024-04-05",
      "mensualite": 3063.3693737045,
      "echeanceglobale": 3213.3693737045,
      "taux": "1.32",
      "interets": 297.7306,
      "assurance": 150,
      "capitalamorti": 2765.6393737045,
      "capitalrestantdu": 267898.5751
    },
    {
      "date": "05.05.2024",
      "dateStd": "2024-05-05",
      "mensualite": 3063.3693737045,
      "echeanceglobale": 3213.3693737045,
      "taux": "1.32",
      "interets": 294.6884,
      "assurance": 150,
      "capitalamorti": 2768.6793737045,
      "capitalrestantdu": 265129.8957
    },
    {
      "date": "05.06.2024",
      "dateStd": "2024-06-05",
      "mensualite": 3063.3693737045,
      "echeanceglobale": 3213.3693737045,
      "taux": "1.32",
      "interets": 291.6429,
      "assurance": 150,
      "capitalamorti": 2771.7293737045,
      "capitalrestantdu": 262358.1664
    },
    {
      "date": "05.07.2024",
      "dateStd": "2024-07-05",
      "mensualite": 3063.3693737045,
      "echeanceglobale": 3213.3693737045,
      "taux": "1.32",
      "interets": 288.594,
      "assurance": 150,
      "capitalamorti": 2774.7793737045,
      "capitalrestantdu": 259583.387
    },
    {
      "date": "05.08.2024",
      "dateStd": "2024-08-05",
      "mensualite": 3063.3693737045,
      "echeanceglobale": 3213.3693737045,
      "taux": "1.32",
      "interets": 285.5417,
      "assurance": 150,
      "capitalamorti": 2777.8293737045,
      "capitalrestantdu": 256805.5576
    },
    {
      "date": "05.09.2024",
      "dateStd": "2024-09-05",
      "mensualite": 3063.3693737045,
      "echeanceglobale": 3213.3693737045,
      "taux": "1.32",
      "interets": 282.4861,
      "assurance": 150,
      "capitalamorti": 2780.8793737045,
      "capitalrestantdu": 254024.6782
    },
    {
      "date": "05.10.2024",
      "dateStd": "2024-10-05",
      "mensualite": 3063.3693737045,
      "echeanceglobale": 3213.3693737045,
      "taux": "1.32",
      "interets": 279.4271,
      "assurance": 150,
      "capitalamorti": 2783.9393737045,
      "capitalrestantdu": 251240.7389
    },
    {
      "date": "05.11.2024",
      "dateStd": "2024-11-05",
      "mensualite": 3063.3693737045,
      "echeanceglobale": 3213.3693737045,
      "taux": "1.32",
      "interets": 276.3648,
      "assurance": 150,
      "capitalamorti": 2787.0093737045,
      "capitalrestantdu": 248453.7295
    },
    {
      "date": "05.12.2024",
      "dateStd": "2024-12-05",
      "mensualite": 3063.3693737045,
      "echeanceglobale": 3213.3693737045,
      "taux": "1.32",
      "interets": 273.2991,
      "assurance": 150,
      "capitalamorti": 2790.0693737045,
      "capitalrestantdu": 245663.6601
    },
    {
      "date": "05.01.2025",
      "dateStd": "2025-01-05",
      "mensualite": 3063.3693737045,
      "echeanceglobale": 3213.3693737045,
      "taux": "1.32",
      "interets": 270.23,
      "assurance": 150,
      "capitalamorti": 2793.1393737045,
      "capitalrestantdu": 242870.5208
    },
    {
      "date": "05.02.2025",
      "dateStd": "2025-02-05",
      "mensualite": 3063.3693737045,
      "echeanceglobale": 3213.3693737045,
      "taux": "1.32",
      "interets": 267.1576,
      "assurance": 150,
      "capitalamorti": 2796.2093737045,
      "capitalrestantdu": 240074.3114
    },
    {
      "date": "05.03.2025",
      "dateStd": "2025-03-05",
      "mensualite": 3063.3693737045,
      "echeanceglobale": 3213.3693737045,
      "taux": "1.32",
      "interets": 264.0817,
      "assurance": 150,
      "capitalamorti": 2799.2893737045,
      "capitalrestantdu": 237275.022
    },
    {
      "date": "05.04.2025",
      "dateStd": "2025-04-05",
      "mensualite": 3063.3693737045,
      "echeanceglobale": 3213.3693737045,
      "taux": "1.32",
      "interets": 261.0025,
      "assurance": 150,
      "capitalamorti": 2802.3693737045,
      "capitalrestantdu": 234472.6526
    },
    {
      "date": "05.05.2025",
      "dateStd": "2025-05-05",
      "mensualite": 3063.3693737045,
      "echeanceglobale": 3213.3693737045,
      "taux": "1.32",
      "interets": 257.9199,
      "assurance": 150,
      "capitalamorti": 2805.4493737045,
      "capitalrestantdu": 231667.2033
    },
    {
      "date": "05.06.2025",
      "dateStd": "2025-06-05",
      "mensualite": 3063.3693737045,
      "echeanceglobale": 3213.3693737045,
      "taux": "1.32",
      "interets": 254.8339,
      "assurance": 150,
      "capitalamorti": 2808.5393737045,
      "capitalrestantdu": 228858.6639
    },
    {
      "date": "05.07.2025",
      "dateStd": "2025-07-05",
      "mensualite": 3063.3693737045,
      "echeanceglobale": 3213.3693737045,
      "taux": "1.32",
      "interets": 251.7445,
      "assurance": 150,
      "capitalamorti": 2811.6293737045,
      "capitalrestantdu": 226047.0345
    },
    {
      "date": "05.08.2025",
      "dateStd": "2025-08-05",
      "mensualite": 3063.3693737045,
      "echeanceglobale": 3213.3693737045,
      "taux": "1.32",
      "interets": 248.6517,
      "assurance": 150,
      "capitalamorti": 2814.7193737045,
      "capitalrestantdu": 223232.3151
    },
    {
      "date": "05.09.2025",
      "dateStd": "2025-09-05",
      "mensualite": 3063.3693737045,
      "echeanceglobale": 3213.3693737045,
      "taux": "1.32",
      "interets": 245.5555,
      "assurance": 150,
      "capitalamorti": 2817.8093737045,
      "capitalrestantdu": 220414.5058
    },
    {
      "date": "05.10.2025",
      "dateStd": "2025-10-05",
      "mensualite": 3063.3693737045,
      "echeanceglobale": 3213.3693737045,
      "taux": "1.32",
      "interets": 242.456,
      "assurance": 150,
      "capitalamorti": 2820.9093737045,
      "capitalrestantdu": 217593.5964
    },
    {
      "date": "05.11.2025",
      "dateStd": "2025-11-05",
      "mensualite": 3063.3693737045,
      "echeanceglobale": 3213.3693737045,
      "taux": "1.32",
      "interets": 239.353,
      "assurance": 150,
      "capitalamorti": 2824.0193737045,
      "capitalrestantdu": 214769.577
    },
    {
      "date": "05.12.2025",
      "dateStd": "2025-12-05",
      "mensualite": 3063.3693737045,
      "echeanceglobale": 3213.3693737045,
      "taux": "1.32",
      "interets": 236.2465,
      "assurance": 150,
      "capitalamorti": 2827.1193737045,
      "capitalrestantdu": 211942.4576
    },
    {
      "date": "05.01.2026",
      "dateStd": "2026-01-05",
      "mensualite": 3063.3693737045,
      "echeanceglobale": 3213.3693737045,
      "taux": "1.32",
      "interets": 233.1367,
      "assurance": 150,
      "capitalamorti": 2830.2293737045,
      "capitalrestantdu": 209112.2283
    },
    {
      "date": "05.02.2026",
      "dateStd": "2026-02-05",
      "mensualite": 3063.3693737045,
      "echeanceglobale": 3213.3693737045,
      "taux": "1.32",
      "interets": 230.0235,
      "assurance": 150,
      "capitalamorti": 2833.3493737045,
      "capitalrestantdu": 206278.8789
    },
    {
      "date": "05.03.2026",
      "dateStd": "2026-03-05",
      "mensualite": 3063.3693737045,
      "echeanceglobale": 3213.3693737045,
      "taux": "1.32",
      "interets": 226.9068,
      "assurance": 150,
      "capitalamorti": 2836.4593737045,
      "capitalrestantdu": 203442.4195
    },
    {
      "date": "05.04.2026",
      "dateStd": "2026-04-05",
      "mensualite": 3063.3693737045,
      "echeanceglobale": 3213.3693737045,
      "taux": "1.32",
      "interets": 223.7867,
      "assurance": 150,
      "capitalamorti": 2839.5793737045,
      "capitalrestantdu": 200602.8401
    },
    {
      "date": "05.05.2026",
      "dateStd": "2026-05-05",
      "mensualite": 3063.3693737045,
      "echeanceglobale": 3213.3693737045,
      "taux": "1.32",
      "interets": 220.6631,
      "assurance": 150,
      "capitalamorti": 2842.7093737045,
      "capitalrestantdu": 197760.1308
    },
    {
      "date": "05.06.2026",
      "dateStd": "2026-06-05",
      "mensualite": 3063.3693737045,
      "echeanceglobale": 3213.3693737045,
      "taux": "1.32",
      "interets": 217.5361,
      "assurance": 150,
      "capitalamorti": 2845.8293737045,
      "capitalrestantdu": 194914.3014
    },
    {
      "date": "05.07.2026",
      "dateStd": "2026-07-05",
      "mensualite": 3063.3693737045,
      "echeanceglobale": 3213.3693737045,
      "taux": "1.32",
      "interets": 214.4057,
      "assurance": 150,
      "capitalamorti": 2848.9593737045,
      "capitalrestantdu": 192065.342
    },
    {
      "date": "05.08.2026",
      "dateStd": "2026-08-05",
      "mensualite": 3063.3693737045,
      "echeanceglobale": 3213.3693737045,
      "taux": "1.32",
      "interets": 211.2719,
      "assurance": 150,
      "capitalamorti": 2852.0993737045,
      "capitalrestantdu": 189213.2427
    },
    {
      "date": "05.09.2026",
      "dateStd": "2026-09-05",
      "mensualite": 3063.3693737045,
      "echeanceglobale": 3213.3693737045,
      "taux": "1.32",
      "interets": 208.1346,
      "assurance": 150,
      "capitalamorti": 2855.2393737045,
      "capitalrestantdu": 186358.0033
    },
    {
      "date": "05.10.2026",
      "dateStd": "2026-10-05",
      "mensualite": 3063.3693737045,
      "echeanceglobale": 3213.3693737045,
      "taux": "1.32",
      "interets": 204.9938,
      "assurance": 150,
      "capitalamorti": 2858.3793737045,
      "capitalrestantdu": 183499.6239
    },
    {
      "date": "05.11.2026",
      "dateStd": "2026-11-05",
      "mensualite": 3063.3693737045,
      "echeanceglobale": 3213.3693737045,
      "taux": "1.32",
      "interets": 201.8496,
      "assurance": 150,
      "capitalamorti": 2861.5193737045,
      "capitalrestantdu": 180638.1045
    },
    {
      "date": "05.12.2026",
      "dateStd": "2026-12-05",
      "mensualite": 3063.3693737045,
      "echeanceglobale": 3213.3693737045,
      "taux": "1.32",
      "interets": 198.7019,
      "assurance": 150,
      "capitalamorti": 2864.6693737045,
      "capitalrestantdu": 177773.4352
    },
    {
      "date": "05.01.2027",
      "dateStd": "2027-01-05",
      "mensualite": 3063.3693737045,
      "echeanceglobale": 3213.3693737045,
      "taux": "1.32",
      "interets": 195.5508,
      "assurance": 150,
      "capitalamorti": 2867.8193737045,
      "capitalrestantdu": 174905.6158
    },
    {
      "date": "05.02.2027",
      "dateStd": "2027-02-05",
      "mensualite": 3063.3693737045,
      "echeanceglobale": 3213.3693737045,
      "taux": "1.32",
      "interets": 192.3962,
      "assurance": 150,
      "capitalamorti": 2870.9693737045,
      "capitalrestantdu": 172034.6464
    },
    {
      "date": "05.03.2027",
      "dateStd": "2027-03-05",
      "mensualite": 3063.3693737045,
      "echeanceglobale": 3213.3693737045,
      "taux": "1.32",
      "interets": 189.2381,
      "assurance": 150,
      "capitalamorti": 2874.1293737045,
      "capitalrestantdu": 169160.517
    },
    {
      "date": "05.04.2027",
      "dateStd": "2027-04-05",
      "mensualite": 3063.3693737045,
      "echeanceglobale": 3213.3693737045,
      "taux": "1.32",
      "interets": 186.0766,
      "assurance": 150,
      "capitalamorti": 2877.2893737045,
      "capitalrestantdu": 166283.2277
    },
    {
      "date": "05.05.2027",
      "dateStd": "2027-05-05",
      "mensualite": 3063.3693737045,
      "echeanceglobale": 3213.3693737045,
      "taux": "1.32",
      "interets": 182.9116,
      "assurance": 150,
      "capitalamorti": 2880.4593737045,
      "capitalrestantdu": 163402.7683
    },
    {
      "date": "05.06.2027",
      "dateStd": "2027-06-05",
      "mensualite": 3063.3693737045,
      "echeanceglobale": 3213.3693737045,
      "taux": "1.32",
      "interets": 179.743,
      "assurance": 150,
      "capitalamorti": 2883.6293737045,
      "capitalrestantdu": 160519.1389
    },
    {
      "date": "05.07.2027",
      "dateStd": "2027-07-05",
      "mensualite": 3063.3693737045,
      "echeanceglobale": 3213.3693737045,
      "taux": "1.32",
      "interets": 176.5711,
      "assurance": 150,
      "capitalamorti": 2886.7993737045,
      "capitalrestantdu": 157632.3395
    },
    {
      "date": "05.08.2027",
      "dateStd": "2027-08-05",
      "mensualite": 3063.3693737045,
      "echeanceglobale": 3213.3693737045,
      "taux": "1.32",
      "interets": 173.3956,
      "assurance": 150,
      "capitalamorti": 2889.9693737045,
      "capitalrestantdu": 154742.3702
    },
    {
      "date": "05.09.2027",
      "dateStd": "2027-09-05",
      "mensualite": 3063.3693737045,
      "echeanceglobale": 3213.3693737045,
      "taux": "1.32",
      "interets": 170.2166,
      "assurance": 150,
      "capitalamorti": 2893.1493737045,
      "capitalrestantdu": 151849.2208
    },
    {
      "date": "05.10.2027",
      "dateStd": "2027-10-05",
      "mensualite": 3063.3693737045,
      "echeanceglobale": 3213.3693737045,
      "taux": "1.32",
      "interets": 167.0341,
      "assurance": 150,
      "capitalamorti": 2896.3393737045,
      "capitalrestantdu": 148952.8814
    },
    {
      "date": "05.11.2027",
      "dateStd": "2027-11-05",
      "mensualite": 3063.3693737045,
      "echeanceglobale": 3213.3693737045,
      "taux": "1.32",
      "interets": 163.8482,
      "assurance": 150,
      "capitalamorti": 2899.5193737045,
      "capitalrestantdu": 146053.362
    },
    {
      "date": "05.12.2027",
      "dateStd": "2027-12-05",
      "mensualite": 3063.3693737045,
      "echeanceglobale": 3213.3693737045,
      "taux": "1.32",
      "interets": 160.6587,
      "assurance": 150,
      "capitalamorti": 2902.7093737045,
      "capitalrestantdu": 143150.6527
    },
    {
      "date": "05.01.2028",
      "dateStd": "2028-01-05",
      "mensualite": 3063.3693737045,
      "echeanceglobale": 3213.3693737045,
      "taux": "1.32",
      "interets": 157.4657,
      "assurance": 150,
      "capitalamorti": 2905.8993737045,
      "capitalrestantdu": 140244.7533
    },
    {
      "date": "05.02.2028",
      "dateStd": "2028-02-05",
      "mensualite": 3063.3693737045,
      "echeanceglobale": 3213.3693737045,
      "taux": "1.32",
      "interets": 154.2692,
      "assurance": 150,
      "capitalamorti": 2909.0993737045,
      "capitalrestantdu": 137335.6539
    },
    {
      "date": "05.03.2028",
      "dateStd": "2028-03-05",
      "mensualite": 3063.3693737045,
      "echeanceglobale": 3213.3693737045,
      "taux": "1.32",
      "interets": 151.0692,
      "assurance": 150,
      "capitalamorti": 2912.2993737045,
      "capitalrestantdu": 134423.3545
    },
    {
      "date": "05.04.2028",
      "dateStd": "2028-04-05",
      "mensualite": 3063.3693737045,
      "echeanceglobale": 3213.3693737045,
      "taux": "1.32",
      "interets": 147.8657,
      "assurance": 150,
      "capitalamorti": 2915.4993737045,
      "capitalrestantdu": 131507.8552
    },
    {
      "date": "05.05.2028",
      "dateStd": "2028-05-05",
      "mensualite": 3063.3693737045,
      "echeanceglobale": 3213.3693737045,
      "taux": "1.32",
      "interets": 144.6586,
      "assurance": 150,
      "capitalamorti": 2918.7093737045,
      "capitalrestantdu": 128589.1458
    },
    {
      "date": "05.06.2028",
      "dateStd": "2028-06-05",
      "mensualite": 3063.3693737045,
      "echeanceglobale": 3213.3693737045,
      "taux": "1.32",
      "interets": 141.4481,
      "assurance": 150,
      "capitalamorti": 2921.9193737045,
      "capitalrestantdu": 125667.2264
    },
    {
      "date": "05.07.2028",
      "dateStd": "2028-07-05",
      "mensualite": 3063.3693737045,
      "echeanceglobale": 3213.3693737045,
      "taux": "1.32",
      "interets": 138.2339,
      "assurance": 150,
      "capitalamorti": 2925.1393737045,
      "capitalrestantdu": 122742.0871
    },
    {
      "date": "05.08.2028",
      "dateStd": "2028-08-05",
      "mensualite": 3063.3693737045,
      "echeanceglobale": 3213.3693737045,
      "taux": "1.32",
      "interets": 135.0163,
      "assurance": 150,
      "capitalamorti": 2928.3493737045,
      "capitalrestantdu": 119813.7377
    },
    {
      "date": "05.09.2028",
      "dateStd": "2028-09-05",
      "mensualite": 3063.3693737045,
      "echeanceglobale": 3213.3693737045,
      "taux": "1.32",
      "interets": 131.7951,
      "assurance": 150,
      "capitalamorti": 2931.5693737045,
      "capitalrestantdu": 116882.1683
    },
    {
      "date": "05.10.2028",
      "dateStd": "2028-10-05",
      "mensualite": 3063.3693737045,
      "echeanceglobale": 3213.3693737045,
      "taux": "1.32",
      "interets": 128.5704,
      "assurance": 150,
      "capitalamorti": 2934.7993737045,
      "capitalrestantdu": 113947.3689
    },
    {
      "date": "05.11.2028",
      "dateStd": "2028-11-05",
      "mensualite": 3063.3693737045,
      "echeanceglobale": 3213.3693737045,
      "taux": "1.32",
      "interets": 125.3421,
      "assurance": 150,
      "capitalamorti": 2938.0293737045,
      "capitalrestantdu": 111009.3396
    },
    {
      "date": "05.12.2028",
      "dateStd": "2028-12-05",
      "mensualite": 3063.3693737045,
      "echeanceglobale": 3213.3693737045,
      "taux": "1.32",
      "interets": 122.1103,
      "assurance": 150,
      "capitalamorti": 2941.2593737045,
      "capitalrestantdu": 108068.0802
    },
    {
      "date": "05.01.2029",
      "dateStd": "2029-01-05",
      "mensualite": 3063.3693737045,
      "echeanceglobale": 3213.3693737045,
      "taux": "1.32",
      "interets": 118.8749,
      "assurance": 150,
      "capitalamorti": 2944.4993737045,
      "capitalrestantdu": 105123.5808
    },
    {
      "date": "05.02.2029",
      "dateStd": "2029-02-05",
      "mensualite": 3063.3693737045,
      "echeanceglobale": 3213.3693737045,
      "taux": "1.32",
      "interets": 115.6359,
      "assurance": 150,
      "capitalamorti": 2947.7293737045,
      "capitalrestantdu": 102175.8514
    },
    {
      "date": "05.03.2029",
      "dateStd": "2029-03-05",
      "mensualite": 3063.3693737045,
      "echeanceglobale": 3213.3693737045,
      "taux": "1.32",
      "interets": 112.3934,
      "assurance": 150,
      "capitalamorti": 2950.9793737045,
      "capitalrestantdu": 99224.8721
    },
    {
      "date": "05.04.2029",
      "dateStd": "2029-04-05",
      "mensualite": 3063.3693737045,
      "echeanceglobale": 3213.3693737045,
      "taux": "1.32",
      "interets": 109.1474,
      "assurance": 150,
      "capitalamorti": 2954.2193737045,
      "capitalrestantdu": 96270.6527
    },
    {
      "date": "05.05.2029",
      "dateStd": "2029-05-05",
      "mensualite": 3063.3693737045,
      "echeanceglobale": 3213.3693737045,
      "taux": "1.32",
      "interets": 105.8977,
      "assurance": 150,
      "capitalamorti": 2957.4693737045,
      "capitalrestantdu": 93313.1833
    },
    {
      "date": "05.06.2029",
      "dateStd": "2029-06-05",
      "mensualite": 3063.3693737045,
      "echeanceglobale": 3213.3693737045,
      "taux": "1.32",
      "interets": 102.6445,
      "assurance": 150,
      "capitalamorti": 2960.7293737045,
      "capitalrestantdu": 90352.4539
    },
    {
      "date": "05.07.2029",
      "dateStd": "2029-07-05",
      "mensualite": 3063.3693737045,
      "echeanceglobale": 3213.3693737045,
      "taux": "1.32",
      "interets": 99.3877,
      "assurance": 150,
      "capitalamorti": 2963.9793737045,
      "capitalrestantdu": 87388.4746
    },
    {
      "date": "05.08.2029",
      "dateStd": "2029-08-05",
      "mensualite": 3063.3693737045,
      "echeanceglobale": 3213.3693737045,
      "taux": "1.32",
      "interets": 96.1273,
      "assurance": 150,
      "capitalamorti": 2967.2393737045,
      "capitalrestantdu": 84421.2352
    },
    {
      "date": "05.09.2029",
      "dateStd": "2029-09-05",
      "mensualite": 3063.3693737045,
      "echeanceglobale": 3213.3693737045,
      "taux": "1.32",
      "interets": 92.8634,
      "assurance": 150,
      "capitalamorti": 2970.5093737045,
      "capitalrestantdu": 81450.7258
    },
    {
      "date": "05.10.2029",
      "dateStd": "2029-10-05",
      "mensualite": 3063.3693737045,
      "echeanceglobale": 3213.3693737045,
      "taux": "1.32",
      "interets": 89.5958,
      "assurance": 150,
      "capitalamorti": 2973.7693737045,
      "capitalrestantdu": 78476.9564
    },
    {
      "date": "05.11.2029",
      "dateStd": "2029-11-05",
      "mensualite": 3063.3693737045,
      "echeanceglobale": 3213.3693737045,
      "taux": "1.32",
      "interets": 86.3247,
      "assurance": 150,
      "capitalamorti": 2977.0493737045,
      "capitalrestantdu": 75499.9071
    },
    {
      "date": "05.12.2029",
      "dateStd": "2029-12-05",
      "mensualite": 3063.3693737045,
      "echeanceglobale": 3213.3693737045,
      "taux": "1.32",
      "interets": 83.0499,
      "assurance": 150,
      "capitalamorti": 2980.3193737045,
      "capitalrestantdu": 72519.5877
    },
    {
      "date": "05.01.2030",
      "dateStd": "2030-01-05",
      "mensualite": 3063.3693737045,
      "echeanceglobale": 3213.3693737045,
      "taux": "1.32",
      "interets": 79.7715,
      "assurance": 150,
      "capitalamorti": 2983.5993737045,
      "capitalrestantdu": 69535.9883
    },
    {
      "date": "05.02.2030",
      "dateStd": "2030-02-05",
      "mensualite": 3063.3693737045,
      "echeanceglobale": 3213.3693737045,
      "taux": "1.32",
      "interets": 76.4896,
      "assurance": 150,
      "capitalamorti": 2986.8793737045,
      "capitalrestantdu": 66549.109
    },
    {
      "date": "05.03.2030",
      "dateStd": "2030-03-05",
      "mensualite": 3063.3693737045,
      "echeanceglobale": 3213.3693737045,
      "taux": "1.32",
      "interets": 73.204,
      "assurance": 150,
      "capitalamorti": 2990.1693737045,
      "capitalrestantdu": 63558.9396
    },
    {
      "date": "05.04.2030",
      "dateStd": "2030-04-05",
      "mensualite": 3063.3693737045,
      "echeanceglobale": 3213.3693737045,
      "taux": "1.32",
      "interets": 69.9148,
      "assurance": 150,
      "capitalamorti": 2993.4593737045,
      "capitalrestantdu": 60565.4802
    },
    {
      "date": "05.05.2030",
      "dateStd": "2030-05-05",
      "mensualite": 3063.3693737045,
      "echeanceglobale": 3213.3693737045,
      "taux": "1.32",
      "interets": 66.622,
      "assurance": 150,
      "capitalamorti": 2996.7493737045,
      "capitalrestantdu": 57568.7308
    },
    {
      "date": "05.06.2030",
      "dateStd": "2030-06-05",
      "mensualite": 3063.3693737045,
      "echeanceglobale": 3213.3693737045,
      "taux": "1.32",
      "interets": 63.3256,
      "assurance": 150,
      "capitalamorti": 3000.0393737045,
      "capitalrestantdu": 54568.6915
    },
    {
      "date": "05.07.2030",
      "dateStd": "2030-07-05",
      "mensualite": 3063.3693737045,
      "echeanceglobale": 3213.3693737045,
      "taux": "1.32",
      "interets": 60.0256,
      "assurance": 150,
      "capitalamorti": 3003.3393737045,
      "capitalrestantdu": 51565.3521
    },
    {
      "date": "05.08.2030",
      "dateStd": "2030-08-05",
      "mensualite": 3063.3693737045,
      "echeanceglobale": 3213.3693737045,
      "taux": "1.32",
      "interets": 56.7219,
      "assurance": 150,
      "capitalamorti": 3006.6493737045,
      "capitalrestantdu": 48558.7027
    },
    {
      "date": "05.09.2030",
      "dateStd": "2030-09-05",
      "mensualite": 3063.3693737045,
      "echeanceglobale": 3213.3693737045,
      "taux": "1.32",
      "interets": 53.4146,
      "assurance": 150,
      "capitalamorti": 3009.9593737045,
      "capitalrestantdu": 45548.7433
    },
    {
      "date": "05.10.2030",
      "dateStd": "2030-10-05",
      "mensualite": 3063.3693737045,
      "echeanceglobale": 3213.3693737045,
      "taux": "1.32",
      "interets": 50.1036,
      "assurance": 150,
      "capitalamorti": 3013.2693737045,
      "capitalrestantdu": 42535.474
    },
    {
      "date": "05.11.2030",
      "dateStd": "2030-11-05",
      "mensualite": 3063.3693737045,
      "echeanceglobale": 3213.3693737045,
      "taux": "1.32",
      "interets": 46.789,
      "assurance": 150,
      "capitalamorti": 3016.5793737045,
      "capitalrestantdu": 39518.8946
    },
    {
      "date": "05.12.2030",
      "dateStd": "2030-12-05",
      "mensualite": 3063.3693737045,
      "echeanceglobale": 3213.3693737045,
      "taux": "1.32",
      "interets": 43.4708,
      "assurance": 150,
      "capitalamorti": 3019.8993737045,
      "capitalrestantdu": 36498.9952
    },
    {
      "date": "05.01.2031",
      "dateStd": "2031-01-05",
      "mensualite": 3063.3693737045,
      "echeanceglobale": 3213.3693737045,
      "taux": "1.32",
      "interets": 40.1489,
      "assurance": 150,
      "capitalamorti": 3023.2193737045,
      "capitalrestantdu": 33475.7758
    },
    {
      "date": "05.02.2031",
      "dateStd": "2031-02-05",
      "mensualite": 3063.3693737045,
      "echeanceglobale": 3213.3693737045,
      "taux": "1.32",
      "interets": 36.8234,
      "assurance": 150,
      "capitalamorti": 3026.5493737045,
      "capitalrestantdu": 30449.2265
    },
    {
      "date": "05.03.2031",
      "dateStd": "2031-03-05",
      "mensualite": 3063.3693737045,
      "echeanceglobale": 3213.3693737045,
      "taux": "1.32",
      "interets": 33.4941,
      "assurance": 150,
      "capitalamorti": 3029.8793737045,
      "capitalrestantdu": 27419.3471
    },
    {
      "date": "05.04.2031",
      "dateStd": "2031-04-05",
      "mensualite": 3063.3693737045,
      "echeanceglobale": 3213.3693737045,
      "taux": "1.32",
      "interets": 30.1613,
      "assurance": 150,
      "capitalamorti": 3033.2093737045,
      "capitalrestantdu": 24386.1377
    },
    {
      "date": "05.05.2031",
      "dateStd": "2031-05-05",
      "mensualite": 3063.3693737045,
      "echeanceglobale": 3213.3693737045,
      "taux": "1.32",
      "interets": 26.8248,
      "assurance": 150,
      "capitalamorti": 3036.5493737045,
      "capitalrestantdu": 21349.5883
    },
    {
      "date": "05.06.2031",
      "dateStd": "2031-06-05",
      "mensualite": 3063.3693737045,
      "echeanceglobale": 3213.3693737045,
      "taux": "1.32",
      "interets": 23.4845,
      "assurance": 150,
      "capitalamorti": 3039.8893737045,
      "capitalrestantdu": 18309.699
    },
    {
      "date": "05.07.2031",
      "dateStd": "2031-07-05",
      "mensualite": 3063.3693737045,
      "echeanceglobale": 3213.3693737045,
      "taux": "1.32",
      "interets": 20.1407,
      "assurance": 150,
      "capitalamorti": 3043.2293737045,
      "capitalrestantdu": 15266.4696
    },
    {
      "date": "05.08.2031",
      "dateStd": "2031-08-05",
      "mensualite": 3063.3693737045,
      "echeanceglobale": 3213.3693737045,
      "taux": "1.32",
      "interets": 16.7931,
      "assurance": 150,
      "capitalamorti": 3046.5793737045,
      "capitalrestantdu": 12219.8902
    },
    {
      "date": "05.09.2031",
      "dateStd": "2031-09-05",
      "mensualite": 3063.3693737045,
      "echeanceglobale": 3213.3693737045,
      "taux": "1.32",
      "interets": 13.4419,
      "assurance": 150,
      "capitalamorti": 3049.9293737045,
      "capitalrestantdu": 9169.9609
    },
    {
      "date": "05.10.2031",
      "dateStd": "2031-10-05",
      "mensualite": 3063.3693737045,
      "echeanceglobale": 3213.3693737045,
      "taux": "1.32",
      "interets": 10.087,
      "assurance": 150,
      "capitalamorti": 3053.2793737045,
      "capitalrestantdu": 6116.6815
    },
    {
      "date": "05.11.2031",
      "dateStd": "2031-11-05",
      "mensualite": 3063.3693737045,
      "echeanceglobale": 3213.3693737045,
      "taux": "1.32",
      "interets": 6.7283,
      "assurance": 150,
      "capitalamorti": 3056.6393737045,
      "capitalrestantdu": 3060.0421
    },
    {
      "date": "05.12.2031",
      "dateStd": "2031-12-05",
      "mensualite": 3063.3693737045,
      "echeanceglobale": 3213.3693737045,
      "taux": "1.32",
      "interets": 3.366,
      "assurance": 150,
      "capitalamorti": 3059.9993737045,
      "capitalrestantdu": 0.0427
    }
  ],
  "tableau_amortissement_charges_par_annee": {
    "2017": {
      "interets": 6416.8588,
      "assurance": 1800
    },
    "2018": {
      "interets": 6013.8917,
      "assurance": 1800
    },
    "2019": {
      "interets": 5605.573,
      "assurance": 1800
    },
    "2020": {
      "interets": 5191.8313,
      "assurance": 1800
    },
    "2021": {
      "interets": 4772.5958,
      "assurance": 1800
    },
    "2022": {
      "interets": 4347.7925,
      "assurance": 1800
    },
    "2023": {
      "interets": 3917.3477,
      "assurance": 1800
    },
    "2024": {
      "interets": 3481.1864,
      "assurance": 1800
    },
    "2025": {
      "interets": 3039.2328,
      "assurance": 1800
    },
    "2026": {
      "interets": 2591.4104,
      "assurance": 1800
    },
    "2027": {
      "interets": 2137.6406,
      "assurance": 1800
    },
    "2028": {
      "interets": 1677.8446,
      "assurance": 1800
    },
    "2029": {
      "interets": 1211.9426,
      "assurance": 1800
    },
    "2030": {
      "interets": 739.853,
      "assurance": 1800
    },
    "2031": {
      "interets": 261.494,
      "assurance": 1800
    }
  },
  "prix_bien_a_amortir": 428571.432,
  "loyer_annuel": [
    12000,
    12120,
    12241.2,
    12363.612,
    12487.24812,
    12612.1206012,
    12738.241807212,
    12865.624225284,
    12994.280467537,
    13124.223272212,
    13255.465504934,
    13388.020159984,
    13521.900361584,
    13657.119365199,
    13793.690558851,
    13931.62746444,
    14070.943739084,
    14211.653176475,
    14353.76970824,
    14497.307405322
  ],
  "loyer_remplissage_annuel": [
    12000,
    12120,
    12241.2,
    12363.612,
    12487.24812,
    12612.1206012,
    12738.241807212,
    12865.624225284,
    12994.280467537,
    13124.223272212,
    13255.465504934,
    13388.020159984,
    13521.900361584,
    13657.119365199,
    13793.690558851,
    13931.62746444,
    14070.943739084,
    14211.653176475,
    14353.76970824,
    14497.307405322
  ],
  "fraisgs_annuels": [
    0,
    0,
    0,
    0,
    0,
    0,
    0,
    0,
    0,
    0,
    0,
    0,
    0,
    0,
    0,
    0,
    0,
    0,
    0,
    0
  ],
  "chargesr_annuels": [
    936,
    954.72,
    973.8144,
    993.290688,
    1013.15650176,
    1033.4196317952,
    1054.0880244311,
    1075.1697849197,
    1096.6731806181,
    1118.6066442305,
    1140.9787771151,
    1163.7983526574,
    1187.0743197105,
    1210.8158061048,
    1235.0321222268,
    1259.7327646714,
    1284.9274199648,
    1310.6259683641,
    1336.8384877314,
    1363.575257486
  ],
  "travauxa_annuels": [
    0,
    0,
    0,
    0,
    0,
    0,
    0,
    0,
    0,
    0,
    0,
    0,
    0,
    0,
    0,
    0,
    0,
    0,
    0,
    0
  ],
  "assuranceli_annuels": [
    0,
    0,
    0,
    0,
    0,
    0,
    0,
    0,
    0,
    0,
    0,
    0,
    0,
    0,
    0,
    0,
    0,
    0,
    0,
    0
  ],
  "assurancepno_annuels": [
    0,
    0,
    0,
    0,
    0,
    0,
    0,
    0,
    0,
    0,
    0,
    0,
    0,
    0,
    0,
    0,
    0,
    0,
    0,
    0
  ],
  "tf_annuels": [
    2700,
    2727,
    2754.27,
    2781.8127,
    2809.630827,
    2837.72713527,
    2866.1044066227,
    2894.7654506889,
    2923.7131051958,
    2952.9502362478,
    2982.4797386103,
    3012.3045359964,
    3042.4275813563,
    3072.8518571699,
    3103.5803757416,
    3134.616179499,
    3165.962341294,
    3197.6219647069,
    3229.598184354,
    3261.8941661975
  ],
  "chargest_annuels": [
    3750,
    3825,
    3901.5,
    3979.53,
    4059.1206,
    4140.303012,
    4223.10907224,
    4307.5712536848,
    4393.7226787585,
    4481.5971323337,
    4571.2290749803,
    4662.6536564799,
    4755.9067296095,
    4851.0248642017,
    4948.0453614858,
    5047.0062687155,
    5147.9463940898,
    5250.9053219716,
    5355.923428411,
    5463.0418969792
  ],
  "charges_poste_annuels": [
    20,
    20,
    20,
    20,
    20,
    20,
    20,
    20,
    20,
    20,
    20,
    20,
    20,
    20,
    20,
    20,
    20,
    20,
    20,
    20
  ],
  "charges_assurance_emprunt_annuels": [
    1800,
    1800,
    1800,
    1800,
    1800,
    1800,
    1800,
    1800,
    1800,
    1800,
    1800,
    1800,
    1800,
    1800,
    1800,
    0,
    0,
    0,
    0,
    0
  ],
  "cga_annuels": [
    0,
    0,
    0,
    0,
    0,
    0,
    0,
    0,
    0,
    0,
    0,
    0,
    0,
    0,
    0,
    0,
    0,
    0,
    0,
    0
  ],
  "cfe_annuels": [
    0,
    0,
    0,
    0,
    0,
    0,
    0,
    0,
    0,
    0,
    0,
    0,
    0,
    0,
    0,
    0,
    0,
    0,
    0,
    0
  ],
  "comptable_annuels": [
    0,
    0,
    0,
    0,
    0,
    0,
    0,
    0,
    0,
    0,
    0,
    0,
    0,
    0,
    0,
    0,
    0,
    0,
    0,
    0
  ],
  "charges_annuelles_non_rc": [
    7334,
    7417.28,
    7501.9556,
    7588.052012,
    7675.59492524,
    7764.6105154748,
    7855.1254544316,
    7947.166919454,
    8040.7626033362,
    8135.940724351,
    8232.7300364755,
    8331.1598398189,
    8431.2599912553,
    8533.0609152669,
    8636.5936150005,
    6941.8896835431,
    7048.981315419,
    7157.9013183144,
    7268.6831250336,
    7381.3608056908
  ],
  "charges_amorti": [
    26253.967797333,
    26253.967797333,
    26253.967797333,
    26253.967797333,
    26253.967797333,
    26253.967797333,
    8000.000064,
    8000.000064,
    8000.000064,
    8000.000064,
    8000.000064,
    8000.000064,
    8000.000064,
    8000.000064,
    8000.000064,
    8000.000064,
    8000.000064,
    8000.000064,
    8000.000064,
    8000.000064
  ],
  "amortissementsImmeubleTotaux": 245714.28768,
  "apayer_an": [
    -32094.432484454,
    -32057.712484454,
    -32021.188084454,
    -31984.872496454,
    -31948.779289694,
    -31912.922398728,
    -31877.316131673,
    -31841.975178623,
    -31806.914620253,
    -31772.149936592,
    -31737.697015995,
    -31703.572164289,
    -31669.792114125,
    -31636.374034521,
    -31603.335540603,
    6989.7377808969,
    7021.9624236654,
    7053.7518581608,
    7085.0865832064,
    7115.9465996316
  ],
  "foncier2044": {
    "211": [
      12000,
      12120,
      12241.2,
      12363.612,
      12487.24812,
      12612.1206012,
      12738.241807212,
      12865.624225284,
      12994.280467537,
      13124.223272212,
      13255.465504934,
      13388.020159984,
      13521.900361584,
      13657.119365199,
      13793.690558851,
      13931.62746444,
      14070.943739084,
      14211.653176475,
      14353.76970824,
      14497.307405322
    ],
    "213": [
      0,
      0,
      0,
      0,
      0,
      0,
      0,
      0,
      0,
      0,
      0,
      0,
      0,
      0,
      0,
      0,
      0,
      0,
      0,
      0
    ],
    "215": [
      12000,
      12120,
      12241.2,
      12363.612,
      12487.24812,
      12612.1206012,
      12738.241807212,
      12865.624225284,
      12994.280467537,
      13124.223272212,
      13255.465504934,
      13388.020159984,
      13521.900361584,
      13657.119365199,
      13793.690558851,
      13931.62746444,
      14070.943739084,
      14211.653176475,
      14353.76970824,
      14497.307405322
    ],
    "224": [
      0,
      0,
      0,
      0,
      0,
      0,
      0,
      0,
      0,
      0,
      0,
      0,
      0,
      0,
      0,
      0,
      0,
      0,
      0,
      0
    ],
    "228": [
      0,
      0,
      0,
      0,
      0,
      0,
      0,
      0,
      0,
      0,
      0,
      0,
      0,
      0,
      0,
      0,
      0,
      0,
      0,
      0
    ],
    "240": [
      5534,
      5617.28,
      5701.9556,
      5788.052012,
      5875.59492524,
      5964.6105154748,
      6055.1254544316,
      6147.166919454,
      6240.7626033362,
      6335.940724351,
      6432.7300364755,
      6531.1598398189,
      6631.2599912553,
      6733.0609152669,
      6836.5936150005,
      6941.8896835431,
      7048.981315419,
      7157.9013183144,
      7268.6831250336,
      7381.3608056908
    ],
    "250": [
      20116.858665703,
      7813.8913462552,
      7405.5725589313,
      6991.831235416,
      6572.5953635961,
      6147.7919750265,
      5717.3471322298,
      5281.1859158277,
      4839.2324115009,
      4391.4096967765,
      3937.6398276391,
      3477.8438249651,
      3011.9416607757,
      2539.8522443084,
      2061.493407903,
      0,
      0,
      0,
      0,
      0
    ],
    "263": [
      -13650.858665703,
      -1311.1713462552,
      -866.32815893128,
      -416.27124741602,
      39.057831163861,
      499.7181106987,
      965.76922055061,
      1437.2713900025,
      1914.2854526999,
      2396.8728510849,
      2885.0956408198,
      3379.0164951998,
      3878.6987095526,
      4384.2062056242,
      4895.6035359479,
      6989.7377808969,
      7021.9624236654,
      7053.7518581608,
      7085.0865832064,
      7115.9465996316
    ]
  },
  "rendement_brut_charges": [
    0.85,
    0.86,
    0.87,
    0.87,
    0.88,
    0.89,
    0.89,
    0.9,
    0.91,
    0.91,
    0.92,
    0.93,
    0.93,
    0.94,
    0.94,
    1.28,
    1.29,
    1.29,
    1.3,
    1.3
  ],
  "apayer_mois": -2674.5360403711,
  "rendement_brut": 2.2,
  "rendement_brut_remplissage": 2.2,
  "reste_interets": 0,
  "prix_revente": 0,
  "annee_revente": 20,
  "plusvalue_impot": 0,
  "correctif_rendement": 0,
  "otherFoncierEnabled": false,
  "otherFoncierExists": false,
  "otherRevenus": null,
  "otherCharges": null,
  "otherInterets": null,
  "otherFoncier": null,
  "otherMicroFoncier": null,
  "otherBic": null,
  "otherMicroBic": null,
  "otherBicRecettes": null,
  "otherBicCharges": null,
  "otherBicAmortissements": null,
  "otherReductionImpot": null,
  "tip": {
    "solde_total": [
      "Loyers (12000) - Mensualités Sans Assurance (36760) - Charges (7334) - Prél. Sociaux (0) - Impôts (-1528)",
      "Loyers (12120) - Mensualités Sans Assurance (36760) - Charges (7417) - Prél. Sociaux (0) - Impôts (-393)",
      "Loyers (12241) - Mensualités Sans Assurance (36760) - Charges (7502) - Prél. Sociaux (0) - Impôts (-260)",
      "Loyers (12364) - Mensualités Sans Assurance (36760) - Charges (7588) - Prél. Sociaux (0) - Impôts (-125)",
      "Loyers (12487) - Mensualités Sans Assurance (36760) - Charges (7676) - Prél. Sociaux (0) - Impôts (0)",
      "Loyers (12612) - Mensualités Sans Assurance (36760) - Charges (7765) - Prél. Sociaux (0) - Impôts (0)",
      "Loyers (12738) - Mensualités Sans Assurance (36760) - Charges (7855) - Prél. Sociaux (0) - Impôts (0)",
      "Loyers (12866) - Mensualités Sans Assurance (36760) - Charges (7947) - Prél. Sociaux (0) - Impôts (0)",
      "Loyers (12994) - Mensualités Sans Assurance (36760) - Charges (8041) - Prél. Sociaux (0) - Impôts (0)",
      "Loyers (13124) - Mensualités Sans Assurance (36760) - Charges (8136) - Prél. Sociaux (0) - Impôts (0)",
      "Loyers (13255) - Mensualités Sans Assurance (36760) - Charges (8233) - Prél. Sociaux (313) - Impôts (607)",
      "Loyers (13388) - Mensualités Sans Assurance (36760) - Charges (8331) - Prél. Sociaux (524) - Impôts (1014)",
      "Loyers (13522) - Mensualités Sans Assurance (36760) - Charges (8431) - Prél. Sociaux (601) - Impôts (1164)",
      "Loyers (13657) - Mensualités Sans Assurance (36760) - Charges (8533) - Prél. Sociaux (680) - Impôts (1315)",
      "Loyers (13794) - Mensualités Sans Assurance (36760) - Charges (8637) - Prél. Sociaux (759) - Impôts (1469)",
      "Loyers (13932) - Mensualités Sans Assurance (-1800) - Charges (6942) - Prél. Sociaux (1083) - Impôts (2097)",
      "Loyers (14071) - Mensualités Sans Assurance (-1800) - Charges (7049) - Prél. Sociaux (1088) - Impôts (2107)",
      "Loyers (14212) - Mensualités Sans Assurance (-1800) - Charges (7158) - Prél. Sociaux (1093) - Impôts (2116)",
      "Loyers (14354) - Mensualités Sans Assurance (-1800) - Charges (7269) - Prél. Sociaux (1098) - Impôts (2126)",
      "Loyers (14497) - Mensualités Sans Assurance (-1800) - Charges (7381) - Prél. Sociaux (1103) - Impôts (2135)"
    ]
  },
  "balance_impot": [
    -2547.2027070378,
    -2638.7260403711,
    -2646.7827070378,
    -2654.9568737045,
    -2662.4185403711,
    -2659.4202070378,
    -2656.4635403711,
    -2653.4668737045,
    -2650.5993737045,
    -2647.6977070378,
    -2721.5477070378,
    -2770.1118737045,
    -2786.2452070378,
    -2802.5843737045,
    -2819.2418737045,
    467.47166666667,
    468.88416666667,
    470.39416666667,
    471.76166666667,
    473.13833333333
  ],
  "loyer_": [
    12000,
    12120,
    12241,
    12364,
    12487,
    12612,
    12738,
    12866,
    12994,
    13124,
    13255,
    13388,
    13522,
    13657,
    13794,
    13932,
    14071,
    14212,
    14354,
    14497
  ],
  "charges": [
    -19234,
    -7417.28,
    -7501.96,
    -7588.05,
    -7675.59,
    -7764.61,
    -7855.13,
    -7947.17,
    -8040.76,
    -8135.94,
    -8232.73,
    -8331.16,
    -8431.26,
    -8533.06,
    -8636.59,
    -6941.89,
    -7048.98,
    -7157.9,
    -7268.68,
    -7381.36
  ],
  "imposable": [
    -5534,
    -1311,
    -867,
    -416,
    0,
    0,
    0,
    0,
    0,
    0,
    2022,
    3379,
    3879,
    4384,
    4896,
    6990,
    7022,
    7054,
    7085,
    7116
  ],
  "imposableCalculActuel": [
    -5534,
    -1311,
    -867,
    -416,
    0,
    0,
    0,
    0,
    0,
    0,
    2022,
    3379,
    3879,
    4384,
    4896,
    6990,
    7022,
    7054,
    7085,
    7116
  ],
  "ps": [
    0,
    0,
    0,
    0,
    0,
    0,
    0,
    0,
    0,
    0,
    -313.41,
    -523.75,
    -601.25,
    -679.52,
    -758.88,
    -1083.45,
    -1088.41,
    -1093.37,
    -1098.18,
    -1102.98
  ],
  "impot": [
    1528,
    393,
    260,
    125,
    0,
    0,
    0,
    0,
    0,
    0,
    -607,
    -1014,
    -1164,
    -1315,
    -1469,
    -2097,
    -2107,
    -2116,
    -2126,
    -2135
  ],
  "decote": [
    0,
    0,
    0,
    0,
    0,
    0,
    0,
    0,
    0,
    0,
    0,
    0,
    0,
    0,
    0,
    0,
    0,
    0,
    0,
    0
  ],
  "tmis": [
    14,
    30,
    30,
    30,
    30,
    30,
    30,
    30,
    30,
    30,
    30,
    30,
    30,
    30,
    30,
    30,
    30,
    30,
    30,
    30
  ],
  "abbatements": [
    3500,
    3517.5,
    3535.0875,
    3552.7629375,
    3570.5267521875,
    3588.3793859484,
    3606.3212828782,
    3624.3528892926,
    3642.474653739,
    3660.6870270077,
    3678.9904621428,
    3697.3854144535,
    3715.8723415257,
    3734.4517032334,
    3753.1239617495,
    3771.8895815583,
    3790.7490294661,
    3809.7027746134,
    3828.7512884865,
    3847.8950449289
  ],
  "revenu_reference": [
    25966,
    30346.5,
    30948.7875,
    31558.8664375,
    32134.740769687,
    32295.414473536,
    32456.891545904,
    32619.176003633,
    32782.271883651,
    32946.18324307,
    35132.914159285,
    36655.468730081,
    37321.851073732,
    37994.0653291,
    38674.115655746,
    40937.006234025,
    41138.741265195,
    41341.324971521,
    41543.761596378,
    41747.05540436
  ],
  "taxe_excep": [
    0,
    0,
    0,
    0,
    0,
    0,
    0,
    0,
    0,
    0,
    0,
    0,
    0,
    0,
    0,
    0,
    0,
    0,
    0,
    0
  ],
  "revenus_annuels": [
    35000,
    35175,
    35350.875,
    35527.629375,
    35705.267521875,
    35883.793859484,
    36063.212828782,
    36243.528892926,
    36424.74653739,
    36606.870270077,
    36789.904621428,
    36973.854144535,
    37158.723415257,
    37344.517032334,
    37531.239617495,
    37718.895815583,
    37907.490294661,
    38097.027746134,
    38287.512884865,
    38478.950449289
  ],
  "revenus2_annuels": [
    "",
    "",
    "",
    "",
    "",
    "",
    "",
    "",
    "",
    "",
    "",
    "",
    "",
    "",
    "",
    "",
    "",
    "",
    "",
    ""
  ],
  "ps_total": [
    0,
    0,
    0,
    0,
    0,
    0,
    0,
    0,
    0,
    0,
    313.41,
    523.75,
    601.25,
    679.52,
    758.88,
    1083.45,
    1088.41,
    1093.37,
    1098.18,
    1102.98
  ],
  "impot_total_revenus": [
    2277.24,
    3459.39,
    3640.07625,
    3823.09993125,
    3995.8622309062,
    4044.0643420608,
    4092.5074637711,
    4141.1928010899,
    4190.1215650954,
    4239.2949729209,
    4895.3142477855,
    5352.0806190244,
    5551.9953221195,
    5753.6595987301,
    5957.6746967238,
    6636.5418702074,
    6697.0623795584,
    6757.8374914562,
    6818.5684789135,
    6879.556621308
  ],
  "impot_bic": [
    0,
    0,
    0,
    0,
    0,
    0,
    0,
    0,
    0,
    0,
    0,
    0,
    0,
    0,
    0,
    0,
    0,
    0,
    0,
    0
  ],
  "impot_type": "foncier",
  "impot_gain_defisc": [
    0,
    0,
    0,
    0,
    0,
    0,
    0,
    0,
    0,
    0,
    0,
    0,
    0,
    0,
    0,
    0,
    0,
    0,
    0,
    0
  ],
  "impot_gain_defisc_max": [
    null,
    null,
    null,
    null,
    null,
    null,
    null,
    null,
    null,
    null,
    null,
    null,
    null,
    null,
    null,
    null,
    null,
    null,
    null,
    null
  ],
  "impot_gain_defisc_projet": [
    null,
    null,
    null,
    null,
    null,
    null,
    null,
    null,
    null,
    null,
    null,
    null,
    null,
    null,
    null,
    null,
    null,
    null,
    null,
    null
  ],
  "rendement": [
    1.13,
    0.93,
    0.92,
    0.9,
    0.88,
    0.89,
    0.89,
    0.9,
    0.91,
    0.91,
    0.75,
    0.64,
    0.61,
    0.57,
    0.54,
    0.7,
    0.7,
    0.7,
    0.71,
    0.71
  ],
  "interets_tableau": [
    -20116.858665703,
    -7813.8913462552,
    -7405.5725589313,
    -6991.831235416,
    -6572.5953635961,
    -6147.7919750265,
    -5717.3471322298,
    -5281.1859158277,
    -4839.2324115009,
    -4391.4096967765,
    -3937.6398276391,
    -3477.8438249651,
    -3011.9416607757,
    -2539.8522443084,
    -2061.493407903,
    0,
    0,
    0,
    0,
    0
  ],
  "solde": [
    -13922.858665703,
    -2718.1713462552,
    -2406.5325589313,
    -2090.881235416,
    -1761.1853635961,
    -1300.4019750265,
    -834.4771322298,
    -362.35591582766,
    114.00758849911,
    596.65030322353,
    164.22017236088,
    41.24617503493,
    313.54833922433,
    589.56775569158,
    868.03659209696,
    3809.66,
    3826.61,
    3844.73,
    3861.14,
    3877.66
  ],
  "solde_total": [
    -30566.432484454,
    -31664.712484454,
    -31761.392484454,
    -31859.482484454,
    -31949.022484454,
    -31913.042484454,
    -31877.562484454,
    -31841.602484454,
    -31807.192484454,
    -31772.372484454,
    -32658.572484454,
    -33241.342484454,
    -33434.942484454,
    -33631.012484454,
    -33830.902484454,
    5609.66,
    5626.61,
    5644.73,
    5661.14,
    5677.66
  ],
  "impot_revenu_foncier_total": [
    1528,
    393,
    260,
    125,
    0,
    0,
    0,
    0,
    0,
    0,
    -607,
    -1014,
    -1164,
    -1315,
    -1469,
    -2097,
    -2107,
    -2116,
    -2126,
    -2135
  ],
  "excedent": [
    8117,
    null,
    null,
    null,
    null,
    null,
    null,
    null,
    null,
    null,
    null,
    null,
    null,
    null,
    null,
    null,
    null,
    null,
    null,
    null
  ],
  "deficit_financier": [
    {
      "montant": -8116.8586657028
    },
    {
      "montant": 4306.1086537448
    },
    {
      "montant": 4835.4274410687
    },
    {
      "montant": 5372.168764584
    },
    {
      "montant": 5914.4046364039
    },
    {
      "montant": 6464.2080249735
    },
    {
      "montant": 7020.6528677702
    },
    {
      "montant": 7584.8140841723
    },
    {
      "montant": 8154.7675884991
    },
    {
      "montant": 8732.5903032235
    },
    {
      "montant": 9317.3601723609
    },
    {
      "montant": 9910.1561750349
    },
    {
      "montant": 10510.058339224
    },
    {
      "montant": 11117.147755692
    },
    {
      "montant": 11732.506592097
    },
    {
      "montant": 13932
    },
    {
      "montant": 14071
    },
    {
      "montant": 14212
    },
    {
      "montant": 14354
    },
    {
      "montant": 14497
    }
  ],
  "deficit_non_financier": [
    {
      "montant": -5534
    },
    {
      "montant": -1311.1713462552
    },
    {
      "montant": -866.53255893128
    },
    {
      "montant": -415.88123541602
    },
    {
      "montant": 38.81463640386
    },
    {
      "montant": 499.5980249735
    },
    {
      "montant": 965.5228677702
    },
    {
      "montant": 1437.6440841723
    },
    {
      "montant": 1914.0075884991
    },
    {
      "montant": 2396.6503032235
    },
    {
      "montant": 2884.6301723609
    },
    {
      "montant": 3378.9961750349
    },
    {
      "montant": 3878.7983392243
    },
    {
      "montant": 4384.0877556916
    },
    {
      "montant": 4895.916592097
    },
    {
      "montant": 6990.11
    },
    {
      "montant": 7022.02
    },
    {
      "montant": 7054.1
    },
    {
      "montant": 7085.32
    },
    {
      "montant": 7115.64
    }
  ],
  "montant_reporte": [
    0,
    0,
    0,
    0,
    -39,
    -500,
    -966,
    -1438,
    -1914,
    -2397,
    -863,
    0,
    0,
    0,
    0,
    0,
    0,
    0,
    0,
    0
  ],
  "deficitReporteGlobal": [
    -5534,
    -1311,
    -867,
    -416,
    0,
    0,
    0,
    0,
    0,
    0,
    0,
    0,
    0,
    0,
    0,
    0,
    0,
    0,
    0,
    0
  ],
  "explicationsAnnuelles": [
    {
      "deficit_non_financier": "<u>Déficits fonciers</u>: -5 534&euro; sont imputables sur le revenu global (salaires) de cette ann&eacute;e.",
      "deficit_financier": "-8 117&euro; sont reportables sur les revenus fonciers seuls (positifs) pendant les 10 ann&eacute;es qui suivent."
    },
    {
      "deficit_non_financier": "<u>Déficits fonciers</u>: -1 311&euro; sont imputables sur le revenu global (salaires) de cette ann&eacute;e.",
      "texte_reporte_global": "<u>Déficits globaux</u>: -1 311&euro; ont &eacute;t&eacute; imput&eacute;s sur le revenu global (salaires) cette ann&eacute;e."
    },
    {
      "deficit_non_financier": "<u>Déficits fonciers</u>: -867&euro; sont imputables sur le revenu global (salaires) de cette ann&eacute;e.",
      "texte_reporte_global": "<u>Déficits globaux</u>: -867&euro; ont &eacute;t&eacute; imput&eacute;s sur le revenu global (salaires) cette ann&eacute;e."
    },
    {
      "deficit_non_financier": "<u>Déficits fonciers</u>: -416&euro; sont imputables sur le revenu global (salaires) de cette ann&eacute;e.",
      "texte_reporte_global": "<u>Déficits globaux</u>: -416&euro; ont &eacute;t&eacute; imput&eacute;s sur le revenu global (salaires) cette ann&eacute;e."
    },
    {
      "texte_reporte": "<u>Déficits fonciers</u>: 39&euro; (report&eacute;s des ann&eacute;es pr&eacute;c&eacute;dentes) ont &eacute;t&eacute; imput&eacute;s sur les revenus fonciers imposables cette ann&eacute;e."
    },
    {
      "texte_reporte": "<u>Déficits fonciers</u>: 500&euro; (report&eacute;s des ann&eacute;es pr&eacute;c&eacute;dentes) ont &eacute;t&eacute; imput&eacute;s sur les revenus fonciers imposables cette ann&eacute;e."
    },
    {
      "texte_reporte": "<u>Déficits fonciers</u>: 966&euro; (report&eacute;s des ann&eacute;es pr&eacute;c&eacute;dentes) ont &eacute;t&eacute; imput&eacute;s sur les revenus fonciers imposables cette ann&eacute;e."
    },
    {
      "texte_reporte": "<u>Déficits fonciers</u>: 1 438&euro; (report&eacute;s des ann&eacute;es pr&eacute;c&eacute;dentes) ont &eacute;t&eacute; imput&eacute;s sur les revenus fonciers imposables cette ann&eacute;e."
    },
    {
      "texte_reporte": "<u>Déficits fonciers</u>: 1 914&euro; (report&eacute;s des ann&eacute;es pr&eacute;c&eacute;dentes) ont &eacute;t&eacute; imput&eacute;s sur les revenus fonciers imposables cette ann&eacute;e."
    },
    {
      "texte_reporte": "<u>Déficits fonciers</u>: 2 397&euro; (report&eacute;s des ann&eacute;es pr&eacute;c&eacute;dentes) ont &eacute;t&eacute; imput&eacute;s sur les revenus fonciers imposables cette ann&eacute;e."
    },
    {
      "texte_reporte": "<u>Déficits fonciers</u>: 863&euro; (report&eacute;s des ann&eacute;es pr&eacute;c&eacute;dentes) ont &eacute;t&eacute; imput&eacute;s sur les revenus fonciers imposables cette ann&eacute;e."
    },
    [],
    [],
    [],
    [],
    [],
    [],
    [],
    [],
    []
  ],
  "noRevenus": false,
  "deficit_reportable": [
    {
      "1": 0,
      "2": 0,
      "3": 0,
      "4": 0,
      "5": 0,
      "6": 0,
      "7": 0,
      "8": 0,
      "9": 0,
      "10": 0,
      "11": 0,
      "12": 0,
      "13": 0,
      "14": 0,
      "15": 0,
      "16": 0,
      "17": 0,
      "18": 0,
      "19": 0,
      "20": 0
    },
    {
      "1": 0,
      "2": 0,
      "3": 0,
      "4": 0,
      "5": 0,
      "6": 0,
      "7": 0,
      "8": 0,
      "9": 0,
      "10": 0,
      "11": 0,
      "12": 0,
      "13": 0,
      "14": 0,
      "15": 0,
      "16": 0,
      "17": 0,
      "18": 0,
      "19": 0,
      "20": 0
    },
    {
      "1": 0,
      "2": 0,
      "3": 0,
      "4": 0,
      "5": 0,
      "6": 0,
      "7": 0,
      "8": 0,
      "9": 0,
      "10": 0,
      "11": 0,
      "12": 0,
      "13": 0,
      "14": 0,
      "15": 0,
      "16": 0,
      "17": 0,
      "18": 0,
      "19": 0,
      "20": 0
    },
    {
      "1": 0,
      "2": 0,
      "3": 0,
      "4": 0,
      "5": 0,
      "6": 0,
      "7": 0,
      "8": 0,
      "9": 0,
      "10": 0,
      "11": 0,
      "12": 0,
      "13": 0,
      "14": 0,
      "15": 0,
      "16": 0,
      "17": 0,
      "18": 0,
      "19": 0,
      "20": 0
    },
    {
      "1": 0,
      "2": 0,
      "3": 0,
      "4": 0,
      "5": 0,
      "6": 0,
      "7": 0,
      "8": 0,
      "9": 0,
      "10": 0,
      "11": 0,
      "12": 0,
      "13": 0,
      "14": 0,
      "15": 0,
      "16": 0,
      "17": 0,
      "18": 0,
      "19": 0,
      "20": 0
    },
    {
      "1": 0,
      "2": 0,
      "3": 0,
      "4": 0,
      "5": 0,
      "6": 0,
      "7": 0,
      "8": 0,
      "9": 0,
      "10": 0,
      "11": 0,
      "12": 0,
      "13": 0,
      "14": 0,
      "15": 0,
      "16": 0,
      "17": 0,
      "18": 0,
      "19": 0,
      "20": 0
    },
    {
      "1": 0,
      "2": 0,
      "3": 0,
      "4": 0,
      "5": 0,
      "6": 0,
      "7": 0,
      "8": 0,
      "9": 0,
      "10": 0,
      "11": 0,
      "12": 0,
      "13": 0,
      "14": 0,
      "15": 0,
      "16": 0,
      "17": 0,
      "18": 0,
      "19": 0,
      "20": 0
    },
    {
      "1": 0,
      "2": 0,
      "3": 0,
      "4": 0,
      "5": 0,
      "6": 0,
      "7": 0,
      "8": 0,
      "9": 0,
      "10": 0,
      "11": 0,
      "12": 0,
      "13": 0,
      "14": 0,
      "15": 0,
      "16": 0,
      "17": 0,
      "18": 0,
      "19": 0,
      "20": 0
    },
    {
      "1": 0,
      "2": 0,
      "3": 0,
      "4": 0,
      "5": 0,
      "6": 0,
      "7": 0,
      "8": 0,
      "9": 0,
      "10": 0,
      "11": 0,
      "12": 0,
      "13": 0,
      "14": 0,
      "15": 0,
      "16": 0,
      "17": 0,
      "18": 0,
      "19": 0,
      "20": 0
    },
    {
      "1": 0,
      "2": 0,
      "3": 0,
      "4": 0,
      "5": 0,
      "6": 0,
      "7": 0,
      "8": 0,
      "9": 0,
      "10": 0,
      "11": 0,
      "12": 0,
      "13": 0,
      "14": 0,
      "15": 0,
      "16": 0,
      "17": 0,
      "18": 0,
      "19": 0,
      "20": 0
    },
    {
      "1": 0,
      "2": 0,
      "3": 0,
      "4": 0,
      "5": 0,
      "6": 0,
      "7": 0,
      "8": 0,
      "9": 0,
      "10": 0,
      "11": 0,
      "12": 0,
      "13": 0,
      "14": 0,
      "15": 0,
      "16": 0,
      "17": 0,
      "18": 0,
      "19": 0,
      "20": 0
    },
    {
      "1": 0,
      "2": 0,
      "3": 0,
      "4": 0,
      "5": 0,
      "6": 0,
      "7": 0,
      "8": 0,
      "9": 0,
      "10": 0,
      "11": 0,
      "12": 0,
      "13": 0,
      "14": 0,
      "15": 0,
      "16": 0,
      "17": 0,
      "18": 0,
      "19": 0,
      "20": 0
    },
    {
      "1": 0,
      "2": 0,
      "3": 0,
      "4": 0,
      "5": 0,
      "6": 0,
      "7": 0,
      "8": 0,
      "9": 0,
      "10": 0,
      "11": 0,
      "12": 0,
      "13": 0,
      "14": 0,
      "15": 0,
      "16": 0,
      "17": 0,
      "18": 0,
      "19": 0,
      "20": 0
    },
    {
      "1": 0,
      "2": 0,
      "3": 0,
      "4": 0,
      "5": 0,
      "6": 0,
      "7": 0,
      "8": 0,
      "9": 0,
      "10": 0,
      "11": 0,
      "12": 0,
      "13": 0,
      "14": 0,
      "15": 0,
      "16": 0,
      "17": 0,
      "18": 0,
      "19": 0,
      "20": 0
    },
    {
      "1": 0,
      "2": 0,
      "3": 0,
      "4": 0,
      "5": 0,
      "6": 0,
      "7": 0,
      "8": 0,
      "9": 0,
      "10": 0,
      "11": 0,
      "12": 0,
      "13": 0,
      "14": 0,
      "15": 0,
      "16": 0,
      "17": 0,
      "18": 0,
      "19": 0,
      "20": 0
    },
    {
      "1": 0,
      "2": 0,
      "3": 0,
      "4": 0,
      "5": 0,
      "6": 0,
      "7": 0,
      "8": 0,
      "9": 0,
      "10": 0,
      "11": 0,
      "12": 0,
      "13": 0,
      "14": 0,
      "15": 0,
      "16": 0,
      "17": 0,
      "18": 0,
      "19": 0,
      "20": 0
    },
    {
      "1": 0,
      "2": 0,
      "3": 0,
      "4": 0,
      "5": 0,
      "6": 0,
      "7": 0,
      "8": 0,
      "9": 0,
      "10": 0,
      "11": 0,
      "12": 0,
      "13": 0,
      "14": 0,
      "15": 0,
      "16": 0,
      "17": 0,
      "18": 0,
      "19": 0,
      "20": 0
    },
    {
      "1": 0,
      "2": 0,
      "3": 0,
      "4": 0,
      "5": 0,
      "6": 0,
      "7": 0,
      "8": 0,
      "9": 0,
      "10": 0,
      "11": 0,
      "12": 0,
      "13": 0,
      "14": 0,
      "15": 0,
      "16": 0,
      "17": 0,
      "18": 0,
      "19": 0,
      "20": 0
    },
    {
      "1": 0,
      "2": 0,
      "3": 0,
      "4": 0,
      "5": 0,
      "6": 0,
      "7": 0,
      "8": 0,
      "9": 0,
      "10": 0,
      "11": 0,
      "12": 0,
      "13": 0,
      "14": 0,
      "15": 0,
      "16": 0,
      "17": 0,
      "18": 0,
      "19": 0,
      "20": 0
    },
    {
      "1": 0,
      "2": 0,
      "3": 0,
      "4": 0,
      "5": 0,
      "6": 0,
      "7": 0,
      "8": 0,
      "9": 0,
      "10": 0,
      "11": 0,
      "12": 0,
      "13": 0,
      "14": 0,
      "15": 0,
      "16": 0,
      "17": 0,
      "18": 0,
      "19": 0,
      "20": 0
    }
  ],
  "residentEtranger": [
    false,
    false,
    false,
    false,
    false,
    false,
    false,
    false,
    false,
    false,
    false,
    false,
    false,
    false,
    false,
    false,
    false,
    false,
    false,
    false
  ],
  "prixReventeAuto": {
    "1": 480952.3848,
    "2": 485761.908648,
    "3": 490619.52773448,
    "4": 495525.72301182,
    "5": 500480.98024194,
    "6": 505485.79004436,
    "7": 510540.64794481,
    "8": 515646.05442425,
    "9": 520802.5149685,
    "10": 526010.54011818,
    "11": 531270.64551936,
    "12": 536583.35197456,
    "13": 541949.1854943,
    "14": 547368.67734925,
    "15": 552842.36412274,
    "16": 558370.78776397,
    "17": 563954.49564161,
    "18": 569594.04059802,
    "19": 575289.981004,
    "20": 581042.88081404
  },
  "TRI": {
    "1": "",
    "2": "",
    "3": -36.43,
    "4": -18.9,
    "5": -10.89,
    "6": -6.65,
    "7": -4.17,
    "8": -2.71,
    "9": -1.74,
    "10": -1.05,
    "11": -0.59,
    "12": -0.26,
    "13": 0,
    "14": 0.2,
    "15": 0.36,
    "16": 0.54,
    "17": 0.69,
    "18": 0.81,
    "19": 0.91,
    "20": 1
  },
  "VAN": {
    "1": -65356.47,
    "2": -61835.54,
    "3": -58904.16,
    "4": -56542.08,
    "5": -54720.58,
    "6": -53308.76,
    "7": -52293.08,
    "8": -52613.58,
    "9": -53507.88,
    "10": -54598.17,
    "11": -56599.52,
    "12": -59241.18,
    "13": -62206.88,
    "14": -65486.52,
    "15": -69071.13,
    "16": -71114.81,
    "17": -73047.54,
    "18": -74874.2,
    "19": -76601.36,
    "20": -78234.14
  },
  "IRR10": -1.05,
  "VAN10": -54598.17,
  "IRR": "",
  "VAN_revente": -435826.54,
  "cash_flow_annuel_default": {
    "1": [
      -65356.4738657
    ],
    "2": [
      -76652.432484454,
      15187.311120501
    ],
    "3": [
      -76652.432484454,
      -31664.712484454,
      51103.110132507
    ],
    "4": [
      -76652.432484454,
      -31664.712484454,
      -31761.392484454,
      87479.816658892
    ],
    "5": [
      -76652.432484454,
      -31664.712484454,
      -31761.392484454,
      -31859.482484454,
      124333.37100987
    ],
    "6": [
      -76652.432484454,
      -31664.712484454,
      -31761.392484454,
      -31859.482484454,
      -31949.022484454,
      161786.80132172
    ],
    "7": [
      -76652.432484454,
      -31664.712484454,
      -31761.392484454,
      -31859.482484454,
      -31949.022484454,
      -31913.042484454,
      199720.22457439
    ],
    "8": [
      -76652.432484454,
      -31664.712484454,
      -31761.392484454,
      -31859.482484454,
      -31949.022484454,
      -31913.042484454,
      -31877.562484454,
      237006.75021565
    ],
    "9": [
      -76652.432484454,
      -31664.712484454,
      -31761.392484454,
      -31859.482484454,
      -31949.022484454,
      -31913.042484454,
      -31877.562484454,
      -31841.602484454,
      274479.94617045
    ],
    "10": [
      -76652.432484454,
      -31664.712484454,
      -31761.392484454,
      -31859.482484454,
      -31949.022484454,
      -31913.042484454,
      -31877.562484454,
      -31841.602484454,
      -31807.192484454,
      312582.69081753
    ],
    "11": [
      -76652.432484454,
      -31664.712484454,
      -31761.392484454,
      -31859.482484454,
      -31949.022484454,
      -31913.042484454,
      -31877.562484454,
      -31841.602484454,
      -31807.192484454,
      -31772.372484454,
      350402.0413812
    ],
    "12": [
      -76652.432484454,
      -31664.712484454,
      -31761.392484454,
      -31859.482484454,
      -31949.022484454,
      -31913.042484454,
      -31877.562484454,
      -31841.602484454,
      -31807.192484454,
      -31772.372484454,
      -32658.572484454,
      389171.04673468
    ],
    "13": [
      -76652.432484454,
      -31664.712484454,
      -31761.392484454,
      -31859.482484454,
      -31949.022484454,
      -31913.042484454,
      -31877.562484454,
      -31841.602484454,
      -31807.192484454,
      -31772.372484454,
      -32658.572484454,
      -33241.342484454,
      428984.15884835
    ],
    "14": [
      -76652.432484454,
      -31664.712484454,
      -31761.392484454,
      -31859.482484454,
      -31949.022484454,
      -31913.042484454,
      -31877.562484454,
      -31841.602484454,
      -31807.192484454,
      -31772.372484454,
      -32658.572484454,
      -33241.342484454,
      -33434.942484454,
      469458.56425666
    ],
    "15": [
      -76652.432484454,
      -31664.712484454,
      -31761.392484454,
      -31859.482484454,
      -31949.022484454,
      -31913.042484454,
      -31877.562484454,
      -31841.602484454,
      -31807.192484454,
      -31772.372484454,
      -32658.572484454,
      -33241.342484454,
      -33434.942484454,
      -33631.012484454,
      510601.85554392
    ],
    "16": [
      -76652.432484454,
      -31664.712484454,
      -31761.392484454,
      -31859.482484454,
      -31949.022484454,
      -31913.042484454,
      -31877.562484454,
      -31841.602484454,
      -31807.192484454,
      -31772.372484454,
      -32658.572484454,
      -33241.342484454,
      -33434.942484454,
      -33631.012484454,
      -33830.902484454,
      555083.71474187
    ],
    "17": [
      -76652.432484454,
      -31664.712484454,
      -31761.392484454,
      -31859.482484454,
      -31949.022484454,
      -31913.042484454,
      -31877.562484454,
      -31841.602484454,
      -31807.192484454,
      -31772.372484454,
      -32658.572484454,
      -33241.342484454,
      -33434.942484454,
      -33631.012484454,
      -33830.902484454,
      5609.66,
      560341.75813292
    ],
    "18": [
      -76652.432484454,
      -31664.712484454,
      -31761.392484454,
      -31859.482484454,
      -31949.022484454,
      -31913.042484454,
      -31877.562484454,
      -31841.602484454,
      -31807.192484454,
      -31772.372484454,
      -32658.572484454,
      -33241.342484454,
      -33434.942484454,
      -33631.012484454,
      -33830.902484454,
      5609.66,
      5626.61,
      565803.54551433
    ],
    "19": [
      -76652.432484454,
      -31664.712484454,
      -31761.392484454,
      -31859.482484454,
      -31949.022484454,
      -31913.042484454,
      -31877.562484454,
      -31841.602484454,
      -31807.192484454,
      -31772.372484454,
      -32658.572484454,
      -33241.342484454,
      -33434.942484454,
      -33631.012484454,
      -33830.902484454,
      5609.66,
      5626.61,
      5644.73,
      571469.00976548
    ],
    "20": [
      -76652.432484454,
      -31664.712484454,
      -31761.392484454,
      -31859.482484454,
      -31949.022484454,
      -31913.042484454,
      -31877.562484454,
      -31841.602484454,
      -31807.192484454,
      -31772.372484454,
      -32658.572484454,
      -33241.342484454,
      -33434.942484454,
      -33631.012484454,
      -33830.902484454,
      5609.66,
      5626.61,
      5644.73,
      5661.14,
      577342.81976595
    ]
  },
  "cash_flow_annuel_revente": [
    -76652.432484454,
    -31664.712484454,
    -31761.392484454,
    -31859.482484454,
    -31949.022484454,
    -31913.042484454,
    -31877.562484454,
    -31841.602484454,
    -31807.192484454,
    -31772.372484454,
    -32658.572484454,
    -33241.342484454,
    -33434.942484454,
    -33631.012484454,
    -33830.902484454,
    5609.66,
    5626.61,
    5644.73,
    5661.14,
    5677.66
  ],
  "enrichissement_default": -18743.75154255,
  "enrichissement_revente": -501675.7872668,
  "cash_flow_placement_annuel_default": {
    "1": [
      -76652.432484454
    ],
    "2": [
      -76652.432484454,
      -31664.712484454
    ],
    "3": [
      -76652.432484454,
      -31664.712484454,
      -31761.392484454
    ],
    "4": [
      -76652.432484454,
      -31664.712484454,
      -31761.392484454,
      -31859.482484454
    ],
    "5": [
      -76652.432484454,
      -31664.712484454,
      -31761.392484454,
      -31859.482484454,
      -31949.022484454
    ],
    "6": [
      -76652.432484454,
      -31664.712484454,
      -31761.392484454,
      -31859.482484454,
      -31949.022484454,
      -31913.042484454
    ],
    "7": [
      -76652.432484454,
      -31664.712484454,
      -31761.392484454,
      -31859.482484454,
      -31949.022484454,
      -31913.042484454,
      -31877.562484454
    ],
    "8": [
      -76652.432484454,
      -31664.712484454,
      -31761.392484454,
      -31859.482484454,
      -31949.022484454,
      -31913.042484454,
      -31877.562484454,
      -31841.602484454
    ],
    "9": [
      -76652.432484454,
      -31664.712484454,
      -31761.392484454,
      -31859.482484454,
      -31949.022484454,
      -31913.042484454,
      -31877.562484454,
      -31841.602484454,
      -31807.192484454
    ],
    "10": [
      -76652.432484454,
      -31664.712484454,
      -31761.392484454,
      -31859.482484454,
      -31949.022484454,
      -31913.042484454,
      -31877.562484454,
      -31841.602484454,
      -31807.192484454,
      -31772.372484454
    ],
    "11": [
      -76652.432484454,
      -31664.712484454,
      -31761.392484454,
      -31859.482484454,
      -31949.022484454,
      -31913.042484454,
      -31877.562484454,
      -31841.602484454,
      -31807.192484454,
      -31772.372484454,
      -32658.572484454
    ],
    "12": [
      -76652.432484454,
      -31664.712484454,
      -31761.392484454,
      -31859.482484454,
      -31949.022484454,
      -31913.042484454,
      -31877.562484454,
      -31841.602484454,
      -31807.192484454,
      -31772.372484454,
      -32658.572484454,
      -33241.342484454
    ],
    "13": [
      -76652.432484454,
      -31664.712484454,
      -31761.392484454,
      -31859.482484454,
      -31949.022484454,
      -31913.042484454,
      -31877.562484454,
      -31841.602484454,
      -31807.192484454,
      -31772.372484454,
      -32658.572484454,
      -33241.342484454,
      -33434.942484454
    ],
    "14": [
      -76652.432484454,
      -31664.712484454,
      -31761.392484454,
      -31859.482484454,
      -31949.022484454,
      -31913.042484454,
      -31877.562484454,
      -31841.602484454,
      -31807.192484454,
      -31772.372484454,
      -32658.572484454,
      -33241.342484454,
      -33434.942484454,
      -33631.012484454
    ],
    "15": [
      -76652.432484454,
      -31664.712484454,
      -31761.392484454,
      -31859.482484454,
      -31949.022484454,
      -31913.042484454,
      -31877.562484454,
      -31841.602484454,
      -31807.192484454,
      -31772.372484454,
      -32658.572484454,
      -33241.342484454,
      -33434.942484454,
      -33631.012484454,
      -33830.902484454
    ],
    "16": [
      -76652.432484454,
      -31664.712484454,
      -31761.392484454,
      -31859.482484454,
      -31949.022484454,
      -31913.042484454,
      -31877.562484454,
      -31841.602484454,
      -31807.192484454,
      -31772.372484454,
      -32658.572484454,
      -33241.342484454,
      -33434.942484454,
      -33631.012484454,
      -33830.902484454,
      0
    ],
    "17": [
      -76652.432484454,
      -31664.712484454,
      -31761.392484454,
      -31859.482484454,
      -31949.022484454,
      -31913.042484454,
      -31877.562484454,
      -31841.602484454,
      -31807.192484454,
      -31772.372484454,
      -32658.572484454,
      -33241.342484454,
      -33434.942484454,
      -33631.012484454,
      -33830.902484454,
      0,
      0
    ],
    "18": [
      -76652.432484454,
      -31664.712484454,
      -31761.392484454,
      -31859.482484454,
      -31949.022484454,
      -31913.042484454,
      -31877.562484454,
      -31841.602484454,
      -31807.192484454,
      -31772.372484454,
      -32658.572484454,
      -33241.342484454,
      -33434.942484454,
      -33631.012484454,
      -33830.902484454,
      0,
      0,
      0
    ],
    "19": [
      -76652.432484454,
      -31664.712484454,
      -31761.392484454,
      -31859.482484454,
      -31949.022484454,
      -31913.042484454,
      -31877.562484454,
      -31841.602484454,
      -31807.192484454,
      -31772.372484454,
      -32658.572484454,
      -33241.342484454,
      -33434.942484454,
      -33631.012484454,
      -33830.902484454,
      0,
      0,
      0,
      0
    ],
    "20": [
      -76652.432484454,
      -31664.712484454,
      -31761.392484454,
      -31859.482484454,
      -31949.022484454,
      -31913.042484454,
      -31877.562484454,
      -31841.602484454,
      -31807.192484454,
      -31772.372484454,
      -32658.572484454,
      -33241.342484454,
      -33434.942484454,
      -33631.012484454,
      -33830.902484454,
      0,
      0,
      0,
      0,
      0
    ]
  },
  "benefice_placement": 59755.4,
  "total_placement": 422854.22,
  "solde_placement": [
    78568.743296565,
    112989.29217554,
    148369.4517765,
    184734.65761747,
    222100.77210448,
    260364.15995365,
    299547.76549906,
    339674.1021831,
    380768.32703424,
    422854.21700666
  ],
  "interets_placement": [
    1916.3108121113,
    2755.8363945255,
    3618.7671164999,
    4505.7233565238,
    5417.0920025482,
    6350.3453647232,
    7306.0430609527,
    8284.7341995878,
    9287.0323666889,
    10313.517487967
  ],
  "TRI_placement": 2.98,
  "cash_flow_placement_annuel_default_revente": {
    "1": [
      -76652.432484454
    ],
    "2": [
      -76652.432484454,
      -31664.712484454
    ],
    "3": [
      -76652.432484454,
      -31664.712484454,
      -31761.392484454
    ],
    "4": [
      -76652.432484454,
      -31664.712484454,
      -31761.392484454,
      -31859.482484454
    ],
    "5": [
      -76652.432484454,
      -31664.712484454,
      -31761.392484454,
      -31859.482484454,
      -31949.022484454
    ],
    "6": [
      -76652.432484454,
      -31664.712484454,
      -31761.392484454,
      -31859.482484454,
      -31949.022484454,
      -31913.042484454
    ],
    "7": [
      -76652.432484454,
      -31664.712484454,
      -31761.392484454,
      -31859.482484454,
      -31949.022484454,
      -31913.042484454,
      -31877.562484454
    ],
    "8": [
      -76652.432484454,
      -31664.712484454,
      -31761.392484454,
      -31859.482484454,
      -31949.022484454,
      -31913.042484454,
      -31877.562484454,
      -31841.602484454
    ],
    "9": [
      -76652.432484454,
      -31664.712484454,
      -31761.392484454,
      -31859.482484454,
      -31949.022484454,
      -31913.042484454,
      -31877.562484454,
      -31841.602484454,
      -31807.192484454
    ],
    "10": [
      -76652.432484454,
      -31664.712484454,
      -31761.392484454,
      -31859.482484454,
      -31949.022484454,
      -31913.042484454,
      -31877.562484454,
      -31841.602484454,
      -31807.192484454,
      -31772.372484454
    ],
    "11": [
      -76652.432484454,
      -31664.712484454,
      -31761.392484454,
      -31859.482484454,
      -31949.022484454,
      -31913.042484454,
      -31877.562484454,
      -31841.602484454,
      -31807.192484454,
      -31772.372484454,
      -32658.572484454
    ],
    "12": [
      -76652.432484454,
      -31664.712484454,
      -31761.392484454,
      -31859.482484454,
      -31949.022484454,
      -31913.042484454,
      -31877.562484454,
      -31841.602484454,
      -31807.192484454,
      -31772.372484454,
      -32658.572484454,
      -33241.342484454
    ],
    "13": [
      -76652.432484454,
      -31664.712484454,
      -31761.392484454,
      -31859.482484454,
      -31949.022484454,
      -31913.042484454,
      -31877.562484454,
      -31841.602484454,
      -31807.192484454,
      -31772.372484454,
      -32658.572484454,
      -33241.342484454,
      -33434.942484454
    ],
    "14": [
      -76652.432484454,
      -31664.712484454,
      -31761.392484454,
      -31859.482484454,
      -31949.022484454,
      -31913.042484454,
      -31877.562484454,
      -31841.602484454,
      -31807.192484454,
      -31772.372484454,
      -32658.572484454,
      -33241.342484454,
      -33434.942484454,
      -33631.012484454
    ],
    "15": [
      -76652.432484454,
      -31664.712484454,
      -31761.392484454,
      -31859.482484454,
      -31949.022484454,
      -31913.042484454,
      -31877.562484454,
      -31841.602484454,
      -31807.192484454,
      -31772.372484454,
      -32658.572484454,
      -33241.342484454,
      -33434.942484454,
      -33631.012484454,
      -33830.902484454
    ],
    "16": [
      -76652.432484454,
      -31664.712484454,
      -31761.392484454,
      -31859.482484454,
      -31949.022484454,
      -31913.042484454,
      -31877.562484454,
      -31841.602484454,
      -31807.192484454,
      -31772.372484454,
      -32658.572484454,
      -33241.342484454,
      -33434.942484454,
      -33631.012484454,
      -33830.902484454,
      0
    ],
    "17": [
      -76652.432484454,
      -31664.712484454,
      -31761.392484454,
      -31859.482484454,
      -31949.022484454,
      -31913.042484454,
      -31877.562484454,
      -31841.602484454,
      -31807.192484454,
      -31772.372484454,
      -32658.572484454,
      -33241.342484454,
      -33434.942484454,
      -33631.012484454,
      -33830.902484454,
      0,
      0
    ],
    "18": [
      -76652.432484454,
      -31664.712484454,
      -31761.392484454,
      -31859.482484454,
      -31949.022484454,
      -31913.042484454,
      -31877.562484454,
      -31841.602484454,
      -31807.192484454,
      -31772.372484454,
      -32658.572484454,
      -33241.342484454,
      -33434.942484454,
      -33631.012484454,
      -33830.902484454,
      0,
      0,
      0
    ],
    "19": [
      -76652.432484454,
      -31664.712484454,
      -31761.392484454,
      -31859.482484454,
      -31949.022484454,
      -31913.042484454,
      -31877.562484454,
      -31841.602484454,
      -31807.192484454,
      -31772.372484454,
      -32658.572484454,
      -33241.342484454,
      -33434.942484454,
      -33631.012484454,
      -33830.902484454,
      0,
      0,
      0,
      0
    ],
    "20": [
      -76652.432484454,
      -31664.712484454,
      -31761.392484454,
      -31859.482484454,
      -31949.022484454,
      -31913.042484454,
      -31877.562484454,
      -31841.602484454,
      -31807.192484454,
      -31772.372484454,
      -32658.572484454,
      -33241.342484454,
      -33434.942484454,
      -33631.012484454,
      -33830.902484454,
      0,
      0,
      0,
      0,
      0
    ]
  },
  "benefice_placement_revente": 214660.59,
  "total_placement_revente": 744556.17,
  "solde_placement_revente": [
    78568.743296565,
    112989.29217554,
    148369.4517765,
    184734.65761747,
    222100.77210448,
    260364.15995365,
    299547.76549906,
    339674.1021831,
    380768.32703424,
    422854.21700666,
    466900.6092284,
    512645.50050567,
    559732.45406488,
    608197.55321306,
    658079.16708995,
    674531.1462672,
    691394.42492388,
    708679.28554698,
    726396.26768566,
    744556.1743778
  ],
  "interets_placement_revente": [
    1916.3108121113,
    2755.8363945255,
    3618.7671164999,
    4505.7233565238,
    5417.0920025482,
    6350.3453647232,
    7306.0430609527,
    8284.7341995878,
    9287.0323666889,
    10313.517487967,
    11387.819737278,
    12503.548792821,
    13652.011074753,
    14834.086663733,
    16050.711392438,
    16451.979177249,
    16863.27865668,
    17284.860623097,
    17716.982138675,
    18159.906692141
  ],
  "TRI_placement_revente": 2.69,
  "prix_revente_auto": 526010.54011818,
  "nb_years_default_tri_van": 10,
  "min_detention": 0,
  "score": 1,
  "valorisationAnnuellePrixBien": 1,
  "smartParams": {
    "plafondLoyer": "1",
    "autreFoncierAnnee": null,
    "tauxEmprunt": {
      "7": 0.85,
      "10": 1.01,
      "12": 1.2,
      "15": 1.32,
      "20": 1.55,
      "25": 1.92,
      "30": 2.3
    },
    "valorisationAnnuellePrixBien": 1,
    "tauxAssuranceVie": 2.5,
    "cfe": 250,
    "adhesioncga": "1",
    "reductionCgaImpot": "reductionImpot",
    "cga": 150,
    "comptable": 500,
    "duree_amortissement_gros_oeuvre": 50,
    "duree_amortissement_facades": 50,
    "duree_amortissement_equipements": 30,
    "duree_amortissement_agencements": 6,
    "p_amortissement_gros_oeuvre": 50,
    "p_amortissement_facades": 10,
    "p_amortissement_equipements": 20,
    "p_amortissement_agencements": 20,
    "p_prix_terrain": 10
  },
  "success": 1,
  "favorite": null,
  "sameCalcul": true,
  "id": "57ebb6c548177e12138b4567",
  "URL": "http://www.rendementlocatif.com/calcul/rendement?utm_source=android&utm_medium=cpc&utm_campaign=Traffic+Clic+Android#/calcul/type_appartement,neuf_0,prix_500000,travauxr_0,surface_150,ville_Paris%201%7C75001,loyer_1000,loyerannuel_,chargesr_78,tf_2700,chargest_3750,travauxa_0,emprunt_on,apport_34186,duree_15,tauxemprunt_1.32,assurance_0.36,assuranceeuros_,fraisdossier_5000,creditlogement_6900,regime_rr,mbles_0,dureeanah_6,prime_0,tauxmi_30,revenus_35000,revenus2_,parts_1,enfants_0,agence_5,fraisg_0,fraisrecherche_0,assuranceli_0,tauxmois_0,tauxan_1,pno_0,valorisation_1,valorisationc_2,valorisations_0.5,typeemprunt_amorti,emprunt110_off,fraisnotaire_,tauxps_15.5,revente_off,reventeannee_,reventeprix_0,fonciers2_"
}
```

### HTTP Request

`POST https://www.rendementlocatif.com/apiV2/calculate`


### Parameters

Should be sent with the request in the body as "application/x-www-form-urlencoded".

Parameter | Description | Type | Default
--------- | ------- | ------- | -------
prix | The price (mandatory) | float | N/A
username | The user name or email address | string | N/A
password | The user password | string | N/A
type | The type of bien | enum (“Appartement”, “maison”, “parking”, “immeuble”) | 
neuf | New | “0” (ancien), “1” (neuf) | 0
ville | City | string “nomdeville|codepostal” | “Angers|49000”
loyer | Rent | string | loyer
agence | % agency fee  |  |
surface |  |  |
tf |  |  | 
chargest |  |  |
chargesr |  |  |
creditlogement |  |  |
fraisdossier |  |  |
apport |  |  |
travauxa |  |  |
tauxmi |  |  |
tauxps |  |  |
regime |  |  |
duree  |  |  |
assurance |  |  |
tauxemprunt |  |  |
fraisg |  |  |
assuranceli |  |  |
valorisation |  |  |
valorisationc |  |  |
pno |  |  |


### Returned parameters

Parameter | Type | Description
--------- | ------- | -------
result | boolean | true if operation was marked successfully otherwise false
code | integer | 0 if ok otherwise error code of error
message | string | error message if error
