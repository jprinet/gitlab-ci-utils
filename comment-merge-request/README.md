## Description

Automatically comment Gradle build link when a merge request build is triggered.
The build scan link is parsed from the console log.
An enriched mode allow to comment as well some advanced build information (see screenshots).

## Prerequisites
- The build is triggered from a merge request
- A build scan is published to Gradle Enterprise
- _jq_ is available on the runner
- the _before_script_ is not overriden

## Note
4 HTTP calls will be triggered on each build:
- 1X HTTP GET: Fetch build log from Gitlab API
- 1X HTTP GET: Fetch build data from Gradle Enterprise API (Enriched mode only)
- 1X HTTP GET: Fetch build detailed data from Gradle Enterprise API (Enriched mode only)
- 1X HTTP POST: Post merge request comment on Gitlab API

## Screenshots

- Classic mode
![Classic mode](classic.png)

- Enriched mode
![Advanced mode](enriched.png)

## Inputs
- _GITLAB_API_TOKEN_: access token to Gitlab API
- _GRADLE_ENTERPRISE_URL_: Gradle Enterprise URL
- _GRADLE_ENTERPRISE_API_TOKEN_: access token to Gradle Enterprise API

_GRADLE_ENTERPRISE_URL_ and _GRADLE_ENTERPRISE_API_TOKEN_ parameters can be omitted to comment the build scan link but not the advanced build attributes.

## Usage
```
[...]

include:
  - 'comment-merge-request-with-build-scan.yml'

[...]

build-job:
  stage: build
  script:
    - ./gradlew build
    - comment_merge_request ${GITLAB_API_TOKEN} ${GE_URL} ${GE_API_TOKEN}
```