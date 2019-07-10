# CircleCI, Dockerhub Demo

## Steps to reproduce.

## Create repository on Github

1. Create a new repository on GitHub.

## Subscript project to CircleCI

1. Visit https://circleci.com
1. Under "Add Projects" find the new repository and click "Set Up Project".
1. Click "Start Building"

## In your working directory

1. Add `config.yml` to `.circleci`-folder with appropriate config,
    valdiation, build, test-images etc.
1. `git init`
1. `git add .`
1. `git commit -am "Initial commit"`
1. `git remote add origin git@github.com:<repository>.git`
1. `git push -u origin master`
1. Make a change to the repository and push to a `ready/setup` branch.

## Visit the CircleCI UI

1. Go to `https://circleci.com/gh/<username>/<repository>` in a browser,
    you should have a job that's succeeded.

> Otherwise keep making changes and pushing until you have a successful job; it registers as a `ci/circleci: build` status check on Github

## Visit you repository on GitHub

1. Go to `Settings -> Branches`
1. Add a Branch Protection Rule for the `master` branch, choose
    - Require status checks to pass before merging
        - Require branches to be up to date before merging
        - Select the `ci/circleci: build` status check
    - Include Administrators

1. Click "Create."

## Add a Dockerfile

1. Create a file called `Dockerfile` with your image build logic
1. (Optional) Create a file called `.dockerignore` with a white/blacklist of all files that aren't relevant for your image
1. Create a new commit, push this to a branch and create a pull-request. (Don't merge it yet.)

## Create a new repository on Docker Hub

1. Go to [https://hub.docker.com/](https://hub.docker.com/) and log in.
1. Click "Create Repository"
1. If you haven't already, link your GitHub to your Docker Hub.
1. Give your new repository a name, and click "Create"

On your new repository, visit the "Builds" tab.

1. Click on the "Link to GitHub" button.
1. Select your organization and the repository you created earlier. Click "Save and Build."
1. (On GitHub) Merge your pull-request to the `master`-branch
1. (On Docker Hub), A new build is triggered after a short while. (You may have to reload the page, to see the effect.)
