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
1. `git push origin master:ready/setup`
