# Welcome to Svipe developer portal  <!-- {docsify-ignore} -->

## Intro

Svipe enables the user to verify ID credentials from biometric id-documents issued by 145+ countries. The verified ID credentials can then be used for strong authentication and to share verified information. 

Svipe does not store any personal data from identity documents on our servers. After verifying the data that we have read from the user’s document, we create verified ID credentials that are returned to the phone and then delete the data from our servers. The ID credentials are stored encrypted inside the secure vault of your user’s phone and shared only with the users explicit permission. That is what we mean with privacy-by-design.

## Getting started

By utilizing the Svipe API, you have the capability to authenticate users of your application through the OIDC protocol.

Download the Svipe iD app for iOS or Android. Then verify your identity with a biometric document and you will get immediate access to our developer portal. From there, you can configure sample OIDC applications, try the demos, or learn how to integrate with WordPress, Nextcloud, or other applications.

## Checklist to get started
### 1. Download the app
Download the app and onboard either with your biometric ID-document as an real user or by setting up a [demo-identity](demo-identity.md). 

[Here](onboarding.md) is the guide how to onboard.

### 2. Log in to the developer page using the Svipe app
Log in to developer portal to get access to the testing page where you can try the verification process or create your own OIDC application

### 3. Try the credential sharing
Now you can test credential sharing and the consent mechanism under the Test menu.

### 4. Create your own OIDC-Application
Create your own OIDC-application under the Application menu. Make sure to enable demo-users in the application if needed. 
[Here](oidc.md) is the guide how to create OIDC-applications

### 5. Try Sample Integrations

Our documentation illustrates the straightforward process of integrating Svipe with popular platforms such as WordPress, Matrix, Nextcloud, Mattermost, and Keycloak using the OIDC protocol. If you possess knowledge of utilizing Docker and the command line, you can have a functional sample up and running in under 5 minutes.

[Netcloud](app-nextcloud.md)

[Mattermost](app-mattermost.md)

[Matrix](app-matrix.md)

[Keycloak](app-keycloak.md)

[Wordpress](app-wordpress.md)
