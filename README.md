# [Part 11 - Full Stack open CI/CD](https://fullstackopen.com/en/part11/introduction_to_ci_cd)

This repository is used for the CI/CD module of the Full stack open course

Fork the repository to complete course exercises

## Commands

Start by running `npm install` inside the project folder

`npm start` to run the webpack dev server
`npm test` to run tests
`npm run eslint` to run eslint
`npm run build` to make a production build
`npm run start-prod` to run your production build

## Continuous Integration (CI)

When we refer to CI in industry, we're usually talking about what happens after the actual merge happens.

We'd likely want to do some of these steps:


- Lint: to keep our code clean and maintainable
- Build: put all of our code together into software
- Test: to ensure we don't break existing features
- Package: Put it all together in an easily movable batch
- Upload/Deploy: Make it available to the world

Usually, strict definitions act as a constraint on creativity/development speed. This, however, should usually not be true for CI. This strictness should be set up in such a way as to allow for easier development and working together. Using a good CI system (such as GitHub Actions that we'll cover in this part) will allow us to do this all automagically.

## GitHub Actions

GitHub Actions work on a basis of workflows. A workflow is a series of jobs that are run when a certain triggering event happens. The jobs that are run then themselves contain instructions for what GitHub Actions should do.

A typical execution of a workflow looks like this:

- Triggering event happens (for example, there is a push to master branch).
- The workflow with that trigger is executed.
- Cleanup

Each workflow must specify at least one Job, which contains a set of Steps to perform individual tasks. The jobs will be run in parallel and the steps in each job will be executed sequentially.

or GitHub to recognize your workflows, they must be specified in .github/workflowsfolder in your repository. Each Workflow is its own separate file which needs to be configured using the YAMLdata-serialization language.

YAML is a recursive acronym for "YAML Ain't Markup Language". As the name might hint its goal is to be human-readable and it is commonly used for configuration files. You will notice below that it is indeed very easy to understand!

Notice that indentations are important in YAML. You can learn more about the syntax [here](https://docs.ansible.com/ansible/latest/reference_appendices/YAMLSyntax.html).

A basic workflow contains three elements in a YAML document. These three elements are:

- name: Yep, you guessed it, the name of the workflow
- (on) triggers: The events that trigger the workflow to be executed
- jobs: The separate jobs that the workflow will execute (a basic workflow might contain only one job).
- A simple workflow definition looks like this:

    name: Hello World!

    on:
      push:
        branches:
          - master

    jobs:
      hello_world_job:
        runs-on: ubuntu-18.04
        steps:
          - name: Say hello
            run: |
              echo "Hello World!"

## Setting up the environment

Setting up the environment is an important task while configuring a pipeline. We're going to use an ubuntu-18.04virtual environment because this is the version of Ubuntu we're going to be running in production.

    name: Deployment pipeline

    on:
      push:
        branches:
          - master

    jobs:
      simple_deployment_pipeline:
        runs-on: ubuntu-18.04
        steps:
          - uses: actions/checkout@v2
          - uses: actions/setup-node@v1
            with:
              node-version: '12.x'
          - name: npm install
            run: npm install
          - name: lint
                  run: npm run eslint

## Deployment

Defining definitive rules or requirements for a deployment system is difficult, let's try anyway:

- Our deployment system should be able to fail gracefully at any step of the deployment.
- Our deployment system should never leave our software in a broken state.
- Our deployment system should let us know when a failure has happened. It's more important to notify about failure than about success.
- Our deployment system should allow us to roll back to a previous deployment
  - Preferably this rollback should be easier to do and less prone to failure than a full deployment
  - Of course, the best option would be an automatic rollback in case of deployment failures
-Our deployment system should handle the situation where a user makes an HTTP request just before/during a deployment.
-Our deployment system should make sure that the software we are deploying meets the requirements we have set for this (e.g. don't deploy if tests haven't been run).

## Pull requests

Let us change events that trigger of the workflow as follows:

    on:
      push:
        branches:
          - main
      pull_request:
        branches: [main]
        types: [opened, synchronize]


## Versioning

    name: Bump version
    on:
      push:
        branches:
          - master
    jobs:
      build:
        runs-on: ubuntu-latest
        steps:
        - uses: actions/checkout@v2
          with:
            fetch-depth: '0'
        - name: Bump version and push tag
          uses: anothrNick/github-tag-action@1.26.0
          env:
            GITHUB_TOKEN: ${{ secrets.GITHUB_TOKEN }}
            WITH_V: true

[Authentication in GitHub](https://docs.github.com/en/actions/reference/authentication-in-a-workflow)
[anothrNick/github-tag-action](https://github.com/anothrNick/github-tag-action)


## Notifications

[Slack](https://fullstackopen.com/en/part11/expanding_further#exercise-11-19)
[Telegram](https://fullstackopen.com/en/part11/expanding_further#exercise-11-19-alternative-version-for-telegram)

















