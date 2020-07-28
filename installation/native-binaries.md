# Install as native binaries for Linux or MacOS

{% hint style="info" %}
This install method is available for Linux and MacOS machines. Other [installation options](./) available.
{% endhint %}

In case installing the .NET Core framework is not possible, SpecSync can be downloaded as native binaries for Linux and MacOS. The native binaries are precompiled with all dependencies, so no special prerequisite is necessary for the execution. Because of the dependencies, the download package is larger, approximately 35 MB \(the SpecSync .NET Core Tool package is 5 MB\).

The precompiled binaries work with most Linux distributions and MacOS versions. If your system is not supported, please [contact support](../contact/specsync-support.md).

### Step 1: Download and extract files

The SpecSync native binaries can be downloaded directly as a zip file from our website. The download URLs can be found on the [Downloads](../downloads.md) page.

### Step 2: Check/set executable permissions

After extracting the package, the `SpecSync4AzureDevOps` executable can be found in root folder of the zip file. Depending on your operating system you might need to set execution permissions to the file with

```bash
chmod +x SpecSync4AzureDevOps
```

### Step 3: Verify installation

You can test the installation by checking the installed SpecSync version. Open a bash shell from your project folder and invoke the following command.

```bash
<EXTRACTED-SPECSYNC-FOLDER>/SpecSync4AzureDevOps version
```

If the correct version number is displayed, you are ready to move on to setup and configure SpecSync for the first synchronization. Check the [Setup and Configure](setup-and-configure.md) page for the details.

