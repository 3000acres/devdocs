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
  merge any of your changes (eg. bugfixes) into the central repository
  because we have to painstakingly separate the local customisations from
  the actual improvements
* Therefore, we want to create a system where you can customise your own
  site (change the logo, colours, text on the homepage, etc) without
  having to modify the core code of the app.
* We have had this in mind since the start, and support it in some ways,
  but it is not yet fully supported.
* Work on this will occur through 2014-2015.  We are seeking
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
* Git/Github - familiarity with forking, cloning, branches, merging, pushing, etc
* Heroku - some experience (or willingness to read docs)

## Required infrastructure

* Your own development machine - preferably OSX or Linux (Rails dev on
  Windows is quite fiddly to set up.)
* Register a domain name
* Set up an email address for contact, eg. hello@yoursite.org
* [Github](http://github.com) account (free) -- code repository, required
* [Heroku](http://heroku.com) account (free) -- hosting, req
* [Mailchimp](http://mailchimp.com) account (starts free, costs a small amount as you send more newsletters)
* [Mapbox](http://mapbox.com) account ($5/month tier) -- displays maps
  on your site

### Highly recommended

* [Travis CI](http://travis-ci.org) account (free) -- automated testing
* [Coveralls](http://coveralls.io) account (free) -- automated testing
* Analytics -- shows how many visitors you have to your site
  * We recommend [Clicky](http://clicky.com) (free or lowest paid tier)
    or [Google Analytics](http://google.com/analytics) (free)

## Create your own code repository

* Set up a Github organisation
  * We recommend setting up a github organisation for your actual
    organisation, rather than having the repo under a personal account,
    unless you are certain that only one person will ever be responsible
    for the app.  Setting up an organisation is free.
* Fork the 3000 Acres github repo into your organisation's github space
* Clone it to your own development environment

## Set up your own dev environment and get the 3000acres code working

* Follow all the steps in [README.md](README.md) for setting up your dev
  environment, eg. bundle install, rake db:setup, etc.
* Set up a branch for active development and one for production
  releases.  3000acres uses "dev" for active development and "master"
  for production-ready code, and we recommend following this as it will
  make life a little easier when collaborating between Acres projects.
* Push your dev branch back up to github
* In the github settings for your repo, make the dev branch the default
  branch.  This will make it easier when people do pull requests.

## Set up Mailchimp newsletter

* Set up a Mailchimp newsletter to keep your community informed of
  what's going on
* The free tier should be fine for starters; you might have to chip in
  ~$10 every so often if you send a lot of mail
* You need to do this up front so you can get the ID of it for
  configuring your Acres app

## Set up a Mapbox project

* Set up a Mapbox project under your account there; no particular
  settings are required.
* You need to do this up front so you can get the ID of it for
  configuring your Acres app

## Set configuration variables

* Copy config/application.yml.example to config/application.yml
* Edit config/application.yml to reflect your own org's settings eg.
  * Site name = "YourCity Acres"
  * set the contact email address
  * set the region, map central point, etc
  * various API keys etc
  * Note the snippet of Ruby code to get your Mailchimp newsletter ID -
    you can run this from within 'irb'

## Run the app locally

* Run 'rake' to check that all the tests pass
* Run 'unicorn' to see the app running locally
* You should see your site's own name in the header, title, etc
* You can login as test users test1/password1, test2/password2,
  test3/password3, or admin1/password1

## Set up the CMS

* Note that you're doing this on your dev machine just to gain
  familiarity -- you'll need to do this again when you push to your
  staging/production sites, but you can copy-paste between them
* Login as admin1/password1.  You should see an admin menu in the top
  nav.
* Create pages in your CMS for your org, eg. "About", "Resources", etc
* Instructions for this are in the devdocs [README.md](README.md)
* We are working on automating the CMS setup to create sample/stub pages
  out of the box.

## Modify code

These are the areas we would like to turn into more themeable
customisations; at present, if you make these changes, you will make
your site incompatible with 3000 Acres and will be unable to share new
features in either direction.  Therefore, these steps also represent a
to-do list of things we need to do to make it easier to collaborate.

* Replace the logo and the header background image (in
  app/assets/images)
* Modify the CSS to match your colour scheme and other needs (in
  app/assets/stylesheets)
* Modify the content of the homepage (app/views/home/) to match your desired
  phrasing, show your own sponsors, etc (we are actively working on putting
  this content into the CMS, so stay tuned for that)
* Likewise for the site footer in (app/views/layout/)
* Modify the list of local government areas in db/seeds/ to reflect your
  local area.  You will need to re-create the database for this to take
  effect.
  * You may also edit these via the admin web interface

## Commit your changes

* Make sure the tests continue to pass at each stage, as you make
  changes, by running "rake".
* We strongly recommend that you work on each new feature or chunk of
  work in an appropriately named branch off dev (see
  [README.md](README.md) for how we do this on 3000acres itself)
* Commit in small, self-contained chunks, with clear commit messages, as
  you go along.
* When you are done with your branch, push it up to github
* Make a pull request within your own repo (from your branch to dev).
  * Or merge however else you like.  We prefer the pull request method
    because it improves transparency and helps people see what's going on
    in the project.
* Back on your own development machine, update your dev branch

## Set up hosting on Heroku

* Create staging and production apps on heroku
  * The free tier should be fine unless your site gets huge
  * We recommend buying the $9/month Postgres add-on so you get
    automated backups for your production database
* Add the Mandrill add-on (to send email)

## Pushing to Heroku

* See [README.md](README.md) for how we do this on 3000acres itself --
  this works well for us and we recommend it as a process, though we'd
  welcome suggestions/improvements
* The first time you push to Heroku, or after making a configuration
  change, make sure you run 'rake figaro:heroku[appname]' to set
  all the config variables
