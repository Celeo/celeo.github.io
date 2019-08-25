---
title: Building Rust on CircleCI
date: 2019-08-24 17:22:17
tags:
- rust
- programming
---

How to use [CircleCI](https://circleci.com/) to build, test, and report on your [Rust](https://www.rust-lang.org/) project.

<!-- more -->

## Quick starts

If you just want the quick copy-paste info, here you go.

Create a file in your project at `./.circleci/config.yml`, and populate it with either of these options:

### Simple build and test

```yml
version: 2

jobs:
  build:
    docker:
      - image: rust:latest
    environment:
      TZ: "/usr/share/zoneinfo/your/location"
    steps:
      - checkout
      - restore_cache:
          key: project-cache
      - run:
          name: Check formatting
          command: cargo fmt -- --check
      - run:
          name: Stable Build
          command: cargo build
      - run:
          name: Test
          command: cargo test
      - save_cache:
          key: project-cache
          paths:
            - "~/.cargo"
            - "./target"
```

Thanks to [abronan.com](https://abronan.com/building-a-rust-project-on-circleci/) for getting me started.

### Build, test, and calculate and report coverage to CodeCov

First sign into [CodeCov](https://codecov.io), import your repo, copy the token, and put it into your CircleCI environment variables.

```yml
version: 2

jobs:
  build:
    machine: true
    steps:
      - checkout
      - run:
          name: Pull xd009642/tarpaulin
          command: docker pull xd009642/tarpaulin:latest
      - run:
          name: Generate report
          command: >
            docker run --security-opt seccomp=unconfined -e CODECOV_TOKEN=${CODECOV_TOKEN}
            -v $PWD:/volume xd009642/tarpaulin cargo tarpaulin -v --ciserver circle-ci
            --out Xml --all-features
      - run:
          name: Upload
          command: bash <(curl -s https://codecov.io/bash) -Z -f cobertura.xml

```

## Explanations

The first configuration uses the [official Rust image](https://hub.docker.com/_/rust) to build run the Cargo commands you're almost certainly familiar with: formatting check, build, and test.

The second configuration uses a Docker image provided by the [tarpaulin](https://github.com/xd009642/tarpaulin) project, which is the simplest way I've found to get test coverage for Rust projects. This image builds on top of the official Rust image, so you won't be lacking anything you were expecting to be there. This CI configuration makes use of the [machine flag](https://circleci.com/docs/2.0/executor-types/#using-machine) to gain more control over the build which is required because the tarpaulin tool won't run inside a Docker container without the additional run flags for the security configuration. Although the configuration is only supposed to be generating the test coverage, it has to build the project and run all the tests to do so, so there are fewer steps. After the tarpaulin tool runs and writes the results to a file, the [CodeCov bash script](https://docs.codecov.io/docs/about-the-codecov-bash-uploader) is used to send the data off to CodeCov.

## Notes

I started setting up my CircleCI configuration with the info from the article linked below the first yml block, but had to tweak it for a few things in order to get it to work. When I started working on getting my coverage to display, I looked at using [kcov](https://docs.codecov.io/docs/about-the-codecov-bash-uploader) and [grcov](https://github.com/mozilla/grcov) to get the coverage data, but neither were particularly easy to setup and get running, so I stuck with tarpaulin, which I've used before. Tarpaulin, however, isn't perfect, as it segfaults often when running on my system (though I haven't had that problem when running in the Docker image on CircleCI, so it's probably some environment weirdness or something on my machine).

I also started to go down the path of using CircleCI's [workflows](https://circleci.com/docs/2.0/workflows/) feature to run multiple jobs in parallel - one to build/test like the first snippet, and the other to do coverage like the second snippet - but then realized that I didn't need both jobs, as the second did everything that the first was doing already. If you don't need/want coverage data, however, the first configuration is simpler, should be more familiar to you, and runs much faster than the second.

Regardless of which setup you go with, be sure to include CircleCI's build badge in your project's README, which you can get from your project's settings page on CircleCI (go to your project's build logs, then click the settings cog in the upper-right, then click on "Status Badges" under "Notifications" on the left nav menu). If you're using the test coverage approach, you can use CodeCov's badge as well (go to your project's overview, then click on the settings tab on the top, then click on "Badge" on the left nav menu).

For any Rust project that's been deployed to [crates.io](https://crates.io), you can use the badges from [shields.io](https://shields.io) (type in "rust" into the search bar).
