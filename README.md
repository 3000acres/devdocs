3000 Acres developer documentation
==================================

This documentation is intended for developers on the 3000 Acres app,
specifically those who are working on the original Melbourne variant of
it.

It covers such topics as:

* things to sign up for
* setting up your development environment
* the development process
* how we deploy changes

If you are forking 3000 Acres for another city, and want to learn how to
set it up and run it for your own site, see [NewSite.md](NewSite.md).

## Things to sign up for

Dev tools etc:

* http://github.com (also add your ssh key)
* http://heroku.com (also add your ssh key)
* http://travis-ci.org
* http://pivotaltracker.com/

3000 Acres instances (you should have an account on each):

* http://acres-staging.herokuapp.com -- staging
* http://3000acres.org -- production

## Setting up your developer environment

These instructions are largely based on the [Growstuff developer
docs](http://wiki.growstuff.org/index.php/Development/Getting_Started),
as 3000 Acres is largely based on Growstuff in terms of code layout and
dev process.  If you want more detail/get stuck/are interested you might
like to take a look at the Growstuff docs, as they have a bit more
detail in some areas.  (If needed, we can copy more of the Growstuff
docs over here.)

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

Let's set up a git remote for "upstream" i.e. the 3000acres/3000acres
repo:

    git remote add upstream https://github.com/3000acres/3000acres.git

### Recommended bashrc settings

These may make your dev experience more pleasant.

    # this will help find the paths for rails and stuff
    # Load RVM into a shell session *as a function*
    [[ -s "$HOME/.rvm/scripts/rvm" ]] && source "$HOME/.rvm/scripts/rvm"

    # use magic git completion in bash (default in ubuntu; you'll want
    # to download the bash_completion script and put it somewhere in
    # your $HOME if you're on OSX
    if [ -f /etc/bash_completion ]; then
        . /etc/bash_completion
    fi

    # set up magic awesome prompt to tell you what branch you're on
    export PS1='[\u@\h \W$(__git_ps1 " (%s)")]\$ '


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

* Pick something from near the top of the backlog (it doesn't have to be
  the top item -- just something reasonably near the top that fits
  your skills and available time.)
* Click "start".  The story will be moved to the "Current" column.
* Examine the story as written.  If you need more information to
  proceed, seek it from 3A staff, and update the description to be more
  informative.  You can also attach pics etc if needed..
* Break down the story into tasks.  For instance, a simple "add a field"
  story might read something like:
  * write feature tests (marked as pending)
  * rails g migration...
  * update model
  * update form and controller to add the field
  * update site detail page to show the data
* Create a git branch to work on: `git checkout -b storyname`
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

### Set up heroku

* You need to be added as a collaborator on Heroku, by someone who
  already is one
* Install the heroku toolbelt
* Set up your git remotes

Like this:

    git remote add staging git@heroku.com:acres-staging.git
    git remote add production git@heroku.com:acres-production.git

### Staging

* Once a new feature has been merged to dev, it is pushed to our Heroku
  "staging" environment, http://acres-staging.herokuapp.com

To deploy to staging:

    # get the latest dev branch
    git checkout dev
    git pull upstream dev

    # run tests and make sure they are ok
    rake

    # push code up to staging
    git push staging dev:master

    # set config variables (especially if any have changed/been added)
    rake figaro:heroku[acres-staging]
    # to check that config vars were set correctly
    heroku config --app=acres-staging

    # make database changes
    heroku run rake db:migrate --app=acres-staging
    heroku run script/deploy-tasks.sh --app=acres-staging

    # restart
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

    git checkout master
    git pull upstream master
    rake

    # in production, we switch maintenance mode on before pushing
    heroku maintenance:on --app=acres-production

    git push production master
    rake figaro:heroku[acres-production]
    heroku config --app=acres-production
    heroku run rake db:migrate --app=acres-production
    heroku run script/deploy-tasks.sh --app=acres-production
    heroku restart --app=acres-production

    # and switch maintenance mode off again
    heroku maintenance:off --app=acres-production


