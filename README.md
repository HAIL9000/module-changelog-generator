# Puppet Module Changelog Generator

This is a tool which uses the [github-changelog-generator](https://github.com/skywinder/github-changelog-generator) gem
to generated changelogs for Puppet modules. This tool provides a Rake task which handles the configuration needed to use the gem for a module.

## Installation

To install this tool, clone this repo and run a bundle install to get the gems you need:

```
  cd /path/to/module-changelog-generator
  bundle install
```

## Usage

At its most simple, generating a changelog is just a matter of running this command from within the project directory:

```
  bundle exec rake changelog
```

However, you need to do some configuration first.

The following environment variables are supported:

`MODULE_PATH`: The module path is one of the two things that must be specified in order to generate a changelog. Specify the full path to the root directory of the module you want to generate a changelog for. It can either be set as
an environment variable or passed in as the first argument of the Rake task.

`LAST_TAG`: The last tag in the second thing that must be specified. This is the git tag from which you want to begin the
changelog generation. For example, if you were doing a '4.0.2' release, you would want to specify your last
tag as '4.0.1'. Similiar to MODULE_PATH, this can either be set as an environment variable or passed in as the second argument of the Rake task.

`CHANGELOG_GITHUB_TOKEN`: Because the changelog generator needs to make a lot of requests via the GitHub API, you will
want to authenticate with a [personal access token](https://github.com/blog/1509-personal-api-tokens). Once you have one
generated, set it via the environment varible and the app will discover it. Technically, you don't have to set this, but if you don't, you will probably run out of API requests.

`GITHUB PROJECT` (optional): This is an environment variable that you can set to define the name of the project (for example, puppetlabs-concat). If it is not set, then the name is extracted from the metadata.

`GITHUB_USER` (optional): This is the namespace that the repository lives under (for example, puppetlabs/puppetlabs-concat).
It can be set with the environment variable, but if not specified, it defaults to 'puppetlabs'.


If you don't want to set environment variables for the module path and last tag, or if you want to override the ones that
you've set, then you can call the Rake task like so:

```
  bundle exec rake changelog Users/username/puppetlabs-concat 4.0.1
```

## Pull Request Labels

For the changelog to be generated in a way that's readable and helpful, each pull request must be labeled appropriately with one of the following:

- `bug`: this label should be applied to pull requests that fix an existing bug
- `enhancement`: this label should be applied to pull requests that add a new feature or enhancement

Pull requests with the following labels are ignored when the changelog is generated:

- `duplicate`
- `question`
- `invalid`
- `wontfix`
- `wont-fix`
- `modulesync`


## Workflow

 1. Ensure all PR titles accurately describe the work.
 2. Ensure all PRs are labeled appropriately.
 3. Bump the version in the metadata to the version you're planning to release.
 4. Ensure that your environment variables are set appropriately.
 5. Run the Rake task to generate the new changelog.
 6. Open the usual release PR with metadata and changelog updates.

