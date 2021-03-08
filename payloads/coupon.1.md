# **COUPON** Payload

The **COUPON** QR code is designated to communicate eligibility for vaccination.

Fields in the **serialization** order:
1. `number`: *Required.* **NUMERIC**. The unique identifying number assigned to this coupon.
1. `total`: *Required.* **NUMERIC**. The total number of coupons issued in the batch of coupons this one was issued from.
1. `city`: *Required.* **STRING**. The name of the city, town, or other local area which designates vaccination eligibility and delivery schedule for the **HOLDER**.
    1. When the city name contains characters which cannot be encoded to QR, the city name may be Percent Encoded as part of QR Code generation. Readers
    must decode any substitutions prior to signature verification.
    1. In the event the city name exceeds 255 bytes when encoded to UTF-8, the last Unicode code point is removed until the resulting encoding is less than or equal to 255 bytes.
1. `phase`: *Required.* **SHORTSTRING**. The vaccination phase assigned to the **HOLDER**.
1. `indicator`: *Required.* **SHORTSTRING**. An indication of the priority assignment for **HOLDER**, or the literal string `"none"` if there is no priority assignment.

## Example:
```
CRED:COUPON:1:GBDAEIIA42QDQ5BDUUXVMSQ4VIMMA7RETIZSXB573OL24M4L67LYB24CZYVQEIIA2EZ5W2QXLR7LUSL
QW6MLAFV3N7OTT3BDAZCNCRMYBMUYC6WMXMNQ:PCF.VITORPAMPLONA.COM:1/5000/SOMERVILLE%20MA%20US/1A/%3E65
```