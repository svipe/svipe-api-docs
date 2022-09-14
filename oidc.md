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

# OIDC Scopes and Claims

## Scopes

The [OIDC
standard](https://openid.net/specs/openid-connect-core-1_0.html#ScopeClaims)
defines a set of standard scopes that can be used to request a set of claims.
Svipe supports all of the standard scopes, except the `address` scope. Note that
the claim values returned from the `profile` scope is quite limited as compared
to the standard (where `middle_name`, `nickname`, `preferred_username`,
`profile`, `picture`, `website`, `gender`, `birthdate`, `zoneinfo`, `locale`,
and `updated_at` are also included). 

| Scope     | Claims
| :---      | :---
| openid    | svipeid
| profile   | name, given_name, family_name
| email     | email, email_verified
| phone     | phone_number, phone_number_verified


## Standard Claims

Svipe supports many, but not all, of the standard claims specified by the [OIDC standard](https://openid.net/specs/openid-connect-core-1_0.html#StandardClaims).

The following claims are derived directly from the identity document used to create the Svipe iD:

| Claim                 | Description       |
| :---                  | :---              |
| sub                   | A unique identifier for the user. Svipe returns the Svipe iD. |
| name                  | The full name.    |
| given_name            | Given name(s).    |
| family_name           | Last name(s).     |
| gender                | Gender, one of `male`, `female` or `unspecified` |
| birthdate             | Birthday, represented as an [ISO 8601](https://en.wikipedia.org/wiki/ISO_8601) compliant date. |

These claims can be added by the user and are verified by Svipe through a round-trip verification where the user proves the ownership:

| Claim                 | Description       |
| :---                  | :---              |
| email                 | E-mail address    |
| email_verified        | Always **True** if present, since all emails are verified. |
| phone_number          | Telephone number in [E.164](https://en.wikipedia.org/wiki/E.164) compliant format. |
| phone_number_verified | Always **True** if present, since all phone numbers are verified. |

The following claims are preferences configured by the user or system settings :

| Claim                 | Description       |
| :---                  | :---              |
| locale                | End-User's locale |

We currently do not support the following standard claims:

* middle_name
* nickname
* preferred_username
* profile
* picture
* website
* zoneinfo
* address
* updated_at

## Custom Claims

We also support the following non-standard claims, which are all derived from the identity document used to create the Svipe iD:

| Claim                                     | Description   |
| :---                                      | :---          |
| com.svipe:svipeid                         | Same as `sub` above. A globally unique identifier issued by Svipe to the user. Under normal conditions, a given person will retain the same Svipe ID even after renewing the underlying identity document. |
| com.svipe:document_portrait               | Photo from the document, provided as a [data url](https://en.wikipedia.org/wiki/Data_URI_scheme) in JPEG or JPEG2000 format.|
| com.svipe:document_nationality            | Nationality, provided in [ISO 3166-1 alpha-3 format](https://en.wikipedia.org/wiki/ISO_3166-1_alpha-3) |
| com.svipe:document_nationality_en         | The english name for the nationality |
| com.svipe:document_type                   | The raw version of the document type from the document. Note that the notation is not uniform across countries, so some countries may use `P` for a passport while others use `PN`. We recommend that you use `com.svipe:document_type_sdn` or `com.svipe:document_type_sdn_en` instead to build more reliable business logic |
| com.svipe:document_type_sdn               | The standardized version of the document type, which is one of `PN` (Passport), `PC` (Passport Card), `NID` National Identity Card) or `RP` (Residence Permit) |
| com.svipe:document_type_sdn_en            | The standardized version of the document type in english, which is one of `Passport`, `Passport Card`, `National Identity Card` or `Residence Permit` |
| com.svipe:document_number                 | The document number, as per the standard of the corresponding issuing country. This number is unique for the issuing country, but may not be unique globally and will change as the user renews the underlying identity document. |
| com.svipe:document_issuing_country        | Country that issued the document provided in [ISO 3166-1 alpha-3 format](https://en.wikipedia.org/wiki/ISO_3166-1_alpha-3) |
| com.svipe:document_issuing_country_en     | The english name for the country that issued the document |
| com.svipe:document_expiry_date            | Date of expiry for the document, represented as an [ISO 8601](https://en.wikipedia.org/wiki/ISO_8601) compliant date. |
| com.svipe:document_administrative_number  |  The national administrative number assigned to the individual. This could be a TID (Tax Identification Number) or any other national identifier such as a Swedish personal number. |



## Authentication Method

The OIDC standard describes a method to dictate the context of the
authentication called [ACR (Authentication Context Class
Reference)](https://openid.net/specs/openid-connect-core-1_0.html#acrSemantics).
ACR can either be specified with the `acr_values` request parameter or with the
`acr` claim for the id_token.

Svipe supports the following ACR values:


| Value                     | Description           |
| :------------------------ | :-------------------- |
| face_present              | The user is required to perform a biometric verification |
| document_present          | The user is required to verify the presence of the document used to create the Svipe iD with NFC |
| face_and_document_present | Both of the above     |

To request `face_precent`, either specify `acr` as an `id_token` claim:

    "claims": {
        "id_token": {
            "acr" : {
                "essential": true,
                "values": [
                    "face_precent"
                ]
            }
        }
    }

or in the acr_values request parameter:

    &acr_values=face_present
