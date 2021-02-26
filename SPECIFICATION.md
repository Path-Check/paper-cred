# PathCheck Vaccination Credential - Version 1
**© 2021 PathCheck Foundation**<br/>
Authors: [Justin Dosse](mailto:justin.dossey@pathcheck.org) and [Vitor Pamplona](vitor.pamplona@pathcheck.org)<br/>
Status: *DRAFT*<br/>
Date: Feb 26, 2021<br/>

# Purpose
This document describes the format and data fields to PathCheck's Verifiable Credential QR Code stickers for the off-line vaccination user journey.

# Introduction

This specification creates 4 signed QR Codes to manage the user journey. The separate QR Codes are intended to decouple the health information (PHI) and personally identifiable information (PII) as well as separate vaccination eligibility verification from the excecution of it. 

## Terms and Definitions
For the purposes of brevity, this document refers to the following terms which are defined as follows:
1. **COUPON**: The **COUPON** QR code is designated to communicate eligibility for vaccination.
1. **BADGE**: The **BADGE** QR code is designated to contain information about a vaccination event received by the **HOLDER**.
1. **STATUS**: The **STATUS** QR code is designated to contain information about the vaccination status of the **HOLDER**.
1. **PASSKEY**: The **PASSKEY** QR code contains information that can be correlated with other identification carried by the HOLDER to authenticate them.
1. **HOLDER**: The **HOLDER** is the party who has been or will be vaccinated, and is holding a pre-printed vaccination credential card.
1. **ISSUER**: The **ISSUER** is the party who delivers the vaccine and credential to a **HOLDER**.

## Data Types

This document will use the following terms to define data types.
1. **NUMERIC**: The **NUMERIC** data type is a sequence of integers between 0 and 99999999, inclusive.
2. **STRING**: The **STRING** data type is a sequence of unicode characters encoded as UTF-8, up to 255 bytes after encoding. All Unicode strings MUST be [NFC Normalized](https://www.unicode.org/faq/normalization.html).
3. **HASH**: The **HASH** data type is a sequence of alphanumeric characters containing a hexadecimal cryptographic hash. It is 64 bytes long.
3. **SIGNATUREHEX**: The **SIGNATUREHEX** data type is a sequence of alphanumeric characters containing a hexadecimal cryptographic digest. It is up to 72 bytes long.
4. **DATE**: a date, in [ISO 8601 (YYYYMMDD) Basic Notation](https://en.wikipedia.org/wiki/ISO_8601). Example: `20200201` is 1 February, 2020.
5. **SHORTSTRING**: a sequence of US-ASCII characters which is limited to 8 bytes in length.
6. **SHORTNUMERIC**: a **NUMERIC** with a maximum value of 9.
7. **PHONE**: a E.164 formatted phone number as string. US-ASCII, maximum 15 characters.

# General Structure
All QR codes contain a message with: 
1. the type of certificate
1. the version
1. the payload itself
1. the reference to a public key
1. and a cryptographic signature of the payload

The **type** field defines the payload type (coupon, passkey, badge, status) and the **version** is a **NUMERIC** field defining the version of the type of certificate in this QR code. The payload section contains if information related to the certificate itself. The cryptographic signature is a SHA256 signature in hexadecimal form, calculated using the private ECDSA key of the **ISSUER**. The Public Key can be: 
1. a URI to a DNS TXT Record containing the key for download.
1. a URL address to download keys from. 
1. a database and key ID to facilitate trusted lists of issuers. 

For signature verification, devices should maintain an indexed local key-value stores of approved public keys in PEM format. 

Data represented in QR codes can be encoded in JSON and URI formats, but URI formats are strongly preferred due to their smaller message size. Examples are in JSON for easier readability but both encodings are described in this document.

## Case Insensitivity

All fields (keys as well as values) are case-insensitive in both JSON and URI format. When performing operations such as hash comparison, a case-insensitive comparison function MUST be used. Additionally, the Alphanumeric QR Code character set does not include lowercase characters, so implementations MUST encode output in uppercase only.

For clarity and ease of reading, examples in this document are given in mixed case. 

## Encoding

Percent encoding is used to address QR code character set limitations. 

## Signing and Hashing Constraints

* Cryptographic signatures and hashes *MUST* be calculated against *uppercased*, *percent encoded* versions of the underlying payload.
* Data to be used for hashes is serialized in the specified order this document describes. 
* Signatures should be calculated against the final format and order encoded in the payload field of the QR to permit signature verification before any decoding.

# The JSON Format

When data length is not at issue, this format is simple to read and parse. With the Json format, the payload is organized in the following schema:
```json
{
  "type": "",
  "version": 0,
  "data": {},
  "signature": {}
}
```

The payload section is designated the **DATA** block and the cryptographic signature is contained in the **SIGNATURE** block. A block is an object containing a number of key-value pairs. 

## Order of Fields
In the JSON format, blocks and key/value pairs may occur in any order. For example, a JSON document with the **DATA** block after the **SIGNATURE** block is equivalent to a document with the **SIGNATURE** block after the **DATA** block. Similarly, the key/value pairs within a block may appear in any order.

## The **SIGNATURE** Block
The signature block contains the hexadecimal ECDSA signature digest of the prepared **DATA** block and a *keyId* referencing the database and public key used to verify the ECDSA signature. In the example below, the public key used to verify the signature is “1a9” in the “cdc” (local key/value) store. The colon character (`:`) is used as a delimiter to separate the key-value store identifier and the key identifier.

1. *keyId*: **SHORTSTRING**. a string describing the database and index, the url to download the PEM, or the DNS TXT record of the
   public key to be used when verifying the cryptographic signature of the **DATA** block.
2. *hex*: **SIGNATUREHEX**. The hexadecimal SHA256 digest ECDSA signature of the
   **DATA** block, calculated according to the rules above.

### JSON Example
```json
{
  "type": "coupon",
  "version": 1,
  "data": { ... },
  "signature": {
    "keyId": "cdc:1a9",
    "hex": "3045022100ee898ba22454a92d972bc2573dbdb61b4cddbde9d90b264089d12201c5833e4402205e6c193f608906bdd3b395ead4ddbc602ee3cba08c2fbc5cb95ea9718d68ad2a"
  }
}
```

# The URI Format
When data length is a concern, this format may use fewer bytes than JSON. Keys should be compressed according to the URI Compression rules in this document. Values in the URI format are encoded per the standard using [Percent Encoding](https://en.wikipedia.org/wiki/Percent-encoding). Note that all characters not present in the Alphanumeric QR scheme must be percent encoded.

## URI Schema
With URI format, payload is organized according to the following URI schema starting with "cred":
```
cred:type:version:signatureHex.keyId?payload
```
The payload should be represented as a series of uppercased, percent-encoded values delimited by the slash (`/`) character. The serialization order is defined in each type of payload specification and key names are omitted.

## URI Example

The example below is a valid credential
```
cred:coupon:1:3046022100f82e28019428220d47be9b7dc9a50b4f0e6f9a6c95852a9272827cdbd8cb38d2022100b5d8738178cc1a12b590b25933857d967eb10178c5bbe045d132ec2513ddfa94.cdc:1a9?37/5000/San%20Francisco/1B/Teacher
```

## Percent Encoding Payload Fields

All characters not present in the Alphanumeric QR scheme must be percent
encoded when presented in data fields. RFC 2936 requires percent encoding a
number of characters, but some of the characters not required to be encoded are
not included in the Alphanumeric QR character set. As a result, those characters
MUST also be percent encoded. 

## Serializing Fields on the URI format

Unfilled fields MUST be submitted as empty between slash (`/`) characters. Only add empty delimiters if there is data after. Given fields A (required), B (optional), C (optional) the implementation MUST follow the following example:

| A | B | C | Output        |
|-----|-----|-----|---------|
| `1` |     |     | `1`     |
| `1` | `2` |     | `1/2`   |
| `1` | `2` | `3` | `1/2/3` |
| `1` |     | `3` | `1//3`  |

## Which Characters Need Encoding?

The columns in the table below indicate encoding requirements for each
representable character. Any non-listed characters MUST be percent encoded. The
"URI" column indicates whether the URI format rules (RFC 2396) requires encoding the character.
The "Alphanumeric QR" column indicates whether the character is missing from the
Alphanumeric QR character set (thus, requiring encoding). The "Must Encode?" column indicates whether this
specification requires percent encoding of the character. The "Output Value"
column indicates the expected output from processing the listed character.

| Character | URI Requires | Alphanumeric QR Requires | Must Encode? | Output Value |
| --------- | -------- | --------------- | ---- | ------------ |
| ` ` | YES | NO  | YES | `%20` |
| `!` | YES | YES | YES | `%21` |
| `"` | YES | YES | YES | `%22` |
| `#` | YES | YES | YES | `%23` |
| `$` | YES | NO  | YES | `%24` |
| `%` | YES | NO  | YES | `%25` |
| `&` | YES | YES | YES | `%26` |
| `'` | YES | YES | YES | `%27` |
| `(` | YES | YES | YES | `%28` |
| `)` | YES | YES | YES | `%29` |
| `*` | YES | NO  | YES | `%2A` |
| `+` | YES | NO  | YES | `%2B` |
| `,` | YES | YES | YES | `%2C` |
| `-` | YES | NO  | YES | `%2D` |
| `.` | YES | NO  | YES | `%2E` |
| `/` | YES | NO  | YES | `%2F` |
| `0` | NO  | NO  | NO  | `0`   |
| `1` | NO  | NO  | NO  | `1`   |
| `2` | NO  | NO  | NO  | `2`   |
| `3` | NO  | NO  | NO  | `3`   |
| `4` | NO  | NO  | NO  | `4`   |
| `5` | NO  | NO  | NO  | `5`   |
| `6` | NO  | NO  | NO  | `6`   |
| `7` | NO  | NO  | NO  | `7`   |
| `8` | NO  | NO  | NO  | `8`   |
| `9` | NO  | NO  | NO  | `9`   |
| `:` | YES | NO  | YES | `%3A` |
| `;` | YES | YES | YES | `%3B` |
| `<` | YES | YES | YES | `%3C` |
| `=` | YES | YES | YES | `%3D` |
| `>` | YES | YES | YES | `%3E` |
| `?` | YES | YES | YES | `%3F` |
| `@` | YES | YES | YES | `%40` |
| `A` | NO  | NO  | NO  | `A`   |
| `B` | NO  | NO  | NO  | `B`   |
| `C` | NO  | NO  | NO  | `C`   |
| `D` | NO  | NO  | NO  | `D`   |
| `E` | NO  | NO  | NO  | `E`   |
| `F` | NO  | NO  | NO  | `F`   |
| `G` | NO  | NO  | NO  | `G`   |
| `H` | NO  | NO  | NO  | `H`   |
| `I` | NO  | NO  | NO  | `I`   |
| `J` | NO  | NO  | NO  | `J`   |
| `K` | NO  | NO  | NO  | `K`   |
| `L` | NO  | NO  | NO  | `L`   |
| `M` | NO  | NO  | NO  | `M`   |
| `N` | NO  | NO  | NO  | `N`   |
| `O` | NO  | NO  | NO  | `O`   |
| `P` | NO  | NO  | NO  | `P`   |
| `Q` | NO  | NO  | NO  | `Q`   |
| `R` | NO  | NO  | NO  | `R`   |
| `S` | NO  | NO  | NO  | `S`   |
| `T` | NO  | NO  | NO  | `T`   |
| `U` | NO  | NO  | NO  | `U`   |
| `V` | NO  | NO  | NO  | `V`   |
| `W` | NO  | NO  | NO  | `W`   |
| `X` | NO  | NO  | NO  | `X`   |
| `Y` | NO  | NO  | NO  | `Y`   |
| `Z` | NO  | NO  | NO  | `Z`   |
| `[` | YES | YES | YES | `%5B` |
| `\` | YES | YES | YES | `%5C` |
| `]` | YES | YES | YES | `%5D` |
| `^` | YES | YES | YES | `%5E` |
| `_` | NO  | YES | YES | `%5F` |
| `{` | YES | YES | YES | `%7C` |
| `}` | YES | YES | YES | `%7D` |
| `~` | NO  | YES | YES | `%7E` |

## Pseudo-Code describing assembly of the URI:

```
$payload ::= [$number, $total, $city, $phase, $indicator];
for ($i ::= 0; $i < length($payload); $i += 1) do
  $upcasedValue ::= upcase($payload[$i]);
  $encodedValue ::= percentEncode($payload[$i]);
  $payload[$i] ::= $encodedValue;
end

$payloadString ::= join("/", $payload);
$signatureHex ::= ecdsaSign($payloadString);
$base ::= "cred:coupon:" + $version + ":" + $signatureHex + "." + $keyId;
$upcasedBase ::= upcase($base);

$uri ::= $upcasedBase + "?" + $payloadString;
```

# **COUPON** Payload
Fields:
1. *number*: *Required.* **NUMERIC**. The unique identifying number assigned to this coupon.
1. *total*: *Required.* **NUMERIC**. The total number of coupons issued in the batch of coupons this one was issued from.
1. *city*: *Required.* **STRING**. The name of the city, town, or other local area which designates vaccination eligibility and delivery schedule for the **HOLDER**.
    1. When the city name contains characters which cannot be encoded to QR, the city name may be Percent Encoded as part of QR Code generation. Readers
    must decode any substitutions prior to signature verification.
    1. In the event the city name exceeds 255 bytes when encoded to UTF-8, the last Unicode code point is removed until the resulting encoding is less than or equal to 255 bytes.
1. *phase*: *Required.* **SHORTSTRING**. The vaccination phase assigned to the **HOLDER**.
1. *indicator*: *Required.* **SHORTSTRING**. An indication of the priority assignment for **HOLDER**, or the literal string "none" if there is no priority assignment.

## Serialization Order:
In situations requiring data serialization, the fields in the Coupon payload MUST be serialized in the following order:
1. Number
1. Total
1. City
1. Phase
1. Indicator

## JSON example:
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

# **BADGE** Payload
Fields:

1. *date*: *Required.* **DATE**. The date of vaccination of the **HOLDER**.
1. *manuf*: *Required.* **SHORTSTRING**. The name of the manufacturer of the vaccine
1. *product*: *Required.* **SHORTSTRING**. The name of the product of the vaccine.
1. *lot*: *Required.* **SHORTSTRING**. The lot number of bottle of the vaccine.
1. *boosts*: *Required.* An array of **SHORTNUMERIC** representing the distance in days from the first dose. Ex, for Moderna's (two doses): [28], for JnJ (just one dose): []
1. *passkey*: *Required.* **STRING**. The cryptographic hash of the data in the Passkey, as defined in the Passkey specification.
1. *route* *Optional.* **SHORTSTRING**. The route of application. Options are:
    | Route Code | Meaning |
    | ----- | ------- |
    | `C38238` | INTRADERMAL |
    | `C28161` | INTRAMUSCULAR
    | `C38276` | INTRAVENOUS |
    | `C38284` | NASAL |
    | `C38288` | ORAL |
    | `C38676` | PERCUTANEOUS |
    | `C38299` | SUBCUTANEOUS |
    | `C38305` | TRANSDERMAL |
1. *site* *Optional.* **SHORTSTRING**. The site of the application. Options are:
    | Site Code | Meaning |
    | --------- | ------- |
    | `LA` | Left Arm |
    | `LD` | Left Deltoid |
    | `LG` | Left Gluteus Medius |
    | `LLFA` | Left Lower Forearm |
    | `LT` | Left Thigh |
    | `LVL` | Left Vastus Lateralis |
    | `RA` | Right Arm |
    | `RD` | Right Deltoid |
    | `RG` | Right Gluteus Medius |
    | `RLFA` | Right Lower Forearm |
    | `RT` | Right Thigh |
    | `RVL` | Right Vastus Lateralis |
1. *dose* *Optional.* **NUMERIC**. A dose size in μL (micro liters).

## Boosts Field Serialization
When serializing *boosts* data, the array should be represented as decimal
strings joined with plus (`+`) characters. Hence, `[28, 14]` would serialize to
the string `28+14`.

## Serialization Order
In situations requiring data serialization, the fields in the **BADGE** payload MUST be serialized in the following order:

1. Manuf
1. Product
1. Lot
1. Boosts
1. Passkey
1. Route (optional)
1. Site (optional)
1. Dose (optional)

## JSON example:
```json
{
  "type": "badge",
  "version": 1,
  "data": {
    "date": 20210102,
    "manuf" : "Moderna",
    "product" : "Covid19",
    "lot": ":23092",
    "boosts" : [],
    "passkey": "d9116bbdf7e33414b23ce81b2d4b9079a111d7119be010a5dcde68a1e5414d2d",
    "route": "C28161",
    "site": "RA",
    "dose": 500
  },
  "signature": {
    "keyId": "cdc:1a9",
    "hex": "3046022100fdff876e90286a0b06c4181d78b23d5b960cb60a53a94f946b12bbb321ec24c6022100c22e739dfd59b37f8eddc915475190e12f8c10a632560afaab68e498c12de2ac"
  }
}
```

# **STATUS** Payload
Fields:
1. *vaccinated*: *Required.* **SHORTNUMERIC**. The vaccination status of the **HOLDER**. Currently designated values are below. Future versions of this specification may designate other values as required.
   * 0: The **HOLDER** has not received any vaccination.
   * 1: The **HOLDER** has started, but not completed, a course of vaccination.
   * 2: The **HOLDER** has completed the full vaccination course.
1. *passkey*: *Required.* **HASH**. The cryptographic hash of the data in the Passkey, as defined by the Passkey Specification.

## Serialization Order:
In situations requiring payload serialization, the fields in the **STATUS** payload MUST be serialized in the following order:
1. vaccinated
2. passkey

## JSON example:
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

# **PASSKEY** Payload
Fields:
1. *name*: *Required.* **STRING**. The full name of the **HOLDER**, to be used when authenticating the **HOLDER**.
    1. In the event the name exceeds 255 bytes when encoded to UTF-8, the name
    should be truncated until its length does not exceed 255 bytes.
1. *dob*: *Required.* **DATE**. The date of birth of the **HOLDER**, to be used when authenticating the **HOLDER**.
1. *phone* *Optional.* **PHONE**. The phone number of the **HOLDER**, to be used when authenticating the **HOLDER**.
1. *salt*: *Required.* **STRING**. The cryptographic salt, nonce, or IV used for **HASH** calculation.

## Serialization Order
In situations requiring payload serialization, the fields in the Passkey payload MUST be serialized in the following order:
1. Name
1. DoB
1. Salt
1. Phone (Optional)

## Hashing Rules
When generating a passkey hash, the following rules MUST be followed to generate consistent results:
1. The only elements to be serialized should be the ones in the **DATA** block.
1. The elements MUST be concatenated in the order defined in the **PASSKEY** Serialization Order section, with the ctrl-^ (character code 30, hex 1E, RS, or Record Separator) delimiter.
1. The concatenation should be a UTF-8 string.
1. The concatenation MUST be converted to uppercase prior to hashing.
1. The elements MUST NOT be Percent Encoded prior to hashing.
1. The output hash MUST be a SHA256 hash in hexadecimal format.

Thus, the SHA256 hash of the data in the example below would be calculated as in the following pseudo-code:

```
hash(“${name}\x1E${dob}\x1E${phone}\x1E${salt}”) == hash(“JANE DOE\x1E19010101\x1E1BC93AB4AXD3”)
-> “e607c3b9b9448403a6b3cddd83f397bd17084c1db6fdeb081e9bd8392f21a1e6”
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
    "keyId": "cdc:1a9",
    "hex": "3046022100f82e28019428220d47be9b7dc9a50b4f0e6f9a6c95852a9272827cdbd8cb38d2022100b5d8738178cc1a12b590b25933857d967eb10178c5bbe045d132ec2513ddfa94"
  }
}
```
