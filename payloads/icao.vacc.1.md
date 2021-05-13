# **ICAO's Visible Digital Seal: Vaccine** Payload

This Payload is defined by ICAO and contains a COVID-19 Immunization certificate of a **HOLDER** on a JSON Schema.

Fields in the **serialization** order:

1. `uvci`: *Required.* **STRING** Unique Test Certificate Identifier. Max 12.
1. `pid.n`: *Required.* **STRING** Name of the holder (as specified in Doc 9303-3). Max 39.
1. `pid.dob`: *Optional.* **DATE** Vaccinated person’s date of birth. REQUIRED if no Unique identifier is provided. Max 10.
1. `pid.sex`: *Optional.* **STRING** Sex of the holder (as specified in Doc 9303-4 Section 4.1.1.1 – Visual Inspection Zone). Max 1
1. `pid.i`: *Optional.* **STRING** Travel Document Number. Max 11. 
1. `pid.ai`: *Optional.*  **STRING** Additional Identifier. Any other document number at discretion of issuer. Max 24
1. `ve.des`: *Required.* **STRING** Vaccine/prophylaxis. ICD-11 Extension codes (http://id.who.int/icd/entity/164949870). Max 6
1. `ve.nam`: *Required.* **STRING** Vaccine medicinal product 
1. `ve.dis`: *Optional.* **STRING** Disease or agent that the vaccination provides protection against. Max 6
1. `ve.vd.dvc`: *Required.* **DATE** Date on which the vaccine was administered. The ISO8601 full date format YYYY-MM-DD MUST be used.
1. `ve.vd.seq`: *Required.* **STRING** Vaccine dose number. Max 2
1. `ve.vd.ctr`: *Required.* **STRING** The country in which the individual has been vaccinated. Max 3
1. `ve.vd.adm`: *Optional.* **STRING** Administering centre. Name/code of administering centre or a health authority responsible for the vaccination event. Max 20
1. `ve.vd.lot`: *Required.* **STRING** Lot Number. A distinctive combination of numbers and/or letters which specifically identifies a batch. Max 20
1. `ve.vd.dvn`: *Optional.* **DATE** Date on which the next vaccination should be administered. Max 10. 

## Types

1. **DATE**: Date type ISO 8601, in [ISO 8601 (YYYYMMDD) Basic Notation]
1. **TIMESTAMP**: Datetime in seconds since Epoch
1. **STRING**: String with 100 chars. 

## JSON Payload
When converting the credential back to a JSON structure, verifiers must hardcode this JSON template, replacing `${field}` by the content of `field`
```
{
    "data": {
        "hdr": {
            "t": "icao.vacc",
            "v": 1,
            "is": "UTO"
        },
        "msg": {
            "uvci": ${uvci},
            "pid": {
                "n": ${pid.n}
                "dob": ${pid.dob}
                "sex": ${pid.sex}
                "i": ${pid.i}
                "ai": ${pid.ai"}
            },
            "ve": [{
                "des": ${ve.des"}
                "nam": ${ve.nam"}
                "dis": ${ve.dis"}
                "vd": [{
                        "dvc": ${ve.vd.dvc"}
                        "seq": ${ve.vd.seq"}
                        "ctr": ${ve.vd.ctr"}
                        "adm": ${ve.vd.adm"}
                        "lot": ${ve.vd.lot"}
                        "dvn": ${ve.vd.dvn"}
                ]}
            }]
        }
    },
    "sig": {
        "alg": "ES256",
        "cer": "CERTIFICATE",
        "sigvl": "SIGNATURE"
    }
};
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
CRED:ICAO.VACC:1:GBCQEIDBJ6E5JCPHZSO6B22IBSTALXSWVBJLV2YPTF3RJ5URATGBVXXGTYB
CCAF2IQM2M7EPSPU3YRRBFKST4PZW5T7S774E3R7NAZI5HNJG65RBSE:1A9.PCF:U32870/SMITH
%20BILL/631238400/M/A1234567Z/L4567890Z/XM68M6/COMIRNATY/RA01.0/1614729600/1
/UTO/RIVM/VC35679/1616544000
``` 