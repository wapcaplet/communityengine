CommunityEngine

Information at: [http://www.communityengine.org](http://www.communityengine.org)

Requirements:

- Ruby 1.9.2
- Rails 3.1.0.beta1 (lower versions are not supported)
- All other requirements are brought in automatically
- Don't forget to include the gems you plan to include in your application


Getting CommunityEngine Running
--------------------------------

Copy the following into your `Gemfile`:

    source 'http://rubygems.org'

    gem 'rails', '3.1.0.beta1'
    gem 'sprockets', '2.0.0.beta.2'
    gem 'community_engine', :git => 'https://github.com/wapcaplet/communityengine.git', :branch => 'rails3'

From your app's root directory run:

    $ bundle install --binstubs
    $ bin/rake community_engine:install:migrations
    $ bin/rake db:migrate

Add a file called `application.yml` to your `config` directory. In it put (at least):

    community_name: Your Application Name

Mount CommunityEngine in your `config/routes.rb` file:

    mount CommunityEngine::Engine => "/"

Delete the default `views/layouts/application.html.erb` that Rails created for
you. Delete `public/index.html` if you haven't already.

Enable asset pipelining so the stylesheets and images will work, by adding this line to your `config/application.rb`:

    config.assets.enabled = true

Then, start your server!

    $ bin/rails server


Optional Configuration
======================

To override the default configuration, create an `application.yml` file in `Rails.root/config`

The application configuration defined in this file overrides the one defined in the [CommunityEngine gem](https://github.com/bborn/communityengine/blob/rails3/config/application.yml)

This is where you can change commonly used configuration variables, like `configatron.community_name`, etc.


OmniAuth Configuration:
=======================

You can allow users to sign up and log in using their accounts from other social networks (like Facbeook, Twitter, LinkedIn, etc.). To do so, just add an initializer in your app's `config/initializers` directory called `omniauth.rb`. In it, put:

    Rails.application.config.middleware.use OmniAuth::Builder do
      provider :twitter, 'CONSUMER_KEY', 'CONSUMER_SECRET'
      provider :facebook, 'APP_ID', 'APP_SECRET'
      provider :linked_in, 'CONSUMER_KEY', 'CONSUMER_SECRET'
    end

See the [OmniAuth Github repository](https://github.com/intridea/omniauth) for more information and configuration options.


Photo Uploading
---------------

By default CommunityEngine uses the filesystem to store photos.

To use Amazon S3 as the backend for your file uploads, you'll need the aws-s3 gem installed, and you'll need to add a file called `amazon_s3.yml` to the application's root config directory (examples are in `/community_engine/sample_files`).

You'll need to change your configuration in your `application.yml` to tell CommunityEngine to use s3 as the photo backend.

Finally, you'll need an S3 account for S3 photo uploading.


Create an s3.yml file in `Rails.root/config`



Roles
------

CommunityEngine Users have a Role (by default, it's admin, moderator, or member)

Once logged in as an admin, you'll be able to toggle other users between moderator and member (just go to their profile page and look on the sidebar.)

Admins and moderators can edit and delete other users posts.

There is a rake task to make an existing user into an admin:

    rake community_engine:make_admin email=user@foo.com

(Pass in the e-mail of the user you'd like to make an admin)



Localization
------------

Localization is done via Rails native I18n API. We've added some extensions to String and Symbol to let them respond to the `.l` method. That allows for a look up of the symbol (or a symbolized version of the string).

For complex strings with substitutions, Symbols respond to the `.l` method with a hash passed as an argument, for example:

    :welcome.l :name => current_user.name

And in your language file you'd have:

    welcome: "Welcome {{name}}"

To customize the language, or add a new language create a new yaml file in `Rails.root/config/locales`. The name of the file should be `LANG-LOCALE.yml` (`e.g. en-US.yml` or `es-PR`). The language only file (`es.yml`) will support all locales.


Spam Control
------------

Spam sucks. Most likely, you'll need to implement some custom solution to control spam on your site, but CE offers a few tools to help with the basics.

ReCaptcha: to allow non-logged-in commenting and use [ReCaptcha](http://recaptcha.net/) to ensure robots aren't submitting comments to your site, just add the following lines to your `application.yml`:

    allow_anonymous_commenting: true
    recaptcha_pub_key: YOUR_PUBLIC_KEY
    recaptcha_priv_key: YOUR_PRIVATE_KEY

You can also require ReCaptcha on signup (to prevent automated signups) by adding this in your `application.yml` (you'll still need to add your ReCaptcha keys):

    require_captcha_on_signup: true

Akismet: Unfortunately, bots aren't the only ones submitting spam; humans do it too. [Akismet](http://akismet.com/) is a great collaborative spam filter from the makers of Wordpress, and you can use it to check for spam comments by adding one line to your `application.yml`:

    akismet_key: YOUR_KEY


Other notes
-----------

Any views you create in your app directory will override those in CommunityEngine
For example, you could create `Rails.root/app/views/layouts/application.html.haml` and have that include your own stylesheets, etc.

You can also override CommunityEngine's controllers by creating identically-named controllers in your application's `app/controllers` directory.




Contributors - Thanks! :)
-------------------------

- Bryan Kearney - localization
- Alex Nesbitt - forgot password bugs
- Alejandro Raiczyk - Spanish localization
- [Fritz Thielemann](http://github.com/fritzek) - German localization, il8n
- [Oleg Ivanov](http://github.com/morhekil) - `acts_as_taggable_on_steroids`
- David Fugere - French localization
- Barry Paul - routes refactoring
- [Andrei Erdoss](http://github.com/cauta) localization
- [Errol Siegel](http://github.com/eksatx) simple private messages integration, documentation help
- Carl Fyffe - documentation, misc.
- [Juan de Frías](http://github.com/juafrlo) static pages, photo albums, message_controller tests
- [Joel Nimety](http://github.com/jnimety) authlogic authentication
- [Stephane Decleire](http://github.com/sdecleire) i18n, fr-FR locale



Bug tracking is via [GitHub Issues](https://github.com/bborn/communityengine/issues)
