## Mattermost   <!-- {docsify-ignore} -->

[Mattermost](https://mattermost.com) is a free and open-source collaboration platform. User login through OIDC is offered as part of their premium enterprise plan E20 (read more about their pricing [here](https://mattermost.com/pricing-self-managed/)).

## Set up Mattermost

To try this integration using a local demo instance of Mattermost, start a docker instance with:

    docker run --publish 8065:8065 mattermost/mattermost-preview


Then launch a browser to http://localhost:8065 and start setting up the site by choosing email, username and password for the admin user and click on `Create Account`:

![info text](./images/mattermost/mattermost-1.jpg)

Then you need to create a team name so click on `Create a team`:

![info text](./images/mattermost/mattermost-2.jpg)

Enter a name and click on `Next`: 

![info text](./images/mattermost/mattermost-3.jpg)

Now specify the team url and click on `Finish`:

![info text](./images/mattermost/mattermost-4.jpg)

You should now be logged in to Mattermost and need to add some more details before getting started. Enter your name and then `Save profile`:

![info text](./images/mattermost/mattermost-5.jpg)

Enter the team name and `Save team`:

![info text](./images/mattermost/mattermost-6.jpg)

Then click on `Skip Getting Started`:

![info text](./images/mattermost/mattermost-7.jpg)

Now you need to make the team you created available for new users to join. Click on the menu in the upper left corner (the square grid) and then select `System Console`:

![info text](./images/mattermost/mattermost-8.jpg)

Then click on `Teams` under `User Management` and click `Edit` for the team you created:

![info text](./images/mattermost/mattermost-9.jpg)

Enable the option `Anyone can join this team`:

![info text](./images/mattermost/mattermost-10.jpg)

## Upgrade to Enterprise Edition

Now it's time to upgrade to the trial version of the Enterprise edition in order to be able to configure login through OIDC. Click on `Edition and License` in the `System Console`. Then click on `Upgrade to Enterprise Edition` and wait for it to be downloaded which will take a few minutes:

![info text](./images/mattermost/mattermost-11.jpg)


When done, click on `Restart Server`:

![info text](./images/mattermost/mattermost-12.jpg)

Now get the trial license from [mattermost.com/trial](https://mattermost.com/trial/). Once you have received the trial license by email, save it to your local disk. Then click on `Choose File` and select the license file to enable the trial of the enterprise edition:

![info text](./images/mattermost/mattermost-13.jpg)

## Configure OIDC

Now it's time to configure OIDC. Navigate back to the `System Console` and there should be an entry named `OpenID Connect` under the `Authentication` sub menu. Select `OpenID Connect (Other)` from the dropdown menu `Select service provider`:

![info text](./images/mattermost/mattermost-14.jpg)

Then fill in the settings for Svipe iD:

    Button Name:                    Svipe ID
    Button Color:                   #ffa500
    Discovery Endpoint:             {{ oidc_root }}/.well-known/openid-configuration
    Client ID:                      svipe-demo
    Client Secret Key:              svipe-demo-secret

and click `Save`!

![info text](./images/mattermost/mattermost-15.jpg)

## Login with Svipe iD

To test the login functionality you need to log out from Mattermost, so click on the user menu in the top right corner and select `Log Out` at the bottom:

![info text](./images/mattermost/mattermost-16.jpg)

Now you have returned to the login screen but there is a `Svipe ID` button visible. So click on that:

![info text](./images/mattermost/mattermost-17.jpg)

Then scan the QR-code with the Svipe iD app on your phone:

![info text](./images/mattermost/mattermost-18.jpg)

You're now logged into Mattermost and need to select a team to join:

![info text](./images/mattermost/mattermost-19.jpg)

And you have completed the login with Svipe iD!

![info text](./images/mattermost/mattermost-20.jpg)

