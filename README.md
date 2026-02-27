# Tekton Workshop

A hands-on workshop that teaches the basic concepts of OpenShift Pipelines and Tekton.
The content is built with Antora and published as a static site using the Red Hat Demo Platform (Showroom) UI theme.

## Overview

| | |
|---|---|
| **Audience Experience Level** | Beginner to Intermediate |
| **Average Time to Complete** | 2 - 2.5 hours |

---

## Local development

### Preview with Antora viewer (recommended)

To preview the workshop content locally, run the Antora viewer container:

```sh
podman run --rm --name antora -v $PWD:/antora -p 8080:8080 -i -t ghcr.io/juliaaano/antora-viewer
```

For SELinux environments, append `:z` to the volume mount:

```sh
podman run --rm --name antora -v $PWD:/antora:z -p 8080:8080 -i -t ghcr.io/juliaaano/antora-viewer
```

Then open http://localhost:8080 in your browser. When you make changes to the content, stop the container and run it again to see updates. Live-reload is not supported.

## Documentation

This workshop is based on [Antora](https://antora.org/) and [Red Hat scholars template](https://github.com/redhat-scholars/courseware-template) to build HTML based tutorials.

To run this workshop in your own environment, follow the step by step tutorial link for your cluster available below:

https://bmeklund.github.io/tekton-workshop/
