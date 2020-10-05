# myket_iap

Myket In-App Purchase plugin android apps. This plugin is for android apps only.

## Getting Started

### Import this line in Flutter pubspec
```dart
myket_iap: <Last Version>
```

### Updating Your Application's Manifest
Adding the ir.mservices.market.BILLING permission to your AndroidManifest.xml file for In-App Billing
```dart
<uses-permission android:name="ir.mservices.market.BILLING" />
```

### To Use myket iap plugin import below code to your class(your payment class)
```dart
import 'package:myket_iap/myket_iap.dart';
```

* All method be static and don't need to initialize any class

### First of all initialize payment and ras Key
use this method in your `initState()` method.
```dart
IabResult result = await MyketIAP.init(rsaKey:"Your RSA Key From myket", enableDebugLogging: true);
```
1. `rsaKey` : Public key retrieved from myket developer panel
2. `enableDebugLogging` : enable or disable debug log

### To start a purchase request from your app, call the `launchPurchaseFlow` method on the In-app Billing plugin
```dart
Map<String, dynamic> result = await MyketIAP.launchPurchaseFlow(sku: "gas", payload:"payload");
```
1. `sku` : your product id on Myket (created in myket developer panel)
2. `payload` : The `developerPayload` String is used to specify any additional arguments that you want Myket to send back along with the purchase information.

The result will be a map containing `MyketIAP.RESULT` as IabResult object, and `MyketIAP.PURCHASE` as Purchase object.

### after getting result from `launchPurchaseFlow`, if your purchase is consumable, you should consume it
```dart
Map<String, dynamic> consumeResultMap = await MyketIAP.consume(purchase: purchase);
```
1. `purchase` : previously bought item

The result will be a map containing `MyketIAP.RESULT` as IabResult object, and `MyketIAP.PURCHASE` as consumed Purchase object.

### if you want to know state of a purchase use this method
```dart
Map<String, dynamic> result = await MyketIAP.getPurchase(sku: "gas", querySkuDetails: false);
```
1. `sku` : your product id on Myket (created in myket developer panel)
2. `querySkuDetails' : if true, SKU details (price, description, etc) will be queried as well as purchase information.

The result will be a map containing `MyketIAP.RESULT` as IabResult object, and `MyketIAP.PURCHASE` as consumed Purchase object.

### Query the inventory
This will query all owned items from the server, as well as information on additional skus, if specified.
```dart
Map<String, dynamic> result = await MyketIAP.queryInventory(querySkuDetails: false);
```
1. `querySkuDetails' : if true, SKU details (price, description, etc) will be queried as well as purchase information.

The result will be a map containing `MyketIAP.RESULT` as IabResult object, and `MyketIAP.INVENTORY` as Inventory object.

### Dispose
Dispose of object, releasing resources. It's very important to call this method when you are done with this object. It will release any resources used by it such as service connections. Naturally, once the object is disposed of, it can't be used again.
```dart
await MyketIAP.dispose();
```