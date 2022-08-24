# links

This configuration section contains settings to configure how test case links should be updated based on the tags of the scenario.

Read more about synchronizing test case links in the [Linking work items using tags](../../../features/common-synchronization-features/linking-work-items-with-tags.md) concept description.

The following example shows how this sub-section can be used to specify a single link type.

```javascript
{
  ...
  "synchronization": {
    ...
    "links": [
      {
        "tagPrefix": "bug"
      }
    ],
    ...
  },
  ...
}
```

The following example shows the available options within this sub-section. This example configures two link types.

```javascript
{
  ...
  "synchronization": {
    ...
    "links": [
      {
        "tagPrefix": "pbi",
        "targetWorkItemType": "ProductBacklogItem",
        "relationship": "Parent",
        "mode": "createIfMissing"
      },
      {
        "tagPrefix": "bug"
      },
      {
        "tagPrefix": "pr",
        "relationship": "Pull Request"
      }
    ],
    ...
  },
  ...
}
```

## Settings

The `links` sub-section contains an array of link type configurations. Each of them is used to configure a link type. \(The second example above configures two link types.\)

For each link type configuration the following settings can be used.

<table>
  <thead>
    <tr>
      <th style="text-align:left">Setting</th>
      <th style="text-align:left">Description</th>
      <th style="text-align:left">Default</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td style="text-align:left"><code>tagPrefix</code>
      </td>
      <td style="text-align:left">A tag prefix for specifying the relation for the scenario. E.g. specify <code>pbi</code> for
        linking product backlog items using a tag, like <code>@pbi:1234</code>.</td>
      <td
      style="text-align:left">mandatory</td>
    </tr>
    <tr>
      <td style="text-align:left"><code>targetWorkItemType</code>
      </td>
      <td style="text-align:left">The type of the Azure DevOps work item the link refers to.</td>
      <td style="text-align:left">can link to any work item type</td>
    </tr>
    <tr>
      <td style="text-align:left"><code>relationship</code>
      </td>
      <td style="text-align:left">
        <p>Specify the relationship for the created link. E.g. specifying <code>Parent</code> means
          that the linked work item will be the parent of the test case work item.</p>
        <p>Set to <code>Pull Request</code> for Azure DevOps Pull Requests and <code>GitHub Pull Request</code> for GitHub Pull Requests.</p>
      </td>
      <td style="text-align:left"><code>Tests</code>
      </td>
    </tr>
    <tr>
      <td style="text-align:left"><code>linkTemplate</code>
      </td>
      <td style="text-align:left">
        <p>Specifies the HTTP link template of the related artifact (for `GitHub Pull Request` relationship). The link template can use the specified value using the `{id}` placeholder.</p>
      </td>
      <td style="text-align:left">no template used</td>
    </tr>
    <tr>
      <td style="text-align:left"><code>mode</code>
      </td>
      <td style="text-align:left">
        <p>Deprecated and will be removed in a future version.</p>
      </td>
      <td style="text-align:left"></td>
    </tr>
  </tbody>
</table>

{% page-ref page="./" %}

{% page-ref page="../" %}

