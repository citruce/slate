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

# API Access

> To authenticate a user use this code:

```shell
curl "https://www.rendementlocatif.com/api/login"
  -H "X-API-KEY: secretkey"
```

> Remplacez secretkey avec votre clef.

The Rendement locatif API expects an authentication key.

Just add the key to every request as a header like this:

`X-API-KEY: secretkey`

<aside class="notice">
Replace <code>secretkey</code> with your personal key.
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
    "username": "osamu"
}
```

This endpoint logs in a user and returns if the user is premium or not.

### HTTP Request

`POST https://www.rendementlocatif.com/api/login`

### Query Parameters

Parameter | Description
--------- | -------
username | The user name or email address
password | The user password


### Returned parameters

Parameter | Type | Description
--------- | ------- | -------
Auth | boolean | true if the user is authenticated ok and premium
userid | string | the is of the user (use this as the unique and permanent id)
Code | string | 0 : user is premium, -1 : unknown username, -2 : wrong password, -3 : free access granted
Message | string | a message that could be displayed to user (optionnal)
premium_end | date | end of current premium subscription period

<aside class="success">
The user should be authenticated if auth=true only (when premium access only is allowed).
If non premium access is allowed, check Auth=false and code=-3.
</aside>

## User authenticated requests

When requesting the API for user related content the username and password should always be passed as POST parameters with the request.

# Gestion

## Get Immeubles

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
    "count": 3,
    "immeubles": [
        {
            "id": "56b7b15a48177ea7085932cd",
            "name": "Pasteur Double Appart",
            "address": "100 avenue Pasteur",
            "biens": [
                {
                    "active": true,
                    "annnonceDesc": "Appartement lumineux à visiter le plus tôt possible.\n\nIl t'appartient de venir pour le voir ;)                        \n\nmodifié",
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
            ]
        },
        {
            "id": "5723cd6848177e7f298b4567",
            "name": "dsfdsf",
            "address": "sdfdsf",
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
            ]
        },
        {
            "id": "56a7766748177e22348b4567",
            "name": "Studio rue Iena",
            "address": "24, rue d'iena",
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
            ]
        }
    ],
    "result": true,
    "code": "0"
}
```

### HTTP Request

`POST https://www.rendementlocatif.com/api/gestion/2/immeubles`

### Query Parameters

Parameter | Description
--------- | -------
username | The user name or email address
password | The user password