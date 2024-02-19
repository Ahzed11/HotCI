# Erlang CI/CD template

Run unit tests, hot code upgrade tests and build releases thanks to Github actions.

## Configuration

In your [repository variables](https://docs.github.com/en/actions/learn-github-actions/variables#creating-configuration-variables-for-a-repository), set those variables up.

- `RELNAME`: Name of the release

Also, in `erlang/rebar.config`, replace `release_name` with the name of your release.

## Versioning

The project versioning scheme that is used is called `Smoothver`. It has been crafted for OTP projects and is described in [this blog post](https://ferd.ca/my-favorite-erlang-container.html).

> Given a version number RESTART.RELUP.RELOAD, increment the:
>
> - RESTART version when you make a change that requires the server to be rebooted.
> - RELUP version when you make a change that requires pausing workers and migrating state.
> - RELOAD version when you make a change that requires reloading modules with no other transformation.

*Citation from* [ferd.ca - My favorite Erlang Container](https://ferd.ca/my-favorite-erlang-container.html)

## Updating your project

To import the changes made to the *Erlang CI/CD template* into your project, use
[*template-sync*](https://github.com/coopTilleuls/template-sync):

1. Run the script to synchronize your project with the latest version of the skeleton:

    ```console
    curl -sSL https://raw.githubusercontent.com/mano-lis/template-sync/main/template-sync.sh | sh -s -- https://github.com/ahzed11/erlang-ci-cd
    ```

2. Resolve conflicts, if any
3. Run `git cherry-pick --continue`

*This section has been adapted from* [symfony-docker](https://github.com/dunglas/symfony-docker/blob/main/docs/updating.md)

## Create a release

Simply add a git tag and the `publish-tarball` workflow will build and upload a packaged release, create a Github release and link the two together.

## Credits

- These workflows are highly inspired by those described in [ferd.ca - My favorite Erlang Container](https://ferd.ca/my-favorite-erlang-container.html) and their implementation in [the dandelion repository](https://github.com/ferd/dandelion).
