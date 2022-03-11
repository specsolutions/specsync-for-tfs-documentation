# Attach files to Test Cases using tags

{% hint style="info" %}
This feature is supported in SpecSync v3.3 or later.
{% endhint %}

SpecSync can attach files to Test Cases during synchronization using tags. In order to use this feature, it has to be enabled in the configuration file using the [`synchronization/attachments` section](../../reference/configuration/configuration-synchronization/configuration-synchronization-attachments.md). 

```javascript
{
  ...
  "synchronization": {
    ...
    "attachments": {
      "enabled": true
    },
    ...
  },
  ...
}
```

You can specify one or more attachment tags for a scenario and you can also use wildcards \([glob patterns](https://en.wikipedia.org/wiki/Glob_%28programming%29)\) as well in the tag to attach multiple files with a single tag. The attachment tag should be in the `@attachment:<attached-file-name>` format, but the tag prefix can be customized. 

For example, to attach a file `screen.png` to a scenario, you have to tag it with `@attachment:screen.png`. To attach all PNG files from within images folder (including sub-folders), you can tags the scenario with `@attachment:images/**/*.png`.

SpecSync tracks the changes of the attached files and automatically re-attach the updated files when they are changed. The attachments are removed from the Test Case when the attachment tag is removed from the scenario.

{% hint style="info" %}
As the Gherkin tags cannot contain spaces, when specifying a file name with a space character in the attachment tags you have to use the underscore \(`_`\) character instead of the spaces. 
{% endhint %}

{% hint style="info" %}
Multiple attachment tags with the same file name or invalid file path causes synchronization error. 
{% endhint %}

## Location of the attached files

By default the specified files are searched relative to the folder of the feature file that contains the attachment link. For example if you specify an attachment as `@attachment:screen.png`, the file `screen.png` has to be in the same folder as the feature file. You can also specify folder names as well, e.g. `@attachment:img/screen.png` or `@attachment:../img/screen.png`. 

Alternatively the base folder for the attached files can also be specified using the `synchronization/attachments/baseFolder` setting. In this case the files are searched relative to the specified base folder independently of the path of the feature files.

## Custom attachment tags

By default SpecSync uses the tags with the `attachment` prefix (`@attachment:<attached-file-name>` format) to attach files, but this can be customized. You can specify additional tag prefixes to specify different kind of attachments. 

For example if you want to attach both wireframe files and specification document files, you can configure a `wireframe` and a `specification` prefix, so that you can link files with tags like: `@wireframe:screen.png @specification:overview.docx`. For that the `synchronization/attachments/tagPrefixes` setting has to be used as the following example shows.

```javascript
{
  ...
  "synchronization": {
    ...
    "attachments": {
      "enabled": true,
      "tagPrefixes": [
        "wireframe",
        "specification"
      ]
    },
    ...
  },
  ...
}
```
