# Avanak
## سامانه ی پیام صوتی آوانک
آوانک (اولین اپراتور پیام صوتی در ایران) می باشد که با گستردگی زیر ساخت های مخابراتی توانایی برقراری تماس و ارسال صدای شما به صورت همزمان برای انبوهی از مخاطبین تلفن ثابت و همراه را فراهم کرده است.

آدرس وب سایت
https://www.avanak.ir

آدرس پرتال
https://portal.avanak.ir

آدرس مستندات وب سرویس 
https://portal.avanak.ir/documentation

## وب سرویس وب سرویس REST (REST)
آدرس وب سرویس https://portal.avanak.ir/Rest

### متد وضعیت حساب کاربری (AccountStatus)
از طریق این متد می توانید مشخصات حساب کاربری خود را دریافت نمایید .
#### پارامترهای ورودی AccountStatus
|نام|نوع|توضیحات|
| ------------- |:-------------:| -----:|
|Token|String *|توکن ثبت شده در سامانه|
#### خروجی AccountStatus (Object)
```html
خروجی بصورت یک کلاس با فیلدهای زیر میباشد
{
"Status": وضعیت,
"AccountName": نام حساب,
"RemaindCredit": باقیمانده اعتبار به ریال,
"Mobile": موبایل حساب کاربری,
"ExpireDate": تاریخ انقضاء به میلادی,
    "ExpireDatePer": تاریخ انقضاء به شمسی
}

در صورتیکه عملیات موفق باشد خروجی دریافت میکنید در غیر اینصورت Result کد خطای احراز میباشد
```
#### نمونه کد AccountStatus (C#)
```csharp
string Token = "MyToken";
using (var client = new System.Net.WebClient())
{
    client.Headers.Add(System.Net.HttpRequestHeader.Authorization, Token);

    var data = new System.Collections.Specialized.NameValueCollection()
    {

    };

    byte[] bytes = client.UploadValues("https://portal.avanak.ir/rest/AccountStatus", data);

    string json = System.Text.UTF8Encoding.UTF8.GetString(bytes);//خروجی
}
```
#### نمونه کد AccountStatus (PHP)
```php
$url = "https://portal.avanak.ir/rest/AccountStatus";

$url = $url."?"."Token=".urlencode("MyToken");
$json = file_get_contents($url);
echo $json;

//$result = json_decode($json);
//echo $result->Status;
```
### متد ارسال رمز یکبار مصرف (SendOTP)
از طریق این متد می توانید پیام صوتی با رمز یکبار مصرف را به صورت فوری به شماره انتخابی خود ارسال نمایید .
#### پارامترهای ورودی SendOTP
|نام|نوع|توضیحات|
| ------------- |:-------------:| -----:|
|Token|String *|توکن ثبت شده در سامانه|
|password|String *|رمز عبور|
|Length|Int *|طول کد|
|Number|String *|شماره همراه یا ثابت|
|OptionalCode|Int |کد دلخواه خود را وارد نمایید حداقل 4 و حداکثر 8 رقم (تصادفی = 0)|
|ServerID|Int |کد سرور (خودکار=0)|
#### خروجی SendOTP (Object)
```html
result  بصورت Json با فیلدهای زیر میباشد
}
int ErrorCode
int QuickSendID
string GeneratedCode
{

QuickSendID: کد ارسال سریع میباشد

GeneratedCode : کد تولید شده میباشد

ErrorCode کد خطاها :
"-5" طول کد غیرمجاز
"-25" ثبت OTP غیرفعال میباشد
"-2" شماره اشتباه میباشد
"-6" زمان ارسال غیرمجاز میباشد
 "-3" عدم موجودی کافی
"-20" امکان اتصال به سرور وجود ندارد
"-500" خطایی رخ داده است 
```
#### نمونه کد SendOTP (C#)
```csharp
 string Token = "MyToken";
 int Length = 5;
 int OptionalCode = 0;
 int ServerID = 0;
 using (var client = new System.Net.WebClient())
 {
     client.Headers.Add(System.Net.HttpRequestHeader.Authorization, Token);

     var data = new System.Collections.Specialized.NameValueCollection()
     {
         {"Length", Length.ToString() },
         {"OptionalCode", OptionalCode.ToString() },
         {"ServerID", ServerID.ToString() },
     };

     byte[] bytes = client.UploadValues("https://portal.avanak.ir/rest/SendOTP", data);

     string json = System.Text.UTF8Encoding.UTF8.GetString(bytes);//خروجی
 }
```
#### نمونه کد SendOTP (PHP)
```php
$url = "https://portal.avanak.ir/rest/SendOTP";

$url = $url."?"."Token=".urlencode("MyToken");
$url = $url."&"."Length=".urlencode("5");
$url = $url."&"."OptionalCode=".urlencode("0");
$url = $url."&"."ServerID=".urlencode("0"); 

$json = file_get_contents($url);
echo $json;

//$result = json_decode($json);
//echo $result->Status;
```
### متد آپلود فایل صوتی (UploadMessageBase64)
از طریق این متد می توانید فایل صوتی خود را در گالری اصوات خود آپلود نمایید.
#### پارامترهای ورودی UploadMessageBase64
|نام|نوع|توضیحات|
| ------------- |:-------------:| -----:|
|Token|String *|توکن ثبت شده در سامانه|
|password|String *|رمز عبور|
|Title|String *|عنوان|
|Base64|String *|فایل که بصورت Base64 کد گذاری شده باشد|
|Persist|Bool |ماندگار|
|CallFromMobile|String |شماره همراه که در انتهای صوت اضافه میشود (پیش فرض خالی باشد)|
#### خروجی UploadMessageBase64 (Object)
```html
خروجی بصورت یک کلاس با فیلدهای Id و Length میباشد

id > 0 عملیات موفق آمیز بوده و کد فایل صوتی "messageId" را نشان میدهد

کد خطاها
id = -6 طول متن بسیار کوتاه (کمتر از 3 ثانیه)
id = -5 طول متن بیش از حد مجاز (1000 کاراکتر)
id = -3 عدم اعتبار کافی برای تولید صوت
id = -4  خطا در تولید فایل صوتی
id = -2  خطا در تولید فایل صوتی
```
#### نمونه کد UploadMessageBase64 (C#)
```csharp
string Token = "MyToken";
string Title = "فایل صوتی جدید";

byte[] FileBytes = System.IO.File.ReadAllBytes(".....");
string Base64 = Convert.ToBase64String(FileBytes);
using (var client = new System.Net.WebClient())
{
    client.Headers.Add(System.Net.HttpRequestHeader.Authorization, Token);

    var data = new System.Collections.Specialized.NameValueCollection()
    {
        {"Title", Title },
        {"Base64", Base64 },
    };

    byte[] bytes = client.UploadValues("https://portal.avanak.ir/rest/UploadMessageBase64", data);

    string json = System.Text.UTF8Encoding.UTF8.GetString(bytes);//خروجی
}
```
#### نمونه کد UploadMessageBase64 (PHP)
```php
//////////////////////// POST METHOD /////////////////////////
$url = "https://portal.avanak.ir/rest/UploadMessageBase64";

$path = 'myfolder/mysound.mp3';
$type = pathinfo($path, PATHINFO_EXTENSION);
$file = file_get_contents($path);

$base64 = base64_encode($file);

$data = [
	'Token' => 'MyToken',
	'Title' => 'فایل صوتی جدید',
	'Persist' => 'false',
	'Base64'  => $base64,
];

$options = [
    'http' => [
        'header' => "Content-type: application/x-www-form-urlencoded\r\n",
        'method' => 'POST',
        'content' => http_build_query($data),
    ],
];

$context = stream_context_create($options);
$json = file_get_contents($url, false, $context);
echo $json;

//$result = json_decode($json);
//echo $result->Status;

//////////////////////// GET METHOD /////////////////////////
$url = "https://portal.avanak.ir/rest/UploadMessageBase64";

$url = $url."?"."Token=".urlencode("MyToken");
$url = $url."&"."Title=".urlencode("فایل صوتی جدید");
$url = $url."&"."Persist=".urlencode("false"); 

$path = 'myfolder/mysound.mp3';
$type = pathinfo($path, PATHINFO_EXTENSION);
$file = file_get_contents($path);

$url = $url."&"."Base64=".base64_encode($file);

$json = file_get_contents($url);
echo $json;

//$result = json_decode($json);
//echo $result->Status;
```
### متد تولید صوت آواخوان (GenerateTTS)
از طریق این متد می توانید متن مورد نظر خود را بصورت فایل صوتی در گالری اصوات خود ذخیره نمایید.
#### پارامترهای ورودی GenerateTTS
|نام|نوع|توضیحات|
| ------------- |:-------------:| -----:|
|Token|String *|توکن ثبت شده در سامانه|
|password|String *|رمز عبور|
|Text|String *|متن پیام|
|Title|String *|عنوان|
|Speaker|String |انتخاب گوینده - male برای گوینده آقا - female برای گوینده خانم (پیش فرض = "male")|
|CallFromMobile|String |شماره همراه که در انتهای صوت اضافه میشود (پیش فرض خالی باشد)|
#### خروجی GenerateTTS (Object)
```html
خروجی بصورت یک کلاس با فیلدهای Id و Length میباشد

id > 0 عملیات موفق آمیز بوده و کد فایل صوتی "messageId" را نشان میدهد

کد خطاها
id = -6 طول متن بسیار کوتاه (کمتر از 3 ثانیه)
id = -5 طول متن بیش از حد مجاز (1000 کاراکتر)
id = -3 عدم اعتبار کافی برای تولید صوت
id = -4  خطا در تولید فایل صوتی
id = -2  خطا در تولید فایل صوتی
```
#### نمونه کد GenerateTTS (C#)
```csharp
string Token = "MyToken";
string Title = "فایل صوتی جدید";
string Text = "سلام . این یک پیام صوتی میباشد";
string Speaker = "male";
using (var client = new System.Net.WebClient())
{
    client.Headers.Add(System.Net.HttpRequestHeader.Authorization, Token);

    var data = new System.Collections.Specialized.NameValueCollection()
    {
        {"Title", Title },
        {"Text", Text },
        {"Speaker", Speaker },
    };

    byte[] bytes = client.UploadValues("https://portal.avanak.ir/rest/GenerateTTS", data);

    string json = System.Text.UTF8Encoding.UTF8.GetString(bytes);//خروجی
}
```
#### نمونه کد GenerateTTS (PHP)
```php
$url = "https://portal.avanak.ir/rest/GenerateTTS";

$url = $url."?"."Token=".urlencode("MyToken");
$url = $url."&"."Title=".urlencode("فایل صوتی جدید");
$url = $url."&"."Text=".urlencode("سلام . این یک پیام صوتی میباشد"); 
$url = $url."&"."Speaker=".urlencode("male"); 

$json = file_get_contents($url);
echo $json;

//$result = json_decode($json);
//echo $result->Status;
```
### متد ارسال سریع (QuickSend)
از طریق این متد می توانید با وارد کردن کد فایل صوتی و شماره ، یک پیام خود را به شماره انتخابی ارسال سریع نمایید.	
#### پارامترهای ورودی QuickSend
|نام|نوع|توضیحات|
| ------------- |:-------------:| -----:|
|Token|String *|توکن ثبت شده در سامانه|
|password|String *|رمز عبور|
|MessageID|Int *|کد صوت|
|Number|String *|شماره همراه یا ثابت|
|Vote|Bool |فعال سازی امکان نظرسنجی بر روی تماس ها|
|ServerID|Int |کد سرور (خودکار=0)|
|RecordVoice|Bool ||
|RecordVoiceDuration|Short ||
#### خروجی QuickSend (Object)
```html
result > 0 عملیات موفق آمیز بوده و کد ارسال سریع "quickSendId" را نشان میدهد

کد خطاها :
-25 ثبت ارسال سریع غیرفعال میباشد
-2 شماره اشتباه میباشد
-3 عدم موجودی کافی
-6 زمان ارسال غیرمجاز میباشد
-8 کد فایل صوتی اشتباه میباشد
-71 مدت ضبط صدا غیرمجاز میباشد
-72 عدم مجوز ضبط صدا
```
#### نمونه کد QuickSend (C#)
```csharp
 string Token = "MyToken";
 int MessageID = 1000;
 string Number = "09120000000";
 bool Vote = false;
 int ServerID = 0;
 using (var client = new System.Net.WebClient())
 {
     client.Headers.Add(System.Net.HttpRequestHeader.Authorization, Token);

     var data = new System.Collections.Specialized.NameValueCollection()
     {
         {"MessageID", MessageID.ToString() },
         {"Number", Number },
         {"Vote", Vote.ToString() },
         {"ServerID", ServerID.ToString() },
     };

     byte[] bytes = client.UploadValues("https://portal.avanak.ir/rest/QuickSend", data);

     string json = System.Text.UTF8Encoding.UTF8.GetString(bytes);//خروجی
 }
```
#### نمونه کد QuickSend (PHP)
```php
$url = "https://portal.avanak.ir/rest/QuickSend";

$url = $url."?"."Token=".urlencode("MyToken");
$url = $url."&"."MessageID=".urlencode("1000"); 
$url = $url."&"."Number=".urlencode("09120000000");
$url = $url."&"."Vote=".urlencode("false");
$url = $url."&"."ServerID=".urlencode("0");  

$json = file_get_contents($url);
echo $json;

//$result = json_decode($json);
//echo $result->Status;
```
### متد ارسال سریع با آواخوان (QuickSendWithTTS)
از طریق این متد می توانید با وارد کردن متن و شماره ، یک پیام خود را به شماره انتخابی ارسال سریع نمایید.
#### پارامترهای ورودی QuickSendWithTTS
|نام|نوع|توضیحات|
| ------------- |:-------------:| -----:|
|Token|String *|توکن ثبت شده در سامانه|
|password|String *|رمز عبور|
|Text|String *|متن پیام|
|Number|String *|شماره همراه یا ثابت|
|Vote|Bool |فعال سازی امکان نظرسنجی بر روی تماس ها|
|ServerID|Int |کد سرور (خودکار=0)|
|CallFromMobile|String |شماره همراه که در انتهای صوت اضافه میشود (پیش فرض خالی باشد)|
|RecordVoice|Bool ||
|RecordVoiceDuration|Short ||
#### خروجی QuickSendWithTTS (Object)
```html
result > 0 عملیات موفق آمیز بوده و کد ارسال سریع "quickSendId" را نشان میدهد

کد خطاها :
-25 ثبت ارسال سریع غیرفعال میباشد
0 عدم مجوز تایید صوت یا کاربری دمو می باشد
-2 شماره اشتباه میباشد
-3 عدم اعتبار کافی
-5 طول متن بیش از حد مجاز (1000 کاراکتر)
-6 زمان ارسال غیرمجاز میباشد
-7 خطا در تولید فایل صوتی
-8 طول متن بسیار کوتاه (کمتر از 3 ثانیه)
-9 خطا در تولید فایل صوتی
-10 متن خالی می باشد
-11 محدودیت در ارسال تکراری به شماره در فاصله زمانی مشخص
-71 مدت ضبط صدا غیرمجاز میباشد
-72 عدم مجوز ضبط صدا
```
#### نمونه کد QuickSendWithTTS (C#)
```csharp
string Token = "MyToken";
string Text = "سلام . این یک پیام صوتی میباشد";
string Number = "09120000000";
bool Vote = false;
int ServerID = 0;
using (var client = new System.Net.WebClient())
{
    client.Headers.Add(System.Net.HttpRequestHeader.Authorization, Token);

    var data = new System.Collections.Specialized.NameValueCollection()
    {
        {"Text", Text },
        {"Number", Number },
        {"Vote", Vote.ToString() },
        {"ServerID", ServerID.ToString() },
    };

    byte[] bytes = client.UploadValues("https://portal.avanak.ir/rest/QuickSendWithTTS", data);

    string json = System.Text.UTF8Encoding.UTF8.GetString(bytes);//خروجی
}
```
#### نمونه کد QuickSendWithTTS (PHP)
```php
$url = "https://portal.avanak.ir/rest/QuickSendWithTTS";

$url = $url."?"."Token=".urlencode("MyToken");
$url = $url."&"."Text=".urlencode("سلام . این یک پیام صوتی میباشد"); 
$url = $url."&"."Number=".urlencode("09120000000");
$url = $url."&"."Vote=".urlencode("false");
$url = $url."&"."ServerID=".urlencode("0");  

$json = file_get_contents($url);
echo $json;

//$result = json_decode($json);
//echo $result->Status;
```
### متد ایجاد کمپین (CreateCampaign)
از طریق این متد می توانید با وارد کردن کد فایل صوتی و شماره و زمان شروع و پایان ، پیام های خود را به شماره یا شماره های انتخابی ارسال نمایید.
#### پارامترهای ورودی CreateCampaign
|نام|نوع|توضیحات|
| ------------- |:-------------:| -----:|
|Token|String *|توکن ثبت شده در سامانه|
|password|String *|رمز عبور|
|Title|String *|عنوان|
|Numbers|String *|لیست شماره ها که با , ازهم جدا شده اند|
|MessageID|Int *|کد صوت|
|StartDateTime|String *|زمان شروع ارسال لیست به میلادی.  مانند: 2022-11-11 12:00:00|
|EndDateTime|String *|زمان خاتمه ارسال لیست به میلادی یا شمسی مانند: 2022-11-11 18:00:00|
|MaxTryCount|Int |حداکثر تعداد تلاش برای هر شماره (پیش فرض = 1)|
|MinuteBetweenTries|Int |مدت تاخیر بین تلاش ها بر اساس دقیقه (پیش فرض = 10)|
|ServerID|Int |کد سرور (خودکار=0)|
|AutoStart|Bool |شروع خودکار پس از ایجاد (پیش فرض = true)|
|Vote|Bool |فعال سازی امکان نظرسنجی بر روی تماس‌ها (پیش فرض = false)|
#### خروجی CreateCampaign (Object)
```html
result > 0 عملیات موفق آمیز بوده و کد کمپین "campaignId" را نشان میدهد

کد خطاها
-30 امکان شروع لیست ارسال وجود ندارد
-50 جهت شروع ارسال اعتبار کافی نیست
-60 کد فایل صوتی انتخاب شده صحیح نیست
-61 فایل صوتی در انتظار تایید میباشد
-62 فایل صوتی رد تایید شده است
-70 مقدار حداکثر تلاش غیرمجاز میباشد
-71 مدت ضبط صدا غیرمجاز میباشد
-72 عدم مجوز ضبط صدا
-73 ناحیه انتخابی در نقشه موجود نمیباشد
-74 خطا در دریافت شماره های ناحیه انتخابی
-75 مقادیر بطور کلی صحیح نمیباشد
-76 تاریخ شروع باید کوچکتر یا مساوی از تاریخ پایان باشد
-77 ساعت شروع باید کوچکتر از ساعت پایان باشد
-80 لیست متون پیامک خالی می باشد
```
#### نمونه کد CreateCampaign (C#)
```csharp
string Token = "MyToken";
int MessageID = 1000;
DateTime StartDateTime = DateTime.Now;
DateTime EndDateTime = DateTime.Now.AddHours(10);
int MaxTryCount = 1;
int MinuteBetweenTries = 10;
string Title = "کمپین جدید";
string Numbers = "09120000000,09150000000";
bool AutoStart = true;
bool Vote = false;
int ServerID = 0;
using (var client = new System.Net.WebClient())
{
    client.Headers.Add(System.Net.HttpRequestHeader.Authorization, Token);

    var data = new System.Collections.Specialized.NameValueCollection()
    {
        {"MessageID", MessageID.ToString() },
        {"StartDateTime", StartDateTime.ToString() },
        {"EndDateTime", EndDateTime.ToString() },
        {"MaxTryCount", MaxTryCount.ToString() },
        {"MinuteBetweenTries", MinuteBetweenTries.ToString() },
        {"Title", Title },
        {"Numbers", Numbers },
        {"AutoStart", AutoStart.ToString() },
        {"Vote", Vote.ToString() },
        {"ServerID", ServerID.ToString() },
    };

    byte[] bytes = client.UploadValues("https://portal.avanak.ir/rest/CreateCampaign", data);

    string json = System.Text.UTF8Encoding.UTF8.GetString(bytes);//خروجی
}
```
#### نمونه کد CreateCampaign (PHP)
```php
$url = "https://portal.avanak.ir/rest/CreateCampaign";

$url = $url."?"."Token=".urlencode("MyToken");
$url = $url."&"."MessageID=".urlencode("1000"); 
$url = $url."&"."StartDateTime=".urlencode("2024-09-01 12:00:00");  
$url = $url."&"."EndDateTime=".urlencode("2024-09-01 18:00:00");  
$url = $url."&"."MaxTryCount=".urlencode("1");  
$url = $url."&"."MinuteBetweenTries=".urlencode("10");  
$url = $url."&"."Title=".urlencode("کمپین جدید");  
$url = $url."&"."Numbers=".urlencode("09120000000,09150000000");  
$url = $url."&"."AutoStart=".urlencode("true");  
$url = $url."&"."Vote=".urlencode("false");  
$url = $url."&"."ServerID=".urlencode("0");  

$json = file_get_contents($url);
echo $json;

//$result = json_decode($json);
//echo $result->Status;
```
### متد ارسال OCV (SendOCV)
اعتبار سنجی با یک کلیک
#### پارامترهای ورودی SendOCV
|نام|نوع|توضیحات|
| ------------- |:-------------:| -----:|
|Token|String *|توکن ثبت شده در سامانه|
|Number|String *|شماره همراه یا ثابت|
|CallbackURL|String *||
|ServerID|Int |کد سرور (خودکار=0)|
#### خروجی SendOCV (Object)
```html
خروجی بصورت یک کلاس با فیلدهای زیر میباشد
{
Result=کد خطا
Code= کد نظرسنجی (یک عدد بین 1 تا 9)
SendId=کد ارسال سریع (quickSendId)
}

کد خطا:
-2 شماره اشتباه میباشد
-25 ثبت ارسال سریع غیرفعال میباشد
-6 زمان ارسال غیرمجاز میباشد
-3 عدم موجودی کافی
-4 خطا در تولید فایل صوتی
-7 خطا در تولید فایل صوتی
-8 خطای سرور
```
#### نمونه کد SendOCV (C#)
```csharp
 string Token = "MyToken";
 string Number = "09120000000";
 string CallbackURL = "http://mysite.com/OCVCallback?Number=09120000000";
 int ServerID = 0;
 using (var client = new System.Net.WebClient())
 {
     client.Headers.Add(System.Net.HttpRequestHeader.Authorization, Token);

     var data = new System.Collections.Specialized.NameValueCollection()
     {
         {"Number", Number },
         {"CallbackURL", CallbackURL },
         {"ServerID", ServerID.ToString() },
     };

     byte[] bytes = client.UploadValues("https://portal.avanak.ir/rest/SendOCV", data);

     string json = System.Text.UTF8Encoding.UTF8.GetString(bytes);//خروجی
 }
```
#### نمونه کد SendOCV (PHP)
```php
$url = "https://portal.avanak.ir/rest/SendOCV";

$url = $url."?"."Token=".urlencode("MyToken");
$url = $url."&"."Number=".urlencode("09120000000");
$url = $url."&"."CallbackURL=".urlencode("http://mysite.com/OCVCallback?Number=09120000000"); 
$url = $url."&"."ServerID=".urlencode("0");  

$json = file_get_contents($url);
echo $json;

//$result = json_decode($json);
//echo $result->Status;
```
### متد وضعیت ارسال سریع (GetQuickSend)
از طریق این متد می توانید با استفاده از "کد ارسال سریع" ، مشخصات کامل آن را دریافت نمایید.
#### پارامترهای ورودی GetQuickSend
|نام|نوع|توضیحات|
| ------------- |:-------------:| -----:|
|Token|String *|توکن ثبت شده در سامانه|
|QuickSendID|Int *|کد ارسال سریع|
#### خروجی GetQuickSend (Object)
```html
خروجی بصورت یک کلاس با فیلدهای زیر میباشد
{
reason =کد وضعیت ,
status = عنوان وضعیت ,
dst = شماره تلفن مقصد ,
starttime = زمان ارسال,
Id = کد ارسال سریع ,
subscribeid = کد ارسال سریع,
duration = مدت زمان شنیدن ,
vote = عدد نظرسنجی (-1= بدون نظرسنجی)
}

در صورتیکه عملیات موفق باشد خروجی دریافت میکنید در غیر اینصورت null دریافت خواهید کرد
```
#### نمونه کد GetQuickSend (C#)
```csharp
string Token = "MyToken";
int QuickSendID = 1000;
using (var client = new System.Net.WebClient())
{
    client.Headers.Add(System.Net.HttpRequestHeader.Authorization, Token);

    var data = new System.Collections.Specialized.NameValueCollection()
    {
      {"QuickSendID", QuickSendID },
    };

    byte[] bytes = client.UploadValues("https://portal.avanak.ir/rest/GetQuickSend", data); 

    string json = System.Text.UTF8Encoding.UTF8.GetString(bytes);//خروجی
}
```
#### نمونه کد GetQuickSend (PHP)
```php
$url = "https://portal.avanak.ir/rest/GetQuickSend";

$url = $url."?"."Token=".urlencode("MyToken");
$url = $url."&"."QuickSendID=".urlencode("1000");
$json = file_get_contents($url);
echo $json;

//$result = json_decode($json);
//echo $result->Status;
```
### متد دانلود فایل صوتی (DownloadMessage)
از طریق این متد می توانید با استفاده از "کد صوت" ، فایل صوتی خود را دانلود نمایید.
#### پارامترهای ورودی DownloadMessage
|نام|نوع|توضیحات|
| ------------- |:-------------:| -----:|
|Token|String *|توکن ثبت شده در سامانه|
|MessageID|Int *|کد صوت|
#### خروجی DownloadMessage (Object)
```html
خروجی بصورت آرایه ای از byte میباشد

در صورتیکه عملیات موفق باشد خروجی دریافت میکنید در غیر اینصورت null دریافت خواهید کرد
```
#### نمونه کد DownloadMessage (C#)
```csharp
string Token = "MyToken";
int MessageID = 1000;
using (var client = new System.Net.WebClient())
{
    client.Headers.Add(System.Net.HttpRequestHeader.Authorization, Token);

    var data = new System.Collections.Specialized.NameValueCollection()
    {
      {"MessageID", MessageID },
    };

    byte[] bytes = client.UploadValues("https://portal.avanak.ir/rest/DownloadMessage", data); //خروجی
}
```
#### نمونه کد DownloadMessage (PHP)
```php
$url = "https://portal.avanak.ir/rest/DownloadMessage";

$url = $url."?"."Token=".urlencode("MyToken");
$url = $url."&"."MessageID=".urlencode("1000");
$file = file_get_contents($url);
echo $file;

//$result = json_decode($json);
//echo $result->Status;
```
### متد شروع کمپین (StartCampaign)
از طریق این متد می توانیدبا استفاده از "کد کمپین" ، کمپین موردنظر خود را که در حالت انتظار یا توقف ثبت نموده اید را در زمان مورد نظر شروع به ارسال نمایید
#### پارامترهای ورودی StartCampaign
|نام|نوع|توضیحات|
| ------------- |:-------------:| -----:|
|Token|String *|توکن ثبت شده در سامانه|
|CampaignID|Int *|کد کمپین|
|StartDateTime|String *|زمان شروع ارسال لیست به میلادی. مانند: 2022-11-11 12:00:00|
|EndDateTime|String *|زمان خاتمه ارسال لیست به میلادی یا شمسی مانند: 2022-11-11 18:00:00|
|MaxTryCount|Int |حداکثر تعداد تلاش برای هر شماره  (پیش فرض = 1)|
|MinuteBetweenTries|Int |مدت تاخیر بین تلاش ها بر اساس دقیقه (پیش فرض = 10)|
|Title|String |عنوان جدید|
|ServerID|Int |کد سرور (خودکار=0)|
#### خروجی StartCampaign (Long)
```html
result > 0 عملیات موفق آمیز بوده و کد کمپین "campaignId" را نشان میدهد

کد خطاها
-20 امکان شروع لیست ارسال وجود ندارد
-30 امکان شروع لیست ارسال وجود ندارد
-50 جهت شروع ارسال اعتبار کافی نیست
-60 کد فایل صوتی انتخاب شده صحیح نیست
```
#### نمونه کد StartCampaign (C#)
```csharp
string Token = "MyToken";
int CampaignID = 1000;
DateTime StartDateTime = DateTime.Now;
DateTime EndDateTime = DateTime.Now.AddHours(10);
int MaxTryCount = 1;
int MinuteBetweenTries = 10;
string Title = "";
int ServerID = 0;
using (var client = new System.Net.WebClient())
{
    client.Headers.Add(System.Net.HttpRequestHeader.Authorization, Token);

    var data = new System.Collections.Specialized.NameValueCollection()
    {
        {"CampaignID", CampaignID.ToString() },
        {"StartDateTime", StartDateTime.ToString() },
        {"EndDateTime", EndDateTime.ToString() },
        {"MaxTryCount", MaxTryCount.ToString() },
        {"MinuteBetweenTries", MinuteBetweenTries.ToString() },
        {"Title", Title },
        {"ServerID", ServerID.ToString() },
    };

    byte[] bytes = client.UploadValues("https://portal.avanak.ir/rest/StartCampaign", data);

    string json = System.Text.UTF8Encoding.UTF8.GetString(bytes);//خروجی
}
```
#### نمونه کد StartCampaign (PHP)
```php
$url = "https://portal.avanak.ir/rest/StartCampaign";

$url = $url."?"."Token=".urlencode("MyToken");
$url = $url."&"."CampaignID=".urlencode("1000"); 
$url = $url."&"."StartDateTime=".urlencode("2024-09-01 12:00:00");  
$url = $url."&"."EndDateTime=".urlencode("2024-09-01 18:00:00");  
$url = $url."&"."MaxTryCount=".urlencode("1");  
$url = $url."&"."MinuteBetweenTries=".urlencode("10");  
$url = $url."&"."Title=".urlencode("");  
$url = $url."&"."ServerID=".urlencode("0");  

$json = file_get_contents($url);
echo $json;

//$result = json_decode($json);
//echo $result->Status;
```
### متد توقف کمپین (StopCampaign)
از طریق این متد می توانید با استفاده از "کد کمپین" ، کمپین موردنظر خود را که در حال ارسال میباشد را متوقف نمایید.
#### پارامترهای ورودی StopCampaign
|نام|نوع|توضیحات|
| ------------- |:-------------:| -----:|
|Token|String *|توکن ثبت شده در سامانه|
|CampaignID|Int *|کد کمپین|
#### خروجی StopCampaign (Bool)
```html
result = true عملیات موفق آمیز بوده و کمپین مورد نظر خود در حالت توقف قرار گرفت

result = false عملیات ناموفق
```
#### نمونه کد StopCampaign (C#)
```csharp
string Token = "MyToken";
int CampaignID = 1000;
using (var client = new System.Net.WebClient())
{
    client.Headers.Add(System.Net.HttpRequestHeader.Authorization, Token);

    var data = new System.Collections.Specialized.NameValueCollection()
    {
      {"CampaignID",CampaignID },
    };

    byte[] bytes = client.UploadValues("https://portal.avanak.ir/rest/StopCampaign", data);

    string json = System.Text.UTF8Encoding.UTF8.GetString(bytes);//خروجی
}
```
#### نمونه کد StopCampaign (PHP)
```php
$url = "https://portal.avanak.ir/rest/StopCampaign";

$url = $url."?"."Token=".urlencode("MyToken");
$url = $url."&"."CampaignID=".urlencode("1000");
$json = file_get_contents($url);
echo $json;

//$result = json_decode($json);
//echo $result->Status;
```
### متد لیست شماره کمپین‌ها براساس کد کمپین (GetCampaignNumbersByCampaignID)
از طریق این متد می توانید با استفاده از "کد کمپین" ، لیست شماره ها و وضعیت آنها را دریافت نمایید.
#### پارامترهای ورودی GetCampaignNumbersByCampaignID
|نام|نوع|توضیحات|
| ------------- |:-------------:| -----:|
|Token|String *|توکن ثبت شده در سامانه|
|CampaignID|Int *|کد کمپین|
#### نمونه کد GetCampaignNumbersByCampaignID (C#)
```csharp
string Token = "MyToken";
int CampaignID = 1000;
using (var client = new System.Net.WebClient())
{
    client.Headers.Add(System.Net.HttpRequestHeader.Authorization, Token);

    var data = new System.Collections.Specialized.NameValueCollection()
    {
      {"CampaignID",CampaignID },
    };

    byte[] bytes = client.UploadValues("https://portal.avanak.ir/rest/GetCampaignNumbersByCampaignID", data);

    string json = System.Text.UTF8Encoding.UTF8.GetString(bytes);//خروجی
}
```
#### نمونه کد GetCampaignNumbersByCampaignID (PHP)
```php
$url = "https://portal.avanak.ir/rest/GetCampaignNumbersByCampaignID";

$url = $url."?"."Token=".urlencode("MyToken");
$url = $url."&"."CampaignID=".urlencode("1000");
$json = file_get_contents($url);
echo $json;

//$result = json_decode($json);
//echo $result->Status;
```
### متد مشخصات فایل صوتی (GetMessage)
از طریق این متد می توانید با استفاده از "کد صوت" ، مشخصات کامل فایل صوتی خود را دریافت نمایید.
#### پارامترهای ورودی GetMessage
|نام|نوع|توضیحات|
| ------------- |:-------------:| -----:|
|Token|String *|توکن ثبت شده در سامانه|
|MessageID|Int *|کد صوت|
#### نمونه کد GetMessage (C#)
```csharp
string Token = "MyToken";
int MessageID = 1000;
using (var client = new System.Net.WebClient())
{
    client.Headers.Add(System.Net.HttpRequestHeader.Authorization, Token);

    var data = new System.Collections.Specialized.NameValueCollection()
    {
      {"MessageID", MessageID },
    };

    byte[] bytes = client.UploadValues("https://portal.avanak.ir/rest/GetMessage", data); 

    string json = System.Text.UTF8Encoding.UTF8.GetString(bytes);//خروجی
}
```
#### نمونه کد GetMessage (PHP)
```php
$url = "https://portal.avanak.ir/rest/GetMessage";

$url = $url."?"."Token=".urlencode("MyToken");
$url = $url."&"."MessageID=".urlencode("1000");
$json = file_get_contents($url);
echo $json;

//$result = json_decode($json);
//echo $result->Status;
```
### متد حذف فایل صوتی (DeleteMessage)
از طریق این متد می توانید با استفاده از "کد صوت" ، فایل صوتی خود را از گالری اصوات حذف نمایید.
#### پارامترهای ورودی DeleteMessage
|نام|نوع|توضیحات|
| ------------- |:-------------:| -----:|
|Token|String *|توکن ثبت شده در سامانه|
|MessageID|Int *|کد صوت|
#### خروجی DeleteMessage (Object)
```html
result = true عملیات موفق آمیز بوده و فایل صوتی مورد نظر خود را حذف نموده اید

result = false عملیات ناموفق
```
#### نمونه کد DeleteMessage (C#)
```csharp
string Token = "MyToken";
int MessageID = 1000;
using (var client = new System.Net.WebClient())
{
    client.Headers.Add(System.Net.HttpRequestHeader.Authorization, Token);

    var data = new System.Collections.Specialized.NameValueCollection()
    {
      {"MessageID", MessageID },
    };

    byte[] bytes = client.UploadValues("https://portal.avanak.ir/rest/DeleteMessage", data); 

    string json = System.Text.UTF8Encoding.UTF8.GetString(bytes);//خروجی
}
```
#### نمونه کد DeleteMessage (PHP)
```php
$url = "https://portal.avanak.ir/rest/DeleteMessage";

$url = $url."?"."Token=".urlencode("MyToken");
$url = $url."&"."MessageID=".urlencode("1000");
$json = file_get_contents($url);
echo $json;

//$result = json_decode($json);
//echo $result->Status;
```
