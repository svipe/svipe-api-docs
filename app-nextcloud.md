## Nextcloud  <!-- {docsify-ignore} -->

[Nextcloud](https://nextcloud.com) is a suite of client-server software for creating and using file hosting services. Being free and open-source software, anyone is allowed to install and operate it on their own private server devices. Nextcloud is functionally similar to Dropbox, Office 365 or Google Drive when used with its integrated office suite solutions Collabora Online or OnlyOffice.

To try this integration, start by launching an instance of nextcloud locally

    docker run -p 80:80 nextcloud

Then launch a browser to `http://localhost` where you start with creating an account for `admin` and click `Finish setup`.

![info text](./images/nextcloud/nextcloud-1.jpg)

When logged in, click on the user icon in the top, right corner and select `+ Apps`:

![info text](./images/nextcloud/nextcloud-2.jpg)

Select `Social and communication` in the left-side menu:

![info text](./images/nextcloud/nextcloud-3.jpg)

Locate the app `OpenID Connect user backend` and click on `Download and enable`:

![info text](./images/nextcloud/nextcloud-4.jpg)

Then open the user menu again in the top right corner and select `Settings`:

![info text](./images/nextcloud/nextcloud-5.jpg)

Locate `OpenID Connect` in the left-side menu:

![info text](./images/nextcloud/nextcloud-6.jpg)

Now configure the OpenID Connect app with:

    Identifier:         Svipe
    Client ID:          svipe-demo
    Client secret:      svipe-demo-secret
    Discovery endpoint: {{ oidc_root }}/.well-known/openid-configuration

and click `Register`:

![info text](./images/nextcloud/nextcloud-7.jpg)

the select `Log out` in the user menu:

![info text](./images/nextcloud/nextcloud-8.jpg)

Now the login screen has a new option `Login with Svipe`. Click on that!

![info text](./images/nextcloud/nextcloud-9.jpg)

Time to bring up the Svipe iD app, scan the QR code and approve the login:

![info text](./images/nextcloud/nextcloud-10.jpg)

and you're logged in to Nextcloud:

![info text](./images/nextcloud/nextcloud-11.jpg)

