# Paper-first Verifiable Vaccination Credentials using QR Codes

We present a Verifiable Credentials specification for COVID-19 testing and vaccination workflows using digitally signed QR Code stickers on paper cards. Due to its physical nature and simplicity, digitally-signed QR codes may be a convenient and nonintrusive modality for some users seeking vaccination while enabling verification of paper-based user-controlled immunization records. The protocol expects the verifier app to be off-line and that the user doesn't need anything more than a Paper Card (no electronic devices on the user side). All personally identifiable information, contact and health information are stored on the QR Codes themselves, allowing a user to go through the vaccination procedures in a Peer-to-Peer fashion, without the need to ever transfer the user's information to any centralized, private, or public blockchain system. The proposed system improves the efficiency, privacy, equity, and effectiveness by augmenting the existing protocols to work with fully off-line information flows. 

<p align="center"><img src="https://github.com/Path-Check/vaccine-diary/blob/main/Resources/card_visualization.gif" alt="App_gif" width="650" style="margin: auto"/></p>

# Introduction

Vaccination coordination is facing daunting challenges. Citizens are expected to navigate an array of websites and health authorities are using disconnected health IT systems. Reporting lags by several days. Following up with vaccinated subjects to monitor side effects is difficult. The systems to monitor ineffective batches of vaccines are yet to become mature. Vaccine verifications documents are prone to fraud.

We developed a modification of today's vaccination cards to add 4 signed QR Codes to manage the user journey. The separate QR Codes are intended to decouple the health information (PHI) and personally identifiable information (PII) as well as separate the eligibility verification from the vaccination itself. The card dramatically simplifies phased vaccinations, second dose coordination, reporting of side effects, and credentials while allowing fully privacy-centered systems. It also creates data-rich monitoring of vaccination progress while eliminating red tape, privacy-concerns, and fraud. It is ideally suited for vulnerable populations, rural areas, labor unions of essential workers, and employers helping their own employees.

# Patient Journey

The patient journey goes through 3 major stages:
1. Eligibility Check/Scheduling: Vaccination coupon QR codes are distributed to everyone by the appropriate regional vaccination administrator. This can be done either with paper vaccination cards, by SMS, using a web site, or downloading an application. The coupon code behaves as a User ID for the entire vaccination flow. 
2. User Check-in: At the vaccination site, the patient arrives and their vaccination coupon is scanned and their eligibility and appointment are verified.
3. Vaccination Certificate: Once the vaccination is administered, the patient receives another QR code in the form of a sticker. This QR code, a badge, indicates the vaccination was administered providing “proof” of vaccination along with other important information.

The 4 QR Codes designed for vaccinations procedures create a possibility of selective disclosure of health information by choosing which one to show at any point in time. 

## The Coupon QR Code

The distribution of a vaccination coupon signifies the beginning of the vaccination process. When everyone is given a vaccination coupon the fear and anxiety around the vaccination process and when and if someone will be vaccinated is alleviated. By starting the patient journey with the distribution of the vaccination coupon, everyone has more peace of mind with the knowledge that they are on the list to be vaccinated, and that they can play an active role in the process. The coupon only has information about the vaccination phase this user has been approved for and nothing else. 

## The Badge QR Code(s)

The badges contain information about the vaccination itself. Each badge QR code is a full health record for a single vaccination event, including vaccine brand, dosage, site, etc. It is signed by the vaccination provider and serves as a Verifiable Credential informing that the user in the badge was vaccinated under those conditions. If the User receives multiple doses, there will be one badge per dose. These QR Codes should be shown when the User goes to see their Primary Care Provider or other health care professional needing to see the details of the vaccination procedure(s). 

## The Passkey QR Code

The PassKey contains personally identifiable information. It's what links all other QR Codes to the person. A hash of the signed PassKey is used as an identifier on the other QR Codes. 

## The Status QR Code

The Status QR Code contains only one field: vaccinated or not. This QR Code is ideal to use as a check-in credential that verifies the user has been vaccinated, without revealing anymore more information about the vaccine or the user itself. 

# Pros/Cons

Main Benefits are
1. Easy: Everyone knows how to work with paper.
1. Trusted: The signed payload is cryptographically protected and thus impossible to tamper. 
1. Private: 
    1. No centralized PII, no exposure to government, private companies. 
    1. No central point of failure.
    1. No need for PII at the vaccination site or at tracking systems. 
    1. Protection for vulnerable populations 
1. Full Control: Users control a selective disclosure of information
1. Standardized: Any record can be created and signed
1. Small: Complete QR-code payloads range between 100 and 200 bytes
1. Modular: Add QR Codes to app for additional features
    1. Scheduling, Reminders, Backups, Self-reporting, etc...

Disadvantages are: 
1. Traceability of the PassKey or the QR Code itself by a large controller of QR Code reading apps.
1. Chance of losing the card, losing the data
1. No Revocation of cards/credentials (only option is to remove the public key from database)
1. Card information itself is not encrypted.


## Background

* [MIT Safepargs, IDEO and PathCheck work on Paper Credentials](https://github.com/Path-Check/vaccine-diary)

# Protocol Releases

## V1

* [Specification](SPECIFICATION.md)
* [FAQ](FAQ.md)
* [Demo in JavaScript](https://vitorpamplona.com/vaccine-certificate-qrcode-generator/index.v5.html)
* [Snippet in Python](https://github.com/vitorpamplona/vaccine-certificate-qrcode-generator/blob/main/verify.py)
* [Snippet in Ruby](https://github.com/vitorpamplona/vaccine-certificate-qrcode-generator/blob/main/verify.rb)

# Contributing
