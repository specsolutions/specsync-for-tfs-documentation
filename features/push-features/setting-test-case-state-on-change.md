# Setting Test Case State on change

Some team use the _State_ field of the Test Case work items to track whether the particular test has been approved to perform the related verification. In case of any changes to the Test Case the approval process might need to be performed again. 

SpecSync can automatically change the State field upon any changes in the scenario. To enable this feature the [`synchronization/state/setValueOnChangeTo` setting](../../reference/configuration/configuration-synchronization/configuration-synchronization-state.md) has to be configured with a state value. SpecSync will set the _State_ filed of the Test Case to this configured value when the scenario has changed.

The following example shows how to set the state to `Design` when the scenario has changed.

```javascript
{
  ...
  "synchronization": {
    ...
    "state": {
      "setValueOnChangeTo": "Design"
    },
    ...
  },
  ...
}
```

{% hint style="info" %}
The possible allowed values might depend on the process template your project uses.
{% endhint %}

