# OIDC API

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
