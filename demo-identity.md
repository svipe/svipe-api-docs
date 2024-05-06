# Demo Identity

The Svipe iD app allows for provisioning a demo identity that can be used in a
similar fashion as a regular identity. However, the issuing country and the
nationality is set to be `UTO` (for Utopia), and is blocked for use in an OIDC
app unless it has been explicitly enabled.

Note that face and document reverification are not available in demo mode.

Also note that a new random demo identity will be generated each time, so a demo
identity can never be restored.


## Provision a demo identity on iOS

Start with a fresh installation of Svipe iD from the app store, so in case you
have it already installed, then you need to delete your currently installed
version and reinstall from the app store. Then start the app, approve the
terms and conditions and click `Next`:

![start screen](./images/demo-identity/demo-ios-n1.jpg)

Then, do the following:
1) Select a document type (it doesn't matter which one)
2) Click on the Svipe iD logo
3) Click `Next`

![start screen](./images/demo-identity/demo-ios-n2.jpg)

and after 10-15 seconds the demo identity becomes available and can be used as a
regular identity, if the site or app allows it:

![start screen](./images/demo-identity/demo-ios-n3.jpg)

To exit demo mode, delete the app or click the top right menu in the app (the
three dots) and then select `Delete document`. If you chose the latter option,
you can either create a new demo identity or verify an identity from an identity
document.

## Provision a demo identity on Android

Start with a fresh installation of Svipe iD from the app store, so in case you
have it already installed, then you need to delete your currently installed
version and reinstall from the app store. Then start the app:

![start screen](./images/demo-identity/demo-android-n1.jpg)

approve the terms and conditions and click `Continue`:

![start screen](./images/demo-identity/demo-android-n2.jpg)

wait for the app to initialize:

![start screen](./images/demo-identity/demo-android-n3.jpg)

then click `Try the app without using a real document`:

![start screen](./images/demo-identity/demo-android-n4.jpg)

and the demo identity is ready to use:

![start screen](./images/demo-identity/demo-android-n5.jpg)




## Configuring a demo identity for use

Demo identities can only be used with an oidc-app if they have been explicitly
allowed. However, the Svipe demo oidc credentials (svipe-demo/svipe-demo-secret)
will always allow them.

To enable:
* navigate to the [svipe developer portal](https://developer.svipe.com)
* login with your Svipe iD
* check the option "Allow Demo User"

<div class="img-80 center">

![start screen](./images/demo-identity/demo-ok.jpg)

</div>