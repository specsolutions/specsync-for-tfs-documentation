# Add new Test Cases to an Area or an Iteration

The `Area` field can be used in Azure DevOps to assign the Test Case to a team or to a project functionality group. The `Iteration` field can be used to assign the Test Case to a Sprint or project iteration.

The `Area` and the `Iteration` fields can be modified manually, independently from the scenario synchronization \(SpecSync will not override these fields\). When the Test Case is initially created by SpecSync, by default it leaves these fields unset, so they will be initialized to the project default \(usually to the root area and iteration\).

The [`synchronization/areaPath`](../../reference/configuration/configuration-synchronization/configuration-synchronization-areapath.md) and the [`synchronization/iterationPath`](../../reference/configuration/configuration-synchronization/configuration-synchronization-iterationpath.md) settings in the [configuration file](../../reference/configuration/) can be used to change the default behavior and set the `Area` and the `Iteration` fields to a specific value for the initial Test Case creation.

```text
{
  ...
  "synchronization": {
    ...
    "areaPath": {
      "mode": "setOnLink",
      "value": "\\FrontEndTeam"
    },
    "iterationPath": {
      "mode": "setOnLink",
      "value": "\\Sprint 1"
    },
    ...
  },
  ...
}
```

The example above will initialize the `Area` field with `MyProject\FrontEndTeam` and the `Iteration` with `MyProject\Sprint 1`.

It is also possible to specify deeper area or iteration paths:

```text
{
  ...
  "synchronization": {
    ...
    "iterationPath": {
      "mode": "setOnLink",
      "value": "\\Release 1\\Sprint 1"
    },
    ...
  },
  ...
}
```

The project name can be omitted from the path, as the example above shows.

{% hint style="info" %}
In the json configuration file the backslash character \(`\`\) has to be entered as `\\`.
{% endhint %}

{% hint style="info" %}
These settings have no impact on the Test Cases that have been created earlier. SpecSync never updates these fields, only initializes them for new Test Cases.
{% endhint %}
