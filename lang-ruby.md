## Ruby  <!-- {docsify-ignore} -->

Working sample on how to add an OIDC-based authentication, using the OIDC
Authorization flow, to a Ruby on Rails application using Svipe iD. Based on a
sample from OneLogin.

Clone the repo from OneLogin:

    git clone https://github.com/onelogin/onelogin-oidc-ruby.git
    cd onelogin-oidc-ruby/1.\ Auth\ Flow/

Ensure that you have a few required utilities installed:

    brew install rbenv ruby-build gsed jq

Use `rbenv` to configure the correct ruby version:

    rbenv install -s 3.1.0
    rbenv local 3.1.0
    rbenv rehash
    eval "$(rbenv init - bash)"

Remove all version restrictions from the `Gemfile` (to use latest versions):

    gsed -Ei -e "s/'[~>][^']*'//" -e "s/, *$//" Gemfile
    rm Gemfile.lock

Install the required gems

    gem install bundler
    bundle install

Fix the ruby config file to use env settings:

    gsed -Ei \
       -e "s|(identifier:).*|\1              ENV['CLIENT_ID'],|"     \
       -e "s|(secret:).*|\1                  ENV['CLIENT_SECRET'],|" \
       -e "s|(redirect_uri:).*|\1            ENV['REDIRECT_URI'],|"  \
       -e "s|(host:).*|\1                    ENV['OIDC_HOST'],|"     \
       -e "s|(authorization_endpoint:).*|\1  ENV['EP_AUTHORIZE'],|"  \
       -e "s|(token_endpoint:).*|\1          ENV['EP_TOKEN'],|"      \
       -e "s|(userinfo_endpoint:).*|\1       ENV['EP_USERINFO']|"    \
       app/helpers/sessions_helper.rb

Change the branding to Svipe iD:

    gsed -Ei "s/OneLogin/Svipe iD/g" app/views/home/index.html.erb

Set the OIDC config environment variables:

    export REDIRECT_URI="http://localhost:3000/signin-oidc"

    export CLIENT_ID="svipe-demo"
    export CLIENT_SECRET="svipe-demo-secret"

    export OIDC_BASE_URI=https://api.svipe.com/oidc/v1
    export OIDC_HOST=$(echo $OIDC_BASE_URI | cut -d/ -f3)

    OIDC_CONF=$(curl -s $OIDC_BASE_URI/.well-known/openid-configuration)

    get_conf() { echo $OIDC_CONF | jq -r .$1 | cut -d/ -f4- | sed s:^:/: ;}

    export EP_AUTHORIZE=$(get_conf authorization_endpoint)
    export EP_USERINFO=$( get_conf userinfo_endpoint     )
    export EP_TOKEN=$(    get_conf token_endpoint        )

Run the rails server:

    bundle exec rails server

Open the user interface

    open http://localhost:3000

