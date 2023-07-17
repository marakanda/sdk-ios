# iOS

The Device Data Collector (**DDC**) for iOS manual and example implementation in [Swift](./example-app-swift). Latest release: 2.7.463.

[Requirements](#requirements)<br/>
[Installation](#installation)<br/>
[Usage](#usage)<br/>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;[Permissions](#permissions)<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;[Use the framework in Swift project](#use-the-framework-in-swift-project)<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;[Associating collected data with a user/device identity](#associating-collected-data-with-a-userdevice-identity)<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;[Data collection frequency](#data-collection-frequency)<br/>


## Requirements

| iddc.framework | Xcode | Version |
| -------------- | ----- | ------- |
| iddc-xcode14.2 | 14.2 | 2.7.421 |
| iddc-xcode14.3 | 14.3 | 2.7.463 |



## Installation

iOS DDC SDK is available through [CocoaPods](http://cocoapods.org). To install it, simply add the following line to your Podfile:

```ruby
pod 'iddc-xcode14.3', '2.7.463'
```

[SEE EXAMPLE](./example-app-swift/Podfile#L11)

To install iddc.framework, run the script from command-line:

```sh
$ pod install
```

To upgrade iddc.framework, run the script from command-line:

```sh
$ pod update
```



## Usage

#### Permissions
DDC SDK doesn't need any permissions in order to run (and won't ask for any). If no permissions are set by the host application, DDC will only collect data for which permissions are not required.

However, there are some permissions that improve data quality if already granted to the host application:

##### Access WiFi Information

If Access WiFi capability and Access WiFi Information entitlement are enabled, DDC can collect SSID and (hashed) BSSID on iOS 11 and iOS 12 devices.


#### Use the framework in Swift project

Import DDC:
```Swift
import iddc
```

Create an instance:
```Swift
let ddc = DeviceDataCollector.getDefault(key: "YOUR_LICENCE_KEY")
```

Collect data:
```Swift
ddc.run { error in
    if let err = error {
        print("\(err.description)")
    }
}
```



#### Associating collected data with a user/device identity

The following properties can be used to optionally provide additional user/device identifiers:

| Name           | Description                                                  |
| -------------- | ------------------------------------------------------------ |
| advertisingID  | The [Apple advertising ID](https://developer.apple.com/documentation/adsupport/asidentifiermanager) of a device. |
| externalUserID | The host application's user identity. For example a (unique) user name, a user ID, an e-mail - or a hash thereof. |
| phoneNumber    | The user's phone number.                                     |

These can be set in any order, at any time (once there is a ddc instance) and as many times as needed.

In Swift:

```java
ddc.advertisingID(adID);
ddc.externalUserID("c23911a2-c455-4a59-96d0-c6fea09176b8"); 
ddc.phoneNumber("+1234567890");
```

**Note:** user data is encrypted and handled in accordance with EU GDPR.



#### Data collection frequency

The higher the frequency of data collection (DDC events), the greater the business value. The bare minimum is to trigger events on app open ([applicationDidEnterBackground](https://developer.apple.com/documentation/uikit/uiapplicationdelegate/1622997-applicationdidenterbackground )) and/or app close ([applicationWillEnterForeground](https://developer.apple.com/documentation/uikit/uiapplicationdelegate/1623076-applicationwillenterforeground)). Read more about [state transitions here](https://developer.apple.com/documentation/uikit/uiapplicationdelegate#1965924). There's a minimum interval between two events determined by the licence - so calling DDC too often is harmless.  

In our example app data collection is triggered when a button is pressed. 
