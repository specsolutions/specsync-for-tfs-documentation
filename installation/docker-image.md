# Install as Docker image

SpecSync can also be used with Docker. There are pre-built official Docker images available at [https://hub.docker.com/r/specsolutions/specsync](https://hub.docker.com/r/specsolutions/specsync) that are ready-to-use for synchronizing scenarios or publishing test results.

{% hint style="info" %}
You can also install SpecSync to your own Docker container. This page describes the usage of the official SpecSync Docker images. For using SpecSync on your own container, please check [Using SpecSync inside a Docker container](../important-concepts/using-specsync-inside-a-docker-container.md). 
{% endhint %}

Using SpecSync through the official Docker images does not require special installation. Upon the first usage Docker will pull the necessary package and execute SpecSync through it.

The following command displays SpecSync version using the official Docker image tagged with `latest`. See the list of available tags below.

```text
docker run --rm specsolutions/specsync version
```

Every command or option you provide after the image name \(`version` in the example above\) will be passed to the passed to the SpecSync command line tool according to the [Command line reference](../reference/command-line-reference/). 

{% hint style="info" %}
The `--rm` Docker option removes the temporary execution data gathered by Docker. In case of SpecSync this generated data is minimal though.
{% endhint %}

The local folders containing the feature file sets have to be mounted to the container to the `/local` container path. This way the potential changes SpecSync performs on the feature files \(e.g. when linking new scenarios or at pull\) are preserved in your local repository. To mount the local repository, you have to use the `-v <LOCAL-FOLDER>:/local` option for the `docker run` command. The container is set to use the `/local` folder as default working directory.

For example to perform a push command for the local project at `C:\MyProject\src\features`you should invoke 

```text
docker run --rm -v C:\MyProject\src\features:/local specsolutions/specsync push
```

The push command requires the local feature set to be configured. Check the [Setup and Configure](setup-and-configure.md) page for the details on how to setup an initial configuration and getting ready for the first synchronization.

### Provided Docker image tags

The following tags are provided for the official SpecSync Docker images on Docker Hub. To see all available tags, please check [https://hub.docker.com/r/specsolutions/specsync/tags](https://hub.docker.com/r/specsolutions/specsync/tags).

{% hint style="warning" %}
Docker caches images locally, therefore if you use a floating SpecSync image tag \(see below\), Docker might use an outdated version. You can ensure the latest version by invoking the `docker pull <IMAGE>:<TAG>` command.
{% endhint %}

| Tag type | Floating | Example | Description |
| :--- | :--- | :--- | :--- |
| Latest released version | yes | latest | Points to the latest released SpecSync version image. In a case of a new major release using the latest tag might cause breakages, therefore it is recommended to use this for evaluation and experimenting only. |
| Up-to-date version line | yes | 3.0 | Points to the latest patch version of a version line. Recommended for production setup. |
| Specific release | no | 3.0.1 | Points to a specific patch release within a version line. This tag always points to the same image therefore it offers the most predictability.  |
| Specific preliminary release | no | 3.1.0-pre20200903-2 | Points to a specific preliminary release. The preliminary releases are less stable therefore should only be used to validate fixes or improvements. |

