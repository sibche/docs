# نصب کیت توسعه‌دهندگان ری‌اکت نیتیو

## نیازمندیهای نصب کیت توسعه‌دهندگان

1. بارگذاری برنامه در [پنل توسعه‌دهندگان سیبچه](https://sibche.com/developer)
2. دریافت کلید برنامه از پنل توسعه‌دهندگان
3. آخرین نسخه‌ی Xcode

## نصب کیت توسعه‌دهندگان ری‌اکت نیتیو

```bash
npm install --save react-native-sibche-storekit

## OR

yarn add react-native-sibche-storekit
```

کافی است در داخل پروژه ری‌اکت خود دستور روبرو را اجرا نمایید. با این کار، پکیج سیبچه بر روی پروژه شما نصب خواهد شد.

> سپس از طریق پاد، پلاگین سیبچه را نصب نمایید

```shell
pod repo update
pod install
```

## تنظیمات اولیه

### افزودن Scheme مختص برنامه شما

<aside class="success">
اگر برنامه شما دارای scheme اختصاصی می‌باشد این مرحله را رد کنید.
</aside>

برای اضافه کردن scheme اختصاصی بایستی طبق مراحل زیر تنظیمات برنامه را داخل Xcode باز کرده و سپس به تب info مراجعه نمایید:

<a href="https://raw.githubusercontent.com/sibche/SibcheStoreKit/master/Screenshots/sc10.jpg">
    <img src="https://raw.githubusercontent.com/sibche/SibcheStoreKit/master/Screenshots/sc10.jpg"/>
</a>

<a href="https://raw.githubusercontent.com/sibche/SibcheStoreKit/master/Screenshots/sc11.jpg">
    <img src="https://raw.githubusercontent.com/sibche/SibcheStoreKit/master/Screenshots/sc11.jpg"/>
</a>

سپس همانند شکل، url اختصاصی اپلیکیشن را اضافه نمایید:

<a href="https://raw.githubusercontent.com/sibche/SibcheStoreKit/master/Screenshots/sc12.jpg">
    <img src="https://raw.githubusercontent.com/sibche/SibcheStoreKit/master/Screenshots/sc12.jpg"/>
</a>

به جای test، نام دلخواهی را تنظیم کرده و به جای testapp بایستی scheme مورد نظرتان را وارد نمایید. به عنوان مثال scheme تنظیم شده **تلگرام** `tg` و scheme تنظیم شده برای برنامه **اینستاگرام** `instagram` میباشد. لیست کامل scheme برنامه‌های معروف را میتوانید از
[اینجا](https://ios.gadgethacks.com/news/always-updated-list-ios-app-url-scheme-names-0184033/)
مشاهده نمایید.

<aside class="warning">
توصیه اکید می‌شود از scheme استفاده کنید که مختص شما باشد و ترجیحا طولانی باشد. توجه فرمایید که اگر با برنامه‌های دیگر در تداخل باشد، در فرآیند پرداخت دچار مشکل خواهید شد.
</aside>

### افزودن کیت توسعه‌دهنده به AppDelegate

```objective_c
#import "AppDelegate.h"
#import <SibcheStoreKit/SibcheStoreKit.h>

- (BOOL)application:(UIApplication *)application didFinishLaunchingWithOptions:(NSDictionary *)launchOptions
{
  [SibcheStoreKit initWithApiKey:YOUR_API_KEY withScheme:YOUR_SCHEME];

  RCTBridge *bridge = [[RCTBridge alloc] initWithDelegate:self launchOptions:launchOptions];
  RCTRootView *rootView = [[RCTRootView alloc] initWithBridge:bridge
                                                   moduleName:@"CallRecorder"
                                            initialProperties:nil];
  ...
}                                            
```

ابتدا کد اجرای کیت توسعه‌دهندگان را داخل تابع `didFinishLaunchingWithOptions` اضافه نمایید:

(برای دیدن کد این قسمت، بایستی از تب بالا، زبان Objective-c را انتخاب نمایید)

<br/>

لازم به ذکر است که به جای `YOUR_API_KEY` بایستی کلید برنامه گرفته شده از پنل توسعه‌دهندگان سیبچه را قرار دهید و به جای `YOUR_SCHEME` واژه scheme اضافه شده در مرحله قبل را جایگذاری نمایید.

<br/>
<br/>
<br/>

```objective_c
- (BOOL)application:(UIApplication *)application openURL:(NSURL *)url
  sourceApplication:(NSString *)sourceApplication annotation:(id)annotation
{
  [SibcheStoreKit openUrl:url options:nil];
  return [RCTLinkingManager application:application openURL:url
                      sourceApplication:sourceApplication annotation:annotation];
}
```

سپس امکان فراخوانی باز شدن url را نیز به کیت توسعه‌دهندگان بدهید:

## کلاس‌های مورد استفاده در کیت توسعه‌دهندگان

برای خرید، فعال‌سازی و استفاده از بسته‌های سیبچه، کلاسهای زیر ایجاد شده است که در ادامه در مورد هر کدام توضیحاتی ارائه خواهیم کرد.

- `SibchePackage`:
این کلاس، نمایانگر بسته قابل خریدی هست که داخل پنل توسعه‌دهندگان اقدام به تعریفشان کرده‌اید. 

- `SibchePurchasePackage`:
این کلاس، نمایانگر خرید متناظری از کاربر هست که به یک بسته SibchePackage اختصاص یافته است. این کلاس، شامل اطلاعات خرید، زمان خرید، و تاریخ انقضا و ... خواهد بود.

- `SibcheError`:
این کلاس، در مواقع رخداد خطا به شما ارجاع داده خواهد شد که شامل اطلاعات بیشتر از خطاهای رخ داده است. اطلاعاتی از قبیل خطا و Http status code و ...

### SibchePackage
سه نوع بسته قابل خرید داریم که عبارتند از:

- بسته‌های قابل خرید مصرفی یا `SibcheConsumablePackage`:
 بسته‌هایی که قابل مصرف هستند که خریداری شده و داخل بازی یا برنامه مصرف می‌شود. مانند بسته‌ی ۵۰۰ سکه طلا یا شارژ قابل مصرف داخل برنامه.

این بسته ها تا زمانی که مصرف نشده‌اند، فعال و معتبر می‌باشند و پس از مصرف، قابل خرید مجدد هستند. 

- بسته‌های قابل خرید غیر مصرفی یا `SibcheNonConsumablePackage`:
بسته‌هایی که قابل مصرف نیستند و فقط یکبار خریداری می‌شوند. مانند بسته‌ی باز شدن قابلیت آپلود آواتار یا تغییر نام.

این بسته ها، فقط یکبار قابل خرید هستند و پس از خرید، همواره در لیست بسته‌های فعال کاربر خواهند بود.


- بسته‌های اشتراک یا `SibcheSubscriptionPackage`:
بسته‌هایی هستند که به مدت محدود قابل استفاده بوده و پس از اتمام زمان آنها، کاربر مجاز به استفاده از آن قابلیت نیست. به عنوان مثال، قابلیت استفاده از امکانات ویژه به مدت یک سال

```javascript
{
    packageId: string,
    type: string,
    code: string,
    name: string,
    packageDescription: string,
    price: number,
    totalPrice: number,
    discount: number
}
```

همه این سه مدل از مدل والد `SibchePackage` گرفته شده‌اند و به صورت عمومی شامل توابع روبرو هستند.

<br/>
<br/>
<br/>
<br/>
<br/>
<br/>
<br/>
<br/>

```javascript
{
    duration: number,
    group: string
}
```

علاوه بر این توابع، کلاس `SibcheSubscriptionPackage` شامل توابع اضافی تر روبرو نیز هست:

<br/>

### SibchePurchasePackage

```javascript
{
    purchasePackageId: string,
    type: string,
    code: string,
    expireAt: number,
    createdAt: number,
    package: SibchePackage
}
```

این کلاس، شامل تناظر خرید کاربر به بسته‌های شما می‌باشد. این کلاس شامل توابع روبروست:

<br/>
<br/>
<br/>

### SibcheError

```javascript
{
    errorCode: number,
    message: string,
    statusCode: number
}
```

این کلاس در مواقع بروز خطا به شما داده خواهد شد و شامل خصوصیات روبرو می‌باشد:

<br/>
<br/>

- errorCode: همان شماره خطایی هست که مطابق جدول زیر ایجاد شده است.
- message: پیغام خطایی هست که از طرف سرور دریافت شده است.
- statusCode:  همان شماره خطای http هست که سرور در جواب درخواست ما داده است.

| شماره errorCode | دلیل خطای مربوطه                         |
| --------------- | ---------------------------------------- |
| 1000            | خطای نامشخص                              |
| 1001            | این بسته قبلا خریداری شده است            |
| 1002            | کاربر از ادامه عملیات منصرف شد                 |
| 1003            | در فرایند ورود (لاگین) دچار مشکل شده‌ایم |
| 1004            |  برنامه به درستی initiate نشده است |

## گرفتن لیست بسته‌های قابل خرید

```javascript
import SibcheStoreKit from "react-native-sibche-storekit";


SibcheStoreKit.fetchInAppPurchasePackages((isSuccessful, error, packagesArray) => {
    console.log(isSuccessful, error, packagesArray);
});
```

پس از تنظیم برنامه، میتوانید بسته‌های قابل خرید را مشاهده نمایید. کافیست همانند کد روبرو اقدام به فراخوانی تابع مورد نظر نمایید:

<br/>

در پاسخ، در صورت موفقیت، کیت توسعه‌دهندگان بسته‌های قابل خرید را به عنوان پارامتر پاسخ به شما تحویل میدهد. این پارامتر آرایه‌ای از بسته‌های قابل خرید می‌باشد. این بسته‌ها از نوع `SibchePackage` هستند. 

در صورت ناموفق بودن درخواست، پارمتری با نام error از نوع `‍‍SibcheError` ارسال می‌شود.

## گرفتن اطلاعات بسته مشخص

```javascript
SibcheStoreKit.fetchInAppPurchasePackage(PACKAGE_ID_OR_CODE, (isSuccessful, error, packageItem) => {
    console.log("=====> specific package", isSuccessful, error, packageItem);
});
```

با در اختیار داشتن آیدی یا کد باندل بسته مورد نظر می‌توانید اطلاعات آن بسته را در اختیار بگیرید. نحوه استفاده از این API به شکل روبرو است:

<br/>

پارامتر اول داده شده، همان callback ارسال شده ما است که پس از مشخص شدن وضعیت درخواست، فراخوانی خواهد شد. در صورت موفقیت، بسته‌ی مورد نظر در قالب آبجکت `SibchePackage` (بسته به نوع بسته) به شما ارسال خواهد شد.

## خرید بسته مشخص

```javascript
SibcheStoreKit.purchasePackage("ava.test", (isSuccessful, error, purchasePackage) => {
    console.log("Result of purchase", isSuccessful, error, purchasePackage);
    if (isSuccessful) {
        // Success block
    }else{
        // Failure block
    }
});
```

پس از گرفتن لیست بسته‌ها، میتوانید درخواست خرید این بسته‌ها را از طریق کد روبرو به کیت توسعه‌دهندگان بدهید. در ادامه، کیت توسعه‌دهندگان، در صورت نیاز کاربر را لاگین کرده و فرایند پرداخت را پیگیری خواهد کرد. سپس موفق یا ناموفق بودن خرید را به همراه `SibchePurchasePackage` به اطلاع شما خواهد رساند.

<br/>

## گرفتن لیست بسته های خریداری شده

```javascript
SibcheStoreKit.fetchActiveInAppPurchasePackages((isSuccessful, error, purchasePackagesArray) => {
    console.log("=====> Active pacakges", isSuccessful, error, purchasePackagesArray);
});
```

با استفاده از این دستور، میتوانید لیست بسته‌های فعال (خریداری شده) کاربر را بدست آورید. کافیست همانند کد روبر، تابع مربوطه کیت توسعه‌دهندگان را فراخوانی نمایید.

<br/>
<br/>

در پاسخ، کیت توسعه‌دهندگان موفقیت/عدم موفقیت درخواست و نیز آرایه‌ای از بسته‌های خریداری شده‌ی فعال را برمی‌گرداند. توجه نمایید که این آرایه، آرایه‌ای از نوع `SibchePurchasePackage` است.

منظور از بسته‌های فعال، بسته‌هایی هستند که خریداری شده‌اند و هنوز مصرف نشده‌اند و یا تاریخ انقضایشان به اتمام نرسیده است.
تعریف سیبچه از بسته‌های فعال برای هر کدام از نوع بسته‌ها به شرح زیر می‌باشد:

- `SibcheConsumablePackage`: بسته‌هایی که خریداری شده‌اند ولی هنوز مصرف (Consume) نشده‌اند.
- `SibcheNonConsumablePackage`: بسته‌هایی که خریداری شده‌اند. چون این بسته‌ها یکبار خرید هستند، در صورت خرید، به صورت مادام‌العمر فعال هستند.
- `SibcheSubscriptionPackage`: بسته‌هایی که خریداری شده‌اند ولی هنوز از تاریخ انقضای آنها باقی مانده است.

## مصرف کردن بسته‌ها در سمت کلاینت
<aside class="success">
در صورتی که بخواهید بسته‌های قابل مصرف را در سمت کلاینت (درون خود بازی/برنامه) مصرف کنید، بایستی از این روش استفاده کنید. ولی اگر قصد مصرف و اعتبارسنجی سمت سرور داشته باشید، به بخش بعدی مراجعه نمایید.
</aside>

```javascript
SibcheStoreKit.consumePurchasePackage(purchasePackage.purchasePackageId, (isSuccessful, error) => {
    console.log("Result of consume", isSuccessful, error);
});
```

برای مصرف کردن بسته‌های قابل مصرف (Consumable) بایستی شبیه دستور روبرو، تابع مربوطه از کیت توسعه‌دهندگان را فراخوانی کنیم:

در پاسخ پس از مشخص شدن وضعیت درخواست، کیت توسعه‌دهندگان callback داده شده را فراخوانی خواهد کرد.
در صورت موفقیت، یعنی بسته مورد نظر با موفقیت مصرف شده و در صورت عدم موفقیت، در مصرف بسته مورد نظر، دچار مشکلی شده‌ایم.

## مصرف کردن بسته‌ها در سمت سرور

برای این کار بایستی، کد `purchasePackageId` که بخشی از کلاس `SibchePurchasePackage` می‌باشد را در دست داشته باشید. سپس لینکی به شکل زیر درست کنید و دیتای وریفای را از سرور بگیرید **(آدرس‌ها Case sensitive هستند!)**:

`https://api.sibche.com/sdk/userInAppPurchasePackages/{purchasePackageId}/verifyConsume`

همچنین برای این درخواست، بایستی هدر HTTP با اسم `App-Key` تنظیم نمایید. این کلید از طریق پنل توسعه‌دهندگان به صورت اختصاصی برای هر برنامه قابل دریافت است. اسم این کلید، **کلید سرور** نام دارد و عبارت ۳۰ کاراکتری است.

```shell
curl --header "App-Key: YOUR_SERVER_KEY" -X POST \
'https://api.sibche.com/sdk/userInAppPurchasePackages/1/verifyConsume'
```

نمونه کد قابل فراخوانی برای curl به شکل روبرو خواهد بود:

<br/>
<br/>

در جواب، سرور پاسخی با یکی از **HTTP Response Code** های زیر خواهد داد:

```json
{
  "message" : "your package is already is used or not found",
  "status_code" : 404
}
```

- HTTP Status Code 404 (Not Found):
در صورتی که آدرس لینک را اشتباه وارد کرده باشید، یا بسته مورد نظر با purchasePackageId اشتباه وارد شده باشد، یا بسته مورد نظر قبلا مصرف شده باشد این خطا رخ خواهد داد.  
در این صورت، سرور در متن پاسخ، پاسخی شبیه روبرو خواهد داد:

<br/>
<br/>

```json
{
  "message" : "app-key is not valid",
  "status_code" : 401
}
```

- HTTP Status Code 401 (Unauthorized):
یعنی اینکه کلید سروری که بر روی App-Key تنظیم شده است، غیر معتبر و اشتباه است.
در این صورت، سرور در متن پاسخ، پاسخی شبیه روبرو خواهد داد:

<br/>
<br/>

- HTTP Status Code 202 (Accepted / OK):
در این صورت یعنی، بسته مورد نظر با موفقیت مصرف شد و خرید کاربر، معتبر بوده است.
برای این حالت،‌ سرور در متن پاسخ، هیچ نوشته‌ای بر نخواهد گرداند.

## درخواست وارد شدن (Login) کاربر به کیت توسعه‌دهندگان سیبچه

<aside class="warning">
توصیه می‌کنیم از این بخش به صورت دستی استفاده نکنید چرا که در فرآیند پرداخت، اگر کاربر لاگین نکرده باشد، کتابخانه سیبچه خود اقدام به لاگین کاربر می‌کند.
</aside>

```javascript
SibcheStoreKit.loginUser((isSuccessful, error, userName, userId) => {
    if (isSuccessful) {
        // Successful login
    }else{
        // Failed login
    }
})
```

با استفاده از این دستور، میتوانید به کتابخانه سیبچه، درخواست لاگین کاربر را بدهید. کافیست همانند کد روبر، تابع مربوطه کیت توسعه‌دهندگان را فراخوانی نمایید.

<br/>
<br/>

در جواب، کیت توسعه‌دهندگان، موفقیت لاگین و اروری که بهش برخورده را برمیگرداند. همچنین، در صورت موفقیت و در صورت موجود بودن، نام کاربر و آیدی کاربر را برمی‌گرداند.

## درخواست خارج شدن (Logout) کاربر از کیت توسعه‌دهندگان سیبچه

<aside class="warning">
توصیه اکید میکنیم، زمانی از این تابع استفاده نمایید که کاربر دکمه بازیابی خرید را زده و درخواست بازیابی خرید خود را دارد.
</aside>

```javascript
SibcheStoreKit.logoutUser(() => {
    console.log("Logging out finished");
})
```

با استفاده از این دستور می‌توانید کاربر فعلی لاگین کرده داخل سیستم را از کیت توسعه‌دهندگان خارج نمایید و کلیه اطلاعات session او را پاک نمایید.
این تابع پس از اتمام کار، در قالب callback به شما جواب خواهد داد.

## گرفتن اطلاعات کاربر فعلی

```javascript
SibcheStoreKit.getCurrentUserData((isSuccessful, error, loginStatus, userCellphoneNumber, userId) => {
    console.log("Get current user data", isSuccessful, error, loginStatus, userCellphoneNumber, userId);
})
```

زمانی که نیاز دارید تا شماره تلفن و نیز شماره کاربری (userId) کاربر را دریافت نمایید، می‌توانید از تابع روبرو استفاده نمایید.

<br/>
<br/>

در جواب، کیت توسعه‌دهندگان، نتیجه موفق بودن عملیات و نیز ارور رخ داده را برمی‌گرداند. در حالت عادی، بایستی عملیات موفق باشد ولی ناموفق بودن عملیات می‌تواند به دلایل شبکه‌ای مختلفی همانند موارد زیر باشد:

- دسترسی به اینترنت مقدور نبود
- هر اتفاق ناخواسته شبکه‌ای رخ داده است
- سرور سیبچه در دسترس نیست

همچنین در جواب، پارامتر سومی هم دارد که وضعیت کاربر را از لحاظ لاگین بودن/نبودن به اطلاع شما می‌رساند. در صورتی که لاگین باشد، از نوع `‍loginStatusTypeIsLoggedIn‍‍` جواب خواهد داد. در صورت لاگین نبودن نیز جواب داده شده از نوع `loginStatusTypeIsLoggedOut` می‌باشد. در صورتی که عملیات ناموفق باشد و از وضعیت لاگین بودن کاربر مطمئن نباشیم نیز جواب ‍`loginStatusTypeHaveTokenButFailedToCheck` را خواهیم داد.
پارامتر‌های بعدی نیز شامل شماره تلفن کاربر و نیز id کاربر می‌باشد که در پاسخ به شما داده خواهد شد.
