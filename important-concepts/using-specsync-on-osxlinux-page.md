# Using SpecSync on OSX/Linux

SpecSync can synchronize any scenarios that are written in Gherkin format. The synchronization tool works on Windows, OSX and Linux.

Regardless of the execution platform, the synchronization settings have to be specified in a configuration file \(`specsync.json`\). Typically the [`remote`](../configuration/configuration-remote.md) and [`local`](../configuration/configuration-local.md) configuration sections have to be specified. Please check the  Please check the [Getting started using Cucumber or other Gherkin-based BDD tool](../getting-started/getting-started-cucumber.md) or the [Configuration](../configuration/) pages for details.

In order to use the SpecSync for Azure DevOps command line tool on OSX or Linux, there are two options: you can run it as a native Linux binary or using Mono.

Native .NET Core executable will be provided soon. Please contact support \(specsync@specsolutions.eu\) for details on the release date.

## 1. Using SpecSync command line tool as a native binary \(on Linux only\)

The command line tool can be used as a native binary on Linux systems \(tested on Ubuntu\) and it does **not require any special dependencies** \(like Mono\) to be installed.

This option can also be used with [**Docker**](https://www.docker.com/), we have tested it successfully on a pure [ubuntu image](https://hub.docker.com/_/ubuntu/). \(See sample `Dockerfile` below.\)

For this option, you have to

1. Download SpecSync from the [downloads page](../downloads.md) \(SpecSync.Mono.VER.zip\) and unzip it to a folder on your system.
2. \(optional\) Set an environment variable pointing to the unzipped `SpecSync.Mono` folder: e.g. `export SPECSYNC_DIR=$HOME/SpecSync.Mono` \(if unzipped in `$HOME`\)
3. Mark `SpecSync4AzureDevOps` as executable: `chmod +x $SPECSYNC_DIR/SpecSync4AzureDevOps`
4. From the project folder, invoke `SpecSync4AzureDevOps` to synchronize feature files. E.g.:

   ```text
   $SPECSYNC_DIR/SpecSync4AzureDevOps push
   ```

{% hint style="warning" %}
In versions prior to 2.1.8, the native library libMonoPosixHelper.so has to be copied to the project folder for the time of the synchronization. With the recent versions this step is not required anymore.
{% endhint %}

### `Dockerfile` for native binary execution

The following `Dockerfile` can be used to setup the synchronization environment using the native image with [Docker](https://www.docker.com/). \(Don't forget to change the version URL to the latest version from [Downloads](../downloads.md).\)

```text
FROM ubuntu
RUN apt-get update && apt-get install -y unzip
RUN wget https://www.specsolutions.eu/media/specsync/SpecSync.AzureDevOps.Mono.2.1.8.zip -O /specsync.zip -q
RUN mkdir /specsync && unzip /specsync.zip -d /specsync
RUN bash -c "chmod +x /specsync/SpecSync.Mono/SpecSync4AzureDevOps"
# Invoke synchronization in this image using
#   /specsync/SpecSync.Mono/SpecSync4AzureDevOps push
```

{% hint style="info" %}
To reduce the Docker image size, you can delete the `SpecSync.Mono/Assemblies` folder with all its contents from the image when using native image execution \(only the `SpecSync4AzureDevOps` executable is needed\).
{% endhint %}

## 2. Using SpecSync command line tool with Mono

If the system has Mono installed, SpecSync can also be invoked through Mono. To install Mono runtime, visit [http://www.mono-project.com/docs/getting-started/install/](http://www.mono-project.com/docs/getting-started/install/).

This option can also be used with [**Docker**](https://www.docker.com/), we have tested it successfully on the [mono image](https://hub.docker.com/_/mono/). \(See sample `Dockerfile` below.\)

For this option, you have to

1. Download SpecSync from the [downloads page](../downloads.md) \(SpecSync.Mono.VER.zip\) and unzip it to a folder on your system.
2. \(optional\) Set an environment variable pointing to the unzipped `SpecSync.Mono` folder: e.g. `export SPECSYNC_DIR=$HOME/SpecSync.Mono` \(if unzipped in `$HOME`\)
3. From the project folder, invoke `$SPECSYNC_DIR/Assemblies/SpecSync4AzureDevOps.exe` to synchronize feature files. E.g.:

   ```text
   mono $SPECSYNC_DIR/Assemblies/SpecSync4AzureDevOps.exe push
   ```

### `Dockerfile` for Mono execution

The following `Dockerfile` can be used to setup the synchronization environment using Mono with [Docker](https://www.docker.com/). \(Don't forget to change the version URL to the latest version from [Downloads](../downloads.md).\)

```text
FROM mono:6
RUN apt-get update && apt-get install -y unzip
RUN wget https://www.specsolutions.eu/media/specsync/SpecSync.AzureDevOps.Mono.2.1.8.zip -O /specsync.zip -q
RUN mkdir /specsync && unzip /specsync.zip -d /specsync
# Invoke synchronization in this image using
#   mono /specsync/SpecSync.Mono/Assemblies/SpecSync4AzureDevOps.exe push
```

{% hint style="info" %}
To reduce the Docker image size, you can delete the `SpecSync.Mono/SpecSync4AzureDevOps` file from the image when using Mono execution \(only the `SpecSync.Mono/Assemblies` folder is needed\).
{% endhint %}

