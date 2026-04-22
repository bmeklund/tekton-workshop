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

## Workshop basics setup

### Build and push the lab-guide container image to a repository

The Containerfile builds a UBI9-based container with Apache httpd that serves the pre-built workshop site on port 8080. The container runs as a non-root user and is suitable for deployment on OpenShift.

To build the site content first, run the Antora viewer as described above, which generates output in the www/ directory.

Then use the *build-push-image.sh* script to build a multi-architecture image (amd64 and arm64) and push it to *Quay.io*:

```sh
export QUAY_USER=your-quay-username
./build-push-container.sh
```

The script builds the image as **quay.io/$QUAY_USER/tekton-tutorial:latest** and pushes it to the registry.

### To install the needed components(operators and instances) run the below commands.

#### As Cluster admin

1. Install the OpenShift GitOps Operator

```sh
oc apply -f https://raw.githubusercontent.com/bmeklund/tekton-workshop/refs/heads/main/argocd/argo-cd.yaml
```

2. When OpenShift GitOps is installed and ready - Install the workshops needed components(operators and instances)

```sh
oc apply -f https://raw.githubusercontent.com/bmeklund/tekton-workshop/refs/heads/main/argocd/base/workshop-base.yaml
```

3. When OpenShift Pipelines is installed - the Pipelines console-plugin needs to be enabled - otherwise the clustertasks from the OpenShift Pipelines operator won't be accessible from user projects. The below action will enable Red Hat tasks to be used in user projects.

```sh
Go to Ecosystem > Installed operators > OpenShift Pipelines operator. In the overview page on the right hand side click on the link showing piplines-console-plugin: Disabled below the Console-plugin header. Click Enable then Save in the pop-up. 
```

### To install the user distribution application for this workshop.

####

We're using the user-distribution app from the following repo: https://github.com/mostmark/user-distribution

To install it we use gitops and ArgoCD from the user-distribution-gitops repository: https://github.com/mostmark/user-distribution-gitops
Follow the guide from the repository. More or less the following:

##### To install in your OpenShift cluster

1. Clone the repository

```sh
git clone https://github.com/mostmark/user-distribution-gitops.git && cd user-distribution-gitops
```

2. Update manifests/secret.yaml and edit the credentials

3. Create the resources

```
oc new-project portal
oc create -f manifests/
```

4. Get the url to the admin interface

```
echo "https://$(oc get route user-distribution -o jsonpath='{.spec.host}')/admin"
```

### Cleanup / Troubleshooting

To clean up the cluster of gitops managed components run the below command

```sh
oc delete applicationset workshop-base -n openshift-gitops
```

```sh
oc delete project portal
```
