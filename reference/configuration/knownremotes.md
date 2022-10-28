# knownRemotes

This configuration section can be used to specify the connection and authentication details of Azure DevOps projects that are used by multiple synchronization projects. Specifying `knownRemotes` section can be specified in parent or user-specific configuration files to avoid repeating the same settings or including credentials to the project repository. See [Hierarchical configuration files](../../features/general-features/hierarchical-configuration-files.md) for details.

The following example shows a usage of this section.

```javascript
{
  ...
  "knownRemotes": [
    {
      "projectUrl": "https://dev.azure.com/myorg/*",
      "user": "g2x.....................ac2vx57i4a"
    },
    {
      "projectUrl": "https://dev.azure.com/otherorg/OtherProject",
      "user": "y6s.....................ksuc7tsts"
    }
  ],
  ...
}
```

## Settings

The `knownRemotes` section specify an array of remote configurations. The possible values are the same as described in the [remote section](configuration-remote.md), with the exception that for `knownRemotes` the `projectUrl` can contain a leading asterisk \(`*`\) to be able to match the settings to all projects with the same URL prefix.

{% page-ref page="./" %}

{% page-ref page="configuration-remote.md" %}

