= Tekton Workshop

A hands-on workshop that teaches OpenShift Pipelines and Tekton.


The content is built with Antora and published as a static site using the Red Hat Demo Platform (Showroom) UI theme.


== Local development

=== Preview with Antora viewer (recommended)

To preview the workshop content locally, run the Antora viewer container:

[source,sh]
----
podman run --rm --name antora -v $PWD:/antora -p 8080:8080 -i -t ghcr.io/juliaaano/antora-viewer
----

For SELinux environments, append `:z` to the volume mount:

[source,sh]
----
podman run --rm --name antora -v $PWD:/antora:z -p 8080:8080 -i -t ghcr.io/juliaaano/antora-viewer
----

Then open http://localhost:8080 in your browser. When you make changes to the content, stop the container and run it again to see updates. Live-reload is not supported.

