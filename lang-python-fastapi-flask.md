## Python / FastAPI / Flask  <!-- {docsify-ignore} -->

These examples cover integration using the OIDC Auth Flow, both with a server
and a client generated qr-code, as well as with/without PKCE.

Clone the repo:

    git clone https://github.com/svipe/svipe-oidc-rp-samples
    cd svipe-oidc-rp-samples

Please check out the more detailed documentation in the Readme.md, but for a
quick start, first install prerequisites:

    pip3 install pipenv
    pipenv install -r requirements.txt

Then use one of the following to start the RP server:

    pipenv run 1_Auth_Flow/app-authlib-fastapi.py
    pipenv run 1_Auth_Flow/app-authlib-flask.py
    pipenv run 1_Auth_Flow/app-nolib-fastapi.py

    pipenv run 2_Auth_Flow_Preloaded_Qrcode/app-authlib-fastapi.py
    pipenv run 2_Auth_Flow_Preloaded_Qrcode/app-authlib-flask.py
    pipenv run 2_Auth_Flow_Preloaded_Qrcode/app-nolib-fastapi.py

Open the user interface:

    open http://localhost:9000
