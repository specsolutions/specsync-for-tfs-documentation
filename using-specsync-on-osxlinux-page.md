# Using SpecSync on OSX/Linux

SpecSync can synchronize any scenarios that are written in Gherkin format. The synchronization tool works on Windows, OSX and Linux. \(The OSX and Linux support has been introduced in v1.4.\)

In order to use the SpecSync for TFS command line tool on OSX or Linux, there are two options.

## 1. Using SpecSync command line tool as a native binary \(on Linux only\)

The command line tool can be used as a native binary on Linux systems \(tested on Ubuntu\) and it does not require any special dependencies \(like Mono\) to be installed.

This option can also be used with Docker, we have tested it successfully on a pure [ubuntu image](https://hub.docker.com/_/ubuntu/).

For this option, you have to

1. Download SpecSync from the [downloads page](downloads.md) \(SpecSync.Mono.VER.zip\) and unzip it to a folder on your system.
2. \(optional\) Set an environment variable pointing to the unzipped `SpecSync.Mono` folder: e.g. `export SPECSYNC_DIR=$HOME/SpecSync.Mono` \(if unzipped in `$HOME`\)
3. For non-SpecFlow projects, use the `local/featureFileSource` configuration setting setting to specify the feature files to be synchronized \(see more at [`local` Configuration](configuration/configuration-local.md)\), e.g.
  ```
  {
    ...
    "local": {
      "featureFileSource": {
        "type": "stdIn"
      }
    },
    ...
  }
  ```
4. Mark `SpecSync4TFS` as executable: `chmod +x $SPECSYNC_DIR/SpecSync4TFS`
5. Copy the file `libMonoPosixHelper.so` from the SpecSync folder to the project folder \(this is a temporary limitation\): `cp $SPECSYNC_DIR/libMonoPosixHelper.so .`
6. From the project folder, invoke `SpecSync4TFS` to synchronize feature files. E.g. if you have used the `stdIn` setting, the list of feature files can be provided with pipping the commands:
  ```
  find Features/ -name *.feature | $SPECSYNC_DIR/SpecSync4TFS push
  ```

A sample script performing steps 5-6 can also be found in the zip file: `specsync4tfs_sample.sh`, similar to the following.

```
#!/bin/bash
SPECSYNC_DIR=$HOME/SpecSync.Mono

echo Copy libMonoPosixHelper.so to the current folder
cp $SPECSYNC_DIR/libMonoPosixHelper.so .

echo Synchronize feature files within "Features" folder
find Features/ -name *.feature | $SPECSYNC_DIR/SpecSync4TFS push

echo Delete libMonoPosixHelper.so from the current folder
rm -f libMonoPosixHelper.so
```

## 2. Using SpecSync command line tool with Mono

If the system has Mono installed, SpecSync can also be invoked through Mono. To install Mono runtime, visit [http://www.mono-project.com/docs/getting-started/install/](http://www.mono-project.com/docs/getting-started/install/).

This option can also be used with Docker, we have tested it successfully on the [mono image](https://hub.docker.com/_/mono/).

For this option, you have to

1. Download SpecSync from the [downloads page](downloads.md) \(SpecSync.Mono.VER.zip\) and unzip it to a folder on your system.
2. \(optional\) Set an environment variable pointing to the unzipped `SpecSync.Mono` folder: e.g. `export SPECSYNC_DIR=$HOME/SpecSync.Mono` \(if unzipped in `$HOME`\)
3. For non-SpecFlow projects, use the `local/featureFileSource` configuration setting setting to specify the feature files to be synchronized \(see more at [`local` Configuration](configuration/configuration-local.md)\), e.g.
  ```
  {
    ...
    "local": {
      "featureFileSource": {
        "type": "stdIn"
      }
    },
    ...
  }
  ```
4. From the project folder, invoke `$SPECSYNC_DIR/Assemblies/SpecSync4TFS.exe` to synchronize feature files. E.g. if you have used the `stdIn` setting, the list of feature files can be provided with pipping the commands:
  ```
  find Features/ -name *.feature | $SPECSYNC_DIR/Assemblies/SpecSync4TFS.exe push
  ```

A sample script performing step 4 can be similar to the following.

```
#!/bin/bash
SPECSYNC_DIR=$HOME/SpecSync.Mono

echo Synchronize feature files within "Features" folder
find Features/ -name *.feature | mono $SPECSYNC_DIR/Assemblies/SpecSync4TFS.exe push
```



