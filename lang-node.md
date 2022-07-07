## Node.js  <!-- {docsify-ignore} -->

Working samples on how to add an OIDC-based authentication, using the OIDC
Authorization flow, to a Node.js application with Passport.js using Svipe iD.
Based on samples from OneLogin.

### Regular Auth Flow using Passport.js

Clone the repo:

    git clone https://github.com/onelogin/onelogin-oidc-node.git
    cd onelogin-oidc-node/1.\ Auth\ Flow/

Ensure that you have a few required utilities installed:

    brew install gsed jq

Set the OIDC config environment variables:

    export CLIENT_ID="svipe-demo"
    export CLIENT_SECRET="svipe-demo-secret"
    export REDIRECT_URI="http://localhost:3000/oauth/callback"
    export OIDC_ISSUER=https://api.svipe.com/oidc/v1

    OIDC_CONF=$(curl -s $OIDC_ISSUER/.well-known/openid-configuration)
    get_conf() { echo $OIDC_CONF | jq -r .$1 ;}

    ## FEP = Full Endpoint Paths
    export FEP_AUTHORIZE=$(get_conf authorization_endpoint)
    export FEP_USERINFO=$( get_conf userinfo_endpoint     )
    export FEP_TOKEN=$(    get_conf token_endpoint        )
    export FEP_LOGOUT=$(   get_conf end_session_endpoint  )

Fix the node config files to use env settings:

    gsed -Ei \
        -e "s|^( *issuer:).*|\1             process.env.OIDC_ISSUER,|"   \
        -e "s|^( *clientID:).*|\1           process.env.CLIENT_ID,|"     \
        -e "s|^( *clientSecret:).*|\1       process.env.CLIENT_SECRET,|" \
        -e "s|^( *callbackURL:).*|\1        process.env.REDIRECT_URI,|"  \
        -e "s|^( *authorizationURL:).*|\1   process.env.FEP_AUTHORIZE,|" \
        -e "s|^( *userInfoURL:).*|\1        process.env.FEP_USERINFO,|"  \
        -e "s|^( *tokenURL:).*|\1           process.env.FEP_TOKEN,|"     \
        -e "/const *baseUri *=/d"                                        \
        -e "s|env.*/oidc/2/logout|env.FEP_LOGOUT}|"                      \
        app.js

    gsed -Ei 's|env.*/oidc/2/me|env.FEP_USERINFO}|' routes/users.js

Change the branding to Svipe iD:

    gsed -Ei "s/OneLogin/Svipe iD/g" views/users.hbs
    gsed -Ei "s/OneLogin/Svipe iD/g" views/index.hbs
    gsed -Ei "s/OneLogin/Svipe iD/g" routes/index.js

Install and run:

    rm package-lock.json
    npm install
    npm start

Open the user interface

    open http://localhost:3000



### PKCE Auth Flow using Passport.js

Clone the repo:

    git clone https://github.com/onelogin/onelogin-oidc-node.git
    cd onelogin-oidc-node/4.\ Auth\ Flow\ -\ PKCE/

Set the OIDC config environment variables:

    export CLIENT_ID="svipe-demo"
    export OIDC_ISSUER=https://api.svipe.com/oidc/v1

Ensure that you have a few required utilities installed:

    brew install gsed

Fix the code to use the Svipe OIDC provider:

    gsed -Ei \
        -e "s|^( *authority:).*|\1          '$OIDC_ISSUER',|" \
        -e "s|^( *client_id:).*|\1          '$CLIENT_ID',|" \
        public/javascripts/main.js

Change the branding to Svipe iD:

    gsed -Ei "s/OneLogin/Svipe iD/g" views/index.hbs
    gsed -Ei "s/OneLogin/Svipe iD/g" routes/index.js

Install and run:

    rm package-lock.json
    npm install
    npm start

Open the user interface

    open http://localhost:3000
