3000 Acres developer documentation
==================================

This documentation is intended for developers on the 3000 Acres app,
specifically those who are working on the original Melbourne variant of
it.

It covers such topics as:

* setting up your development environment
* the development process
* how we deploy changes

## Setting up your developer environment

These instructions are for Mac users on OSX.  They should also work for
Linux users.  We do not support developers on Windows at this time.

### Install rvm

    curl -L https://get.rvm.io | bash -s stable

### Install rails

    rvm install 2.0.0

### Fork and clone the 3000Acres repo

First go to http://github.com/3000acres/3000acres, click "Fork", and make your
own copy of our code repository.

Then (replacing your name as appropriate):

    git clone https://github.com/YOURNAME/3000acres.git

If you have set up an ssh key on github (and you should!) you can use this instead, and avoid having to type in your password so much:

   git clone git@github.com:YOURNAME/3000acres.git

### Set up your rails environment

If you're not there already, go into the 3000acres directory:

    cd 3000acres

Now install the bundler, which helps manage add-on gems for the project:

    gem install bundler
    bundle install --without production staging

If at this point `bundle install` tells you "Can't find blah in any of the
sources" (for blah = some package we need), run `bundle update` and then
`bundle install` again and that should fix it.

Now you need to set up your databases:

    rake db:setup
    rake db:test:prepare

Run the test suite just to make sure everything's working:

    rake

(You should see a row of green dots and no red "F"s.)

And finally, run the web server on your local machine so you can see 3000 Acres in action:

    unicorn

Point your browser at http://localhost:8080/ to visit your local dev site. 

## The development process

Work to be done is kept on [Pivotal
Tracker](https://www.pivotaltracker.com/s/projects/938508).

The most important/next things to be done are those at the top of the
"backlog" column.  3000 Acres staff will keep the columns sorted so that
what they want done next is always at the top of the backlog.

To work on a story:

* Pick something from near the top of the backlog
* Click "start".
* Examine the story as written.  If you need more information to
  proceed, seek it from 3A staff.
* Break down the story into tasks
* Work through the tasks, ticking them off as you go
* Commit frequently, making sure you have working tests at each stage
* When you are done (or taking an extended break) push your work up to
  github
* Open a pull request to 3000acres/dev.  In the PR, link to the PT
  story.
* Click "Finish" in PT.
* Your code will be reviewed and, hopefully, merged.
* Alternately you might be asked to revise some things.
* If you are the code reviewer, look over the code for clarity and good
  design, check that new features have tests, and that all existing
  tests pass on Travis-CI (this last one is automated).

## The deployment process

### Staging

* Once a new feature has been merged to dev, it is pushed to our Heroku
  "staging" environment, http://acres-staging.herokuapp.com

  git push staging dev:master
  heroku run rake db:migrate --app=acres-staging
  heroku run script/deploy-tasks.sh --app=acres-staging
  heroku restart --app=acres-staging

* Click "Deliver" in PT.
* Add a comment in PT under "activity", letting 3A staff know that the
  story has been deployed to staging and that they should check it out.
  It's a good idea to include a URL and any instructions to make it
  clear for them.
* They will either click "Accept" or else they'll come back with
  requests for changes.  Iterate until they are happy.

### Production

* From time to time, we deploy to our Heroku production environment,
  http://acres-production.herokuapp.com aka http://3000acres.org
* A good time to do this is whenever the "Current" column in PT has
  nothing in the "Delivered" state (i.e. anything that's made it to
  staging has been approved)
* Open a pull request from 3000acres/dev to 3000acres/master.  Title it
  something like "Production release: blah, blah, blah" with a summary
  of changes included in this release.  Add additional detail in the
  body of the pull request message.
* Wait to see that the tests pass on Travis-CI.
* Deploy to production.

To deploy to production:

  heroku maintenance:on --app=acres-production
  git push production master
  heroku run rake db:migrate --app=acres-production
  heroku run script/deploy-tasks.sh --app=acres-production
  heroku restart --app=acres-production
  heroku maintenance:off --app=acres-production
  


