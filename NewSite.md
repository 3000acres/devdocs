# Instructions for starting a new "Acres" site

So you want to start a new "YourCity Acres" site! This document lays out
the steps involved.

We are preparing to do a bunch of work to improve this process in two
ways:

1. Make it less complicated
2. Make it so that you don't wind up with a fork that's incompatible
with 3000 Acres

To elaborate further on that second point:

* We want to collaborate with other cities who are using this codebase
* We would like it if a feature added by one city were available to all
* We would also like it if improvements and bugfixes from the central
  codebase (3000acres) could flow easily to other sites using our code
* If you fork the code and make changes to it, which are incompatible
  with 3000acres itself (eg. changing the logo), it makes it hard to
  merge any of your other changes (eg. bugfixes) into the central repository
  because we have to painstakingly separate the local customisations from
  the actual improvements
* Therefore, we want to create a system where you can customise your own
  site (change the logo, colours, text on the homepage, etc) without
  having to modify the core code of the app.
* We have had this in mind since the start, and support it in some ways,
  but it is not yet 100% supported.
* Work on improving this will occur through 2014-2015.  We are seeking
  assistance/collaboration/funding from other cities using the 3000acres
  codebase to help us achieve this.  If you can help in any way, please
  get in touch via 3000 Acres contact address 
  [hello@3000acres.org](mailto:hello@3000acres.org) or contact the lead 
  developer, Alex, at [skud@growstuff.org](skud@growstuff.org).

Having said that, here's how to create your own 3000 Acres site, using
our codebase.  There are also notes about where we hope to make
improvements.

## Required skills/knowledge

* HTML/CSS/design skills - to customise your site
* Ruby on Rails - advanced beginner to intermediate level
  * including automated testing using RSpec and Capybara
  * we are working on making this more like "beginner" level and
    requiring less coding experience, but at present you will still have
    to make numerous changes to the app and its test suite to get it
    working, and be familiar with where Rails keeps things
* Git/Github - familiarity with forking, cloning, branches, merging, pushing, etc
* Heroku - some experience (or willingness to read docs)

## Required infrastructure

* Your own development machine - preferably OSX or Linux (Rails dev on
  Windows is quite fiddly to set up, and basically unsupported)
* Register a domain name
* Set up an email address for contact, eg. hello@yoursite.org
* [Github](http://github.com) account (free) -- code repository
* [Heroku](http://heroku.com) account (free) -- hosting
* [Mailchimp](http://mailchimp.com) account (free or low cost) -- sends newsletters
* [Mapbox](http://mapbox.com) account ($5/month tier) -- displays maps
  on your site

### Highly recommended

* [Travis CI](http://travis-ci.org) account (free) -- automated testing
* [Coveralls](http://coveralls.io) account (free) -- automated testing
* Analytics -- shows how many visitors you have to your site
  * We recommend [Clicky](http://clicky.com) (free or lowest paid tier)
    or [Google Analytics](http://google.com/analytics) (free)

## Create your own code repository

The first step is to get a copy of the code you can work on.

1. Set up a Github organisation
  * We recommend setting up a github organisation for your actual
    organisation, rather than having the repo under a personal account,
    unless you are certain that only one person will ever be responsible
    for the app.  Setting up an organisation is free.
1. Fork the 3000 Acres github repo into your organisation's github space
1. Clone it to your own development environment

## Set up your own dev environment and get the 3000acres code working

1. Follow all the steps in [README.md](README.md) for setting up your dev
  environment, eg. bundle install, rake db:setup, etc.
1. Set up a branch for active development and one for production
  releases.  3000acres uses "dev" for active development and "master"
  for production-ready code, and we recommend following this as it will
  make life a little easier when collaborating between Acres projects.
1. Push your dev branch back up to github
1. In the github settings for your repo, make the dev branch the default
  branch.  This will make it easier when people do pull requests.

## Set up hosting on Heroku

For the next few sections we'll just be setting up lots of third-party
services for various uses.  We set these up in advance because you will
need some information from them (eg. API keys) to configure your 3000
Acres app.

1. Create staging and production apps on heroku
  * The free tier should be fine unless your site gets huge
  * We recommend buying the $9/month Postgres add-on so you get
    automated backups for your production database
1. Add the Mandrill add-on (to send email)
1. You need to do this up front so you can get the Mandrill API key for
  configuring your Acres app

## Set up Mailchimp newsletter

1. Set up a Mailchimp newsletter to keep your community informed of
  what's going on
1. The free tier should be fine for starters; you might have to chip in
  ~$10 every so often if you send a lot of mail
1. You need to do this up front so you can get the ID of it for
  configuring your Acres app

## Set up a Mapbox project

1. Set up a Mapbox project under your account there; no particular
  settings are required.
1. You need to do this up front so you can get the ID of it for
  configuring your Acres app

## Set configuration variables

1. Copy config/application.yml.example to config/application.yml
1. Edit config/application.yml to reflect your own org's settings eg.
  * Site name = "YourCity Acres"
  * set the contact email address
  * set the region, map central point, etc
  * various API keys etc
  * Note the snippet of Ruby code to get your Mailchimp newsletter ID -
    you can run this from within 'irb'

## Run the app locally

1. Run 'rake' to check that all the tests pass
1. Run 'unicorn' to see the app running locally at http://localhost:8080
1. You should see your site's own name (as you configured in
    application.yml) in the header, title, etc
1. You can login as test users test1/password1, test2/password2,
  test3/password3, or admin1/password1

## Set up the CMS

1. Note that you're doing this on your dev machine just to gain
  familiarity -- you'll need to do this again when you push to your
  staging/production sites, but you can copy-paste between them
1. Login as admin1/password1.  You should see an admin menu in the top
  nav.
1. Create pages in your CMS for your org, eg. "About", "Resources", etc
1. Instructions for this are in the devdocs [README.md](README.md)
1. We are working on automating the CMS setup to create sample/stub pages
  out of the box.

## Modify code

These are the areas we would like to turn into more themeable
customisations; at present, if you make these changes, you will make
your site incompatible with 3000 Acres and will be unable to share new
features in either direction.  Therefore, these steps also represent a
to-do list of things we need to do to make it easier to collaborate.

1. Replace the logo and the header background image (in
  app/assets/images)
1. Modify the CSS to match your colour scheme and other needs (in
  app/assets/stylesheets)
1. Modify the content of the homepage (app/views/home/) to match your desired
  phrasing, show your own sponsors, etc (we are actively working on putting
  this content into the CMS, so stay tuned for that)
1. Likewise for the site footer in (app/views/layout/)
1. Modify config/application.rb to put your Mapbox key there (we intend
  to make this a configuration variable vi config/application.yml but
  have some technical hurdles to figure out)
1. Modify app/assets/javascripts/sites.js.erb to centre the map on your
  city, with the desired zoom level (we intend to make these options into
  configuration variables, but have the same issue as with the mapbox
  key)
1. Modify the list of local government areas in db/seeds/ to reflect your
  city's LGAs.  You will need to re-create the database for this to take
  effect, so make sure you do it before deploying outside your local dev
  environment.
  * You may also edit these after the fact via the admin web interface

## Commit your changes

1. As you make changes, make sure the tests continue to pass at each stage,
  by running "rake".
1. We strongly recommend that you work on each new feature or chunk of
  work in an appropriately named branch off dev (see
  [README.md](README.md) for how we do this on 3000acres itself)
1. Commit in small, self-contained chunks, with clear commit messages, as
  you go along.
1. When you are done with your branch, push it up to github
1. Make a pull request within your own repo (from your branch to dev).
  * Or merge however else you like.  We prefer the pull request method
    because it improves transparency and helps people see what's going on
    in the project.
1. Back on your own development machine, update your dev branch

## Pushing to Heroku

1. See [README.md](README.md) for how we do this on 3000acres itself --
  this works well for us and we recommend it as a process, though we'd
  welcome suggestions/improvements
1. The first time you push to Heroku, or after making a configuration
  change, make sure you run 'rake figaro:heroku[appname]' to set
  all the config variables
