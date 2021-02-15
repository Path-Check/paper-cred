# PathCheck Vaccination Credential
## Printed Card Specification Version 1
### Status
This document is a DRAFT, written by Justin Dossey
<justin.dossey@newcontext.com>.

### Purpose
This document describes the data fields and format of the inputs to the
PathCheck Vaccination Credential pre-printed card.

### Terms and Definitions
For the purposes of brevity, this document refers to the following terms which are defined as follows:
1. **COUPON**: The **COUPON** QR code is designated to communicate eligibility for vaccination.
1. **BADGE**: The **BADGE** QR code is designated to contain information about the vaccines received by the **HOLDER**.
1. **STATUS**: The **STATUS** QR code is designated to contain information about the vaccination status of the **HOLDER**.
1. **PASSKEY**: The **PASSKEY** QR code contains information that can be correlated with other identification carried by the HOLDER to authenticate them.
1. **HOLDER**: The **HOLDER** is the party who has been or will be vaccinated, and is holding a pre-printed vaccination credential card.
1. **ISSUER**: The **ISSUER** is the party who delivers the vaccine and credential to a **HOLDER**.

### Data Types
This document will use the following terms to define data types.

1. **NUMERIC**: The **NUMERIC** data type is a sequence of integers between 0 and 99999999, inclusive.
2. **STRING**: The **STRING** data type is a sequence of unicode characters encoded as UTF-8, up to 255 bytes after encoding. All Unicode strings MUST be [NFC Normalized](https://www.unicode.org/faq/normalization.html).
3. **HASH**: The **HASH** data type is a sequence of alphanumeric characters containing a hexadecimal cryptographic hash. It is 64 bytes long.
3. **SIGNATUREHEX**: The **SIGNATUREHEX** data type is a sequence of alphanumeric characters containing a hexadecimal cryptographic digest. It is up to 72 bytes long.
4. **DATE**: a date, in [ISO 8601 (YYYYMMDD) Basic Notation](https://en.wikipedia.org/wiki/ISO_8601). Example: `20200201` is 1 February, 2020.
5. **SHORTSTRING**: a sequence of US-ASCII characters which is limited to 8 bytes in length.
6. **SHORTNUMERIC**: a **NUMERIC** with a maximum value of 9.

## General Offline Credential Data Format
All QR codes contain a type, a version, a payload and a cryptographic signature. The cryptographic signature is a SHA256 signature in hexadecimal form, calculated using the private ECDSA key of the **ISSUER**. The payload sections are designated the **DATA** block and the **SIGNATURE** block. A block is an object containing a number of key-value pairs. The **type** field defines the payload type and the **version** is a **NUMERIC** field defining the version of the type communicated in this QR code.

Data represented in QR codes can be encoded in the following formats:
1. [JSON](https://json.org) Format. When data length is not at issue, this format is simple to read and parse.
2. URI Format. When data length is a concern, this format may use fewer bytes than JSON. Keys should be compressed according to the URI Compression rules in this document. Values in the URI format are encoded per the standard using [Percent Encoding](https://en.wikipedia.org/wiki/Percent-encoding) .

With URI format, payload is organized according to the following URI schema:
```
cred:type:version:signatureHex@keyId?payload
```
The payload should be represented as a series of percent-encoded values delimited by the slash (`/`) character.
Example (with URI Compression, below):

```
cred:coupon:1:3046022100f82e28019428220d47be9b7dc9a50b4f0e6f9a6c95852a9272827cdbd8cb38d2022100b5d8738178cc1a12b590b25933857d967eb10178c5bbe045d132ec2513ddfa94@1a9?37/5000/San%20Francisco/1B/Teacher
```
Pseudo-Code describing assembly of the URI is below. 

```
$payload ::= [$number, $total, $city, $phase, $indicator];
for ($i ::= 0; $i < length($payload); $i += 1) do
  $upcasedValue ::= upcase($payload[$i]);
  $encodedValue ::= percentEncode($payload[$i]);
  $payload[$i] ::= $encodedValue;
end

$payloadString ::= join("/", $payload);
$signatureHex ::= ecdsaSign($payloadString);
$base ::= "cred:coupon:" + $version + ":" + $signatureHex + "@" + $keyId;
$upcasedBase ::= upcase($base);

$uri ::= $upcasedBase + "?" + $payloadString;
```

With the Json format, the payload is organized in the following schema:
```json
{
  "type": "",
  "version": 0,
  "data": {},
  "signature": {}
}
```

JSON example:
```json
{
  "type": "coupon",
  "version": 1,
  "data": {
    "number": 37,
    "total": 5000,
    "city": "San Francisco",
    "phase": "1B",
    "indicator": "Teacher"
  },
  "signature": {
    "keyId": "cdc:1a9",
    "hex": "3045022100ee898ba22454a92d972bc2573dbdb61b4cddbde9d90b264089d12201c5833e4402205e6c193f608906bdd3b395ead4ddbc602ee3cba08c2fbc5cb95ea9718d68ad2a"
  }
}
```
### URI Compression
To reduce data size, URI payloads are serialized after each field has been encoded. Serialization order is defined in each QR payload specification and key names are omitted.

### Data Ordering
In the JSON format, blocks and key/value pairs may occur in any order. For example, a JSON document with the **DATA** block after the **SIGNATURE** block is equivalent to a document with the **SIGNATURE** block after the **DATA** block.  Similarly, the key/value pairs within a block may appear in any order.

### Case Sensitivity
All fields (keys as well as values) are case-insensitive in both JSON and URI format. For clarity and ease of reading, examples in this document are given in mixed case. When performing operations such as hash comparison, a case-insensitive comparison function MUST be used.

### Signature and Hash Verification
Due to the Alphanumeric QR code character set, cryptographic signatures and hashes MUST be calculated against uppercased versions of the underlying data. Data to be used for hashes is serialized in the specified order. Signatures should be calculated against the actual data and order encoded to QR to permit
signature verification. In some cases, Percent encoding is used to address QR
code character set limitations. This encoding should be reversed before signature or hash verification.

### Format of the **SIGNATURE** Block
The signature block contains the hexadecimal ECDSA signature digest of the prepared **DATA** block and a *keyId* referencing the database and public key used to verify the ECDSA signature. For signature verification, devices should maintain indexed local key-value stores of approved public keys in PEM format. In the example below, the public key used to verify the signature is “1a9” in the “cdc” (local key/value) store. The colon character (`:`) is used as a delimiter to separate the key-value store identifier and the key identifier.

#### Signature Fields
1. *keyId*: **SHORTSTRING**. a string describing the database and index of the
   public key to be used when verifying the cryptographic signature of the
   **DATA** block.
2. *hex*: **SIGNATUREHEX**. The hexadecimal SHA256 digest ECDSA signature of the
   **DATA** block, calculated according to the rules above.

Example Signature Block (JSON fragment)
```json
"signature": {
    "keyId": "cdc:1a9",
   "hex": "3045022100f239e3cc363a0ef44a72c3458ea69620375b0875c541b3e1e4973c0d14bd4b4b02205aec76cb66eafbd0ec232c55cd95248cfa80e01dfd71ecbce7afc3196cdf178d"
}
```

## Coupon Payload Specification
Fields:
1. *number*: **NUMERIC**. The unique identifying number assigned to this coupon.
1. *total*: **NUMERIC**. The total number of coupons issued in the batch of coupons this one was issued from.
1. *city*: **STRING**. The name of the city, town, or other local area which designates vaccination eligibility and delivery schedule for the **HOLDER**.
    1. When the city name contains characters which cannot be encoded to QR, the city name may be Percent Encoded as part of QR Code generation. Readers
    must decode any substitutions prior to signature verification.
    1. In the event the city name exceeds 255 bytes when encoded to UTF-8, the last Unicode code point is removed until the resulting encoding is less than or equal to 255 bytes.
1. *phase*: **SHORTSTRING**. The vaccination phase assigned to the **HOLDER**.
1. *indicator*: **SHORTSTRING**. An indication of the priority assignment for **HOLDER**, or the literal string "none" if there is no priority assignment.
### Coupon Serialization Order:
In situations requiring data serialization, the fields in the Coupon payload MUST be serialized in the following order:
1. Number
1. Total
1. City
1. Phase
1. Indicator
### Hashing Rules:
When generating a passkey hash (for inclusion in the **BADGE** structure), the following rules MUST be followed to generate consistent results:
1. The only elements to be serialized should be the ones in the **DATA** block.
1. The elements MUST be concatenated in the order defined in the Coupon Serialization Order section of the specification, with the ctrl-^ (character code 30, hex 1E, RS, or Record Separator) delimiter:
1. The concatenation should be a UTF-8 string.
1. The concatenation MUST be converted to uppercase prior to hashing per the
   rules defined by Unicode, e.g. á is converted to Á.
1. The elements MUST NOT be URL Encoded prior to hashing.
1. The output MUST be in hexadecimal format.

Thus, the SHA256 hash of the data in the example below would be calculated as in the following pseudo-code:

```
$fields ::= [$number, $total, $city, $phase, $indicator];
for ($i ::= 0; $i < length($fields); $i += 1) do
  $uppercasedValue ::= upcase($fields[$i]);
  $fields[$i] ::= $uppercasedValue;
end
hash(join("\x1E", $fields))
== hash(“37\x1E5000\x1EBOSTON\x1E1B\x1ETEACHER”)
-> “5a688b8230705d88b9f3bef1f23e099f8ee140e04c05a8575531808810019487”
```

JSON example:
```json
{
  "type": "coupon",
  "version": 1,
  "data": {
    "number": 37,
    "total": 5000,
    "city": "San Francisco",
    "phase": "1B",
    "indicator": "Teacher"
  },
  "signature": {
    "keyId": "cdc:1a9",
    "hex": "3045022100ee898ba22454a92d972bc2573dbdb61b4cddbde9d90b264089d12201c5833e4402205e6c193f608906bdd3b395ead4ddbc602ee3cba08c2fbc5cb95ea9718d68ad2a"
  }
}
```

## Badge Payload Specification
Fields:
1. *coupon*: **HASH**. The cryptographic hash of the data in the coupon.
1. *doseInfo*: **DOSEINFO**. Information about the dose or doses received by the **HOLDER**.
1. *passkey*: **HASH**. The cryptographic hash of the data in the Passkey, as defined in the Passkey specification.


### The **DOSEINFO** Structure
The **DOSEINFO** structure is an array of **DOSE**s, delimited by the plus (`+`) character. Example of a **DOSEINFO** structure: `"1 PFIZER 13a056+2 PFIZER 29a063"`
#### **DOSEINFO** Serialization
1. For URI formats, the **DOSEINFO** structure must be serialized to a string.
1. This serialization should be accomplished by joining each serialized **DOSE**
structure inside the **DOSEINFO** structure.
1. The elements MUST be concatenated in order with the ctrl-^ (character code 30, hex 1E, RS, or Record Separator) delimiter.
Pseudo-code:
```
serialize(DOSEINFO) ::= join("\x1E", [DOSE, DOSE])
```
Combined with the **DOSE** serialization below, this would create the following
output with the data in the JSON example:
```
"1\x1DPFIZER\x1D13a056\x1E2\x1DPFIZER\x1D29a063"
```

### The **DOSE** Structure
A **DOSE** is an array containing a dose ID, a vaccine producer designation, and a lot number. Example: `[1, "PFIZER", "13a056"]` indicates "Dose 1, from Pfizer, lot number 13a056." In the event of the Vaccine Producer Designation exceeding the storage capacity of SHORTSTRING, only the first eight (8) bytes of the Vaccine Producer Designation should be used.
Fields:
1. Dose ID: **SHORTNUMERIC**.
1. Vaccine Producer Designation: **SHORTSTRING**.
1. Lot number: **SHORTSTRING**.

#### **DOSE** Serialization
1. For URI formats, the **DOSEINFO** structure must be serialized to a string.
1. This serialization should be accomplished by joining each serialized **DOSE**
structure inside the **DOSEINFO** structure.
1. The elements MUST be concatenated in order with the ctrl-] (character code 29, hex 1D, GS, or Group Separator) delimiter.
Pseudo-code:
```
serialize(DOSE) ::= join("\x1D", [1, "PFIZER", "13a056"])
-> "1\x1DPFIZER\x1D13a056"
```

### Badge Serialization Order:
In situations requiring payload serialization, the fields in the Badge payload MUST be serialized in the following order:
1. coupon
2. doseinfo
3. passkey

JSON example:
```json
{
  "type": "badge",
  "version": 1,
  "data": {
    "coupon": "5a688b8230705d88b9f3bef1f23e099f8ee140e04c05a8575531808810019487",
    "doseInfo": [[1, "PFIZER", "13a056"], [2, "PFIZER", "29a063"]],
    "passkey": "d9116bbdf7e33414b23ce81b2d4b9079a111d7119be010a5dcde68a1e5414d2d"
  },
  "signature": {
    "keyId": "cdc:1a9",
    "hex": "3046022100fdff876e90286a0b06c4181d78b23d5b960cb60a53a94f946b12bbb321ec24c6022100c22e739dfd59b37f8eddc915475190e12f8c10a632560afaab68e498c12de2ac"
  }
}
```

## Status Payload Specification
Fields:
1. *vaccinated*: **SHORTNUMERIC**. The vaccination status of the **HOLDER**. Currently designated values are below. Future versions of this specification may designate other values as required.
   * 0: The **HOLDER** has not received any vaccination.
   * 1: The **HOLDER** has started, but not completed, a course of vaccination.
   * 2: The **HOLDER** has completed the full vaccination course.
1. *passkey*: **HASH**. The cryptographic hash of the data in the Passkey, as defined by the Passkey Specification.

### Status Serialization Order:
In situations requiring payload serialization, the fields in the Status payload MUST be serialized in the following order:
1. vaccinated
2. passkey

JSON example:
```json
{
  "type": "status",
  "version": 1,
  "data": {
    "vaccinated": 2,
    "passkey": "d9116bbdf7e33414b23ce81b2d4b9079a111d7119be010a5dcde68a1e5414d2d"
  },
  "signature": {
    "keyId": "cdc:1a9",
    "hex": "304402203210213d35685bed9c9df83218839d0ea1f10cf50e64b0a3a8fbdcfbde0a6bf00220494bfc07481d285d00de9cf5b30a81754314b9cfccb6651597c733cc680ca588"
  }
}
```

## Passkey Payload Specification
Fields:
1. *name*: **STRING**. The full name of the **HOLDER**, to be used when authenticating the **HOLDER**.
    1. In the event the name exceeds 255 bytes when encoded to UTF-8, the name
    should be truncated until its length does not exceed 255 bytes.
1. *dob*: **DATE**. The date of birth of the **HOLDER**, to be used when authenticating the **HOLDER**.
1. *salt*: **STRING**. The cryptographic salt, nonce, or IV used for **HASH** calculation.

### Passkey Serialization Order:
In situations requiring payload serialization, the fields in the Passkey payload MUST be serialized in the following order:
1. Name
2. DoB
3. Salt

### Passkey Hashing Rules:
When generating a passkey hash (for inclusion in the **BADGE** structure), the
following rules MUST be followed to generate consistent results:
1. The only elements to be serialized should be the ones in the **DATA** block.
1. The elements MUST be concatenated in the order defined in the Passkey Serialization Order section,, with the ctrl-^ (character code 30, hex 1E, RS, or Record Separator) delimiter.
1. The concatenation should be a UTF-8 string.
1. The concatenation MUST be converted to uppercase prior to hashing.
1. The elements MUST NOT be URL encoded prior to hashing.
1. The output MUST be in hexadecimal format.
Thus, the SHA256 hash of the data in the example below would be calculated as in the following pseudo-code:

```
hash(“${name}\x1E${DoB}\x1E${salt}”) == hash(“JANE DOE\x1E19010101\x1E1BC93AB4AXD3”)
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
    "dob": 19010101,
    "salt": "1Bc93ab4axd3"
  },
  "signature": {
    "keyId": "cdc:1a9",
    "hex": "3046022100f82e28019428220d47be9b7dc9a50b4f0e6f9a6c95852a9272827cdbd8cb38d2022100b5d8738178cc1a12b590b25933857d967eb10178c5bbe045d132ec2513ddfa94"
  }
}
```
