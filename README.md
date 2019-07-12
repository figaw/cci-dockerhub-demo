# CircleCI, Dockerhub Demo

A demo project using

- GitHub as a repository manager with a write-protected master-branch and a PR-based workflow.
- CircleCi to "validate a website." On success you're allowed to merge your pull-request.
- Docker Hub automatically builds the latest master-branch and makes the image available.

## Build

    docker build -t figaw/cci-dockerhub-demo .

## Run

    docker run -d -p 80:80 figaw/cci-dockerhub-demo

> It's just an Nginx container with some static content.

## Steps to reproduce

### Create repository on Github

1. Create a new repository on GitHub.

### Subscribe project to CircleCI

1. Visit [https://circleci.com](https://circleci.com)
1. Under "Add Projects" find the new repository and click "Set Up Project".
1. Click "Start Building"

### In your working directory

1. Add `config.yml` to `.circleci`-folder with appropriate config,
    valdiation, build, test-images etc.
1. `git init`
1. `git add .`
1. `git commit -am "Initial commit"`
1. `git remote add origin git@github.com:<repository>.git`
1. `git push -u origin master`
1. Make a change to the repository and push to a `ready/setup` branch.

### Visit the CircleCI UI

1. Go to `https://circleci.com/gh/<username>/<repository>` in a browser,
    you should have a job that's succeeded.
    > Otherwise keep making changes and pushing until you have a successful job; it registers as a `ci/circleci: build` status check on Github

### Visit your repository on GitHub

1. Go to `Settings -> Branches`
1. Add a Branch Protection Rule for the `master` branch, choose
    - Require status checks to pass before merging
        - Require branches to be up to date before merging
        - Select the `ci/circleci: build` status check
    - Include Administrators; nobody skips the pipeline.

1. Click "Create."

### Add a Dockerfile

1. Create a file called `Dockerfile` with your image build logic
1. (Optional) Create a file called `.dockerignore` with a white/blacklist of all files that aren't relevant for your image
1. Create a new commit, push this to a branch and create a pull-request. Don't merge it yet.

### Create a new repository on Docker Hub

1. Go to [https://hub.docker.com/](https://hub.docker.com/) and log in.
1. Click "Create Repository"
1. If you haven't already, link your GitHub to your Docker Hub.
1. Give your new repository a name, and click "Create"

On your new repository, visit the "Builds" tab.

1. Click on the "Link to GitHub" button.
1. Select your organization and the repository you created earlier.
1. Add build rules. I'd like:
    - the latest `master` branch on my GitHub repository, to be a `latest` image
    - a tag like `v1.2.3`, to be the docker image tag `1.2.3`
    - tags like `v1.2.3-rc.1`, `v1.2.3-alpha.1`.. to give images tagged `1.2.3-rc.1` or `1.2.3-alpha`.
1. Add a rule type: `Branch`, source: `master` and docker tag: `latest`.
1. (Click the blue `+` next to Build Rules) Add a rule type: `Tag`, source: `/^v(.*)/` and docker tag `{\1}`.
    > The `/^v(.*)/` is a regular expression that,
    >
    > - matches any tag that begins with "v", and
    > - captures whatever comes after, into the "first capture group."
    >
    > I can then reference the first capture group with `{\1}`.
    >
    > When I push a new tag to GitHub that starts with "v",
    >
    > - it triggers a build, and
    > - converts a `v1.2.3` tag to `1.2.3` or `v1.2.3-rc.1` to `1.2.3-rc` on Docker Hub.

Click "Save and Build."
> It'll do an empty build on Docker Hub, because our `master` branch doesn't yet contain a Dockerfile.

1. (On GitHub) Merge your pull-request to the `master` branch.
1. (On Docker Hub), A new build is triggered after a short while. (You may have to reload the page, to see the effect.)

After a little while your Docker Hub repository should be offering the latest image, the contents of the README.md on GitHub and the Dockerfile.

## (Optional) Build an image with a version-tag

### The Release Candidate

1. Use Git to checkout a specific commit from the master branch.
1. Use `git tag v1.0.0-rc.1` to tag the commit.
    > Of course, we create a release candidate before we make a release!
1. Push the tag to your GitHub repository with `git push --tags`
1. Go to Docker Hub, a build should trigger and an image tagged `1.0.0-rc.1`
  should be available shortly.

### The Release

1. Run the release candidate docker container and verify that it's good.
1. Use Git to checkout the release candidate commit, `git checkout v1.0.0-rc.1`
1. Tag the commit as a version `git tag v1.0.0`.
1. Push the tag to you GitHub repository, `git push --tags`
1. You can inspect the build and image shortly after on Docker Hub.
