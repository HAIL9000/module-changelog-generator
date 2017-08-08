# Puppet Module Changelog Generator

This is a tool which uses the [github-changelog-generator](https://github.com/skywinder/github-changelog-generator) gem
to generated changelogs for Puppet modules. This tool providers a rake task which handles the configuration needed to
use the gem for a module.

## Installation

To install this tool, simply clone this repo and run a bundle install to get the gems you need:

```
  cd /path/to/module-changelog-generator
  bundle install
```

## Usage

At it's most simple, generating a changelog is just a matter of running this command from within the project directory:

```
  bundle exec rake changelog
```

However there is some configuration that must be handled first.

The following environment variables are supported:

`MODULE_PATH`: The module path is one of the two things that must be specified in order to generate a changelog and is
the full path to the root directory of the module you would like to generate a changelog for. It can either be set as
an environment variable or passed in as the first argument of the Rake task.

`LAST_TAG`: The last tag in the second thing that must be specified. This is the git tag where you want to begin the
generation of your changelog from. So for example if you were doing a '4.0.2' release you would want to specify your last
tag as '4.0.1'. Similiar to MODULE_PATH, it can either be set as an environment variable or passed in as the second
argument of the Rake task.

`CHANGELOG_GITHUB_TOKEN`: Because the changelog generator needs to make a lot of requests via the GitHub API, you will
want to authenticate with a [personal access token](https://github.com/blog/1509-personal-api-tokens). Once you have one
generated, set it via the environment varible ant the app will discover it. Technically it does not need to be set but
chances are you will run out of API requests without a token.

`GITHUB PROJECT` (optional): This is an environment variable that can be set to define the name of the project (e.g.
puppetlabs-concat). If it is not set then it will be extracted from the metadata.

`GITHUB_USER` (optional): This is the namespace that the repository lives under (for example puppetlabs/puppetlabs-concat).
It can be set with the environment variable but will be set to 'puppetlabs' if not specified.


If you don't want to set environment variables for the module path and last tag, or if you want to override the ones that
you've set, then you can call the rake task like so:

```
  bundle exec rake changelog Users/username/puppetlabs-concat 4.0.1
```

## Pull Request Labels

In order for the changelog to be generated in a way thats readable and helpful, each pull request must be labled
appropriately with one of the following:

- `bug`: this label should be applied to pull requests that fix an existing bug
- `enhancement`: this label should be applied to pull requests that add a new feature or enhancement

Pull requests with the following labels will be ignored when the changelog is generated:

- `duplicate`
- `question`
- `invalid`
- `wontfix`
- `wont-fix`
- `modulesync`


## Workflow

 1. Ensure all PR titles accurately describe the work
 2. Ensure all PRs are labeled appropriately
 3. Bump the version in the metadata to the version you're planning to release
 4. Ensure that your environment variables are set appropriately
 5. Run the Rake task to generate the new changelog
 6. Open the usual release PR with metadata and changelog updates

