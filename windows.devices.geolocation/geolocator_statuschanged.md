---
-api-id: E:Windows.Devices.Geolocation.Geolocator.StatusChanged
-api-type: winrt event
---

<!-- Event syntax
public event Windows.Foundation.TypedEventHandler StatusChanged<Windows.Devices.Geolocation.Geolocator,  Windows.Devices.Geolocation.StatusChangedEventArgs>
-->

# Windows.Devices.Geolocation.Geolocator.StatusChanged

## -description
Raised when the ability of the [Geolocator](geolocator.md) to provide updated location changes.

## -remarks
You can access information about the event with the [StatusChangedEventArgs](statuschangedeventargs.md) object that is passed to your event handler.

When using a geofence, use the [GeofenceMonitor](../windows.devices.geolocation.geofencing/geofencemonitor.md) 's [StatusChanged](../windows.devices.geolocation.geofencing/geofencemonitor_statuschanged.md) event to monitor changes in location permissions instead of this  event from the [Geolocator](geolocator.md) class. A [GeofenceMonitorStatus](../windows.devices.geolocation.geofencing/geofencemonitorstatus.md) of **Disabled** is equivalent to a **Disabled **[PositionStatus](positionstatus.md) - both indicate that the app does not have permission to access location.

## -examples
This code example demonstrates how the [StatusChanged](geolocator_statuschanged.md) event is handled. The [Geolocator](geolocator.md) object triggers the [StatusChanged](geolocator_statuschanged.md) event to indicate that the user's location settings changed. That event passes the corresponding status via the argument's **Status** property (of type [PositionStatus](positionstatus.md)). Note that this method is not called from the UI thread and the [Dispatcher](../windows.ui.core/coredispatcher.md) object invokes the UI changes. For more info, see [Get current location](http://msdn.microsoft.com/library/24dc9a41-8cc1-48b0-bc6d-24bf571afcc8).

```csharp

using Windows.UI.Core;
...
async private void OnStatusChanged(Geolocator sender, StatusChangedEventArgs e)
{
    await Dispatcher.RunAsync(CoreDispatcherPriority.Normal, () =>
    {
        // Show the location setting message only if status is disabled.
        LocationDisabledMessage.Visibility = Visibility.Collapsed;

        switch (e.Status)
        {
            case PositionStatus.Ready:
                // Location platform is providing valid data.
                ScenarioOutput_Status.Text = "Ready";
                _rootPage.NotifyUser("Location platform is ready.", NotifyType.StatusMessage);
                break;

            case PositionStatus.Initializing:
                // Location platform is attempting to acquire a fix. 
                ScenarioOutput_Status.Text = "Initializing";
                _rootPage.NotifyUser("Location platform is attempting to obtain a position.", NotifyType.StatusMessage);
                break;

            case PositionStatus.NoData:
                // Location platform could not obtain location data.
                ScenarioOutput_Status.Text = "No data";
                _rootPage.NotifyUser("Not able to determine the location.", NotifyType.ErrorMessage);
                break;

            case PositionStatus.Disabled:
                // The permission to access location data is denied by the user or other policies.
                ScenarioOutput_Status.Text = "Disabled";
                _rootPage.NotifyUser("Access to location is denied.", NotifyType.ErrorMessage);

                // Show message to the user to go to location settings
                LocationDisabledMessage.Visibility = Visibility.Visible;

                // Clear cached location data if any
                UpdateLocationData(null);
                break;

            case PositionStatus.NotInitialized:
                // The location platform is not initialized. This indicates that the application 
                // has not made a request for location data.
                ScenarioOutput_Status.Text = "Not initialized";
                _rootPage.NotifyUser("No request for location is made yet.", NotifyType.StatusMessage);
                break;

            case PositionStatus.NotAvailable:
                // The location platform is not available on this version of the OS.
                ScenarioOutput_Status.Text = "Not available";
                _rootPage.NotifyUser("Location is not available on this version of the OS.", NotifyType.ErrorMessage);
                break;

            default:
                ScenarioOutput_Status.Text = "Unknown";
                _rootPage.NotifyUser(string.Empty, NotifyType.StatusMessage);
                break;
        }
    });
}
```



## -see-also
[Get current location](http://msdn.microsoft.com/library/24dc9a41-8cc1-48b0-bc6d-24bf571afcc8), [Get current location](http://msdn.microsoft.com/library/24dc9a41-8cc1-48b0-bc6d-24bf571afcc8), [Set up a geofence](http://msdn.microsoft.com/library/a3a46e03-0751-4dbd-a2a1-2323db09bdba), [StatusChangedEventArgs](statuschangedeventargs.md), [geolocation sample](http://go.microsoft.com/fwlink/p/?linkid=533278)

## -capabilities
location
ID_CAP_LOCATION [Windows Phone]
