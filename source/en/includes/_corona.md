#How to install the Corona developer kit

##Corona plugin requirements

1.  Uploading the app in [Sibche Developer Dashboard](https://sibche.com/developer)
2.  Receiving app key from Dashboard
3.  Latest version of Xcode
4. Latest version of Corona (Above 2020.3577)

##Installing the Corona plugin

###Adding the sibche plugin to the corona project

<aside class="success">
Please note that you must install the project plugin "self-hosted" due to Corona's limitations, and since this option is only avaiable in the most recent versions of Corona, you must install and use the version '2020.3577' or above.
</aside>

```lua
	plugins =
    {
        ["plugin.SibcheStoreKit"] =
        {
            publisherId = "com.sibche",
            supportedPlatforms = { 
                iphone = { url="https://raw.githubusercontent.com/sibche/SibcheStoreKit-Corona/master/iphone.tgz" },
                ["iphone-sim"] = { url="https://raw.githubusercontent.com/sibche/SibcheStoreKit-Corona/master/iphone-sim.tgz" },
                android = false,
                macos = false,
                win32 = false
            },
        },
    },
```
In order to install Sibche Plugin, you must copy the following code in `build.settings` file and then click on the (`Download Plugins`) from Xcode menu.
<br/>

##Default settings (corona)

### Adding your app’s exclusive Schemeا

<aside class="success">
If your app already has an exclusive Scheme skip this step.
</aside>

In order to add exclusive Scheme, you need to open the settings in Xcode and go to **info** tab as such:

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

Chose a name and put it in the testapp field, this name will be the name of your Scheme. For example, the Scheme name of **Telegram** is set to `tg` and for **Instagram**, it is set to `Instagram`. You can view a list of popular apps [here]([اینجا](https://ios.gadgethacks.com/news/always-updated-list-ios-app-url-scheme-names-0184033/).

<aside class="warning">
It is strongly recommended for you to use scheme exclusive to your app with a lot of characters. Be mindful that if your scheme interferes with other apps on the list, you will face problems during payment procedures.
</aside>


###Adding initial init Sibche libraryه

```lua
local SibcheStoreKit = require "sibche.wrapper"

SibcheStoreKit.init(YOUR_API_KEY, YOUR_SCHEME)
```

  First you need to enter the sibche plugin and recall the `init` method using the following code

<br/>
<br/>

Instead of `YOUR_API_KEY`, you must enter the api key which you received from the Sibche developer panel. Also, instead of `YOUR_SCHEME`, you must enter the scheme which was defined in the previous stages.

##Available objects of use for the sibche plugin (corona):

In order to buy, activate and use Sibche packages, the following objects (table) have been created. Some explanations regarding each of them will be presented in the following:
-	`SibchePackage`: this class represents the purchasable package which was defined in the developer panel.
-	`SibchePurchasePackage`: this class represents the corresponding purchase of a user which has been assigned to a SibchePackage. This class consists of the purchase information, time of purchase, expiry date etc.
-	`SibcheError`: this class will be referred to you should an error occur and will provide more information regarding the occurred error(s).

 
### SibchePackage
There are three types of package available:
- `SibcheConsumablePackage`: consumable packages which can be used in games or programs, like a 500 gold coin package or in program currency. These packages will remain active and valid until they are consumed and one can repurchase them once they run out.
- `SibcheNonConsumablePackage`: these packages cannot be gradually used and can only be purchased once, like packages which unlock the ability to upload an avatar or change your username. These packages can only be bought once and will remain in the user’s account forever.
- `SibcheSubscriptionPackage`: packages which have a limited time of use. After the time runs out, the user can no longer make use of them. For example, packages which grant the use of special options for only one year.

<br/>

```lua
sibchePackage.packageId (string)
sibchePackage.type (string)
sibchePackage.code (string)
sibchePackage.name (string)
sibchePackage.packageDescription (string)
sibchePackage.price (int)
sibchePackage.totalPrice (int)
sibchePackage.discount (int)
```

All of these objects fall under the umbrella object, `SibchePackage` and include the following functions:

<br/>
<br/>
<br/>
<br/>
<br/>
<br/>

```lua
sibchePackage.duration (int)
sibchePackage.group (string)
```

also, the `SibcheSubscriptionPackage` object includes the following additional functions:


### SibchePurchasePackage

```lua
sibchePurchasePackage.purchasePackageId (string)
sibchePurchasePackage.type (string)
sibchePurchasePackage.code (string)
sibchePurchasePackage.expireAt ()
sibchePurchasePackage.createdAt ()
sibchePurchasePackage.package (SibchePackage)
```

This object includes the user’s purchases corresponding with your packages. This object includes the following functions:
<br/>
<br/>
<br/>

### SibcheError

```lua
event.errorMessage;
event.errorCode;
event.errorStatusCode;
```

This object will be provided to you should an error occur in the same parameter callback and has the following features:

- ErrorCode: the error code number can be observed in the following table.
- Message: the error message which will be received from the server.
- StatusCode: the http error code which the server provides in response to our request.


| ErrorCode number | Reason                        |
| --------------- | ---------------------------------------- |
| 1000            | Unknown reason                            |
| 1001            | This package has been already purchased            |
| 1002            | User has quit the process                 |
| 1003            | Login error |
| 1004            | Program has not initiated properly |

##How to get the list of available packages (corona)

```lua
local function fetchInAppPurchasePackagesCallback(event)
    print(inspect(event))
    if(event.isSuccessful) then
        -- success block: e.g. fill packages with event.packagesArray
    else
        -- failure block: e.g. show error to user based on event.errorCode
    end
end

SibcheStoreKit.fetchInAppPurchasePackages(fetchInAppPurchasePackagesCallback)
```

After tuning the options for the program, you can view the available packages. You must simply use the following code in order to receive the function of interest:


<aside class="success">
Note: the “inspect” that has been used is actually a library which can be used to observe the cable contents and can be obtained from <a href="https://raw.githubusercontent.com/sibche/SibcheStoreKit-Corona/master/Corona/inspect.lua">here
                                                                                                                                      </a>.
</aside>

<br/>
<br/>
<br/>

If successful, the developer kit will provide you with the available package plugins in the form of an answer parameter. This parameter is an array of the packages which are available for purchase. These packages are of the `SibchePackage` kind.

In case the request is not successful, parameters called `errormessage`, `errorcode` and `errorstatuscode` will be sent. If such is not the case, null will be sent.

##How to get package information (corona)

```lua
local function fetchInAppPurchasePackageCallback(event)
    print(inspect(event))
    if(event.isSuccessful) then
        -- success block: e.g. use event.package into your game
    else
        -- failure block: e.g. show error to user based on event.errorCode
    end
end

SibcheStoreKit.fetchInAppPurchasePackage(ِYOUR_PACKAGE_ID_OR_CODE, fetchInAppPurchasePackageCallback)
```

If you have the bundle code or ID of the package, you can obtain its information. The following is how to use this API:

<br/>
<br/>

The parameter presented secondly is the very callback that we sent which, after getting checked, was recalled. If successful, the package of interest will be sent to you in the object form of `SibchePackage`.

##How to purchase a package (corona):

```lua
local function purchasePackageCallback(event)
    print(inspect(event))
    if(event.isSuccessful and event.purchasePackage) then
        -- success block
    else
        -- failure block
    end
end

SibcheStoreKit.purchasePackage(ِYOUR_PACKAGE_ID_OR_CODE, purchasePackageCallback)
```

After obtaining the package list, you can use the following code in order to set a purchase request to the plugin. Next, the developer kit will, if necessary, log the client in and check up on the payment procedure. Then, it will tell you if the purchase has been successful or not and will provide you with a `SibchePurchasePackage`.
## How to obtain the list of previous purchases (corona)

```lua
local function fetchActiveInAppPurchasePackagesCallback( event )
    print(inspect(event))
end

SibcheStoreKit.fetchActiveInAppPurchasePackages(fetchActiveInAppPurchasePackagesCallback)
```

You can use the following code to obtain your active (purchased) packages. Just simply call in the developer kit function as in the following code.
<br/>
<br/>
<br/>

In response, the kit will provide you with information as to whether the request was successful or not, as well as an array of active purchased packages. Take note that this array is of the `SibchePurchasePackage` kind.

Active packages are those which have not yet run out of time or have not been used up completely. The Sibche definition for active packages for each of the package types is as follows:
- `SibcheConsumablePackage`: packages which have been purchased but have not yet been consumed.
-	`SibcheNonConsumablePackage`: packages which can be bought once and therefore will remain active forever.
-	`SibcheSubscriptionPackage`: packages which still have time until their expiry date.

## Using up packages on the part of the client (corona)


<aside class="warning">
Note: this method should be used in order for the client to use the package in programs or games. However, if the server is to use or validate them, please refer to the next section.
</aside>

```lua
local function consumePurchasePackageCallback( event )
    print(inspect(event))
end

local function purchasePackageCallback( event )
    print(inspect(event))
    if(event.isSuccessful and event.purchasePackage) then
        if(event.purchasePackage.package.type == "ConsumableInAppPackage") then
            SibcheStoreKit.consumePurchasePackage(event.purchasePackage.purchasePackageId, consumePurchasePackageCallback)
        end
    end
end

SibcheStoreKit.purchasePackage(ِYOUR_PACKAGE_ID_OR_CODE, purchasePackageCallback)
```

In order to use the consumable packages, you must recall the associated function from the developer kit, similar to the following code:
<br/>
<br/>

<aside class="success">
The following piece of code, after a successful purchase, attempts to consume the package and is simply written in order to show how the package is used and must be changed in order to fit your game.
</aside>

<br/>
<br/>
<br/>

In response, after the request status has been checked, the developer kit will recall the requested callback. If it is successful, it means that the package has been used up successfully, but if it is unsuccessful, it means that an error has occurred in the usage procedure.

## Using up packages on the part of the server(corona):

To do this, you must have `purchasePackageId` which is part of the `SibchePurchasePackage` class. Then, you must make a link similar to the following and receive the verification data from the server **(addresses are case sensitive!)**:
`https://api.sibche.com/sdk/userInAppPurchasePackages/{purchasePackageId}/verifyConsume`

Also, the HTTP header must be set onto the App-Key for this operation. This key is available for any specific program through the developer panel. This key is called the **server key** and has 30 characters.

```shell
curl --header "App-Key: YOUR_SERVER_KEY" -X POST \
'https://api.sibche.com/sdk/userInAppPurchasePackages/1/verifyConsume'
```

A sample of code which can be recalled for curl can be as follows:

<br/>
<br/>

In response, the server will provide one of these **HTTP Response Codes**:

- HTTP Status Code 404 (Not Found):

 this occurs either when you misspell the address link or package, or if the package has already been used. In this case the server will respond with something like this code:

```json
{
  "message" : "your package is already is used or not found",
  "status_code" : 404
}
```

- HTTP Status Code 401 (Unauthorized):

 this means that the server key which has been set on the app-key is invalid or wrong. In this case, the server will provide a similar response to the following:

```json
{
  "message" : "app-key is not valid",
  "status_code" : 401
}
```

- HTTP Status Code 202 (Accepted / OK):
this means that the package has been successfully consumed and the purchase is valid. In this case, the server will return no written message in its response.


##Client login request to the Sibche developer kit (corona)

<aside class="success">
Note: I recommend that you don’t use this section manually because if the client has not logged in during the payment process, the Sibche library will automatically attempt to do so.
</aside>


```lua
local function loginCallback(event)
    print(inspect(event))
end

SibcheStoreKit.loginUser(loginCallback)
```

By using this code, you can send a login request to the Sibche library. All you need to do is, as it is done below, recall the function relating to the developer kit.
<br/>
<br/>
<br/>

In response, the plugin will provide information regarding the successfulness of the login and the errors it faced in the process. Also, it will recall the client’s ID and username if the process was successful and the items exist in the database.

## How to logout of the Sibche developer kit (corona)

<aside class="warning">
Note: I highly recommend that you do not use this function if you do not have a specific reason, because the logout button has been implemented in the sibche plugin.
</aside>

```lua
local function logoutCallback(event)
    print(inspect(event))
end

SibcheStoreKit.logoutUser(logoutCallback)
```

By the use of this code, you can logout the client who is already logged in to the plugin and delete their session information. This function will provide a response to you in the form of a callback.

##How to obtain information on current clients (corona)

```lua
local function getCurrentUserCallback(event)
    print(inspect(event))
end

SibcheStoreKit.getCurrentUserData(getCurrentUserCallback)
```


When you need to get the client’s phone number and userID you can use the following function:

<br/>
<br/>
<br/>

In response, the developer kit will tell you if the process was successful or not and also recalls the error that may have occurred. In most cases the process will go smoothly, but in some rare cases, the following reasons could be why the process wasn’t successful:
-	Connectivity issues
-	Network issues
-	Sibche server is not accessible


In addition, a third parameter exists which tells you whether the client is logged in or not. if the client is logged in, the response type will be of this kind `loginStatusTypeIsLoggedIn`. If they are not logged in, the response type will be of the `loginStatusTypeIsLoggedOut` kind. If the process is not successful and we are not sure whether the client is logged in or not, we will get a response which is of the `loginStatusTypeHaveTokenButFailedToCheck` kind. The next parameters in the received response will include the client’s phone number and ID.
