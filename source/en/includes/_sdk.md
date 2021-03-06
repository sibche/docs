# Installing SDK

##SDK requirements:

1. Uploading the app in [Sibche Developer Dashboard](https://sibche.com/developer)
2. Receiving app key from Dashboard
3. Latest version of Xcode

##Installing SDK

###Installation using Cocoapods:

You only need to input the following code into your podfile: 

```ruby
pod 'SibcheStoreKit', '~> 4.1'
```

>Then install the added pad

```shell
pod repo update
pod install
```

###Manual Installation:

You can download the Sibche SDK from [here](https://github.com/sibche/SibcheStoreKit/releases/latest/download/SibcheStoreKit.zip) and add it to your project. For that you need to extract the downloaded file and copy SibcheStoreKit.framework into your project folder and add it to your project as such:


<a href="https://raw.githubusercontent.com/sibche/SibcheStoreKit/master/Screenshots/sc1.jpg">
    <img src="https://raw.githubusercontent.com/sibche/SibcheStoreKit/master/Screenshots/sc1.jpg"/>
</a>

Then you need to select a plugin file and click on “add”. After that, as displayed below, delete SibcheStoreKit.framework from Linked Frameworks and Libraries in the “General” tab:

<a href="https://raw.githubusercontent.com/sibche/SibcheStoreKit/master/Screenshots/sc2.jpg">
    <img src="https://raw.githubusercontent.com/sibche/SibcheStoreKit/master/Screenshots/sc2.jpg"/>
</a>

Then click on the + button in Embedded Binaries tab and select SibcheStoreKit.framework and click “add” as displayed below:

<a href="https://raw.githubusercontent.com/sibche/SibcheStoreKit/master/Screenshots/sc3.jpg">
    <img src="https://raw.githubusercontent.com/sibche/SibcheStoreKit/master/Screenshots/sc3.jpg"/>
</a>

<a href="https://raw.githubusercontent.com/sibche/SibcheStoreKit/master/Screenshots/sc4.jpg">
    <img src="https://raw.githubusercontent.com/sibche/SibcheStoreKit/master/Screenshots/sc4.jpg"/>
</a>

<a href="https://raw.githubusercontent.com/sibche/SibcheStoreKit/master/Screenshots/sc5.jpg">
    <img src="https://raw.githubusercontent.com/sibche/SibcheStoreKit/master/Screenshots/sc5.jpg"/>
</a>

##Default settings:

###Adding your apps exclusive Scheme

<aside class="success">
If your app already has an exclusive Scheme skip this step.
</aside>

In order to add exclusive Scheme, you need to open the settings in Xcode and go to “info” tab as such:

<a href="https://raw.githubusercontent.com/sibche/SibcheStoreKit/master/Screenshots/sc10.jpg">
    <img src="https://raw.githubusercontent.com/sibche/SibcheStoreKit/master/Screenshots/sc10.jpg"/>
</a>

<a href="https://raw.githubusercontent.com/sibche/SibcheStoreKit/master/Screenshots/sc11.jpg">
    <img src="https://raw.githubusercontent.com/sibche/SibcheStoreKit/master/Screenshots/sc11.jpg"/>
</a>

Then, like so, add your app’s URL in the field:

<a href="https://raw.githubusercontent.com/sibche/SibcheStoreKit/master/Screenshots/sc12.jpg">
    <img src="https://raw.githubusercontent.com/sibche/SibcheStoreKit/master/Screenshots/sc12.jpg"/>
</a>


Chose a name and put it in the testapp field, this name will be the name of your Scheme. For example, the Scheme name of **Telegram** is set to `tg` and for **Instagram**, it is set to `Instagram`. You can view a list of popular apps [here](https://ios.gadgethacks.com/news/always-updated-list-ios-app-url-scheme-names-0184033/).


<aside class="warning">
It is strongly recommended for you to use scheme exclusive to your app with a lot of characters. Be mindful that if your scheme interferes with other apps on the list, you will face problems during payment procedures.
</aside>

###How to add the developer kit to AppDelegate:


First, add the developer kit activation code into the following function:

```objective_c
#import "AppDelegate.h"
#import <SibcheStoreKit/SibcheStoreKit.h>

- (BOOL)application:(UIApplication *)application didFinishLaunchingWithOptions:(NSDictionary *)launchOptions {
    [SibcheStoreKit initWithApiKey:YOUR_API_KEY withScheme:YOUR_SCHEME];
    return YES;
}
```

```swift
import SibcheStoreKit

func application(_ application: UIApplication, didFinishLaunchingWithOptions launchOptions: [UIApplication.LaunchOptionsKey: Any]?) -> Bool {
    SibcheStoreKit.initWithApiKey(YOUR_API_KEY, withScheme: YOUR_SCHEME)
    return true
}
```

<br/>

It is noteworthy that you should insert the program key obtained from the Sibche developer panel in the place of `YOUR_API_KEY`. Also, the word scheme, which was added in the previous stage, must be placed instead of `YOUR_SCHEME`.

<br/>
<br/>

Then, give the developer kit the permission to open urls:

```objective_c
- (BOOL)application:(UIApplication *)app openURL:(NSURL *)url options:(NSDictionary<UIApplicationOpenURLOptionsKey,id> *)options{
    [SibcheStoreKit openUrl:url options:options];
    return YES;
}
```

```swift
func application(_ app: UIApplication, open url: URL, options: [UIApplication.OpenURLOptionsKey : Any] = [:]) -> Bool {
    SibcheStoreKit.open(url, options: options)
    return true
}
```


##Available classes of use for the developer kit:

In order to buy, activate and use Sibche packages, the following classes have been created. Some explanations regarding each of them will be presented in the following:

- `SibchePackage`:
this class represents the purchasable package which was defined in the developer panel. 

- `SibchePurchasePackage`:
this class represents the corresponding purchase of a user which has been assigned to a SibchePackage. This class consists of the purchase information, time of purchase, expiry date etc.

- `SibcheError`:
this class will be referred to you should an error occur and will provide more information regarding the occurred error(s).

###SibchePackage
There are three types of package available:

-  `SibcheConsumablePackage`:
consumable packages which can be used in games or programs, like a 500 gold coin package or in program currency. These packages will remain active and valid until they are consumed and one can repurchase them once they run out.
  

-  `SibcheNonConsumablePackage`:
these packages cannot be gradually used and can only be purchased once, like packages which unlock the ability to upload an avatar or change your username. These packages can only be bought once and will remain in the user’s account forever.

-  `SibcheSubscriptionPackage`:
packages which have a limited time of use. After the time runs out, the user can no longer make use of them. For example, packages which grant the use of special options for only one year.

All of these packages fall under the umbrella package, SibchePackage and include the following functions:

```objective_c
- (NSString*)packageId;
- (NSString*)type;
- (NSString*)code;
- (NSString*)name;
- (NSString*)packageDescription;
- (NSNumber*)price;
- (NSNumber*)totalPrice;
- (NSNumber*)discount;
```

```swift
var packageId: String
var type: String
var code: String
var name: String
var packageDescription: String
var price: NSNumber
var totalPrice: NSNumber
var discount: NSNumber
```

<br/>
<br/>
<br/>

also, the SibcheSubscriptionPackage includes the following additional functions:

```objective_c
- (NSNumber*)duration;
- (NSString*)group;
```

```swift
var duration: String
var group: String
```


###SibchePurchasePackage

This package includes the user’s purchases corresponding with your packages. This class includes the following functions:

```objective_c
- (NSString*)purchasePackageId;
- (NSString*)type;
- (NSString*)code;
- (NSDate*)expireAt;
- (NSDate*)createdAt;
- (SibchePackage*)package;
```

```swift
var purchasePackageId: String
var type: String
var code: String
var expireAt: NSDate
var createdAt: NSDate
var package: SibchePackage
```

<br/>
<br/>
<br/>

###SibcheError

You will be provided with this class should an error with the following characteristics occur:
-	ErrorCode: the error code number can be observed in the following table.
-	Message: the error message which will be received from the server.
-	StatusCode: the http error code which the server provides in response to our request.


| ErrorCode number | Reason                     |
| --------------- | ---------------------------------------- |
| 1000            | Unknown reason                          |
| 1001            | This package has be already purchased            |
| 1002            | User has quit the process                 |
| 1003            | Login error|
| 1004            | Program has not initiated properly |


```objective_c
@property NSNumber* errorCode;
@property NSString* message;
@property NSNumber* statusCode;
```

```swift
var NSNumber: errorCode
var message: String
var NSNumber: statusCode
```

<br/>
<br/>


##How to get the list of available packages:

After tuning the options for the program, you can view the available packages. You must simply use the following code in order to receive the function of interest:

```objective_c
[SibcheStoreKit fetchInAppPurchasePackages:^(BOOL isSuccessful, SibcheError* error, NSArray *packagesArray) {
    // Your block code for handling of packages list
}];
```

```swift
SibcheStoreKit.fetch { (isSuccessful, error, packagesArray) in
    // Your block code for handling of packages list
}
```


<br/>

If successful, the developer kit will provide you with the available packages in the form of a parameter. This parameter is an array of the packages which are available for purchase. These packages are of the `SibchePackage` kind. 

In case the request is not successful, a parameter called error, which is of the `SibcheError` kind, will be sent.

##How to get package information:

If you have the bundle code or ID of the package, you can obtain its information. The following is how to use this API:

```objective_c
// Both package bundle code & package id is acceptable

[SibcheStoreKit fetchInAppPurchasePackage:@"com.example.testapp" withPackagesCallback:^(BOOL isSuccessful, SibcheError* error, SibchePackage *package) {
   // Your block code for handling of packages list
}];
```

```swift
// Both package bundle code & package id is acceptable

SibcheStoreKit.fetch(inAppPurchasePackage: "com.example.testapp") { (isSuccessful, error, package) in
    // Your block code for handling of packages list
}
```

The initially presented parameter is the very callback that we sent which, after getting checked, was recalled. If successful, the package of interest will be sent to you in the object form of `SibchePackage`.


## How to purchase a package:

After obtaining the package list, you can use the following code in order to set a purchase request through the developer kit. Next, the kit will, if necessary, log the client in and check up on the payment procedure. Then, it will tell you if the purchase has been successful or not and will provide you with a `SibchePurchasePackage`.

```objective_c
// Both package bundle code & package id is acceptable

[SibcheStoreKit purchasePackage:@"com.example.testapp" withCallback:^(BOOL isSuccessful, SibcheError* error, SibchePurchasePackage* purchasePackage) {
    // Your block code for handling of purchase callback
}];
```

```swift
// Both package bundle code & package id is acceptable

SibcheStoreKit.purchasePackage("com.example.testapp") { (isSuccessful, error, purchasePackage) in
  // Your block code for handling of purchase callback
}
```

<br/>

##How to obtain the list of previous purchases:

You can use the following code to obtain your active (purchased) packages. Just simply call in the developer kit function as in the following code.

```objective_c
[SibcheStoreKit fetchActiveInAppPurchasePackages:^(BOOL isSuccessful, SibcheError* error, NSArray *purchasePackagesArray) {
  // Your block code for handling of packages list
}];
```

```swift
SibcheStoreKit.fetchActive { (isSuccessful, error, purchasePackagesArray) in
  // Your block code for handling of packages list
}
```
In response, the kit will provide you with information as to whether the request was successful or not, as well as an array of active purchased packages. Take note that this array is of the `SibchePurchasePackage` kind


Active packages are those which have not yet run out of time or have not been used up completely. The Sibche definition for active packages for each of the package types is as follows:

- `SibcheConsumablePackage`: packages which have been purchased but have not yet been consumed.
- `SibcheNonConsumablePackage`: packages which can be bought once and therefore will remain active forever.
- `SibcheSubscriptionPackage`: : packages which still have time until their expiry date.

## Using up packages on the part of the client:
<aside class="success">
Note: this method should be used in order for the client to use the package in programs or games. However, if the server is to use or validate them, please refer to the next section. In order to use the consumable packages, you must recall the associated function from the developer kit, similar to the following code:
</aside>

```objective_c
[SibcheStoreKit consumePurchasePackage:purchasePackageData.purchasePackageId withCallback:^(BOOL isSuccessful, SibcheError* error) {
   // Your block code for handling of package consume
}];
```

```swift
SibcheStoreKit.consumePurchasePackage(purchasePackageData.purchasePackageId) { (isSuccessful, error) in
  // Your block code for handling of package consume
}
```

In response, after the request status has been checked, the developer kit will recall the requested callback. If it is successful, it means that the package has been used up successfully, but if it is unsuccessful, it means that an error has occurred in the usage procedure.


##Using up packages on the part of the server:

To do this, you must have `purchasePackageId` which is part of the `SibchePurchasePackage` class. Then, you must make a link similar to the following and receive the verification data from the server **(addresses are case sensitive!)**:

`https://api.sibche.com/sdk/userInAppPurchasePackages/{purchasePackageId}/verifyConsume`

Also, the HTTP header must be set onto the `App-Key` for this operation. This key is available for any specific program through the developer panel. This key is called the **server key** and has 30 characters.

A sample of code which can be recalled for curl can be as follows:

```shell
curl --header "App-Key: YOUR_SERVER_KEY" -X POST \
'https://api.sibche.com/sdk/userInAppPurchasePackages/1/verifyConsume'
```

<br/>
<br/>

In response, the server will provide one of these **HTTP Response Codes**:
-	HTTP Status Code 404 (Not Found): 
this occurs either when you misspell the address link or package, or if the package has already been used. In this case the server will respond with something like this code:

```json
{
  "message" : "your package is already is used or not found",
  "status_code" : 404
}
```
<br/>
<br/>

-	HTTP Status Code 401 (Unauthorized): 
this means that the server key which has been set on the app-key is invalid or wrong. In this case, the server will provide a similar response to the following:

```json
{
  "message" : "app-key is not valid",
  "status_code" : 401
}
```
<br/>
<br/>

-	HTTP Status Code 202 (Accepted / OK): 
this means that the package has been successfully consumed and the purchase is valid. In this case, the server will return no written message in its response.

##Client login request to the Sibche developer kit:

<aside class="warning">
Note: I recommend that you don’t use this section manually because if the client has not logged in during the payment process, the Sibche library will automatically attempt to do so.
</aside>

By using this code, you can send a login request to the Sibche library. All you need to do is, as it is done below, recall the function relating to the developer kit.

```objective_c
[SibcheStoreKit loginUser:^(BOOL isLoginSuccessful, SibcheError* error, NSString *userName, NSString *userId) {
    // Your code for handling of login result
}];
```

```swift
SibcheStoreKit.loginUser { (isLogginSuccessful, error, userName, userId) in
    // Your code for handling of login result
}
```


<br/>

In response, the developer kit will provide information regarding the successfulness of the login and the errors it faced in the process.

##How to logout of the Sibche developer kit:

<aside class="warning">
Note: I highly recommend that you only use this function when the client has requested the retrieval of their purchase and has their retrieval request in hand.
</aside>

By the use of this code, you can logout the client who is already logged in to the developer kit and delete their session information. This function will provide a response to you in the form of a callback.

```objective_c
[SibcheStoreKit logoutUser:^{
    // Your code for handling of logout result
}];
```

```swift
SibcheStoreKit.logoutUser {
    // Your code for handling of logout result
}
```


##How to obtain information on current clients:

When you need to get the client’s phone number and userID you can use the following function:

```objective_c
[SibcheStoreKit getCurrentUserData:^(BOOL isSuccessful, SibcheError *error, LoginStatusType loginStatus, NSString *userCellphoneNumber, NSString *userId) {
    // Your code for handling of get current user data result
}];
```

```swift
SibcheStoreKit.getCurrentUserData { (isSuccessful, error, loginStatus, userCellphoneNumber, userId) in
    // Your code for handling of get current user data result
}
```


<br/>
<br/>

In response, the developer kit will tell you if the process was successful or not and also recalls the error that may have occurred. In most cases the process will go smoothly, but in some rare cases, the following reasons could be why the process wasn’t successful:

-	Connectivity issues
-	Network issues
-	Sibche server is not accessible

In addition, a third parameter exists which tells you whether the client is logged in or not. if the client is logged in, the response type will be of this kind `loginStatusTypeIsLoggedIn`. If they are not logged in, the response type will be of the `loginStatusTypeIsLoggedOut` kind. If the process is not successful and we are not sure whether the client is logged in or not, we will get a response which is of the `loginStatusTypeHaveTokenButFailedToCheck` kind. The next parameters in the received response will include the client’s phone number and ID.
