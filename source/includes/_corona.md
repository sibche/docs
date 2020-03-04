# نصب کیت توسعه‌دهندگان کرونا

## نیازمندیهای نصب پلاگین کرونا

1. بارگذاری برنامه در [پنل توسعه‌دهندگان سیبچه](https://sibche.com/developer)
2. دریافت کلید برنامه از پنل توسعه‌دهندگان
3. آخرین نسخه‌ی Xcode
4. داشتن آخرین نسخه از کرونا

## نصب پلاگین کرونا

### فعالسازی پلاگین بر روی اکانت از طریق وب‌سایت کرونا
ابتدا به این لینک مراجعه کرده و با اکانت کرونا لاگین کرده و به صورت رایگان فعالسازی نمایید

### اضافه کردن سیبچه به پروژه و دانلود آن

```lua
plugins =
{
    ["plugin.SibcheStoreKit"] =
    {
        publisherId = "com.sibche",
    }
}
```

فایل `build.settings` را باز کرده و تکه کد روبرو را داخل `settings = {` وارد نمایید.


<br/>
<br/>
<br/>
<br/>
<br/>

و در انتها از طریق پروژه xcode، گزینه `Download Plugins` را اجرا نمایید.

## تنظیمات اولیه (کرونا)

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


### افزودن فایل wrapper پروژه

فایل `wrapper.lua` را از [اینجا](https://raw.githubusercontent.com/sibche/SibcheStoreKit-Corona/master/Corona/sibche/wrapper.lua) دانلود کرده و سپس داخل فولدری به اسم `sibche` داخل پروژه کرونا اضافه کنید.

این فایل وظیفه ساده‌سازی API های پلاگین سیبچه را بر عهده دارد. همچنین ورودی‌ها و خروجی‌های API ها را به صورت خلاصه داخل این فایل توضیح داده‌ایم.

### افزودن init اولیه کتابخانه سیبچه

```lua
local SibcheStoreKit = require "sibche.wrapper"

SibcheStoreKit.init(YOUR_API_KEY, YOUR_SCHEME)
```

بایستی در ابتدای برنامه، پلاگین سیبچه را وارد کرده و متد `init` را همانند کد روبرو فراخوانی نمایید

<br/>
<br/>

 به جای `YOUR_API_KEY` بایستی api key دریافت شده از پنل دولوپری سیبچه را وارد نمایید. همچنین به جای `YOUR_SCHEME‍` بایستی scheme تعریف شده از مراحل قبلی را وارد نمایید.

## آبجکت‌های مورد استفاده در پلاگین سیبچه (کرونا)

برای خرید، فعال‌سازی و استفاده از بسته‌های سیبچه، آبجکت (table) های زیر ایجاد شده است که در ادامه در مورد هر کدام توضیحاتی ارائه خواهیم کرد.

- `SibchePackage`: این آبجکت، نمایانگر بسته قابل خریدی هست که داخل پنل توسعه‌دهندگان اقدام به تعریفشان کرده‌اید. 

- `SibchePurchasePackage`: این آبجکت، نمایانگر خرید متناظری از کاربر هست که به یک بسته SibchePackage اختصاص یافته است. این آبجکت، شامل اطلاعات خرید، زمان خرید، و تاریخ انقضا و ... خواهد بود.

- `SibcheError`:این آبجکت، در مواقع رخداد خطا به شما ارجاع داده خواهد شد که شامل اطلاعات بیشتر از خطاهای رخ داده است. اطلاعاتی از قبیل خطا و Http status code و ...
 
 
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

<br/>

```lua
-- sibchePackage.packageId (string)
-- sibchePackage.type (string)
-- sibchePackage.code (string)
-- sibchePackage.name (string)
-- sibchePackage.packageDescription (string)
-- sibchePackage.price (int)
-- sibchePackage.totalPrice (int)
-- sibchePackage.discount (int)
```

// TODO Check and fill DateTime types () empty brackets

همه این سه آبجکت از آبجکت والد `SibchePackage` گرفته شده‌اند و به صورت عمومی شامل خصوصیات روبرو هستند.

<br/>
<br/>
<br/>
<br/>
<br/>
<br/>

```lua
-- sibchePackage.duration (int)
-- sibchePackage.group (string)
```

علاوه بر این خصوصیات، آبجکت `SibcheSubscriptionPackage` شامل خصوصیات اضافی‌تر روبرو نیز هست:


### SibchePurchasePackage

```lua
-- sibchePurchasePackage.purchasePackageId (string)
-- sibchePurchasePackage.type (string)
-- sibchePurchasePackage.code (string)
-- sibchePurchasePackage.expireAt ()
-- sibchePurchasePackage.createdAt ()
-- sibchePurchasePackage.package (SibchePackage)
```

این آبجکت، شامل تناظر خرید کاربر به بسته‌های شما می‌باشد. این آبجکت شامل خصوصیات روبرو است:

<br/>
<br/>
<br/>

### SibcheError

```lua
-- event.errorMessage;
-- event.errorCode;
-- event.errorStatusCode;
```

این آبجکت در مواقع بروز خطا داخل همان پارامترهای callback به شما داده خواهد شد و شامل خصوصیات زیر می‌باشد:


- errorCode: همان شماره خطایی هست که مطابق جدول زیر ایجاد شده است.
- errorMessage: پیغام خطایی هست که از طرف سرور دریافت شده است.
- errorStatusCode:  همان شماره خطای http هست که سرور در جواب درخواست ما داده است.

| شماره errorCode | دلیل خطای مربوطه                         |
| --------------- | ---------------------------------------- |
| 1000            | خطای نامشخص                              |
| 1001            | این بسته قبلا خریداری شده است            |
| 1002            | کاربر از ادامه عملیات منصرف شد                 |
| 1003            | در فرایند ورود (لاگین) دچار مشکل شده‌ایم |
| 1004            |  برنامه به درستی initiate نشده است |

## گرفتن لیست بسته‌های قابل خرید (کرونا)

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

پس از تنظیم برنامه، میتوانید بسته‌های قابل خرید را مشاهده نمایید. کافیست همانند کد روبرو اقدام به فراخوانی تابع مورد نظر نمایید:


<aside class="success">
توجه نمایید که inspect استفاده شده، لایبرری ای جهت مشاهده محتویات table میباشد که از 
<a href="https://raw.githubusercontent.com/sibche/SibcheStoreKit-Corona/master/Corona/inspect.lua">اینجا
</a>
قابل دریافت و استفاده است
</aside>

<br/>
<br/>
<br/>

در پاسخ، در صورت موفقیت، پلاگین بسته‌های قابل خرید را به عنوان پارامتر پاسخ به شما تحویل میدهد. این پارامتر آرایه‌ای از بسته‌های قابل خرید می‌باشد. این بسته‌ها از نوع `SibchePackage` هستند. 


در صورت ناموفق بودن درخواست، پارمترهایی با نام `errorMessage` و `errorCode` و `errorStatusCode` ارسال می‌شود، وگرنه به صورت null داده خواهد شد.

## گرفتن اطلاعات بسته مشخص (کرونا)

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

با در اختیار داشتن آیدی یا کد باندل بسته مورد نظر می‌توانید اطلاعات آن بسته را در اختیار بگیرید. نحوه استفاده از این API به شکل روبرو است:

<br/>
<br/>

پارامتر دوم داده شده، همان callback ارسال شده ما است که پس از مشخص شدن وضعیت درخواست، فراخوانی خواهد شد. در صورت موفقیت، بسته‌ی مورد نظر در قالب آبجکت `SibchePackage` (بسته به نوع بسته) به شما ارسال خواهد شد.


## خرید بسته مشخص (کرونا)

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

پس از گرفتن لیست بسته‌ها، میتوانید درخواست خرید این بسته‌ها را از طریق کد روبرو به پلاگین بدهید. در ادامه، کیت توسعه‌دهندگان، در صورت نیاز کاربر را لاگین کرده و فرایند پرداخت را پیگیری خواهد کرد. سپس موفق یا ناموفق بودن خرید را به همراه `SibchePurchasePackage` به اطلاع شما خواهد رساند.

## گرفتن لیست بسته های خریداری شده (کرونا)

```lua
local function fetchActiveInAppPurchasePackagesCallback( event )
    print(inspect(event))
end

SibcheStoreKit.fetchActiveInAppPurchasePackages(fetchActiveInAppPurchasePackagesCallback)
```

با استفاده از این دستور، میتوانید لیست بسته‌های فعال (خریداری شده) کاربر را بدست آورید. کافیست همانند کد روبرو، تابع مربوطه کیت توسعه‌دهندگان را فراخوانی نمایید.

<br/>
<br/>
<br/>

در پاسخ، کیت توسعه‌دهندگان موفقیت/عدم موفقیت درخواست و نیز آرایه‌ای از بسته‌های خریداری شده‌ی فعال را برمی‌گرداند. توجه نمایید که این آرایه، آرایه‌ای از نوع `SibchePurchasePackage` است.


منظور از بسته‌های فعال، بسته‌هایی هستند که خریداری شده‌اند و هنوز مصرف نشده‌اند و یا تاریخ انقضایشان به اتمام نرسیده است.
تعریف سیبچه از بسته‌های فعال برای هر کدام از نوع بسته‌ها به شرح زیر می‌باشد:


- `SibcheConsumablePackage`: بسته‌هایی که خریداری شده‌اند ولی هنوز مصرف (Consume) نشده‌اند.
- `SibcheNonConsumablePackage`: بسته‌هایی که خریداری شده‌اند. چون این بسته‌ها یکبار خرید هستند، در صورت خرید، به صورت مادام‌العمر فعال هستند.
- `SibcheSubscriptionPackage`: بسته‌هایی که خریداری شده‌اند ولی هنوز از تاریخ انقضای آنها باقی مانده است.

## مصرف کردن بسته‌ها در سمت کلاینت (کرونا)


<aside class="warning">
در صورتی که بخواهید بسته‌های قابل مصرف را در سمت کلاینت (درون خود بازی/برنامه) مصرف کنید، بایستی از این روش استفاده کنید. ولی اگر قصد مصرف و اعتبارسنجی سمت سرور داشته باشید، به بخش بعدی مراجعه نمایید.
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

برای مصرف کردن بسته‌های قابل مصرف (Consumable) بایستی شبیه دستور روبرو، تابع مربوطه از پلاگین را فراخوانی کنیم:

<br/>
<br/>

<aside class="success">
تکه کد روبرو، پس از خرید موفق، اقدام به مصرف بسته می‌کند و صرفا جهت نمایش نحوه استفاده از بسته نوشته شده است و بایستی متناسب کارکرد بازیتان تغییرش بدهید.
</aside>

<br/>
<br/>
<br/>

در پاسخ پس از مشخص شدن وضعیت درخواست، کیت توسعه‌دهندگان callback داده شده را فراخوانی خواهد کرد.
در صورت موفقیت، یعنی بسته مورد نظر با موفقیت مصرف شده و در صورت عدم موفقیت، در مصرف بسته مورد نظر، دچار مشکلی شده‌ایم.


## مصرف کردن بسته‌ها در سمت سرور (یونیتی)

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

- HTTP Status Code 404 (Not Found):

در صورتی که آدرس لینک را اشتباه وارد کرده باشید، یا بسته مورد نظر با purchasePackageId اشتباه وارد شده باشد، یا بسته مورد نظر قبلا مصرف شده باشد این خطا رخ خواهد داد.  
در این صورت، سرور در متن پاسخ، پاسخی شبیه روبرو خواهد داد:


```json
{
  "message" : "your package is already is used or not found",
  "status_code" : 404
}
```

- HTTP Status Code 401 (Unauthorized):

یعنی اینکه کلید سروری که بر روی App-Key تنظیم شده است، غیر معتبر و اشتباه است.
در این صورت، سرور در متن پاسخ، پاسخی شبیه روبرو خواهد داد:


```json
{
  "message" : "app-key is not valid",
  "status_code" : 401
}
```

- HTTP Status Code 202 (Accepted / OK):


در این صورت یعنی، بسته مورد نظر با موفقیت مصرف شد و خرید کاربر، معتبر بوده است.
برای این حالت،‌ سرور در متن پاسخ، هیچ نوشته‌ای بر نخواهد گرداند.


## درخواست وارد شدن کاربر داخل پلاگین سیبچه (کرونا)

<aside class="success">
توصیه می‌کنیم از این بخش به صورت دستی استفاده نکنید چرا که در فرآیند پرداخت، اگر کاربر لاگین نکرده باشد، کتابخانه سیبچه خود اقدام به لاگین کاربر می‌کند.
</aside>


```lua
local function loginCallback(event)
    print(inspect(event))
end

SibcheStoreKit.loginUser(loginCallback)
```

با استفاده از این دستور، میتوانید به کتابخانه سیبچه، درخواست لاگین کاربر را بدهید. کافیست همانند کد روبرو، تابع مربوطه کیت توسعه‌دهندگان را فراخوانی نمایید.

<br/>
<br/>
<br/>

در جواب، پلاگین سیبچه، موفق بودن ورود (لاگین) و خطایی که با آن برخورد کرده را برمی‌گرداند. همچنین، در صورت موفقیت و در صورت موجود بودن، نام کاربر و آیدی کاربر را برمی‌گرداند.


## درخواست خارج شدن کاربر از پلاگین سیبچه (کرونا)

<aside class="warning">
توصیه می‌کنیم، تا زمانی که دلیل مشخصی جهت استفاده از آن را ندارید، از این تابع استفاده ننمایید. چرا که دکمه خروج داخل پلاگین سیبچه پیاده‌سازی شده است.
</aside>

```lua
local function logoutCallback(event)
    print(inspect(event))
end

SibcheStoreKit.logoutUser(logoutCallback)
```

با استفاده از این دستور می‌توانید کاربر فعلی لاگین کرده داخل سیستم را از پلاگین خارج نمایید و کلیه اطلاعات session او را پاک نمایید.
این تابع پس از اتمام کار، در قالب callback به شما جواب خواهد داد.


## گرفتن اطلاعات کاربر فعلی (کرونا)

```lua
local function getCurrentUserCallback(event)
    print(inspect(event))
end

SibcheStoreKit.getCurrentUserData(getCurrentUserCallback)
```

زمانی که نیاز دارید تا شماره تلفن و نیز شماره کاربری (userId) کاربر را دریافت نمایید، می‌توانید از تابع روبرو استفاده نمایید.

<br/>
<br/>
<br/>

در جواب، کیت توسعه‌دهندگان، نتیجه موفق بودن عملیات و نیز ارور رخ داده را برمی‌گرداند. در حالت عادی، بایستی عملیات موفق باشد ولی ناموفق بودن عملیات می‌تواند به دلایل شبکه‌ای مختلفی همانند موارد زیر باشد:


- دسترسی به اینترنت مقدور نبود
- هر اتفاق ناخواسته شبکه‌ای رخ داده است
- سرور سیبچه در دسترس نیست


همچنین در جواب، پارامتر سومی هم دارد که وضعیت کاربر را از لحاظ لاگین بودن/نبودن به اطلاع شما می‌رساند. در صورتی که لاگین باشد، از نوع `‍loginStatusTypeIsLoggedIn‍‍` جواب خواهد داد. در صورت لاگین نبودن نیز جواب داده شده از نوع `loginStatusTypeIsLoggedOut` می‌باشد. در صورتی که عملیات ناموفق باشد و از وضعیت لاگین بودن کاربر مطمئن نباشیم نیز جواب ‍`loginStatusTypeHaveTokenButFailedToCheck` را خواهیم داد.
پارامتر‌های بعدی نیز شامل شماره تلفن کاربر و نیز id کاربر می‌باشد که در پاسخ به شما داده خواهد شد.

