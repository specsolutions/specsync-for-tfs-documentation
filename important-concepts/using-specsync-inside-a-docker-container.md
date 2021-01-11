# Using SpecSync inside a Docker container

If your development pipeline uses Docker containers it might be also useful to run the SpecSync command line tool from a Docker container. 

When running SpecSync from Docker containers you can choose from multiple options. The table below summarizes these options. The details of the different options can be found below as sub-sections of this guide. Please also check the [general recommendations](using-specsync-inside-a-docker-container.md#general-recommendations) below.

| Option | Description |
| :--- | :--- |
| Use the pre-built official SpecSync Docker image from Docker Hub | **This is the easiest option.** By using the pre-built image you can run SpecSync without installation or maintaining a Dockerfile. You can find the details of this option on the [Install as Docker image](../installation/docker-image.md) page.  |
| Create a derived Docker image from the official SpecSync Docker image | Creating your own Docker image from the official SpecSync Docker image might be useful when accessing your Azure DevOps installation requires **special network configurations** \(e.g. installing a certificate\) or when you would like to **simplify the execution parameters** \(e.g. license file location\). See details and examples [below](using-specsync-inside-a-docker-container.md#create-a-derived-docker-image-from-the-official-specsync-docker-image). |
| Integrate SpecSync into a .NET Core SDK-based image | If you use the Docker container for building or testing .NET Core applications \(i.e. **you have a .NET Core SDK-based image already**\), you can easily integrate SpecSync into that as a [.NET Core Global tool](../installation/dotnet-core-tool.md). See details and examples [below](using-specsync-inside-a-docker-container.md#integrate-specsync-into-a-net-core-sdk-based-image). |
| Integrate SpecSync into a pure Linux image | If **your project is not .NET based**, the most efficient way to include SpecSync into your Docker image is to download the SpecSync [native Linux binaries](../installation/native-binaries.md) to the image. These binaries contain all pre-requisites, so the .NET framework does not need to be installed to the Docker image. See details and examples [below](using-specsync-inside-a-docker-container.md#integrate-specsync-into-a-pure-linux-image). |

## General recommendations

### Mounting vs copying feature files to the container

SpecSync requires to access the feature files and other project and configuration files of your project. In some cases \(when linking new scenarios or pulling changes\) SpecSync also needs to modify the feature files and these modifications have to be preserved \(typically commit and push to Git\). 

Local modifications can be disabled with the `--disableLocalChanges` option, that is useful when the synchronization is done in CI/CD pipeline where the changes cannot be preserved.

The most efficient way to access the local files is to define a mounting point \(volume\) and mount the local project folder when you create the container from the image. This way the changes will be preserved and also the image size can be significantly reduced.

```bash
# define a parametrizable mounting point
ARG LOCAl_DIR=/local
# set it as working folder
WORKDIR ${LOCAl_DIR}
```

The local folders can be mounted to the container when the container is created using the `-v <LOCAL-FOLDER>:<MOUNT-POINT>` argument of the `docker run` command. In the images created for SpecSync, the mounting point is usually the `/local` folder. For example:

```bash
docker run --rm -v C:\MyProject\src\features:/local myspecsyncimage push
```

The alternative would be to use the ADD or COPY command, but that essentially creates a copy of your project codebase _at the time of building the image_. The files copied into the container reduces the reusability of the created image.

With `ADD`/`COPY`, the changes that SpecSync makes on the feature files would be lost, therefore the `--disableLocalChanges` option has to be used.

In the majority of the cases, mounting the local files is a better option, although files that generally remain unchanged \(SpecSync license, configuration file\) might be added to the image using `ADD`/`COPY`.

### Set the working directory to the folder of the feature file set instead of the SpecSync tool folder

The default options of SpecSync assume that the current folder is the root of the feature file set \(SpecFlow project folder or `features` folder of a Cucumber project\). Typically the SpecSync configuration file \(specsync.json\) is located in that folder too. 

Although it is possible to let SpecSync synchronize feature files from other folders as well or find the configuration elsewhere, it is easier if the current working directory is the folder of the feature file set.

If the project folders are mounted to the image to a folder `/local`, the working folder should be the `/local` folder itself or one of its sub-folders.

The following example sets the working folder to the project folder mounting point and sets the default entry point to the SpecSync executable from a different directory \(usually `/specsync`\).

```bash
WORKDIR /local
ENTRYPOINT [ "/specsync/SpecSync4AzureDevOps" ]
```

## Create a derived Docker image from the official SpecSync Docker image

Creating your own Docker image from the official SpecSync Docker image might be useful when accessing your Azure DevOps installation requires special network configurations \(e.g. installing a certificate\) or when you would like to simplify the execution parameters \(e.g. license file location\). 

The official SpecSync Docker images are based on the `ubuntu:latest` image and contain the [native Linux binaries](../installation/native-binaries.md) of SpecSync. Besides that the changes are kept at minimum:

* the `libssl1.1` and `ca-certificates` packages are installed using `apt-get` as these are needed to access the Azure DevOps cloud service.
* the `DOTNET_SYSTEM_GLOBALIZATION_INVARIANT` environment variable is set to `1` to avoid installing globalization dependencies of the Linux distribution \(ICU libs\). \(SpecSync messages are not localized.\)

The official SpecSync Docker image contains the SpecSync binaries in the `/specsync` folder and expects the local repository to be mounted into the `/local` folder \(see [Mounting vs copying feature files to the container](using-specsync-inside-a-docker-container.md#mounting-vs-copying-feature-files-to-the-container)\). The [working folder](using-specsync-inside-a-docker-container.md#set-the-working-directory-to-the-folder-of-the-feature-file-set-instead-of-the-specsync-tool-folder) is set to the `/local` and the default entry point to `/specsync/SpecSync4AzureDevOps`.

You can create derived images from the official SpecSync Docker image and customize it. For example you can

* Install further certificates to access your Azure DevOps installation
* Include base configuration files \(see [Hierarchical configuration files](../features/general-features/hierarchical-configuration-files.md)\)
* Include license file
* Include scripts that specify default parameters for SpecSync \(e.g. the license file location or user [PAT](../features/general-features/tfs-authentication-options.md#vsts-personal-access-tokens)\)

You can find further information on how to use such an image in the [Install as Docker image](../installation/docker-image.md) page.

### Example

The following Dockerfile customizes the official SpecSync Docker image by adding the license file to the image and a shell script that specifies the license file parameter for the SpecSync commands. It also enables logging.

{% code title="Dockerfile" %}
```text
FROM specsolutions/specsync:latest

# Copy SpecSync license file and the utility script to a /shared folder
WORKDIR /shared
COPY specsync.lic .
COPY runspecsync.sh .
RUN chmod +x ./runspecsync.sh

WORKDIR /local

ENTRYPOINT [ "/shared/runspecsync.sh" ]
```
{% endcode %}

{% code title="runspecsync.sh" %}
```bash
#!/bin/bash
/specsync/SpecSync4AzureDevOps $@ --license /shared/specsync.lic --log log.txt
```
{% endcode %}

To run SpecSync from the created image, you need to mount the local repository folder and pass further arguments for SpecSync, like

```bash
docker run --rm -v C:\MyProject\src\features:/local myimage push --tagFilter @foo
```

## Integrate SpecSync into a .NET Core SDK-based image

If you use the Docker container for building or testing .NET Core applications \(i.e. you have a .NET Core SDK-based image already\), you can easily integrate SpecSync into that as a [.NET Core Global tool](../installation/dotnet-core-tool.md).

In this setup the easiest is to install SpecSync into the image as a global .NET Core tool. In the [Install as .NET Core tool](../installation/dotnet-core-tool.md) page we recommend installing SpecSync as a local .NET Core Tool to avoid version conflicts between projects on the same machines, but since the Docker container will be bound to one specific project anyway, you can safely use global tools. To install SpecSync as a global .NET Core Tool in the image, you need to add the following line to the Dockerfile.

```bash
RUN dotnet tool install --global SpecSync.AzureDevOps --version 3.0.2
```

Specifying the version is optional. You can even make your image configurable for different SpecSync versions by specifying the SpecSync version as an argument \(see example below\).

Global .NET Core tools are installed to `/root/.dotnet/tools`, but this folder is not included in the `PATH` by default. You can add this folder to the `PATH` or refer to the SpecSync command line tool as `/root/.dotnet/tools/specsync`.

Just like with the other Docker container options it is recommended to [mount the local folder](using-specsync-inside-a-docker-container.md#mounting-vs-copying-feature-files-to-the-container) instead of copying it. 

You can build the image to be used for interactive development but you can also populate it with scripts to perform complex tasks, e.g. build project and synchronize test cases with SpecSync push command, or run tests and publish test results with SpecSync.

{% hint style="info" %}
If you would like to use the contain only to run SpecSync, it is better to [use the official containers](using-specsync-inside-a-docker-container.md#create-a-derived-docker-image-from-the-official-specsync-docker-image) or [build one on pure Linux](using-specsync-inside-a-docker-container.md#integrate-specsync-into-a-pure-linux-image).
{% endhint %}

### Example

The following Dockerfile creates an image based on the .NET Core SDK to be able to develop the project interactively. 

{% code title="Dockerfile" %}
```text
FROM mcr.microsoft.com/dotnet/core/sdk:3.1

ARG SPECSYNC_VERSION=3.0.2
ARG LOCAl_DIR=/local

RUN dotnet tool install --global SpecSync.AzureDevOps --version ${SPECSYNC_VERSION}
ENV PATH="$PATH:/root/.dotnet/tools"

WORKDIR ${LOCAl_DIR}
```
{% endcode %}

You can build an image and work with that interactively, e.g.:

```bash
> docker run --rm -it -v C:\MyProject\src\features:/local myimage bash
# dotnet build
# specync push --tagFilter @foo
```

## Integrate SpecSync into a pure Linux image

If your project is not .NET based, the most efficient way to include SpecSync into your Docker image is to download the SpecSync [native Linux binaries](../installation/native-binaries.md) to the image. These binaries contain all pre-requisites, so the .NET framework does not need to be installed to the Docker image.

The SpecSync Linux binaries work with most Linux distributions, but we test it with `ubuntu:latest`. The Dockerfile should follow the classic _download package and unzip it_ pattern \(see example below\).

The native Linux binaries of SpecSync by default require the localization packages of the Linux distribution \(ICU libs, e.g. `libicu66` on `ubuntu:20.04`\). This can be avoided by setting the `DOTNET_SYSTEM_GLOBALIZATION_INVARIANT` environment variable to `1`. As SpecSync messages are not localized, this can be done safely.

```bash
ENV DOTNET_SYSTEM_GLOBALIZATION_INVARIANT=1
```

Just like with the other Docker container options it is recommended to [mount the local folder](using-specsync-inside-a-docker-container.md#mounting-vs-copying-feature-files-to-the-container) instead of copying it. 

You can make the image for generic-purpose use \(e.g. for interactive work\), or you can make it specifically for the purpose of executing the SpecSync command line tool using the `ENTRYPOINT` command. In case of the latter, you can consider using the [official SpecSync Docker images](../installation/docker-image.md) or create a [derived image](using-specsync-inside-a-docker-container.md#create-a-derived-docker-image-from-the-official-specsync-docker-image) from them. 

### Example

The following Dockerfile creates an image from `ubuntu:latest`, installs the SpecSync native binaries to it and exposes the SpecSync command line tool as a default entry point.

{% code title="Dockerfile" %}
```text
FROM ubuntu:latest

RUN apt-get update && apt-get install -y \
    wget \
    unzip \
    && rm -rf /var/lib/apt/lists/*

ARG SPECSYNC_VERSION=3.0.2
ARG LOCAl_DIR=/local

WORKDIR /specsync

RUN wget -qO specsync.zip https://www.specsolutions.eu/media/specsync/SpecSync.AzureDevOps.${SPECSYNC_VERSION}-linux-x64.zip \
  && ( unzip -q specsync.zip || [ $? -le 1 ] ) \
  && rm specsync.zip \
  && bash -c "chmod +x ./SpecSync4AzureDevOps"

# As SpecSync output is not culture-dependent, this setting can be used to avoid installing ICU libs (libicu66)
ENV DOTNET_SYSTEM_GLOBALIZATION_INVARIANT=1

WORKDIR ${LOCAl_DIR}

ENTRYPOINT [ "/specsync/SpecSync4AzureDevOps" ]
```
{% endcode %}

To run SpecSync from the created image, you need to mount the local repository folder and pass further arguments for SpecSync, like

```bash
docker run --rm -v C:\MyProject\src\features:/local myimage push --tagFilter @foo
```

{% hint style="info" %}
The pure `ubuntu` image does not contain the packages necessary for HTTPS requests, therefore if your Azure DevOps server needs HTTPS connection, you also need to install the necessary packages. In the example above, we installed the `wget` package that implicitly included these. If you prepare a package that does not need `wget`, you need to add the `libssl1.1` and `ca-certificates` with `apt-get`.
{% endhint %}

