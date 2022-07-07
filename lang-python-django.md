## Python / Django  <!-- {docsify-ignore} -->

Working sample on how to add an OIDC-based authentication, using the OIDC
Authorization flow, to a Python Django application using Svipe iD.

Clone the repo:

    git clone https://github.com/impak-finance/django-oidc-rp
    cd django-oidc-rp

Install prerequisites:

    pip3 install pipenv
    pipenv install 'Django<4.0'
    pipenv shell

Set the OIDC config environment variables:

    export CLIENT_ID="svipe-demo"
    export CLIENT_SECRET="svipe-demo-secret"
    export OIDC_ISSUER=https://api.svipe.com/oidc/v1/

    OIDC_CONF=$(curl -s $OIDC_ISSUER/.well-known/openid-configuration)
    get_conf() { echo $OIDC_CONF | jq -r .$1 ;}

Create the configuration file:

    cd example_project

    cat << ---END > example_project/settings/settings_env.py
    OIDC_RP_CLIENT_ID                       = '$CLIENT_ID'
    OIDC_RP_CLIENT_SECRET                   = '$CLIENT_SECRET'
    OIDC_RP_SCOPES                          = 'openid profile'
    OIDC_RP_PROVIDER_ENDPOINT               = '$OIDC_ISSUER'
    OIDC_RP_PROVIDER_AUTHORIZATION_ENDPOINT = '$(get_conf authorization_endpoint)'
    OIDC_RP_PROVIDER_TOKEN_ENDPOINT         = '$(get_conf token_endpoint)'
    OIDC_RP_PROVIDER_JWKS_ENDPOINT          = '$(get_conf jwks_uri)'
    OIDC_RP_PROVIDER_USERINFO_ENDPOINT      = '$(get_conf userinfo_endpoint)'
    OIDC_RP_PROVIDER_END_SESSION_ENDPOINT   = '$(get_conf end_session_endpoint)'
    ---END

run the server:

    make migrate
    make devserver

Open the user interface:

    open http://localhost:8000
