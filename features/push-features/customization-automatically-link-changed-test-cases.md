# Customization: Automatically link changed Test Cases

The _Link on change_  customization can be used to link changed Test Cases to a work item or pull request, related to the change.

{% hint style="info" %}
The _Link on change_ customization described here is an [Enterprise feature](../../licensing.md).
{% endhint %}

In order to enable this customization, the `customizations/linkOnChange` section of the configuration has to be enabled. The complete reference of the configuration settings can be found in the [customizations configuration reference](../../reference/configuration/configuration-customizations.md#linkonchange).

The following example shows a basic configuration that links the changed Test Cases to the Pull Request of the current build.

```javascript
{
  ...
 "customizations": {
    "linkOnChange": {
      "enabled": true,
      "links": [
        {
          "targetId": "{env:SYSTEM_PULLREQUEST_PULLREQUESTID}",
          "relationship": "Pull Request"
        }
      ]
    }
  }
  ...
}
```

You can use environment variable references in the `targetId` setting as shown above. 

If the `targetId` evaluates to an empty string, no link will be added.

