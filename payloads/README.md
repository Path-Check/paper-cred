# How to Submit your Payload

The goal of the payload specification is to define the syntax and semantic meaning of the fields as well as their order in the serialization process. 

All payload type specifications must be submitted as pull requests to this folder.

Payload file names must match the payload type used in the [SPECIFICATION](../SPECIFICATION.md) followed by a `.` and their version number. Since QR codes can be printed and stored for many years, older versions of a QR can be scanned at the same time as newer versions. 

To submit your payload, follow template below 

# **NAME** Payload

Fields in the **serialization** order:
1. `fieldName`: *Required,Optional* **Type**. Description of the field. 
1. ...

## URI Example: 

```
CRED:NAME:VERSION:SIGNATURE:PUBKEY:FIELDS
```

## JSON example:
```json
{
  "type": "NAME",
  "version": 0,
  "data": {
    "fields": "FIELDS"
  },
  "signature": {
    "keyId": "1a9.cdc",
    "base32": "GBCAEIBS5LZ5JUYHBF3HGJABGROYE7QCP6YOZKTLDE67INBSVZVDBJ6ZQIBCALCKC3LQBOFB2P7TM4RG26526Z5ANE5Y5CPZAPFLM4XPLLPRJYXG"
  }
}
```

