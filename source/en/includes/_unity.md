#Installing Unity SDK

##Unity developer kit installment requirements:

1. Uploading the app in [Sibche Developer Dashboard](https://sibche.com/developer)
2.	Receive the program key from the developer panel
3.	Latest version of Xcode
4.	Unity v2019.1 and above


## How to install the unity developer kit:
You can download the Sibche developer kit specific to unity from [this link](https://github.com/sibche/SibcheStoreKit-Unity/releases/latest/download/SibcheStoreKit.unitypackage) and add it to your project.

## Basic settings (unity):

The required settings for a project are automatically added in the project under the editor section. `SibcheBuildPostProcessor.cs` is the file name.

This file includes two setting sections: one is meant for the addition of the specific scheme or url of your game and the other is meant for the addition of the project file to the project in a dynamic fashion. Both cases will be explained in detail. 

###Adding your program’s specific scheme:


If your program has a specific scheme or url you must turn this setting off from the `SibcheBuildPostProcessor.cs` section. Just like the below mentioned code, you can set your specific url onto the project.
```csharp
AddCustomUrlScheme(path, "testapp", "test");
```

Instead of test, set your preferred name and instead of testapp, place your preferred scheme. For instance, the schemes which are set for **Telegram** and **Instagram** are `tg` and `Instagram`, respectively. A full list of popular schemes can be found [here](https://ios.gadgethacks.com/news/always-updated-list-ios-app-url-scheme-names-0184033/).

I highly recommend that you use a scheme that is specific to you and is preferably long. Take caution that if your scheme interferes with another program you may encounter issues regarding payment.



###How to add a .framework file to the dynamic library (embedded framework):

This section has done all that is needed to be done in regards to making the project dynamic and all that you need to do is leave it as it is:

```csharp
AddSibcheFrameworkAsEmbed(path);
```


###Adding initial init Sibche library:

At the beginning of the main class (start function), you must recall the init Sibche library function in order to use it in the following stages.


```csharp
using SibcheStoreKit;

public class GameController : MonoBehaviour
{
    // ...

    private void Start()
    {
        //Other initiation tasks

        // Sibche StoreKit initiation
        Sibche.Initialize(YOUR_API_KEY, YOUR_SCHEME);
    }

    // ...
}
```

 Instead of `YOUR_API_KEY`, you must enter the api key which you received from the Sibche developer panel. Also, instead of `YOUR_SCHEME`, you must enter the scheme which was defined in the previous stages.

##Classes of use in the developer kit (unity):

In order to buy, activate and use the Sibche packages, the following classes have been created. Some explanations regarding each class are presented as follows:

- `SibchePackage`: this class is the representation of the purchasable package which was defined in the developer panel. 

- `SibchePurchasePackage`: : this class is the representation of a corresponding purchase of a user which has been assigned to a SibchePackage. This class consists of the purchase information, time of purchase, expiry date etc

- `SibcheError`: : this class will be referred to you should an error occur and will provide more information regarding the occurred error(s)
 
 
### SibchePackage
There are three types of package available:

- 
   `SibcheConsumablePackage`:
   consumable packages which can be used in games or programs, like a 500 gold coin package or in program currency. These packages will remain active and valid until they are consumed and one can repurchase them once they run out.

- 
   `SibcheNonConsumablePackage`:
   : these packages cannot be gradually used and can only be purchased once, like packages which unlock the ability to upload an avatar or change your username. These packages can only be bought once and will remain in the user’s account forever.



- 
   `SibcheSubscriptionPackage`:
  packages which have a limited time of use. After the time runs out, the user can no longer make use of them. For example, packages which grant the use of special options for only one year.

All of these packages fall under the umbrella package, `SibchePackage` and include the following characteristics:



```csharp
public string packageId;
public string type;
public string code;
public string name;
public string packageDescription;
public int price;
public int totalPrice;
public int discount;
```

also, the `SibcheSubscriptionPackage` includes the following additional characteristics:

```csharp
public int duration;
public string group;
```

### SibchePurchasePackage

This package includes the user’s purchases corresponding with your packages. This class includes the following characteristics:


```csharp
public string purchasePackageId;
public string type;
public string code;
public DateTime expireAt;
public DateTime createdAt;
public SibchePackage package;
```

### SibcheError

You will be provided with this class should an error with the following characteristics occur:

```csharp
public string message;
public int errorCode;
public int statusCode;
```

- ErrorCode: the error code number can be observed in the following table.
-	Message: the error message which will be received from the server.
- StatusCode: the http error code which the server provides in response to our request.

| ErrorCode number | Reason                        |
| --------------- | ---------------------------------------- |
| 1000            | Unknown reason                             |
| 1001            | This package has be already purchased           |
| 1002            | User has quit the process                |
| 1003            | Login error |
| 1004            | Program has not initiated properly |

## How to get the list of available packages (unity):

After tuning the options for the program, you can view the available packages. You must simply use the following code in order to receive the function of interest:

```csharp
Sibche.FetchPackages((bool isSuccessful, SibcheError error, List<SibchePackage> packages) =>
{
    if (isSuccessful)
    {
        // Success block
    } else
    {
        // failure block
        Debug.Log(error.message);
    }
});
```

If successful, the developer kit will provide you with the available packages in the form of a parameter. This parameter is an array of the packages which are available for purchase. These packages are of the `SibchePackage` kind.

In case the request is not successful, a parameter called error, which is of the `SibcheError` kind, will be sent. If such is not the case, null will be sent.

##How to get package information (unity):

If you have the bundle code or ID of the package, you can obtain its information. The following is how to use this API:


```csharp
// Both package bundle code & package id is acceptable
Sibche.FetchPackage(packageId, (bool isSuccessful1, SibcheError error, SibchePackage package) =>
{
    if (isSuccessful)
    {
        // Success block
    } else
    {
        // failure block
        Debug.Log(error.message);
    }
});
```

The initially presented parameter is the very callback that we sent which, after getting checked, was recalled. If successful, the package of interest will be sent to you in the object form of `SibchePackage`.


##How to purchase a package (unity):

After obtaining the package list, you can use the following code in order to set a purchase request through the developer kit. Next, the kit will, if necessary, log the client in and check up on the payment procedure. Then, it will tell you if the purchase has been successful or not and will provide you with a `SibchePurchasePackage`.


```csharp
Sibche.Purchase(packageCode, (bool isSuccessful, SibcheError error, SibchePurchasePackage purchasedPackage) =>
{
    if (isSuccessful)
    {
        // Success block
    } else
    {
        // failure block
        Debug.Log(error.message);
    }
});
```

## How to obtain the list of previous purchases (unity):

You can use the following code to obtain your active (purchased) packages. Just simply call in the developer kit function as in the following code.

```csharp
Sibche.FetchActivePackages((bool isSuccessful, SibcheError error, List<SibchePurchasePackage> purchasedPackages) =>
{
    if (isSuccessful)
    {
        // Success block
    } else
    {
        // failure block
        Debug.Log(error.message);
    }
});
```

In response, the kit will provide you with information as to whether the request was successful or not, as well as an array of active purchased packages. Take note that this array is of the `SibchePurchasePackage` kind.


Active packages are those which have not yet run out of time or have not been used up completely. The Sibche definition for active packages for each of the package types is as follows:

- `SibcheConsumablePackage`: packages which have been purchased but have not yet been consumed.
- `SibcheNonConsumablePackage`: packages which can be bought once and therefore will remain active forever.
- `SibcheSubscriptionPackage`: packages which still have time until their expiry date.

## Using up packages on the part of the client (unity):


Note: this method should be used in order for the client to use the package in programs or games. However, if the server is to use or validate them, please refer to the next section. In order to use the consumable packages, you must recall the associated function from the developer kit, similar to the following code:

```csharp
Sibche.Consume(purchasedPackage.purchasePackageId, (bool isSuccessful, SibcheError error) =>
{
    if (isSuccessful)
    {
        // Success block
    } else
    {
        // failure block
        Debug.Log(error.message);
    }
});
```
In response, after the request status has been checked, the developer kit will recall the requested callback. If it is successful, it means that the package has been used up successfully, but if it is unsuccessful, it means that an error has occurred in the usage procedure.

## Using up packages on the part of the server (unity):

To do this, you must have `purchasePackageId` which is part of the `SibchePurchasePackage` class. Then, you must make a link similar to the following and receive the verification data from the server **(addresses are case sensitive!)**:

`https://api.sibche.com/sdk/userInAppPurchasePackages/{purchasePackageId}/verifyConsume`

Also, the HTTP header must be set onto the `App-Key` for this operation. This key is available for any specific program through the developer panel. This key is called the **server key** and has 30 characters.


A sample of code which can be recalled for curl can be as follows:

```shell
curl --header "App-Key: YOUR_SERVER_KEY" -X POST \
'https://api.sibche.com/sdk/userInAppPurchasePackages/1/verifyConsume'
```

In response, the server will provide one of these **HTTP Response Codes**:

- •	HTTP Status Code 404 (Not Found): 
this occurs either when you misspell the address link or package, or if the package has already been used. In this case the server will respond with something like this code:

```json
{
  "message" : "your package is already is used or not found",
  "status_code" : 404
}
```

- •	HTTP Status Code 401 (Unauthorized): 
this means that the server key which has been set on the app-key is invalid or wrong. In this case, the server will provide a similar response to the following:


```json
{
  "message" : "app-key is not valid",
  "status_code" : 401
}
```

- •	HTTP Status Code 202 (Accepted / OK):
 this means that the package has been successfully consumed and the purchase is valid. In this case, the server will return no written message in its response

##Client login request to the Sibche developer kit (unity):


Note: I recommend that you don’t use this section manually because if the client has not logged in during the payment process, the Sibche library will automatically attempt to do so.


By using this code, you can send a login request to the Sibche library. All you need to do is, as it is done below, recall the function relating to the developer kit.


```csharp
Sibche.Login((bool isLoginSuccessful, SibcheError error, string userName, string userId) =>
{
    // Login handle block
});
```

In response, the developer kit will provide information regarding the successfulness of the login and the errors it faced in the process.

## How to logout of the Sibche developer kit (unity):


Note: I highly recommend that you only use this function when the client has requested the retrieval of their purchase and has their retrieval request in hand.


```csharp
Sibche.Logout(() =>
{
    // Logout handle block
});
```


By the use of this code, you can logout the client who is already logged in to the developer kit and delete their session information. This function will provide a response to you in the form of a callback.


##How to obtain information on current clients (unity):


When you need to get the client’s phone number and userID you can use the following function:


```csharp
Sibche.GetCurrentUserData((bool isSuccessful, SibcheError error, LoginStatusType loginStatus, string userCellphoneNumber, string userId) =>
{
    // GetCurrentUserData handle block
});
```


In response, the developer kit will tell you if the process was successful or not and also recalls the error that may have occurred. In most cases the process will go smoothly, but in some rare cases, the following reasons could be why the process wasn’t successful:


- Connectivity issues
-	Network issues
- Sibche server is not accessible


In addition, a third parameter exists which tells you whether the client is logged in or not. if the client is logged in, the response type will be of this kind `loginStatusTypeIsLoggedIn`. If they are not logged in, the response type will be of the `loginStatusTypeIsLoggedOut` kind. If the process is not successful and we are not sure whether the client is logged in or not, we will get a response which is of the `loginStatusTypeHaveTokenButFailedToCheck` kind. The next parameters in the received response will include the client’s phone number and ID.
