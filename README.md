# Erlang CI/CD template

## Overview

The Erlang CI/CD Template is designed to facilitate Continuous Integration (CI) and Continuous Deployment (CD) processes for Erlang/OTP releases.

It leverages GitHub Actions to automate tasks such as running unit tests, testing hot code upgrades, and building releases.

The main goal is to increase the developers' confidence when writing code that leverages the hot code upgrade mechanism available in Erlang/OTP.

## Usage

### New project

Using this template with a new project is simple and is done through the following steps:

1. Click on the `Use this template` button located on the top right of this repository's page.
1. Click on `create a new repository`
1. Configure your new repository
1. Clone your repository
1. Navigate inside it
1. Run `rebar3 new release <release_name>` to create a new Erlang/OTP release
1. Run `rm <release_name>/rebar.config`
1. Run `mv <release_name>/* erlang/ && rm <release_name>`
1. Modify `erlang/rebar.config` and replace `release_name` with the name of your release

### Existing project

Using this template with an existing project can be more complex because, depending on the structure of your project, more or less steps might be required.

1. Move all the files related to your project in the `erlang` folder
1. Merge your `rebar.config` with the one provided in this template.

## Configuration

### Repository variables

Set the following [repository variables](https://docs.github.com/en/actions/learn-github-actions/variables#creating-configuration-variables-for-a-repository), up.

| Variable name | Description | Reason | Required |
|---------------|-------------|--------|----------|
|`RELNAME`| Name of the release | Used in the github actions to run tests / build the release | yes |

## Keeping workflows up to date

To import the changes made to the *Erlang CI/CD template* into your project, use
[*template-sync*](https://github.com/coopTilleuls/template-sync):

1. Run the script to synchronize your project with the latest version of the skeleton:

    ```console
    curl -sSL https://raw.githubusercontent.com/mano-lis/template-sync/main/template-sync.sh | sh -s -- https://github.com/ahzed11/erlang-ci-cd
    ```

1. Resolve conflicts, if any
1. Run `git cherry-pick --continue`

*This section has been adapted from* [symfony-docker](https://github.com/dunglas/symfony-docker/blob/main/docs/updating.md)

## Functionalities

### Run unit tests

The `erlang-ci` workflow runs unit tests built with `common_test` located in the `erlang/apps` directory and attempts to build the release.

The results of the tests are uploaded as workflow artifacts.

#### Triggers

- `push` on main
- `pull_request` on main

### Run hot code upgrade/downgrade tests

The `relup-ci` workflow builds both the previous and the current release and launches the `erlang/test/upgrade_downgrade_SUITE.erl` test suite.

This test suite leverages the [peer](https://www.erlang.org/doc/man/peer) module to start a Docker container containing both the previous and the latest release. The peer module also allows us to have interactions with the container such as modifiying its state via functions calls and applying upgrades or downgrades.

This suite is divided in multiple cases:

| Test case name | Is implemented ? |
|----------------|------------------|
| before_upgrade_case | no |
| upgrade_case | yes |
| after_upgrade_case | no |
| before_downgrade_case | no |
| downgrade_case | yes |
| after_downgrade_case | no |

Some cases are not implemented because the state modification before and after downgrades/upgrades is project specific.

The results of the tests are uploaded as workflow artifacts.

#### Triggers

- `pull_request` on main

### Publish a release

The `publish-tarball` workflow builds and uploads a tarball of the OTP release, creates a Github release and adds the built tarball as an artifact.

#### Triggers

- `push` tag with name `re[ "v[0-9]+.[0-9]+.[0-9]+" ]`

## Constraints

### File structure

1. The `erlang` directory **must** contain all the files of your release

### Versioning

The project uses `Smoothver` versioning, tailored for OTP projects. For more details, read [this blog post](https://ferd.ca/my-favorite-erlang-container.html).

> Given a version number RESTART.RELUP.RELOAD, increment the:
>
> - RESTART version when you make a change that requires the server to be rebooted.
> - RELUP version when you make a change that requires pausing workers and migrating state.
> - RELOAD version when you make a change that requires reloading modules with no other transformation.

*Citation from*: [ferd.ca - My favorite Erlang Container](https://ferd.ca/my-favorite-erlang-container.html)

## Projects using this template

- [pixelwar](https://github.com/Ahzed11/pixelwar): A small project used to develop and test these workflows

## Possible improvements

- Test hot code upgrades on multiple docker containers to simulate a distributed system
- Publish the test artifacts on the repository's Github pages

## Suggestions

Feel free to post your suggestions in the [discussions tab](https://github.com/Ahzed11/erlang-ci-cd/discussions/categories/ideas).

## Credits

- These workflows are inspired by [ferd.ca - My favorite Erlang Container](https://ferd.ca/my-favorite-erlang-container.html) and utilize some parts of their implementation from [the dandelion repository](https://github.com/ferd/dandelion).
