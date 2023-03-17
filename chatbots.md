# Integrate with chatbots  <!-- {docsify-ignore} -->

SvipeID can be integrated with different chatbots, to verify users on different platforms. Good example of integration is our Discord bot for age verification:
To configure Svipe iD as an identity provider for Keycloak, first start by launching a local docker instance of keycloak:

## How the bot works?

If you take a look at Svipe verification bot on discord, it allows server(guild) owners to verify their members. To verify, you need to click on the "Verify now!" button and then follow instructions.

![info text](./images/discord/img1.png)

After clicking the button, the bot will ask you to scan QR-code to verify your age.

![info text](./images/discord/img2.png)

When QR is scanned, the app asks you to share your birth date with the bot.

![info text](./images/discord/img3.png)

After sharing the birth date with the bot, you will be granted the role SvipeVerified in any case, and 18+ if you're above 18 years old.

![info text](./images/discord/img4.png)

## Integrate Svipe with your chat bot

The CIBA flow enables a direct Relying Party to OpenID Provider communication, without redirects through the user's browser. It's therefore well suited to scenarios where the authentication is embedded in another context, such as a bot. See the [specification document](https://openid.net/specs/openid-client-initiated-backchannel-authentication-core-1_0-final.html) for more details.

## CIBA configuration in the developer portal
Create a new app in the Svipe Developer portal, enable CIBA and configure push delivery like this:

![info text](./images/discord/ciba.jpeg)
Note that the `Client notification endpoint` needs to be a publicly available host that is reachable from the Svipe backend server. If you use a self-signed https server, then select `Self-signed endpoint` and the SSL certificate will not be verified.

## CIBA walkthrough with curl

Let's do a full authorization using the command line. Start by initiating a CIBA request (but replace the value for `client_id` with the one for the app that you configured in the developer portal):

    curl -X POST -d "client_notification_token=tokenXX&login_hint=authref&client_id=0234e1f1-631e-4386-9b89-39e91b191e62&scope=profile"  https://api.dev.bes.svipeid.com/oidc/v1/authorize_cibaCopy to clipboardErrorCopied

This call will return:

    {
    "auth_req_id": "nmidq2_-kp2lKrLLOwYmTg4ib_t9NBnppV-YXVP6",
    "expires_in": 1800,
    "interval": 5,
        "authref": {
            "qrlink":  "https://app.svipe.com/ad/PFE5poCB5t1QqA",
            "qrcode":  "https://api.dev.bes.svipeid.com/oidc/v1/qr/PFE5poCB5t1QqA.png",
            "applink": "https://app.svipe.com/ad/PFE5poCB5t1QqA?ondevice",
            "qrauth":  "https://api.dev.bes.svipeid.com/oidc/v1/qrauth/PFE5poCB5t1QqA?ondevice"
            "expires_in": 1799,
            "expires_at": 1674599675,
        }
    }Copy to clipboardErrorCopied

 Value      | Description |
| :--------- | :---------- |
| qrlink     | The link in the QR code. This is useful if you are using your own library to build the QR code. |
| qrcode     | If instead you want an image link to a generated QR code, then use this value. The link supports the extra query parameters `scale`, `module_color` and `background` to further define the size of the QR code and the colors used in it. The default value for `scale` is 3.  `module_color` and `background` colors are given as RGB colors. A sample query could be `?scale=3&module_color=000000&background=FFFFFF`.
| applink    | The link used in a button to trigger Svipe iD when used on a mobile device, as in this case the QR code can't be read. |
| qrauth     | The link to the authorization page created by Svipe, which in turns display the qrcode. |
| expires_in | The validity (in seconds) of the QR code. |
| expires_at | The UTC timestamp in seconds when the QR code will expire. |

Now, bring up a local listener on the notification endpoint url. For testing, netcat can be used:

    nc -l 64738Copy to clipboardErrorCopied

Paste the url for qrcode in a browser, scan it with the Svipe app and verify. You should now see an incoming request to the endpoint:

    POST / HTTP/1.1
    Host: 10.10.10.110:64738
    User-Agent: Svipe backend
    Accept-Encoding: gzip, deflate
    Accept: */*
    Connection: keep-alive
    Content-Type: application/json
    Authorization: Bearer tokenXX
    Content-Length: 1451
    
    {"auth_req_id": "t2jXiMScQ_PPtkUpS3CyY-jNdDbNg7DK83fiAJgn", "access_token": "Y_Eb7UhH99BvjfFmmwIVWAaZ0hA", "token_type": "Bearer", "expires_in": 120, "id_token": "eyJhbGciOiJSUzI1NiIsImtpZCI6InZXVnBUQWNlSTdVT1VzX2Z0RUZDeFlfRWV2U0U5d09PZkhkMm4ybE1QNlEiLCJrdHkiOiJSU0EifQ.eyJleHAiOjE2NzQ2MDA2MjcsImlhdCI6MTY3NDU5NzMyNywibmJmIjoxNjc0NTk3MzI3LCJpc3MiOiJodHRwczovL2FwaS5kZXYuYmVzLnN2aXBlaWQuY29tL29pZGMvdjEiLCJhdWQiOiIwNDg2ZTFmMS02MzFlLTQzODYtOWI4OS0zOWU5MWIxOTFlNjIiLCJhdXRoX3RpbWUiOjE2NzQ1OTczMjcsInN1YiI6ImMxY2M0MmVkNzRjZjQ2Y2Q4OWFlN2U2NGUzNTNlMTAxIiwic3ZpcGVpZCI6ImMxY2M0MmVkNzRjZjQ2Y2Q4OWFlN2U2NGUzNTNlMTAxIiwiYXRfaGFzaCI6Im0yVGVYRGVzUGpXV05uQ0daN0VOX0EiLCJnaXZlbl9uYW1lIjoiU3RlZmFuIERpY2siLCJmYW1pbHlfbmFtZSI6IkZhcmVzdGFtIiwibmFtZSI6IlN0ZWZhbiBEaWNrIEZhcmVzdGFtIn0.GQk4ffykLblh-sP4uztgRipQfyZYsDDaA-oF4pMLTcwiypRqr23YdTFnUA74VjuCloqcYogc3mUKyEiaeUz8IjMe621Jammz2l_dfviehyIHVSZKwGmLAlOJJ4WMzioWOMr_u_6cLmE1UOWuydPkV-U9ypvnbNFzCqlygLjUVfruhlTi8ZOu91v6r6PCLrf-BLEmsnKcm1nDQTuBclfP_NrLvTZbAatrJ7Ed33qfhFeA4RauXIWY14qdNoFihFqHY9291pM_WI4bWba7lBwbBo8mAprUGBQbc7vDRgjF2AQW_MlAxHmUu6jeyj4_-5OXIHqU80HR5vK4OPkr9fsQy08oNnEnoNNM_5i4GPtkg_p-XnouFlfANs1VDE7Iqgz-7z_Uy9dI573NrHxHhxWtYzYS770bxILJRqyatdIUDr1UltrztkQqu8M3ARgBHfI_PcL8Cfn_HOEOWXhbvSEMStgF82SvZfeZfvliBrUm-Hh8bxetxKcEiml6gR961D_Ed2XCu-pZsO-dHmvrgT761KCzrrwIbtBhADGjbHeAZYy4Xf1KHvsCSSBra5JwqBkVusu6MjE3KM4Li-i-2w-zs0lTEy65NExIgmHfyM3UrP2oVnsOtF0_wCqgHFKJhbqWIXCOO-NRU5FLOPAra6FxyZYd4QN3zja-ficUqxxfJ8Y"}Copy to clipboardErrorCopied

The id_token is a signed JWT (use jwt.io to view) and contains a body with the result of the authentication request:

    {
    "exp": 1674600627,
    "iat": 1674597327,
    "nbf": 1674597327,
    "iss": "https://api.dev.bes.svipeid.com/oidc/v1",
    "aud": "0486e1f1-631e-4386-9b89-39e91b191e62",
    "auth_time": 1674597327,
    "sub": "c1cc42ed74cf46cd89ae7e64e353e101",
    "svipeid": "c1cc42ed74cf46cd89ae7e64e353e101",
    "at_hash": "m2TeXDesPjWWNnCGZ7EN_A",
    "given_name": "Stefan Dick",
    "family_name": "Farestam",
    "name": "Stefan Dick Farestam"
    }Copy to clipboardErrorCopied

As you can see, the CIBA flow is very simple and well suited for a non-browser-based authentication. You only need to manage the display of the qrcode.