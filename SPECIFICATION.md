# PathCheck Vaccination Credential
## Printed Card Specification Version 1
### Status
This document is a DRAFT, written by Justin Dossey
<justin.dossey@newcontext.com>.

### Purpose
This document describes the data fields and format of the inputs to the
PathCheck Vaccination Credential pre-printed card.

### Terms and Definitions
For the purposes of brevity, this document refers to the following terms which
are defined as follows:
1. **COUPON**: the **COUPON** QR code is designated to communicate eligibility for
   vaccination.
1. **BADGE**: the **BADGE** QR code is designated to contain information about the
   vaccines received by the **HOLDER**.
1. **STATUS**: the **STATUS** QR code is designated to contain information about the
   vaccination status of the **HOLDER**.
1. **PASSKEY**: the **PASSKEY** QR code contains information that can be correlated with other identification carried by the HOLDER to authenticate them.
1. **HOLDER**: The **HOLDER** is the party who has been or will be vaccinated, and is holding a pre-printed vaccination credential card.
1. **ISSUER**: The **ISSUER** is the party who delivers the vaccine and credential to a **HOLDER**.

### Data Types
This document will use the following terms to define data types.

1. **NUMERIC**: The **NUMERIC** data type is a sequence of integers between 0 and
   99999999.
2. **STRING**: The **STRING** data type is a sequence of UTF-8 characters, up to
   255 bytes.
3. **HASH**: The **HASH** data type is a sequence of alphanumeric characters
   containing a cryptographic hash. It is 64 bytes long.
4. **BIRTHDATE**: a date of birth, in
   [ISO 8601 (YYYYMMDD) Basic Notation](https://en.wikipedia.org/wiki/ISO_8601).
   Example:
   `20200201` is 1 February, 2020.
5. **SHORTSTRING**: a **STRING** which is limited to 8 bytes instead of 255.
6. **SHORTNUMERIC**: a **NUMERIC** with a maximum value of 9.

## Data Formats
Data represented in QR codes can be encoded in the following formats:
1. [JSON](https://json.org) Format. When data length is not at issue, this
   format is simple to read and parse.
2. URI Format. When data length is a concern, this format may use fewer bytes
   than JSON. Values in the URI format are
   [Percent Encoded](https://en.wikipedia.org/wiki/Percent-encoding)

   With URI format, data is organized according to the following URI
   schema:

```
cred:type:version:signatureHex@keyId?data
```

Example:

```
cred:coupon:1:3046022100f82e28019428220d47be9b7dc9a50b4f0e6f9a6c95852a9272827cdbd8cb38d2022100b5d8738178cc1a12b590b25933857d967eb10178c5bbe045d132ec2513ddfa94@1a9?number=37&total=5000&city=San%20Francisco&phase=1B&indicator=Teacher
```


## QR Code Specifications
All QR codes contain a data set and a cryptographic signature. The
cryptographic signature is a SHA256 digest in hexadecimal form, calculated using
the private ECDSA key of the **ISSUER**. The two blocks are designated the **DATA**
block and the **SIGNATURE** block.

### Data Ordering
In the JSON format, blocks and key/value pairs may occur in any order. For
example, a JSON document with the **DATA** block after the **SIGNATURE** block is
equivalent to a document with the **SIGNATURE** block after the **DATA** block.
Similarly, the key/value pairs within a block may appear in any order.

### Case Sensitivity
All fields (keys as well as values) are case-insensitive in both JSON and URI
format. For clarity and ease of reading, examples in this document are given
in mixed case. When performing operations such as hash comparison, a
case-insensitive comparison function must be used.

### Signature and Hash Verification
Due to the Alphanumeric QR code character set, cryptographic signatures and hashes must be calculated against uppercased versions of the underlying data. Data to be used for hashes is serialized in the specified order. Signatures should be calculated against the actual data and order encoded to QR to permit signature verification. In some cases, Percent encoding is used to address QR code character set limitations. This encoding should be reversed before signature or hash verification.

### Format of the **SIGNATURE** Block
The signature block contains the hexadecimal ECDSA signature digest of the prepared **DATA** block and a keyId referencing the database and public key used to verify the ECDSA signature. For signature verification, devices should maintain indexed local key-value stores of approved public keys in PEM format. In the example below, the public key used to verify the signature is “1a9” in the “cdc” (local key/value) store.

Example Signature Block *\1* fragment)
```json
"signature": {
    "keyId": "cdc:1a9",
   "hex": "3046022100f82e28019428220d47be9b7dc9a50b4f0e6f9a6c95852a9272827cdbd8cb38d2022100b5d8738178cc1a12b590b25933857d967eb10178c5bbe045d132ec2513ddfa94",
}
```

### Coupon Specification
Fields:
1. *version*: **NUMERIC**. The version of the specification defining the data
   communicated in this QR code.
2. *number*: **NUMERIC**. The unique identifying number assigned to this coupon.
3. *total*: **NUMERIC**. The total number of coupons issued in the batch of coupons this one
   was issued from.
4. *city*: **STRING**. The name of the city, town, or other local
   area which designates vaccination eligibility and delivery schedule for the
   **HOLDER**.
5. *phase*: **SHORTSTRING**. The vaccination phase assigned to the **HOLDER**.
6. *indicator*: **SHORTSTRING**. an indication of the priority assignment for
   **HOLDER**, or "none" if there is no priority.

#### Hashing Rules:
When generating a passkey hash (for inclusion in the **BADGE** structure), the
following rules must be followed to generate consistent results:
1. The only elements to be serialized should be the ones in the **DATA** block.
2. The elements must be concatenated in the following order:
  1. number
  1. total
  1. city
  1. phase
  1. indicator

1. The concatenation should be a UTF-8 string.
1. The concatenation MUST be converted to uppercase prior to hashing.
1. The elements MUST NOT be URL Encoded  prior to hashing.
1. The output must be in hexadecimal format.

Thus, the SHA256 hash of the data in the example below would be calculated as follows:

```
hash(“${number}${total}${city}${phase}${indicator}”) == hash(“37500BOSTON1BTEACHER”)
-> “710183e3780fed3c48fce4b38da83775a7c47e9961b4a7ee822628e8c190359e”
```

JSON example:
```json
{
  "type": "coupon",
  "version": 1,
  "data": {
    "number": 37,
    "total": 5000,
    "city": "Boston",
    "phase": "1B",
    "indicator": "Teacher"
  },
  "signature": {
    "keyId": "cdc:1a9",
    "hex": "3046022100f82e28019428220d47be9b7dc9a50b4f0e6f9a6c95852a9272827cdbd8cb38d2022100b5d8738178cc1a12b590b25933857d967eb10178c5bbe045d132ec2513ddfa94",
  }
}
```

### Badge Specification
Fields:
1. *version*: **NUMERIC**. The version of the specification defining the data
   communicated in this QR code.
2. *coupon*: **HASH**. The cryptographic hash of the data in the coupon.
3. *doseInfo*: **DOSEINFO**. Information about the dose or doses received by the
   **HOLDER**.
4. *passkey*: **HASH**. The cryptographic hash of the data in the Passkey, as defined in the Passkey specification.

The **DOSEINFO** stucture
The **DOSEINFO** structure represents an array of **DOSE**s, delimited by the plus (`+`) character.
Example of a **DOSEINFO** structure: `"1 PFIZER 13a056+2 PFIZER 29a063"`

A **DOSE** is a string containing a vaccine producer designation, a lot number,
and a dose ID. Example: `"1 PFIZER 13a056"` indicates "Dose 1, from Pfizer, lot
number 13a056."

JSON example:
```json
{
  "type": "badge",
  "version": 1,
  "data": {
    "coupon": "710183e3780fed3c48fce4b38da83775a7c47e9961b4a7ee822628e8c190359e",
    "doseInfo": "1 PFIZER 13a056+2 PFIZER 29a063",
    "passkey": "e607c3b9b9448403a6b3cddd83f397bd17084c1db6fdeb081e9bd8392f21a1e6"
  },
  "signature": {
    "keyId": "cdc:1a9",
    "hex": "3046022100f82e28019428220d47be9b7dc9a50b4f0e6f9a6c95852a9272827cdbd8cb38d2022100b5d8738178cc1a12b590b25933857d967eb10178c5bbe045d132ec2513ddfa94"
  }
}
```

### Status Specification
Fields:
1. *version*: **NUMERIC**. The version of the specification defining the data
   communicated in this QR code.
2. *vaccinated*: **SHORTNUMERIC**. The vaccination status of the **HOLDER**.
   Currently designated values:
   * 0: The **HOLDER** has not received any vaccine.
   * 1: The **HOLDER** has received the first (of two) dose of a vaccine.
   * 2: The **HOLDER** has received the second (of two) dose of a vaccine.
3. *passkey*: **HASH**. The cryptographic hash of the data in the Passkey, as defined by the Passkey Specification.

JSON example:
```json
{
  "type": "status",
  "version": 1,
  "data": {
    "vaccinated": 2,
    "passkey": "e607c3b9b9448403a6b3cddd83f397bd17084c1db6fdeb081e9bd8392f21a1e6"
  },
  "signature": {
    "keyId": "cdc:1a9",
    "hex": "3046022100f82e28019428220d47be9b7dc9a50b4f0e6f9a6c95852a9272827cdbd8cb38d2022100b5d8738178cc1a12b590b25933857d967eb10178c5bbe045d132ec2513ddfa94"
  }
}
```

### Passkey Specification
Fields:
1. *version*: **NUMERIC**. The version of the specification defining the data
   communicated in this QR code.
2. *name*: **STRING**. The full name of the **HOLDER**, to be used when authenticating
   the **HOLDER**.
3. *DoB*: **BIRTHDATE**. The date of birth of the **HOLDER**, to be used when authenticating the **HOLDER**.
4. *salt*: **STRING**. The cryptographic salt, nonce, or IV used for **HASH** calculation.
Hashing Rules:
When generating a passkey hash (for inclusion in the **BADGE** structure), the following rules must be followed to generate consistent results:
1. The only elements to be serialized should be the ones in the **DATA** block.
1. The elements must be concatenated in the following order:
  1. Name
  1. DoB
  1. Salt
1. The concatenation should be a UTF-8 string.
1. The concatenation MUST be converted to uppercase prior to hashing.
1. The elements MUST NOT be URL encoded prior to hashing.
1. The output must be in hexadecimal format.
Thus, the SHA256 hash of the data in the example below would be calculated as follows:
```
hash(“${name}${DoB}${salt}”) == hash(“JANE DOE190101011BC93AB4AXD3”)
-> “e607c3b9b9448403a6b3cddd83f397bd17084c1db6fdeb081e9bd8392f21a1e6”
```


JSON example:
```json
{
  "type": "passkey",
  "version": 1,
  "data": {
    "name": "Jane Doe",
    "DoB": 19010101,
    "salt": "1Bc93ab4axd3",
  },
  "signature": {
    "keyId": "cdc:1a9",
    "hex": "3046022100f82e28019428220d47be9b7dc9a50b4f0e6f9a6c95852a9272827cdbd8cb38d2022100b5d8738178cc1a12b590b25933857d967eb10178c5bbe045d132ec2513ddfa94"
  }
}
```