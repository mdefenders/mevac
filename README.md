# mEvac

Simple script to migrate some social networks post dumps to Mastodon

# Introduction

Mastodon doesn't provide any content migration tool. The explanation is - developers wouldn't like to overload the
Fediverse with traffic of imported content. I appreciate this decision, it makes a logical sense. But the migration may
be useful for particular cases. For example, I would like to move a historical archive of my content from FB&X to my
own, self-hosted Mastodon instance based dedicated accounts.

# Features

- built as docker image
- FB posts migration
- FB posts timestamping and auto-threading

# Backlog

- X migration
- Mastodon migration

# Requirements

- docker installed
- mastodon account and access token (may be created using Mastodon UI)
- Downloaded FB backup (archive)

# Variables / parameters

The default script behavior maybe configured using environment variables. Variables without defaults values are prompted

| Env var                      |        Default value |   
|:-----------------------------|---------------------:|
| LOGLEVEL                     |                 INFO |
| MASTODON_DOMAIN              |                    - |
| MASTODON_RATELIMIT_RETRIES   |                    3 |
| MASTODON_CLIENT_ACCESS_TOKEN |                    - |
| MASTODON_TEXT_SIZE_LIMIT     |                  500 |
| MASTODON_WORK_DIR            |                   ./ |
| MASTODON_PUSH_PUBLIC         |                    0 |
| DB_FILE                      | /app/db/evacuator.db |

## Docker container commands

You need to run commands to load archive int internal db and push it to Mastodon
Internal db is used to provide the command reentrancy. You can run the command multiple times in case of errors or
failures and avoid duplicates. Removing the db file will reset the process.

For FB the post timestamp is used as a unique key.

| Command                  | Description                            |   
|:-------------------------|:---------------------------------------|
| load facebook            | loads FB archive int internal database |
| push facebook            | pushes FB archive to Mastodon          |
| load report, push report | prints the current process state       |

# Usage examples

```shell
docker run --rm -ti -v <path to fb backup posts folder>/posts:/app/posts -v <path to loacal db folder>:/app/db mdefenders/mevac:latest command
```

```shell
docker run --rm -ti -v ./tests/testdata/posts:/app/posts -v ./db:/app/db mdefenders/mevac:latest push report
```

```shell
docker run --rm -ti -v ./tests/testdata/posts:/app/posts -v ./db:/app/db mdefenders/mevac:latest push facebook
```