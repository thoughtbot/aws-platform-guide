# AWS Platform Guide

thoughtbot has developed a platform for deploying applications to AWS. This
guide documents our approach and provides help for both operators deploying and
administrating the platform and for developers deploying applications to the
platform.

This repository it the working area for migrating the platform guide from
[Confluence].

[Confluence]: https://thoughtbot.atlassian.net/wiki/spaces/APG/overview

## Build with paperback

Set up [paperback]. Then, run the build command from the project root:

```
podman run --volume $PWD:/src localhost/paperback build
```

[paperback]: https://github.com/thoughtbot/paperback
