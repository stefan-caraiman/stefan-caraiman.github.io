---
title: "Gitlab-CI tips and tricks"
date: 2020-02-19T15:15:33-04:00
categories:
  - blog
tags:
  - blog
---

In this blogpost I'll go over a few more advanced topics related to Gitlab-CI and how to structure pipelines and properly reuse them.

Although some of these tips are specific to Gitlab, some can be easily applied to other CI systems which also use YAML for workflow definition.

## Using `extends`

`extends` is a great way to reuse some YAML parts in multiple places, for example:

```yml
.image_template:
  image:
    name: centos:latest

test:
  extends: .image_template
  script:
    - echo "Testing"

deploy:
  extends: .image_template
  script:
    - echo "Deploying"
```

We are defining a "hidden" key at the top of the file, which we are then reusing across our different stages by making use of the `extends` functionality. Of course this can be extended to keys/stanzas which are a lot more complex, in order to reduce complexity and make the pipeline definition more readable.

## Using `include`

`include` just as the name suggests, is used to include other CI YAML file definitions into the current pipeline. The file can either be a local one or a remote one from an another repository.

* `base-testing.yml` (file found in the `ci-templates` repository)

```yml
stages:
  - test

variables:
  ENV: "none"

test:
  script:
    - echo "Testing in ${ENV}"
```

* `.gitlab-ci.yml` (file found in our main repository)

```yml
include: 'https://my.gitlab/ci-templates/raw/master/base-testing.yml'

variables:
  ENV: "dev"

```

What we will achieve with this, is that we will be able to reuse the `base-testing.yml` file defined in our central repository of CI templates, and we simply overwrite any variables with their needed value after we `include` it. Pretty neat, huh?

# YAML tips and tricks

Recently I came across this thing called `anchors` and I think it simplifies many of the common CI/CD YAML sections by a lot, helping with keeping your CI/CD definition DRY(Dont Repeat Yourself).
Anchors basically define YAML references that can be used elsewhere.

```yml
.job_template: &job_definition  # Hidden key that defines an anchor named 'job_definition'
  image: ruby:2.6
  services:
    - postgres
    - redis

test1:
  <<: *job_definition           # Merge the contents of the 'job_definition' alias
  script:
    - test1 project

test2:
  <<: *job_definition           # Merge the contents of the 'job_definition' alias
  script:
    - test2 project

```

You can read more about this on the official [gitlab][gitlab] documentation page about CI/CD.

### Note for the reader

You can’t use YAML anchors across multiple files when leveraging the include feature. Anchors are only valid within the file they were defined in.

# Conclusion

There are many ways through which you can simplify your YAMLs, be it default capabilities of YAML or custom built features offered by the Gitlab CI/CD complex system.

The only golden rule that you should remind yourself at all times, is that most probably there is another way to simply something and make it more readable, so always try to read the docs and code of others in order to find such ways.

And as always, happy learning!

[gitlab]: https://docs.gitlab.com/ee/ci/yaml/#anchors
