# **ICAO's Visible Digital Seal: Vaccine ** Payload

This Payload is defined by ICAO and contains a COVID-19 Immunization certificate of a **HOLDER** on a JSON Schema.

Fields in the **serialization** order:

1. `utci`: *Required.* **STRING** Unique Test Certificate Identifier. Max 12.
1. `pid.n`: *Required.* **STRING** Name of the holder (as specified in Doc 9303-3). Max 39.
1. `pid.dob`: *Optional.* **DATE** Vaccinated person’s date of birth. REQUIRED if no Unique identifier is provided. Max 10.
1. `pid.dt`: *Required.* **STRING** Document Type
    | Code | Description | 
    | ---- | ----------- |
    | P | Passport (Doc 9303-4) |
    | A | ID Card (Doc 9303-5) |
    | C | ID Card (Doc 9303-5) |
    | I | ID Card Doc 9303-5) |
    | AC |  Crew Member Certificate (Doc 9303-5) |
    | V | Visa (Doc 9303-7) |
    | D | Driving License (ISO 18013-1) |
1. `pid.dn`: *Required.* **STRING** The ID Document Number of the identity document. The ID Document Number is the unique identifier of the test subject. Max 24
1. `sp.spn`: *Required.* **STRING** Name of testing facility or service provider. Max 20
1. `sp.ctr`: *Required.* **STRING** Country of testused. Max 3.
1. `sp.cd.p`: *Required.* **STRING** Contact number of testing facility or service. Max 19
1. `sp.cd.e`: **STRING** Email address of testing facility or service provider
1. `sp.cd.a`: **STRING** Address of testing facility or service provider
1. `dat.sc`: *Required.* **DATE** Date on which the sample was collected
1. `dat.ri`: *Required.* **DATE** Date on which the report was issued
1. `tr.tc`: *Required.* **STRING** Type of test conducted (molecular(PCR), molecular(other), antigen, antibody)
1. `tr.r`: *Required.* **STRING** Result of Test (normal, abnormal, positive, negative)
1. `tr.m`: *Optional.* **STRING** Sampling method (nasopharyngeal, oropharyngeal, saliva, blood, other)
1. `opt`: *Optional.* **STRING** Optional data issued at the discretion of the issuing authority. Max 20. 

## Types

1. DATE: Date type ISO 8601 - date part only, restricted to range 1900-2099. Regex: `[19|20][0-9][0-9]-(0[1-9]|1[0-2])-([0-2][1-9]|3[0|1])`
1. STRING: String with 100 chars. 

## JSON Payload
When converting the credential back to a JSON structure, verifiers must hardcode this JSON template, replacing `${field}` by the content of `field`
```
{
    "data": {
        "hdr": {
            "t": "icao.test",
            "v": 1,
            "is": "UTO"
        },
        "msg": {
            "utci": "U01932",
            "pid": {
                "n": "Cook Gerald",
                "dob": "1990-01-29",
                "dt": "P",
                "dn": "E1234567P"
            },
            "sp": {
                "spn": "General Hospital",
                "ctr": "UTO",
                "cd": {
                    "p": "+00068765432",
                    "e": "genhosp@mail.com",
                    "a": "12 Utopia Street"
                }
            },
            "dat": {
                "sc": "2020-12-12T12:00:00+08:00",
                "ri": "2021-02-11T14:00:00+08:00"
            },
            "tr": {
                "tc": "molecular(PCR)",
                "r": "negative",
                "m": "nasopharyngeal"
            },
            "opt": "ID12345"
        }
    },
    "sig": {
        "alg": "ES256",
        "cer": "MIIBeTCCAR2gAwIBAgIBZzAMBggqhkjOPQ...",
        "sigvl": "z_VZDdMvjjRkg06nYLwHt4BP_APEm3MJ..."
    }
}
```

### JSON Example:
```
{
    "data": {
        "hdr": {
            "t": "icao.vacc",
            "v": 1,
            "is": "UTO"
        },
        "msg": {
            "uvci": "U32870",
            "pid": {
                "n": "Smith Bill",
                "dob": "1990-01-02",
                "sex": "M",
                "i": "A1234567Z",
                "ai": "L4567890Z"
            },
            "ve": [{
                "des": "XM68M6",
                "nam": "Comirnaty",
                "dis": "RA01.0",
                "vd": [{
                        "dvc": "2021-03-03",
                        "seq": 1,
                        "ctr": "UTO",
                        "adm": "RIVM",
                        "lot": "VC35679",
                        "dvn": "2021-03-24"
                    },
                    {
                        "dvc": "2021-03-24",
                        "seq": 2,
                        "ctr": "UTO",
                        "adm": "RIVM",
                        "lot": "VC87540"
                    }]
            }]
        },
        "sig": {
            "alg": "ES256",
            "cer": "MIIBeTCCAR2gAwIBAgIBaDAMBggqhkjOPQ...",
            "sigvl": "cxfyi2vq2XJfZF7ksEkIZJtKbGrRE570..."
        }
    }
}
```

## Example:
```
CRED:EU.DGC.VAX:1:GBCQEIIAZ24WO7SM36K4J6ZKXOMMNHNIJ5L72D2WOIYPIMU3RJG36SVIQWTQEIA7Z
6WMLK3TFPCWL6O2M7NH2ZNDHDIL7M73NIXEVGQ3D467JABILU:1A9.PCF:D'ARS%C3%98NS%20-%20VAN%2
0HALEN/FRAN%C3%87OIS-JOAN/DARSONS%3CVAN%3CHALEN/FRANCOIS%3CJOAN/2009-02-28/84053900
6/1119349007/EU%2F1%2F20%2F1528/ORG-100030215/2/2/2021-04-27/NL/MINISTRY%20OF%20VWS
/01%3ANL%3APLA8UWS60Z4RZXVALL6GAZ
``` 