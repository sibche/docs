# نصب کیت توسعه‌دهندگان

## نیازمندیهای نصب کیت توسعه دهندگان

1. بارگزاری اپلیکیشن در [پنل توسعه‌دهندگان سیبچه](https://sibche.com/developer)
2. دریافت API Key عمومی برنامه از پنل توسعه‌دهندگان
3. آخرین نسخه‌ی Xcode

## نصب کیت توسعه‌دهندگان

### نصب از طریق Cocoapods

#### نصب cocoapods در سیستم

```shell
sudo gem install cocoapods
```

ابتدا بایستی از طریق دستور روبرو، cocoapods را نصب نمایید:

#### راه‌اندازی cocoapods

```shell
pod init
```

سپس با استفاده از دستور روبرو، فایل pod را بر روی پروژه ایجاد نمایید:

#### افزودن کیت توسعه‌دهندگان به cocoapods

```ruby
pod 'SibcheStoreKit', '~> 2.0'
```

کافی است دستور روبرو را بر روی فایل podfile خود اضافه نمایید:

> فایل podfile در نهایت به این شکل خواهد شد:

```ruby
# Uncomment the next line to define a global platform for your project
# platform :ios, '9.0'

target 'TestAppWithPods' do
  # Uncomment the next line if you're using Swift or would like to use dynamic frameworks
  # use_frameworks!

  # Pods for TestAppWithPods
  pod 'SibcheStoreKit', '~> 2.0'
end
```

> سپس پادهای اضافه‌شده را نصب کنید

```shell
pod repo update
pod install
```

### نصب دستی

کیت توسعه‌دهندگان سیبچه را می‌توانید از [اینجا](https://cdn.sibche.com/sibche-developer/SibcheStoreKit-2.0.3.zip) دانلود کرده و به پروژه خود اضافه کنید. برای اینکار فایل دانلود شده را از حالت زیپ در بیاورید. سپس فایل SibcheStoreKit.framework را به داخل پروژه خود کپی کرده و همانند زیر به پروژه اضافه نمایید:

![Screenshot](https://raw.githubusercontent.com/sibche/SibcheStoreKit/master/Screenshots/sc1.jpg)

سپس فایل پلاگین را انتخاب نموده و گزینه `add` را بزنید. سپس همانند شکل زیر، از بخش `General` از داخل تنظیمات پروژه، SibcheStoreKit.framework را از قسمت `Linked Frameworks and Libraries` حذف نمایید:

![Screenshot](https://raw.githubusercontent.com/sibche/SibcheStoreKit/master/Screenshots/sc2.jpg)

سپس دکمه + از بخش `Embedded Binaries` را انتخاب نموده و `SibcheStoreKit.framework` را انتخاب نموده و دکمه `Add` را بزنید. همانند عکسهای زیر:

![Screenshot](https://raw.githubusercontent.com/sibche/SibcheStoreKit/master/Screenshots/sc3.jpg)

![Screenshot](https://raw.githubusercontent.com/sibche/SibcheStoreKit/master/Screenshots/sc4.jpg)

![Screenshot](https://raw.githubusercontent.com/sibche/SibcheStoreKit/master/Screenshots/sc5.jpg)

## تنظیمات اولیه

### افزودن scheme سیبچه به پروژه

ابتدا بایستی scheme مربوط به اپ سیبچه را به لیست scheme های قابل چک خود اضافه نمایید. برای این کار، ابتدا برنامه را درون Xcode باز کرده و فایل info.plist برنامه را باز کرده و همانند شکل تنظیمات مختص بررسی scheme سیبچه را به این لیست اضافه نمایید (ابتدا دکمه + را زده و اسم LSApplicationQueriesSchemes را وارد کرده و سپس نوع کلید را به Array تغییر داده و عبارت newsibche را به عنوان زیربخش این آرایه وارد کنید):

![Screenshot](https://raw.githubusercontent.com/sibche/SibcheStoreKit/master/Screenshots/sc6.jpg)

![Screenshot](https://raw.githubusercontent.com/sibche/SibcheStoreKit/master/Screenshots/sc7.jpg)

![Screenshot](https://raw.githubusercontent.com/sibche/SibcheStoreKit/master/Screenshots/sc8.jpg)

![Screenshot](https://raw.githubusercontent.com/sibche/SibcheStoreKit/master/Screenshots/sc9.jpg)

یا نوشته زیر را به `info.plist` خود اضافه نمایید:

```xml
<key>LSApplicationQueriesSchemes</key>
<array>
	<string>newsibche</string>
</array>
```

### افزودن Scheme مختص برنامه شما

<aside class="success">
اگر برنامه شما دارای scheme اختصاصی می‌باشد این مرحله را رد کنید.
</aside>

برای اضافه کردن scheme اختصاصی بایستی طبق مراحل زیر تنظیمات برنامه را داخل Xcode باز کرده و سپس به تب info مراجعه نمایید:

![Screenshot](https://raw.githubusercontent.com/sibche/SibcheStoreKit/master/Screenshots/sc10.jpg)

![Screenshot](https://raw.githubusercontent.com/sibche/SibcheStoreKit/master/Screenshots/sc11.jpg)

سپس همانند شکل، url اختصاصی اپلیکیشن را اضافه نمایید:

![Screenshot](https://raw.githubusercontent.com/sibche/SibcheStoreKit/master/Screenshots/sc12.jpg)

به جای test، نام این scheme که میتواند باندل آیدی برنامه و یا هر اسم دیگری باشد را وارد کرده و سپس به جای testapp بایستی scheme مورد نظرتان را وارد نمایید. به عنوان مثال scheme تنظیم شده **تلگرام** `tg` و scheme تنظیم شده برای برنامه **اینستاگرام** `instagram` میباشد. لیست کامل scheme برنامه‌های معروف را میتوانید از [اینجا](https://ios.gadgethacks.com/news/always-updated-list-ios-app-url-scheme-names-0184033/) مشاهده نمایید.

<aside class="warning">
توصیه اکید می‌شود از scheme استفاده کنید که مختص شما باشد و ترجیحا طولانی باشد تا با برنامه‌های دیگر در تداخل نباشد.
</aside>

### افزودن کیت توسعه‌دهنده به AppDelegate

ابتدا کد اجرای پلاگین را داخل تابع didFinishLaunchingWithOptions اضافه نمایید:

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

لازم به ذکر است که به جای `YOUR_API_KEY` بایستی API Key گرفته شده از پنل دولوپری سیبچه را قرار دهید و به جای `YOUR_SCHEME` واژه scheme اضافه شده در مرحله قبل را جایگذاری نمایید.

سپس امکان فراخوانی باز شدن url را نیز به پلاگین بدهید:

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

## گرفتن لیست پکیجهای قابل خرید

پس از تنظیم برنامه، میتوانید پکیجهای قابل خرید را مشاهده نمایید. کافیست همانند دستور زیر اقدام به فراخوانی API مورد نظر نمایید:

```objective_c
[SibcheStoreKit fetchInAppPurchasePackages:^(BOOL isSuccessful, NSArray *packagesArray) {
    // Your block code for handling of packages list
}];
```

```swift
SibcheStoreKit.fetch { (isSuccessful, packagesArray) in
    // Your block code for handling of packages list
}
```

سه نوع بسته قابل خرید داریم که عبارتند از:

- `SibcheConsumablePackage`: بسته‌هایی که قابل مصرف هستند که خریداری شده و داخل بازی یا برنامه مصرف می‌شود. مانند بسته‌ی ۵۰۰ سکه طلا
- `SibcheNonConsumablePackage`: بسته‌هایی که قابل مصرف نیستند و فقط یکبار خریداری می‌شوند. مانند بسته‌ی باز شدن قابلیت آپلود آواتار یا تغییر نام
- `SibcheSubscriptionPackage`: بسته‌هایی هستند که به مدت محدود قابل استفاده بوده و پس از اتمام زمان آنها، کاربر مجاز به استفاده از آن قابلیت نیست. به عنوان مثال، قابلیت استفاده از امکانات ویژه به مدت یک سال

توجه نمایید که پارامتر اول، تابع مورد نظر برای فراخوانی، پس از آماده‌سازی بسته‌ها توسط پلاگین می‌باشد.

در پاسخ، در صورت موفقیت، پلاگین بسته‌های قابل خرید را به عنوان پارامتر پاسخ به شما تحویل میدهد. این پارامتر آرایه‌ای از بسته‌های قابل خرید می‌باشد. این بسته‌ها از نوع `SibchePackage` هستند. مدل `SibchePackage` شامل سه نوع متفاوتی از بسته‌ها می‌باشد که در بالا اشاره شد. ساختار `SibchePackage` شامل توابع زیر می‌باشد:

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

بسته‌های `SibcheConsumablePackage` و `SibcheNonConsumablePackage` فقط همین توابع را دارند ولی بسته‌های `SibcheSubscriptionPackage` علاوه بر این توابع، داری توابع زیر نیز هست:

```objective_c
- (NSString*)duration;
- (NSString*)group;
```

```swift
var duration: String
var group: String
```

## گرفتن اطلاعات بسته مشخص

با در اختیار داشتن آیدی بسته مورد نظر می‌توانید اطلاعات آن بسته را در اختیار بگیرید. نحوه استفاده از این API به شکل زیر است:

```objective_c
[SibcheStoreKit fetchInAppPurchasePackage:@"com.example.testapp" withPackagesCallback:^(BOOL isSuccessful, SibchePackage *package) {
   // Your block code for handling of packages list
}];
```

```swift
SibcheStoreKit.fetch(inAppPurchasePackage: "com.example.testapp") { (isSuccessful, package) in
    // Your block code for handling of packages list
}
```

پارامتر اول داده شده، همان callback ارسال شده ما است که پس از مشخص شدن وضعیت درخواست، فراخوانی خواهد شد. در صورت موفقیت، بسته‌ی مورد نظر در قالب آبجکت `SibchePackage` (بسته به نوع بسته) به شما ارسال خواهد شد.

## خرید بسته مشخص

پس از گرفتن لیست پکیج‌ها، میتوانید درخواست خرید این بسته‌ها را از طریق API زیر به پلاگین بدهید. در ادامه ما در صورت نیاز کاربر را لاگین کرده و فرایند پرداخت را هندل میکنیم. سپس موفقیت یا ناموفق بودن خرید را به اطلاع شما میدهیم.

```objective_c
[SibcheStoreKit purchasePackage:packageId withCallback:^(BOOL isSuccessful) {
    // Your block code for handling of purchase callback
}];
```

```swift
SibcheStoreKit.purchasePackage("com.example.testapp") { (isSuccessful) in
  // Your block code for handling of purchase callback
}
```

با استفاده از این دستور، پلاگین فرآیند لاگین و پرداخت را هندل کرده و در نهایت موفقیت و یا عدم موفقیت را به شما خبر خواهد داد.

## گرفتن لیست بسته های خریداری شده

با استفاده از این دستور، میتوانید لیست بسته‌های فعال (خریداری شده) کاربر را بدست آورید. کافیست همانند دستور زیر، API پلاگین را فراخوانی نمایید.

```objective_c
[SibcheStoreKit fetchActiveInAppPurchasePackages:^(BOOL isSuccessful, NSArray *purchasePackagesArray) {
  // Your block code for handling of packages list
}];
```

```swift
SibcheStoreKit.fetchActive { (isSuccessful, purchasePackagesArray) in
  // Your block code for handling of packages list
}
```

در پاسخ پلاگین موفقیت/عدم موفقیت درخواست و نیز آرایه‌ای از بسته‌های خریداری شده فعال را برمیگرداند. توجه نمایید که این آرایه، آرایه‌ای از نوع `SibchePurchasePackage` است و شامل توابع زیر هست:

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

توجه نمایید که منظور از وضعیت فعال بسته‌ها، برای هر نوع از بسته‌ها متفاوت است که به شرح زیر میباشد:

- `SibcheConsumablePackage`: بسته‌هایی که خریداری شده‌اند ولی هنوز مصرف (Consume) نشده‌اند.
- `SibcheNonConsumablePackage`: بسته‌هایی که خریداری شده‌اند.
- `SibcheSubscriptionPackage`: بسته‌هایی که خریداری شده‌اند ولی هنوز از تاریخ انقضای آنها باقی مانده است.

## مصرف کردن بسته‌های قابل مصرف (Consumable packages)

برای مصرف کردن بسته‌های قابل مصرف (Consumable) بایستی شبیه دستور زیر، تابع مربوطه از پلاگین را فراخوانی کنیم:

```objective_c
[SibcheStoreKit consumePurchasePackage:purchasePackageData.purchasePackageId withCallback:^(BOOL isSuccessful) {
   // Your block code for handling of package consume
}];
```

```swift
SibcheStoreKit.consumePurchasePackage(purchasePackageData.purchasePackageId) { (isSuccessful) in
  // Your block code for handling of package consume
}
```

در پاسخ پس از مشخص شدن وضعیت درخواست، پلاگین callback داده شده را فراخوانی خواهد کرد. در صورت موفقیت، یعنی بسته مورد نظر با موفقیت مصرف شده و در صورت عدم موفقیت، در مصرف بسته مورد نظر دچار مشکلی شده‌ایم.
