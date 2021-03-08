# **PASSKEY** Payload

The **PASSKEY** QR code contains PII that can be correlated with other identification to authenticate the **HOLDER**.

Fields in the **serialization** order:
1. `name`: *Required.* **STRING**. The full name of the **HOLDER**, to be used when authenticating the **HOLDER**.
    1. In the event the name exceeds 255 bytes when encoded to UTF-8, the name should be truncated until its length does not exceed 255 bytes.
1. `dob`: *Required.* **DATE**. The date of birth of the **HOLDER**, to be used when authenticating the **HOLDER**.
1. `salt`: *Required.* **STRING**. The cryptographic salt, nonce, or IV used for **HASH** calculation.
1. `phone` *Optional.* **PHONE**. The phone number of the **HOLDER**, to be used when authenticating the **HOLDER**.

## Hashing Rules
When generating a passkey hash, the following rules MUST be followed to generate consistent results:
1. The only elements to be serialized should be the ones in the **DATA** block.
1. The elements MUST be concatenated in the order defined in the **PASSKEY** Serialization Order section, with the `ctrl-^` (character code `30`, hex `1E`, `RS`, or Record Separator) delimiter.
1. The concatenation should be a UTF-8 string.
1. The concatenation MUST be converted to uppercase prior to hashing.
1. The elements MUST NOT be Percent Encoded prior to hashing.
1. The output hash MUST be a SHA256 hash in base32 format without padding symbols (`=`).

Thus, the SHA256 hash of the data in the example below would be calculated as in the following pseudo-code:

```bash
hash(“${name}\x1E${dob}\x1E${phone}\x1E${salt}”) == hash(“JANE DOE\x1E19010101\x1E1BC93AB4AXD3”)
-> “FQTQAAKKCWUJMAEXAVZTERQDYX7VQB76M6R7XEAVWBQ6EOSQOZBA”
```

## JSON example:
```json
{
  "type": "passkey",
  "version": 1,
  "data": {
    "name": "Jane Doe",
    "dob": 19010101,
    "salt": "1Bc93ab4axd3",
    "phone": "16170000000",
  },
  "signature": {
    "keyId": "1a9.cdc",
    "base32": "GBCQEID3T2TRWYE6SWG5HIJIUXA6UXL7FHGR5MNSHCIHZ7KMA5PK5KYGWYBCCAHFBSX7T65JTMLFEQGTBISFNWGLKYVKRQOKEX5RN6TUU3R267ZQ7E"
  }
}
```