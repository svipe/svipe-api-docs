
# Implementation

It is very easy to to use Svipe iD. We use standard Openid Connect according to this specification [OpenID Connect Core 1.0](https://openid.net/specs/openid-connect-core-1_0.html) with a few custom claims, most notably to support eMRTD document requests and verification.

When the new specification [OpenID Connect for Identity Assurance 1.0](https://openid.net/specs/openid-connect-4-identity-assurance-1_0-12.html) is final we will support this as well.

In Openid terminology a Relying Party is an app or webservice that wants to use Openid to authenticate users. The first step towards implemntation is to pick an Openid Service Provider or a software libary to be a Relying Party Client. Lists of certified alternatives can be found here [Certified OpenID Connect Implementations](https://openid.net/developers/certified/)

Many popular software packages have built in support for Openid. We describe reference implementations for [Keycloak](https://developer.svipe.com/documentation#/app-keycloak), [Matrix](https://developer.svipe.com/documentation#/app-matrix), [Keycloak](https://developer.svipe.com/documentation#/app-keycloak), [Mattermost](https://developer.svipe.com/documentation#/app-mattermost), [Nextcloud](https://developer.svipe.com/documentation#/app-nextcloud) and [Wordpress](https://developer.svipe.com/documentation#/app-wordpress)

# OIDC Configuration

## OIDC endpoints

If your application supports dynamic discovery, then point it to:

    {{ oidc_root }}/.well-known/openid-configuration

Otherwise, enter the endpoints manually:

    authorization:  {{ oidc_root }}/authorize
    token:          {{ oidc_root }}/token
    revocation:     {{ oidc_root }}/token/revoke
    end session:    {{ oidc_root }}/logout
    userinfo:       {{ oidc_root }}/userinfo
    jwks:           {{ oidc_root }}/keys

## Client ID and Secret

For demo purposes you can use the following credentials:

    Client ID:      svipe-demo
    Client Secret:  svipe-demo-secret

To define your own custom applications, please visit [this page](/applications ":ignore").


# Claims used in Authentication/Disclosure

Essentially we support standard OIDC claims, add some from mDL and have custom claims for signing, certificate pickup and multi factor authentication.

Just like in OIDC we are using this syntax to mark claim requests as mandatory or optional:

```
{
  ..
  "claims": {
    ..
    "svipeid": {"essential":true}, 
    "given_name":null
    ..
  }
  ..
}
```


## Standard Claims

These are according to the standard as documented here https://openid.net/specs/openid-connect-core-1_0.html#StandardClaims

Currently not available claims are ~~marked~~.

| Claim | Type | Description
| --- | --- | --- |
| sub | string| Subject - Identifier for the End-User at the Issuer. |
| name| string|End-User's full name in displayable form including all name parts, possibly including titles and suffixes, ordered according to the End-User's locale and preferences. |
|given_name | string| Given name(s) or first name(s) of the End-User. Note that in some cultures, people can have multiple given names; all can be present, with the names being separated by space characters.|
|family_name | string| Surname(s) or last name(s) of the End-User. Note that in some cultures, people can have multiple family names or no family name; all can be present, with the names being separated by space characters.|
| ~~middle_name~~| string| Middle name(s) of the End-User. Note that in some cultures, people can have multiple middle names; all can be present, with the names being separated by space characters. Also note that in some cultures, middle names are not used.|
| ~~nickname~~| string|Casual name of the End-User that may or may not be the same as the given_name. For instance, a nickname value of Mike might be returned alongside a given_name value of Michael. |
| ~~preferred_username~~| string| Shorthand name by which the End-User wishes to be referred to at the RP, such as janedoe or j.doe. This value MAY be any valid JSON string including special characters such as @, /, or whitespace. The RP MUST NOT rely upon this value being unique, as discussed in Section 5.7.|
| ~~profile~~ | string| URL of the End-User's profile page. The contents of this Web page SHOULD be about the End-User.|
| picture| string|URL of the End-User's profile picture. This URL MUST refer to an image file (for example, a PNG, JPEG, or GIF image file), rather than to a Web page containing an image. Note that this URL SHOULD specifically reference a profile photo of the End-User suitable for displaying when describing the End-User, rather than an arbitrary photo taken by the End-User. |
| ~~website~~ | string|URL of the End-User's Web page or blog. This Web page SHOULD contain information published by the End-User or an organization that the End-User is affiliated with. |
| email| string| End-User's preferred e-mail address. Its value MUST conform to the RFC 5322 [RFC5322] addr-spec syntax. The RP MUST NOT rely upon this value being unique, as discussed in Section 5.7.|
| email_verified| boolean|True if the End-User's e-mail address has been verified; otherwise false. When this Claim Value is true, this means that the OP took affirmative steps to ensure that this e-mail address was controlled by the End-User at the time the verification was performed. The means by which an e-mail address is verified is context-specific, and dependent upon the trust framework or contractual agreements within which the parties are operating. |
| gender| string|End-User's gender. Values defined by this specification are female and male. Other values MAY be used when neither of the defined values are applicable. |
| birthdate| string| End-User's birthday, represented as an ISO 8601:2004 [ISO8601‑2004] YYYY-MM-DD format. The year MAY be 0000, indicating that it is omitted. To represent only the year, YYYY format is allowed. Note that depending on the underlying platform's date related function, providing just year can result in varying month and day, so the implementers need to take this factor into account to correctly process the dates.|
|  ~~zoneinfo~~| string|String from zoneinfo [zoneinfo] time zone database representing the End-User's time zone. For example, Europe/Paris or America/Los_Angeles. |
| locale  | string|  End-User's locale, represented as a BCP47 [RFC5646] language tag. This is typically an ISO 639-1 Alpha-2 [ISO639‑1] language code in lowercase and an ISO 3166-1 Alpha-2 [ISO3166‑1] country code in uppercase, separated by a dash. For example, en-US or fr-CA. As a compatibility note, some implementations have used an underscore as the separator rather than a dash, for example, en_US; Relying Parties MAY choose to accept this locale syntax as well.|
|phone_number | string|End-User's preferred telephone number. E.164 [E.164] is RECOMMENDED as the format of this Claim, for example, +1 (425) 555-1212 or +56 (2) 687 2400. If the phone number contains an extension, it is RECOMMENDED that the extension be represented using the RFC 3966 [RFC3966] extension syntax, for example, +1 (604) 555-1234;ext=5678. |
| phone_number_verified| boolean|True if the End-User's phone number has been verified; otherwise false. When this Claim Value is true, this means that the OP took affirmative steps to ensure that this phone number was controlled by the End-User at the time the verification was performed. The means by which a phone number is verified is context-specific, and dependent upon the trust framework or contractual agreements within which the parties are operating. When true, the phone_number Claim MUST be in E.164 format and any extensions MUST be represented in RFC 3966 format. |
| ~~address~~ | JSON| End-User's preferred postal address. The value of the address member is a JSON [RFC4627] structure containing some or all of the members defined in Section 5.1.1.|
|  ~~updated_at~~| number| Time the End-User's information was last updated. Its value is a JSON number representing the number of seconds from 1970-01-01 as measured in UTC until the date/time.|

## Custom Claims

From the standard for Mobile Drivers License https://www.iso.org/standard/69084.html and ICAO xx we add these. Dates, images and all the rest are formatted as for OIDC.

| Claim | Type | Description
| --- | --- | --- |
| svipeid | string | A globally unique identifier issued by Svipe. It is the same as sub above.|
| peerid | string | A unique identifier issued by Svipe to a particular Relying Party.|
|  ~~resident_address~~ | string| Currently ABSENT.|
|  ~~resident_city~~ | string| Currently ABSENT.|
|  ~~resident_state~~ | string| Currently ABSENT.|
|  ~~resident_postal_code~~ | string | Currently ABSENT.|
| portrait | string| same as picture above |
| portrait_capture_date | string| same as picture above |
| signature | string| End users signature using the same representation as picture |
| nationality | string | End-User's nationality. Alpha-3.|
| administrative_number | string| This could be a SSN (Social Security Number) or any other national identifier such as Swedish personal number|
| person_number | string| Specifically a Swedish person number|
| document_type | string| P for Passport and I for Identity Card and X for Residence Card (link to spec).|
| document_number | string| A unique number in the underlying document|
| issuing_authority | string| Document issuing authority.|
| issuing_country| string| Country that issued the document. |
| expiry_date| string | When the document expires. Format like birthday.|
| ~~document_issue_date~~ | string | Currently ABSENT. When the document was issued. Format like birthday.|
| document_all| string | All of the document_ attributes|
| ~~document_driving_privileges~~  | string| Currently ABSENT.|
| birth_date| string | Same as birthdate.|
| ~~age_in_years~~ | number | |
| ~~age_over_18~~ | boolean | |
| ~~age_over_21~~ | boolean | |
| twitter|string | the Twitter handle i.e @johansellstrom|
| facebook| string | the Facebook handle i.e @johansellstrom|
| google| string | the Google handle i.e johan.sellstrom@gmail.com or some other email|
| covid19_vaccinated| boolean | if the subject had all the vaccinations for Covid19 more than 14 days ago |
| location| string | geographical coordinates <+59.48693000,+18.30116340> +/- 10408.74m (speed 0.00 mps / course 0.00) @ 2021-05-27 |


### Authentication Method


#### Authentication Context Class Reference

```
{
  ..
  "acr": null // optionally return this 
  }
  ..
}
```

String specifying an Authentication Context Class Reference value that identifies the Authentication Context Class that the authentication performed satisfied. The value "0" indicates the End-User authentication did not meet the requirements of ISO/IEC 29115 [ISO29115] level 1. Authentication using a long-lived browser cookie, for instance, is one example where the use of "level 0" is appropriate. Authentications with level 0 SHOULD NOT be used to authorize access to any resource of any monetary value. (This corresponds to the OpenID 2.0 PAPE [OpenID.PAPE] nist_auth_level 0.) An absolute URI or an RFC 6711 [RFC6711] registered name SHOULD be used as the acr value; registered names MUST NOT be used with a different meaning than that which is registered. Parties using this claim will need to agree upon the meanings of the values used, which may be context-specific. The acr value is a case sensitive string.

A response could be f.i

```
"claims": 
  {
    ..
    "acr": 1 
   ..
  }
```
#### Authentication Methods References

In addition to disclosure requests you can also demand that the user and/or the underlying document is present or that a platform biometric or app pin was used prove presence.

If acm is present we always treat it as essential/mandatory. The request is 

```
{
  ..
  "acm": { 
    "values": 
      ["face_present", "document_present", "pin_present", "face_and_document_present", "face_and_document_and_pin_present", 
      "biometric_present", "face_and_document_and_biometric_pin_present", "face_and_document_and_biometric_present",
      "face_and_biometric_present","face_and_pin_present", "document_and_biometric_present", "pin_and_biometric_present"] 
  }
  ..
}
```

Alternatively the values in the array are AND

```
{
  ..
  "acm": { 
    "values": 
      ["face_present","document_present", "pin_present",  "biometric_present"] 
  }
  ..
}
```

JSON array of strings that are identifiers for authentication methods used in the authentication. For instance, values might indicate that both password and OTP authentication methods were used. The definition of particular values to be used in the amr Claim is beyond the scope of this specification. Parties using this claim will need to agree upon the meanings of the values used, which may be context-specific. The amr value is an array of case sensitive strings.

A response could be f.i

```
"claims": 
  {
    ..
    "acm": "face_present" // a scalar
   ..
  }
```

If one of the factors was not present the request fails and we provide no response.

reference: https://openid.net/specs/openid-connect-core-1_0.html#acrSemantics

## Signature Requests

The assumption is that we only deal with symmetric agreements. The RP requesting a signature would thus enter into the agreement by signing the statement or document using the same key as used for the JWS signing. 

At the moment we only support signing of PDFs or statements expressed as a plain string. These requests are useful both for Relying Parties and peer to peer - possibly offline (not to self: implement UI for this).


| Claim | Type | Description |
|---|---|---|
| sign|JSON| Either a statement or URI to PDF. |


```
{
  ..
  "claims": {
  ..
  "sign": { 
    "payload": "statement or URI to PDF",
    "signature": "JWS style Base64 encoded signature of hash(payload) or hash(URI.data) using the same key as when signing the envelope JWS."
    },
  ..
  }
  ..
}
```

The response has the same JSON format but signed by the peer, so signature is different.


## Certification

This is really a two step process since you need to be authenticated first in order to share your public key with the issuer. 

When picking up a certificate a response is not really needed but maybe we want to tell the issuer that the certificate was accepted? 

We could add:

```
{
"certificate": {
  "receipt": null/{"essential":true},
  "schema": URL", // The schema tells you if it is X509 or JWS
  "payload": "the actual bytes of X509 or a JWS"
}
```

These can later be requested matching iss, name and id.


### Value Requests

Sometimes you want to request only claims with specific values. If the End User is not in possession of such claims they can not respond. Examples include only Swedish people or those in possession of a vaccination certificate or maybe an employee pass or only a certain subject.

Vaccination:

```
{
  "claims": {
    ..
    "certificate": {
      "value": {
        "schema": "some schema URL",
        "type": "some schema type definition"
      }
    }
    ..
  }
}
```

#### Example: Request Corona Vaccination Certificate

To request a EU Green Card certificate the schema is https://id.uvci.eu/DGC.combined-schema.json and the valid types are vaccination|test|recovery.


```
{
  "claims": {
    ..
    "certificate": {
      "value": {
        "schema": "https://id.uvci.eu/DGC.combined-schema.json",
        "type": "vaccination"
      }
    }
    ..
  }
}
```

And the response will be:

```
{
  "claims": {
    ..
    "green_certificate": {
      "value": {
        "schema": "https://id.uvci.eu/DGC.combined-schema.json",
        "type": "vaccination",
        "vaccination_entry": {
          "tg": string, // "Disease or agent targeted"
          "vp": string, // "Vaccine or prophylaxis"
          "mp": string, // "Vaccine medicinal product"
          "ma": string, // "Marketing Authorization Holder - if no MAH present, then manufacturer"
          "dn": string, // "Dose Number"
          "sd": string, // "Total Series of Doses"
          "dt": string, // "Date of Vaccination"
          "co": string, // "Country of Vaccination"
          "is": string, // "Certificate Issuer"
          "ci": string, // "Unique Certificate Identifier: UVCI"
       } 
       "payload_string": string // the full original contents of the QR code
      }
    }
    ..
  }
}
```

in a general case:

```
{
  "claims": {
    ..
    "certificate": {
      "value": {
        "schema": "some url",
        "type": "some type",
       "payload_string": string // the full original contents of the QR code
      }
    }
    ..
  }
}
```

Employee:

```
{
  "claims": {
    ..
    "certificate": {
      {"essential":true}, 
      "value": {
        "type": "ericsson_employee_number"
      }
    }
    ..
  }
}
```


A certain subject:

```
{
  "claims": {
    ..
    "sub": {"value": "248289761001"}
    ..
  }
}
```
