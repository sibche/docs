#نصب کیت توسعه‌دهندگان b4i

##نیازمندیهای نصب کیت توسعه‌دهندگان b4i

1. بارگذاری برنامه در [پنل توسعه‌دهندگان سیبچه](https://sibche.com/developer)
2. دریافت کلید برنامه از پنل توسعه‌دهندگان
3. آخرین نسخه‌ی Xcode
4. داشتن آخرین نسخه b4i

## نصب کیت توسعه‌دهندگان سیبچه برای b4i

کیت توسعه‌دهندگان سیبچه مختص b4i را می‌توانید از [اینجا](https://github.com/sibche/Sibchelibrary-b4i/releases/latest/download/SibcheStoreKit-b4i.zip) دانلود کرده و به پروژه خود اضافه کنید.

برای نصب فریمورک سیبچه بایستی ابتدا فایل `libiSibcheStoreKit.a` را از داخل فایل زیپ دانلود شده بالا برداشته و به پروژه Xcode خود اضافه نمایید. همچنین فایل `SibcheStoreKit.framework` را نیز به پروژه Xcode خود اضافه نمایید.

سپس فایل پلاگین را انتخاب نموده و گزینه `add` را بزنید. سپس همانند شکل زیر، از بخش `General` از داخل تنظیمات پروژه، SibcheStoreKit.framework را از قسمت `Linked Frameworks and Libraries` حذف نمایید:

<a href="https://raw.githubusercontent.com/sibche/SibcheStoreKit/master/Screenshots/sc2.jpg">
    <img src="https://raw.githubusercontent.com/sibche/SibcheStoreKit/master/Screenshots/sc2.jpg"/>
</a>

سپس دکمه + از بخش `Embedded Binaries` را انتخاب نموده و `SibcheStoreKit.framework` را انتخاب نموده و دکمه `Add` را بزنید. همانند عکسهای زیر:

<a href="https://raw.githubusercontent.com/sibche/SibcheStoreKit/master/Screenshots/sc3.jpg">
    <img src="https://raw.githubusercontent.com/sibche/SibcheStoreKit/master/Screenshots/sc3.jpg"/>
</a>

<a href="https://raw.githubusercontent.com/sibche/SibcheStoreKit/master/Screenshots/sc4.jpg">
    <img src="https://raw.githubusercontent.com/sibche/SibcheStoreKit/master/Screenshots/sc4.jpg"/>
</a>

<a href="https://raw.githubusercontent.com/sibche/SibcheStoreKit/master/Screenshots/sc5.jpg">
    <img src="https://raw.githubusercontent.com/sibche/SibcheStoreKit/master/Screenshots/sc5.jpg"/>
</a>


## تنظیمات اولیه (b4i)

```vb
Library2=isibchestorekit

Sub Process_Globals
	Private ssk As SibcheStoreKit
End Sub
```

همانند کد روبرو، کیت توسعه‌دهندگان سیبچه را به پروژه `b4i` خود اضافه نمایید

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


### افزودن init اولیه کتابخانه سیبچه


```vb
Private Sub Application_Start (Nav As NavigationController)
	ssk.Initialize("ssk",YOUR_API_KEY,YOUR_SCHEME)
End Sub
```

در جایی از ورودی پروژه بایستی تابع init کتابخانه سیبچه را فراخوانی نمایید تا برای استفاده‌های آتی آماده شده باشد:

 به جای `YOUR_API_KEY` بایستی api key دریافت شده از پنل دولوپری سیبچه را وارد نمایید. همچنین به جای `YOUR_SCHEME‍` بایستی scheme تعریف شده از مراحل قبلی را وارد نمایید.

## کلاس‌های مورد استفاده در کیت توسعه‌دهندگان (b4i)

برای خرید، فعال‌سازی و استفاده از بسته‌های سیبچه، کلاسهای زیر ایجاد شده است که در ادامه در مورد هر کدام توضیحاتی ارائه خواهیم کرد.

- `SibchePackage`: این کلاس، نمایانگر بسته قابل خریدی هست که داخل پنل توسعه‌دهندگان اقدام به تعریفشان کرده‌اید. 

- `SibchePurchasePackage`: این کلاس، نمایانگر خرید متناظری از کاربر هست که به یک بسته SibchePackage اختصاص یافته است. این کلاس، شامل اطلاعات خرید، زمان خرید، و تاریخ انقضا و ... خواهد بود.

- `SibcheError`:این کلاس، در مواقع رخداد خطا به شما ارجاع داده خواهد شد که شامل اطلاعات بیشتر از خطاهای رخ داده است. اطلاعاتی از قبیل خطا و Http status code و ...
 
 
### SibchePackage
سه نوع بسته قابل خرید داریم که عبارتند از:

- 
  بسته‌های قابل خرید مصرفی یا `SibcheConsumablePackage`:
   بسته‌هایی که قابل مصرف هستند که خریداری شده و داخل بازی یا برنامه مصرف می‌شود. مانند بسته‌ی ۵۰۰ سکه طلا یا شارژ قابل مصرف داخل برنامه.

این بسته ها تا زمانی که مصرف نشده‌اند، فعال و معتبر می‌باشند و پس از مصرف، قابل خرید مجدد هستند. 


- 
  بسته‌های قابل خرید غیر مصرفی یا `SibcheNonConsumablePackage`:
  بسته‌هایی که قابل مصرف نیستند و فقط یکبار خریداری می‌شوند. مانند بسته‌ی باز شدن قابلیت آپلود آواتار یا تغییر نام.

این بسته ها، فقط یکبار قابل خرید هستند و پس از خرید، همواره در لیست بسته‌های فعال کاربر خواهند بود.



- 
  بسته‌های اشتراک یا `SibcheSubscriptionPackage`:
  بسته‌هایی هستند که به مدت محدود قابل استفاده بوده و پس از اتمام زمان آنها، کاربر مجاز به استفاده از آن قابلیت نیست. به عنوان مثال، قابلیت استفاده از امکانات ویژه به مدت یک سال


```vb
Public packageId As String
Public type As String
Public code As String
Public name As String
Public packageDescription As String
Public price As Int
Public totalPrice As Int
Public discount As Int
```

همه این سه کلاس از کلاس والد `SibchePackage` گرفته شده‌اند و به صورت عمومی شامل خصوصیات روبرو هستند.

<br/>
<br/>
<br/>
<br/>
<br/>
<br/>


```vb
Public duration As Int
Public group As String
```

علاوه بر این توابع، کلاس `SibcheSubscriptionPackage` شامل خصوصیات اضافی‌تر روبرو نیز هست:


### SibchePurchasePackage

```vb
Public purchasePackageId As String
Public type As String
Public code As String
Public expireAt As Date
Public createdAt As Date
Public package As SibchePackage
```

این کلاس، شامل تناظر خرید کاربر به بسته‌های شما می‌باشد. این کلاس شامل خصوصیات روبرو است:

<br/>
<br/>

### SibcheError

```vb
Public message As String
Public errorCode As Int
Public statusCode As Int
```

این کلاس در مواقع بروز خطا به شما داده خواهد شد و شامل خصوصیات روبرو می‌باشد:


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

## گرفتن لیست بسته‌های قابل خرید (b4i)

```vb
ssk.FetchAllInAppPurchasePackage
```

پس از تنظیم برنامه، میتوانید بسته‌های قابل خرید را مشاهده نمایید. کافیست همانند کد روبرو، اقدام به فراخوانی تابع مورد نظر نمایید:

<br/>

```vb
Sub ssk_AllPackageListArrived (PackageList As List)
	For Each package As SibchePackage In PackageList
		Log(package.name)
	Next
End Sub
```

در پاسخ، در صورت موفقیت، کیت توسعه‌دهندگان بسته‌های قابل خرید را در کالبک روبرو به شما تحویل میدهد. این پارامتر آرایه‌ای از بسته‌های قابل خرید می‌باشد. این بسته‌ها از نوع `SibchePackage` هستند. 

در صورت ناموفق بودن درخواست نیز، event خطا ارسال خواهد شد

## گرفتن اطلاعات بسته مشخص (b4i)

```vb
ssk.FetchInAppPurchasePackage(packageId)
```

با در اختیار داشتن آیدی یا کد باندل بسته مورد نظر می‌توانید اطلاعات آن بسته را در اختیار بگیرید. نحوه استفاده از این API به شکل روبرو است:

<br/>

```vb
Sub ssk_PackageInfoArrived (Package As SibchePackage)
    ' code
End Sub
```

در پاسخ، در صورت موفقیت، کیت توسعه‌دهندگان اطلاعات بسته‌ را در کالبک روبرو به شما تحویل می‌دهد. این پارامتر از نوع بسته‌های قابل خرید `SibchePackage` است.

در صورت ناموفق بودن درخواست نیز، event خطا ارسال خواهد شد


## خرید بسته مشخص (b4i)

```vb
ssk.PurchasePackage(packageId)
```

پس از گرفتن لیست بسته‌ها، میتوانید درخواست خرید این بسته‌ها را از طریق کد روبرو به کیت توسعه‌دهندگان بدهید. در ادامه، کیت توسعه‌دهندگان، در صورت نیاز کاربر را لاگین کرده و فرایند پرداخت را پیگیری خواهد کرد. سپس موفق یا ناموفق بودن خرید را به همراه `SibchePurchasePackage` به اطلاع شما خواهد رساند.

```vb
Sub ssk_PackagePurchased (PurchasedPackage As SibchePurchasePackage)
    ' code
End Sub
```

## گرفتن لیست بسته های خریداری شده (b4i)

```vb
ssk.FetchActiveInAppPurchasePackages
```

با استفاده از این دستور، میتوانید لیست بسته‌های فعال (خریداری شده) کاربر را بدست آورید. کافیست همانند کد روبر، تابع مربوطه کیت توسعه‌دهندگان را فراخوانی نمایید.

<br/>

```vb
Sub ssk_ActivePackageListArrived (ActivePackageList As List)
    ' code
End Sub
```

در پاسخ، کیت توسعه‌دهندگان موفقیت/عدم موفقیت درخواست و نیز آرایه‌ای از بسته‌های خریداری شده‌ی فعال را برمی‌گرداند. توجه نمایید که این آرایه، آرایه‌ای از نوع `SibchePurchasePackage` است.


منظور از بسته‌های فعال، بسته‌هایی هستند که خریداری شده‌اند و هنوز مصرف نشده‌اند و یا تاریخ انقضایشان به اتمام نرسیده است.
تعریف سیبچه از بسته‌های فعال برای هر کدام از نوع بسته‌ها به شرح روبرو می‌باشد:


- `SibcheConsumablePackage`: بسته‌هایی که خریداری شده‌اند ولی هنوز مصرف (Consume) نشده‌اند.
- `SibcheNonConsumablePackage`: بسته‌هایی که خریداری شده‌اند. چون این بسته‌ها یکبار خرید هستند، در صورت خرید، به صورت مادام‌العمر فعال هستند.
- `SibcheSubscriptionPackage`: بسته‌هایی که خریداری شده‌اند ولی هنوز از تاریخ انقضای آنها باقی مانده است.

## مصرف کردن بسته‌ها در سمت کلاینت (b4i)

<aside class="success">
در صورتی که بخواهید بسته‌های قابل مصرف را در سمت کلاینت (درون خود بازی/برنامه) مصرف کنید، بایستی از این روش استفاده کنید. ولی اگر قصد مصرف و اعتبارسنجی سمت سرور داشته باشید، به بخش بعدی مراجعه نمایید.
</aside>


```vb
ssk.ConsumePurchasePackage(purchasePackageId)
```

برای مصرف کردن بسته‌های قابل مصرف (Consumable) بایستی شبیه دستور روبرو، تابع مربوطه از کیت توسعه‌دهندگان را فراخوانی کنیم:

<br/>

```vb
Sub ssk_ConsumeSuccess
    ' code
End Sub
```

در پاسخ پس از مشخص شدن وضعیت درخواست، کیت توسعه‌دهندگان callback روبرو را فراخوانی خواهد کرد.
در صورت موفقیت، یعنی بسته مورد نظر با موفقیت مصرف شده و در صورت عدم موفقیت، در مصرف بسته مورد نظر، دچار مشکلی شده‌ایم.



## مصرف کردن بسته‌ها در سمت سرور (b4i)

برای این کار بایستی، کد `purchasePackageId` که بخشی از کلاس `SibchePurchasePackage` می‌باشد را در دست داشته باشید. سپس لینکی به شکل زیر درست کنید و دیتای وریفای را از سرور بگیرید **(آدرس‌ها Case sensitive هستند!)**:

`https://api.sibche.com/sdk/userInAppPurchasePackages/{purchasePackageId}/verifyConsume`

همچنین برای این درخواست، بایستی هدر HTTP با اسم `App-Key` تنظیم نمایید. این کلید از طریق پنل توسعه‌دهندگان به صورت اختصاصی برای هر برنامه قابل دریافت است. اسم این کلید، **کلید سرور** نام دارد و عبارت ۳۰ کاراکتری است.


```shell
curl --header "App-Key: YOUR_SERVER_KEY" -X POST \
'https://api.sibche.com/sdk/userInAppPurchasePackages/1/verifyConsume'
```

نمونه کد قابل فراخوانی برای curl به شکل روبرو خواهد بود:


در جواب، سرور پاسخی با یکی از **HTTP Response Code** های زیر خواهد داد:

- HTTP Status Code 404 (Not Found):

```json
{
  "message" : "your package is already is used or not found",
  "status_code" : 404
}
```

در صورتی که آدرس لینک را اشتباه وارد کرده باشید، یا بسته مورد نظر با purchasePackageId اشتباه وارد شده باشد، یا بسته مورد نظر قبلا مصرف شده باشد این خطا رخ خواهد داد.  
در این صورت، سرور در متن پاسخ، پاسخی شبیه روبرو خواهد داد:


- HTTP Status Code 401 (Unauthorized):

```json
{
  "message" : "app-key is not valid",
  "status_code" : 401
}
```

یعنی اینکه کلید سروری که بر روی App-Key تنظیم شده است، غیر معتبر و اشتباه است.
در این صورت، سرور در متن پاسخ، پاسخی شبیه روبرو خواهد داد:



- HTTP Status Code 202 (Accepted / OK):


در این صورت یعنی، بسته مورد نظر با موفقیت مصرف شد و خرید کاربر، معتبر بوده است.
برای این حالت،‌ سرور در متن پاسخ، هیچ نوشته‌ای بر نخواهد گرداند.


## درخواست وارد شدن کاربر به کیت توسعه‌دهندگان سیبچه (b4i)

<aside class="success">
توصیه می‌کنیم از این بخش به صورت دستی استفاده نکنید چرا که در فرآیند پرداخت، اگر کاربر لاگین نکرده باشد، کتابخانه سیبچه خود اقدام به لاگین کاربر می‌کند.
</aside>


```vb
ssk.LoginUser
```

با استفاده از این دستور، میتوانید به کتابخانه سیبچه، درخواست لاگین کاربر را بدهید. کافیست همانند کد روبرو، تابع مربوطه کیت توسعه‌دهندگان را فراخوانی نمایید.

<br/>

```vb
Sub ssk_LoginSuccess(UserName As String, UserId As String)
    ' code
End Sub
```

در جواب، کیت توسعه‌دهندگان، موفقیت لاگین و اروری که بهش برخورده را برمیگرداند. همچنین، در صورت موفقیت و در صورت موجود بودن، نام کاربر و آیدی کاربر را برمی‌گرداند.


## درخواست خارج شدن کاربر از کیت توسعه‌دهندگان سیبچه (b4i)


```vb
Sibche.LogoutUser
```

توصیه می‌کنیم، زمانی از این تابع استفاده نمایید که کاربر دکمه بازیابی خرید را زده و درخواست بازیابی خرید خود را دارد.

<br/>

```vb
Sub ssk_LogoutSuccess
    ' code
End Sub
```

با استفاده از این دستور می‌توانید کاربر فعلی لاگین کرده داخل سیستم را از کیت توسعه‌دهندگان خارج نمایید و کلیه اطلاعات session او را پاک نمایید.
این تابع پس از اتمام کار، در قالب callback به شما جواب خواهد داد.


## گرفتن اطلاعات کاربر فعلی (b4i)


```vb
Sibche.GetCurrentUserData
```

زمانی که نیاز دارید تا شماره تلفن و نیز شماره کاربری (userId) کاربر را دریافت نمایید، می‌توانید از تابع روبرو استفاده نمایید.

<br/>

```vb
Sub ssk_UserDataArrived(UserPhoneNumber As String, UserId As String, LoginStatus As String)
    ' code
End Sub
```

در جواب، کیت توسعه‌دهندگان، نتیجه موفق بودن عملیات و نیز ارور رخ داده را برمی‌گرداند. در حالت عادی، بایستی عملیات موفق باشد ولی ناموفق بودن عملیات می‌تواند به دلایل شبکه‌ای مختلفی همانند موارد زیر باشد:


- دسترسی به اینترنت مقدور نبود
- هر اتفاق ناخواسته شبکه‌ای رخ داده است
- سرور سیبچه در دسترس نیست


همچنین در جواب، پارامتر سومی هم دارد که وضعیت کاربر را از لحاظ لاگین بودن/نبودن به اطلاع شما می‌رساند. در صورتی که لاگین باشد، از نوع `‍loginStatusTypeIsLoggedIn‍‍` جواب خواهد داد. در صورت لاگین نبودن نیز جواب داده شده از نوع `loginStatusTypeIsLoggedOut` می‌باشد. در صورتی که عملیات ناموفق باشد و از وضعیت لاگین بودن کاربر مطمئن نباشیم نیز جواب ‍`loginStatusTypeHaveTokenButFailedToCheck` را خواهیم داد.
پارامتر‌های بعدی نیز شامل شماره تلفن کاربر و نیز id کاربر می‌باشد که در پاسخ به شما داده خواهد شد.

