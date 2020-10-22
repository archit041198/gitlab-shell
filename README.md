# GitLab Shell

## GitLab Shell handles git SSH sessions for GitLab

GitLab Shell handles git SSH sessions for GitLab and modifies the list of authorized keys.
GitLab Shell is not a Unix shell nor a replacement for Bash or Zsh.

When you access the GitLab server over SSH, then GitLab Shell will:

1. Limits you to predefined git commands (git push, git pull).
1. Call the GitLab Rails API to check if you are authorized, and what Gitaly server your repository is on
1. Copy data back and forth between the SSH client and the Gitaly server

If you access a GitLab server over HTTP(S), you end up in [gitlab-workhorse](https://gitlab.com/gitlab-org/gitlab-workhorse).

An overview of the four cases described above:

1. git pull over SSH -> gitlab-shell -> API call to gitlab-rails (Authorization) -> accept or decline -> establish Gitaly session
1. git push over SSH -> gitlab-shell (git command is not executed yet) -> establish Gitaly session -> (in Gitaly) gitlab-shell pre-receive hook -> API call to gitlab-rails (authorization) -> accept or decline push

## Code status

[![pipeline status](https://gitlab.com/gitlab-org/gitlab-shell/badges/master/pipeline.svg)](https://gitlab.com/gitlab-org/gitlab-shell/-/pipelines?ref=master)
[![coverage report](https://gitlab.com/gitlab-org/gitlab-shell/badges/master/coverage.svg)](https://gitlab.com/gitlab-org/gitlab-shell/-/pipelines?ref=master)
[![Code Climate](https://codeclimate.com/github/gitlabhq/gitlab-shell.svg)](https://codeclimate.com/github/gitlabhq/gitlab-shell)

## Requirements

GitLab Shell is written in Go and needs a Go compiler to build. It still requires
Ruby to build and test, but not to run.

Download and install the current version of Go from https://golang.org/dl/

## Setup

    make setup

## Check

Checks if you can reach GitLab API access and Redis via internal API:

    make check

## Testing

Run tests:

    bundle install
    make test

Run gofmt and rubocop:

    bundle install
    make verify

Run both test and verify (the default Makefile target):

    bundle install
    make validate

## Git LFS remark

Starting with GitLab 8.12, GitLab supports Git LFS authentication through SSH.

## Releasing a new version

Git tags version GitLab Shell, and the version used by the Rails
application is store in
[`GITLAB_SHELL_VERSION`](https://gitlab.com/gitlab-org/gitlab-ce/blob/master/GITLAB_SHELL_VERSION).

For each version, there is a raw version and a tag version:

- The **raw version** is the version number. For instance, `15.2.8`.
- The **tag version** is the raw version prefixed with `v`. For instance, `v15.2.8`.

To release a new version of GitLab Shell and have that version available to the
Rails application:

1. Create a merge request to update the [`CHANGELOG`](CHANGELOG) with the
   **tag version** and the [`VERSION`](VERSION) file with the **raw version**.
2. Ask a maintainer to review and merge the merge request. If you are already a
   maintainer, second maintainer review is not required.
3. Add a new git tag with the **tag version**.
4. Update `GITLAB_SHELL_VERSION` in the Rails application to the **raw version**. (Note: we can do this as a separate MR to that, or in and MR
   that will make use of the latest GitLab Shell changes.)

## Contributing

See [CONTRIBUTING.md](./CONTRIBUTING.md).

## License

See [LICENSE](./LICENSE).
