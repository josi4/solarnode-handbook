# Manage Component

The component management page is shown when you click the **Manage** button for a multi-instance
component. Each component instance's settings are independent, allowing you to integrate with
multiple copies of a device or service.

For example if you connected a Modbus power meter and a Modbus solar inverter to a node, you would
create two Modbus Device component instances, and configure them with settings appropriate for each
device.

<figure markdown>
  ![Component management UI](../../../images/users/setup/setup-manage-component%402x.png){width=837}
  <caption>The component management screen allows you to add, update, and remove component instances.</caption>
</figure>

## Add new instance

Add new component instances by clicking the **Add new _X_** button in the top-right, where **_X_**
is the name of the component you are managing. You will be given the opportunity to assign a unique
identifier to the new component instance:

<figure markdown>
  ![New component instance dialog](../../../images/users/setup/setup-manage-component-add-form%402x.png){width=550}
  <caption>When creating a new component instance you can provide a short name to identify it with.</caption>
</figure>

When you add more than one component instance, the identifiers appear as clickable buttons that allow
you to switch between the setting forms for each component.

<figure markdown>
  ![Component instance buttons](../../../images/users/setup/setup-manage-component-identifiers%402x.png){width=400}
  <caption>Component instance buttons let you switch between each component instance.</caption>
</figure>

## Saving changes

After making changes to any component instance's settings, the **Save All Changes** button in the top-left.

!!! tip "Save All Changes works across all component instances"

	You can safely switch between and make changes on multiple component instance settings
	before clicking the **Save All Changes** button: your changes across _all_ instances
	will be saved.

## Remove or reset instances

At the bottom of each component instance are buttons that let you **delete** or **reset** that component
intance.

<figure markdown>
  ![New component instance dialog](../../../images/users/setup/setup-manage-component-actions%402x.png){width=516}
  <caption>Buttons to delete or reset component instance.</caption>
</figure>

The **Delete** button will remove that component instance from appearing, however _the settings associated
with that instance are preserved_. If you re-add an instance _with the same identifier_ then the previous
settings will be restored. You can think of the Delete button as disabling the component, giving you the
option to "undo" the deletion if you like.

The **Restore** button will reset the component to its factory defaults, removing any settings you have
customized on that instance. The instance remains visible and you can re-configure the settings as
needed.

## Remove all instances

The **Remove all** button in the top-right of the page allows you to remove all component instances,
including any customized settings on those instances.
