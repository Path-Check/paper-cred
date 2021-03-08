# How to Submit your Payload

The goal of the payload specification is to define the syntax and semantic meaning of the fields and an order of serialization. Each Payload represents a QR code. 

All payload type specifications must be submitted as pull requests to this folder.

Payload file names must match the payload type used in the [SPECIFICATION](../SPECIFICATION.md) followed by a `.` and their version number. As an example: `status.1.md` contains the specification for the 1st version of the status type. 

To submit your payload file, follow template below 

# **NAME** Payload

The **NAME** QR code is designated to contain information about ...

Fields in the **serialization** order:
1. `fieldName`: *Required,Optional* **Data Type**. Description of the field. 
1. ...

## Hashing Rules (if any)

## Example: 

```
CRED:NAME:VERSION:SIGNATURE:PUBKEY:FIELDS
```
