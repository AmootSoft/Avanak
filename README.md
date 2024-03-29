# Avanak
## سامانه ی پیام صوتی آوانک
آوانک (اولین اپراتور پیام صوتی در ایران) می باشد که با گستردگی زیر ساخت های مخابراتی توانایی برقراری تماس و ارسال صدای شما به صورت همزمان برای انبوهی از مخاطبین تلفن ثابت و همراه را فراهم کرده است.

آدرس وب سایت
https://www.avanak.ir

آدرس پرتال
https://portal.avanak.ir

آدرس مستندات وب سرویس 
https://portal.avanak.ir/documentation

## وب سرویس وب سرویس نگارش 3 (ASMX)
آدرس وب سرویس https://portal.avanak.ir/webservice3.asmx

### متد باقیمانده اعتبار (GetCredit)
از طریق این متد می توانید اعتبار باقیمانده و موجود در پنل را بصورت مبلغ به ریال دریافت نمایید .
#### پارامترهای ورودی GetCredit
|نام|نوع|توضیحات|
| ------------- |:-------------:| -----:|
|userName|String *|نام کاربری|
|password|String *|رمز عبور|
#### خروجی GetCredit (Decimal)
```html
result >= 0  عملیات موفق آمیز بوده و مبلغ ریالی باقیمانده اعتبار را نشان میدهد

در غیر اینصورت کد خطای احراز میباشد
```
#### نمونه کد GetCredit (C#)
```csharp
string UserName = "MyUserName";
string Password = "MyPassword";

ServiceReference1.WebService3SoapClient client = new ServiceReference1.WebService3SoapClient();

decimal result = client.GetCredit(UserName, Password);

if (result >= 0)
{
    //خروجی
}
```
### متد ارسال رمز یکبار مصرف (SendOTP)
از طریق این متد می توانید پیام صوتی با رمز یکبار مصرف را به صورت فوری به شماره انتخابی خود ارسال نمایید .
#### پارامترهای ورودی SendOTP
|نام|نوع|توضیحات|
| ------------- |:-------------:| -----:|
|userName|String *|نام کاربری|
|password|String *|رمز عبور|
|Length|Int *|طول کد|
|number|String *|شماره همراه یا ثابت|
|text|String |کد دلخواه خود را وارد نمایید حداقل 4 و حداکثر 8 رقم|
|serverid|Int |کد سرور (خودکار=0)|
#### خروجی SendOTP (String)
```html
result > 0 عملیات موفق آمیز بوده و کد ارسال سریع "quickSendId" را نشان میدهد

کد خطاها :
"-5" طول کد غیرمجاز
"-25" ثبت OTP غیرفعال میباشد
"-2" شماره اشتباه میباشد
"-6" زمان ارسال غیرمجاز میباشد
 "-3" عدم موجودی کافی
"امکان اتصال به سرور وجود ندارد "
"خطایی رخ داده است "
```
#### نمونه کد SendOTP (C#)
```csharp
string UserName = "MyUserName";
string Password = "MyPassword";
int Length = 5;
string number = "09120000000";
string text = "";
int ServerId = 0;

ServiceReference1.WebService3SoapClient client = new ServiceReference1.WebService3SoapClient();

string result = client.SendOTP(UserName, Password, Length, number, text, ServerId);
```
### متد آپلود فایل صوتی (UploadMessage)
از طریق این متد می توانید فایل صوتی خود را در گالری اصوات خود آپلود نمایید.
#### پارامترهای ورودی UploadMessage
|نام|نوع|توضیحات|
| ------------- |:-------------:| -----:|
|userName|String *|نام کاربری|
|password|String *|رمز عبور|
|title|String *|عنوان|
|file|Byte[] *|فایل|
|Persist|Bool |ماندگار|
|CallFromMobile|String |شماره همراه که در انتهای صوت اضافه میشود (پیش فرض خالی باشد)|
#### خروجی UploadMessage (Int)
```html
result > 0 عملیات موفق آمیز بوده و کد فایل صوتی "messageId" را نشان میدهد

کد خطاها
-10 فرمت فایل نامناسب
-20 خطا در تبدیل
-30 خطا در تبدیل
-500 خطای سرور
```
#### نمونه کد UploadMessage (C#)
```csharp
string UserName = "MyUserName";
string Password = "MyPassword";
string title = "صوت جدید";
byte[] fileBytes = System.IO.File.ReadAllBytes("C:\\MyAudio.mp3");
bool persist = false;
string CallFromMobile = string.Empty;

ServiceReference1.WebService3SoapClient client = new ServiceReference1.WebService3SoapClient();

int messageId = client.UploadMessage(UserName, Password, title, fileBytes, persist, CallFromMobile);

if (messageId >= 0)
{
    //خروجی
}
```
### متد تولید صوت آواخوان (GenerateTTS2)
از طریق این متد می توانید متن مورد نظر خود را بصورت فایل صوتی در گالری اصوات خود ذخیره نمایید.
#### پارامترهای ورودی GenerateTTS2
|نام|نوع|توضیحات|
| ------------- |:-------------:| -----:|
|userName|String *|نام کاربری|
|password|String *|رمز عبور|
|speaker|String *|انتخاب گوینده - male برای گوینده آقا - female برای گوینده خانم|
|text|String *|متن|
|title|String *|عنوان|
|CallFromMobile|String |شماره همراه که در انتهای صوت اضافه میشود (پیش فرض خالی باشد)|
#### خروجی GenerateTTS2 (Object)
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
#### نمونه کد GenerateTTS2 (C#)
```csharp
string UserName = "MyUserName";
string Password = "MyPassword";

string speaker = "male";
string title = "صوت جدید";
string text = "این یک صوت ایجاد شده آزمایشی توسط آواخوان می باشد";
string CallFromMobile = string.Empty;

ServiceReference1.WebService3SoapClient client = new ServiceReference1.WebService3SoapClient();

var result = client.GenerateTTS2(UserName, Password, speaker, text, title, CallFromMobile);

if (result != null)
{
    //خروجی
}
```
### متد ایجاد کمپین (CreateCampaign)
از طریق این متد می توانید با وارد کردن کد فایل صوتی و شماره و زمان شروع و پایان ، پیام های خود را به شماره یا شماره های انتخابی ارسال نمایید.
#### پارامترهای ورودی CreateCampaign
|نام|نوع|توضیحات|
| ------------- |:-------------:| -----:|
|userName|String *|نام کاربری|
|password|String *|رمز عبور|
|title|String *|عنوان|
|numbers|String *|لیست شماره ها که با , ازهم جدا شده اند|
|maxTryCount|Int *|حداکثر تعداد تلاش برای هر شماره|
|minuteBetweenTries|Int *|مدت تاخیر بین تلاش ها بر اساس دقیقه|
|startDate|String *|2022-11-11 تاریخ شروع ارسال لیست به میلادی. مانند:|
|startTime|String *|ساعت شروع ارسال لیست. مانند: 18:11|
|endDate|String *|2022-11-11 تاریخ خاتمه ارسال لیست به میلادی. مانند:|
|endTime|String *|ساعت خاتمه ارسال لیست. مانند: 18:11|
|messageId|Int *|کد صوت|
|removeInvalids|Bool |حذف شماره های اشتباه و تکراری بصورت خودکار|
|serverId|Int |کد سرور (خودکار=0)|
|autoStart|Bool |شروع خودکار پس از ایجاد|
|vote|Bool |فعال سازی امکان نظرسنجی بر روی تماس ها|
#### خروجی CreateCampaign (Long)
```html
result > 0 عملیات موفق آمیز بوده و کد کمپین "campaignId" را نشان میدهد

کد خطاها
-30 امکان شروع لیست ارسال وجود ندارد
-50 جهت شروع ارسال اعتبار کافی نیست
-60 کد فایل صوتی انتخاب شده صحیح نیست
-70 مقدار حداکثر تلاش غیرمجاز میباشد
-71 مدت ضبط صدا غیرمجاز میباشد
-72 عدم مجوز ضبط صدا
-73 ناحیه انتخابی در نقشه موجود نمیباشد
-74 خطا در دریافت شماره های ناحیه انتخابی
-75 مقادیر بطور کلی صحیح نمیباشد
```
#### نمونه کد CreateCampaign (C#)
```csharp
string UserName = "MyUserName";
string Password = "MyPassword";

string numbers = "09120000000,09150000000,02100000000";
string title = "لیست ارسال تست";
int maxtrycount = 1;
int minuteBetweenTries = 60;

string startDate = "2022-09-01";
string startTime = "12:00";

string endDate = "2022-09-01";
string endTime = "15:00";

int messageId = 9999;
bool removeInvalids = true;

int serverid = 0;
bool autostart = true;
bool vote = false;

ServiceReference1.WebService3SoapClient client = new ServiceReference1.WebService3SoapClient();

var CampaignId = client.CreateCampaign(UserName, Password, title, numbers, maxtrycount, minuteBetweenTries,
startDate, startTime, endDate, endTime,
messageId, removeInvalids, serverid, autostart, vote);

if (CampaignId >= 0)
{
    //خروجی
}
```
### متد ارسال سریع (QuickSend)
از طریق این متد می توانید با وارد کردن کد فایل صوتی و شماره ، یک پیام خود را به شماره انتخابی ارسال سریع نمایید.
#### پارامترهای ورودی QuickSend
|نام|نوع|توضیحات|
| ------------- |:-------------:| -----:|
|userName|String *|نام کاربری|
|password|String *|رمز عبور|
|messageId|Int *|کد صوت|
|number|String *|شماره همراه یا ثابت|
|vote|Bool |فعال سازی امکان نظرسنجی بر روی تماس ها|
|serverid|Int |کد سرور (خودکار=0)|
#### خروجی QuickSend (Int)
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
string UserName = "MyUserName";
string Password = "MyPassword";

int messageId = 9999;
string number = "09120000000";
bool vote = false;
int serverid = 0;

ServiceReference1.WebService3SoapClient client = new ServiceReference1.WebService3SoapClient();

var quickSendId = client.QuickSend(UserName, Password, messageId, number, vote, serverid);

if (quickSendId >= 0)
{
    //خروجی
}
```
### متد ارسال سریع با آواخوان (QuickSendWithTTS)
از طریق این متد می توانید با وارد کردن متن و شماره ، یک پیام خود را به شماره انتخابی ارسال سریع نمایید.
#### پارامترهای ورودی QuickSendWithTTS
|نام|نوع|توضیحات|
| ------------- |:-------------:| -----:|
|userName|String *|نام کاربری|
|password|String *|رمز عبور|
|text|String *|متن|
|number|String *|شماره همراه یا ثابت|
|vote|Bool |فعال سازی امکان نظرسنجی بر روی تماس ها|
|serverid|Int |کد سرور (خودکار=0)|
|CallFromMobile|String |شماره همراه که در انتهای صوت اضافه میشود (پیش فرض خالی باشد)|
#### خروجی QuickSendWithTTS (Int)
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
-71 مدت ضبط صدا غیرمجاز میباشد
-72 عدم مجوز ضبط صدا
```
#### نمونه کد QuickSendWithTTS (C#)
```csharp
string UserName = "MyUserName";
string Password = "MyPassword";

string number = "09120000000";
string text = "این یک صوت ایجاد شده آزمایشی توسط آواخوان می باشد";
bool vote = false;
int serverid = 0;
string CallFromMobile = "";

ServiceReference1.WebService3SoapClient client = new ServiceReference1.WebService3SoapClient();

var quickSendId = client.QuickSendWithTTS(UserName, Password, text, number, vote, serverid, CallFromMobile);

if (quickSendId >= 0)
{
    //خروجی
}
```
### متد وضعیت ارسال سریع (GetQuickSend)
از طریق این متد می توانید با استفاده از "کد ارسال سریع" ، مشخصات کامل آن را درافت نمایید.
#### پارامترهای ورودی GetQuickSend
|نام|نوع|توضیحات|
| ------------- |:-------------:| -----:|
|userName|String *|نام کاربری|
|password|String *|رمز عبور|
|quickSendId|Int *|کد ارسال سریع|
|price|Object *|هزینه ارسال را بصورت خروجی دریافت میکنید|
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
string UserName = "MyUserName";
string Password = "MyPassword";
int quickSendId = 0;
decimal Price = 0;

ServiceReference1.WebService3SoapClient client = new ServiceReference1.WebService3SoapClient();

var result = client.GetQuickSend(UserName, Password, quickSendId, ref Price);

if (result != null)
{
    //خروجی
}
```
### متد شروع کمپین (StartCampaign)
از طریق این متد می توانیدبا استفاده از "کد کمپین" ، کمپین موردنظر خود را که در حالت انتظار یا توقف ثبت نموده اید را در زمان مورد نظر شروع به ارسال نمایید
#### پارامترهای ورودی StartCampaign
|نام|نوع|توضیحات|
| ------------- |:-------------:| -----:|
|userName|String *|نام کاربری|
|password|String *|رمز عبور|
|campaignId|Long *|کد کمپین|
|title|String *|عنوان|
|minuteBetweenTries|Int *|مدت تاخیر بین تلاش ها بر اساس دقیقه|
|maxTryCount|Int *|حداکثر تعداد تلاش برای هر شماره|
|startDate|String *|2022-11-11 تاریخ شروع ارسال لیست به میلادی. مانند:|
|endDate|String *|2022-11-11 تاریخ خاتمه ارسال لیست به میلادی. مانند:|
|startTime|String *|ساعت شروع ارسال لیست. مانند: 18:11|
|endTime|String *|ساعت خاتمه ارسال لیست. مانند: 18:11|
|serverid|Int |کد سرور (خودکار=0)|
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
string UserName = "MyUserName";
string Password = "MyPassword";
long CampaignId = 9999;

string title = "لیست ارسال تست";
int maxtrycount = 1;
int minuteBetweenTries = 60;

string startDate = "2022-09-01";
string startTime = "12:00";

string endDate = "2022-09-01";
string endTime = "15:00";

int serverid = 0;

ServiceReference1.WebService3SoapClient client = new ServiceReference1.WebService3SoapClient();

var result = client.StartCampaign(UserName, Password, CampaignId, title, minuteBetweenTries, maxtrycount,  startDate, startTime, endDate, endTime, serverid);

if (result >= 0)
{
    //خروجی
}
```
### متد توقف کمپین (StopCampaign)
از طریق این متد می توانید با استفاده از "کد کمپین" ، کمپین موردنظر خود را که در حال ارسال میباشد را متوقف نمایید.
#### پارامترهای ورودی StopCampaign
|نام|نوع|توضیحات|
| ------------- |:-------------:| -----:|
|userName|String *|نام کاربری|
|password|String *|رمز عبور|
|campaignId|Long *|کد کمپین|
#### خروجی StopCampaign (Bool)
```html
result = true عملیات موفق آمیز بوده و کمپین مورد نظر خود در حالت توقف قرار گرفت

result = false عملیات ناموفق
```
#### نمونه کد StopCampaign (C#)
```csharp
string UserName = "MyUserName";
string Password = "MyPassword";
int campaignId = 0;
decimal Price = 0;

ServiceReference1.WebService3SoapClient client = new ServiceReference1.WebService3SoapClient();

var result = client.StopCampaign(UserName, Password, campaignId);

if (result == true)
{
    //خروجی
}
```
### متد وضعیت کمپین (GetCampaignById)
از طریق این متد می توانید با استفاده از "کد کمپین" ، مشخصات آن را دریافت نمایید.
#### پارامترهای ورودی GetCampaignById
|نام|نوع|توضیحات|
| ------------- |:-------------:| -----:|
|userName|String *|نام کاربری|
|password|String *|رمز عبور|
|campaignId|Long *|کد کمپین|
#### خروجی GetCampaignById (Object)
```html
خروجی بصورت یک کلاس با فیلدهای زیر میباشد
{
Id = کد کمپین,
CampaignTitle = عنوان,
MessageId =کد فایل صوتی,
MessageTitle = عنوان فایل صوتی,
MaxTryCount = حداکثر تلاش,
MinuteBetweenTries = فاصله زمانی به دقیقه بین هر تلاش,
TotalPrice = مجموع هزینه کمپین به ریال,
InternalCount =تعداد شماره های داخل شبکه,
ExternalCount =تعداد شماره های خارج شبکه,
Vote = نظرسنجی,
StatusId = کد وضعیت کمپین,
StartDate = تاریخ شروع به میلادی,
StartTime = زمان شروع,

EndDate = تاریخ پایان به میلادی,
EndTime = زمان پایان,
}


جدول وضعیت کمپین
8=عدم_تایید
9=درانتظار
10=تاییدشده
11=درحال ارسال
12=تمام شده
27=متوقف شده
39=آماده برگشت مبالغ


در صورتیکه عملیات موفق باشد خروجی دریافت میکنید در غیر اینصورت null دریافت خواهید کرد
```
#### نمونه کد GetCampaignById (C#)
```csharp
string UserName = "MyUserName";
string Password = "MyPassword";
int campaignId = 0;

ServiceReference1.WebService3SoapClient client = new ServiceReference1.WebService3SoapClient();

var result = client.GetCampaignById(UserName, Password, campaignId);

if (result != null)
{
    //خروجی
}
```
### متد لیست کمپین ها بر اساس تاریخ (GetCampaignsByDate)
از طریق این متد می توانید لیست کمپین ها را در بازه زمانی مورد نظر خود که ایجاد نموده اید را دریافت نمایید.
#### پارامترهای ورودی GetCampaignsByDate
|نام|نوع|توضیحات|
| ------------- |:-------------:| -----:|
|userName|String *|نام کاربری|
|password|String *|رمز عبور|
|fromDate|String *|از تاریخ|
|toDate|String *|تا تاریخ|
#### خروجی GetCampaignsByDate (Object)
```html
خروجی بصورت لیستی از یک کلاس با فیلدهای زیر میباشد
{
Id = کد کمپین,
CampaignTitle = عنوان,
MessageId =کد فایل صوتی,
MessageTitle = عنوان فایل صوتی,
MaxTryCount = حداکثر تلاش,
MinuteBetweenTries = فاصله زمانی به دقیقه بین هر تلاش,
TotalPrice = مجموع هزینه کمپین به ریال,
InternalCount =تعداد شماره های داخل شبکه,
ExternalCount =تعداد شماره های خارج شبکه,
Vote = نظرسنجی,
StatusId = کد وضعیت کمپین,
StartDate = تاریخ شروع به میلادی,
StartTime = زمان شروع,

EndDate = تاریخ پایان به میلادی,
EndTime = زمان پایان,
}


جدول وضعیت کمپین
8=عدم_تایید
9=درانتظار
10=تاییدشده
11=درحال ارسال
12=تمام شده
27=متوقف شده
39=آماده برگشت مبالغ


در صورتیکه عملیات موفق باشد خروجی دریافت میکنید در غیر اینصورت null دریافت خواهید کرد
```
#### نمونه کد GetCampaignsByDate (C#)
```csharp
string UserName = "MyUserName";
string Password = "MyPassword";
string fromDate = "2022-09-01";
string toDate = "2022-09-10";

ServiceReference1.WebService3SoapClient client = new ServiceReference1.WebService3SoapClient();

var result = client.GetCampaignsByDate(UserName, Password, fromDate, toDate);

if (result != null)
{
    //خروجی
}
```
### متد لیست شماره کمپینها براساس کد کمپین (GetCampaignNumbersByCampaignId)
از طریق این متد می توانید با استفاده از "کد کمپین" ، لیست شماره ها و وضعیت آنها را دریافت نمایید.
#### پارامترهای ورودی GetCampaignNumbersByCampaignId
|نام|نوع|توضیحات|
| ------------- |:-------------:| -----:|
|userName|String *|نام کاربری|
|password|String *|رمز عبور|
|campaignId|Long *|کد کمپین|
#### خروجی GetCampaignNumbersByCampaignId (Object)
```html
خروجی بصورت لیستی از یک کلاس با فیلدهای زیر میباشد
{
CampaignID = کد کمپین,
Id = کد شماره کمپین ,
Number =شماره,
Status =کد وضعیت ,
Duration = مدت زمان شنیده شدن به ثانیه,
Price = هزینه ارسال,
SendDate =تاریخ ارسال به شمسی,
DeliverDate =ساعت ارسال,
TryCount = تعداد تلاش,
Vote = نظرسنجی
IsMobile =خارج شبکه,
RequestId = ,
}

در صورتیکه عملیات موفق باشد خروجی دریافت میکنید در غیر اینصورت null دریافت خواهید کرد
```
#### نمونه کد GetCampaignNumbersByCampaignId (C#)
```csharp
string UserName = "MyUserName";
string Password = "MyPassword";
int campaignId = 0;

ServiceReference1.WebService3SoapClient client = new ServiceReference1.WebService3SoapClient();

var result = client.GetCampaignNumbersByCampaignId(UserName, Password, campaignId);

if (result != null)
{
    //خروجی
}
```
### متد لیست شماره کمپینها براساس کد صوت (GetCampaignNumbersByMessageId)
از طریق این متد می توانید با استفاده از "کد صوت" ، لیست شماره های کمپین و وضعیت آنها را دریافت نمایید.
#### پارامترهای ورودی GetCampaignNumbersByMessageId
|نام|نوع|توضیحات|
| ------------- |:-------------:| -----:|
|userName|String *|نام کاربری|
|password|String *|رمز عبور|
|messageId|Long *|کد صوت|
|lastid|Int |آخرین کد|
|count|Int |تعداد|
#### خروجی GetCampaignNumbersByMessageId (Object)
```html
خروجی بصورت لیستی از یک کلاس با فیلدهای زیر میباشد
{
CampaignID = کد کمپین,
Id = کد شماره کمپین ,
Number =شماره,
Status =کد وضعیت ,
Duration = مدت زمان شنیده شدن به ثانیه,
Price = هزینه ارسال,
SendDate =تاریخ ارسال به شمسی,
DeliverDate =ساعت ارسال,
TryCount = تعداد تلاش,
Vote = نظرسنجی
IsMobile =خارج شبکه,
RequestId = ,
}

در صورتیکه عملیات موفق باشد خروجی دریافت میکنید در غیر اینصورت null دریافت خواهید کرد
```
#### نمونه کد GetCampaignNumbersByMessageId (C#)
```csharp
string UserName = "MyUserName";
string Password = "MyPassword";
int messageId = 0;
int lastid = 9999;
int count = 10;

ServiceReference1.WebService3SoapClient client = new ServiceReference1.WebService3SoapClient();

var result = client.GetCampaignNumbersByMessageId(UserName, Password, messageId, lastid, count);

if (result != null)
{
    //خروجی
}
```
### متد لیست شماره کمپینها بر اساس زمان ارسال (GetCampaignNumbersBySendDate)
از طریق این متد می توانید لیست شماره کمپین ها را در بازه زمانی مورد نظر خود که ایجاد نموده اید را دریافت نمایید.
#### پارامترهای ورودی GetCampaignNumbersBySendDate
|نام|نوع|توضیحات|
| ------------- |:-------------:| -----:|
|userName|String *|نام کاربری|
|password|String *|رمز عبور|
|fromDate|String *|از تاریخ|
|toDate|String *|تا تاریخ|
#### خروجی GetCampaignNumbersBySendDate (Object)
```html
خروجی بصورت لیستی از یک کلاس با فیلدهای زیر میباشد
{
CampaignID = کد کمپین,
Id = کد شماره کمپین ,
Number =شماره,
Status =کد وضعیت ,
Duration = مدت زمان شنیده شدن به ثانیه,
Price = هزینه ارسال,
SendDate =تاریخ ارسال به شمسی,
DeliverDate =ساعت ارسال,
TryCount = تعداد تلاش,
Vote = نظرسنجی
IsMobile =خارج شبکه,
RequestId = ,
}

در صورتیکه عملیات موفق باشد خروجی دریافت میکنید در غیر اینصورت null دریافت خواهید کرد
```
#### نمونه کد GetCampaignNumbersBySendDate (C#)
```csharp
string UserName = "MyUserName";
string Password = "MyPassword";
string fromDate = "2022-09-01";
string toDate = "2022-09-10";

ServiceReference1.WebService3SoapClient client = new ServiceReference1.WebService3SoapClient();

var result = client.GetCampaignNumbersBySendDate(UserName, Password, fromDate, toDate);

if (result != null)
{
    //خروجی
}
```
### متد شماره کمپین بر اساس کد (GetCampaignNumbersBySubscribeId)
از طریق این متد می توانید با استفاده از "کد شماره کمپین" ، مشخصات آن را دریافت نمایید.
#### پارامترهای ورودی GetCampaignNumbersBySubscribeId
|نام|نوع|توضیحات|
| ------------- |:-------------:| -----:|
|userName|String *|نام کاربری|
|password|String *|رمز عبور|
|subscribeId|Long *|کد شماره کمپین|
#### خروجی GetCampaignNumbersBySubscribeId (Object)
```html
خروجی بصورت یک کلاس با فیلدهای زیر میباشد
{
CampaignID = کد کمپین,
Id = کد شماره کمپین ,
Number =شماره,
Status =کد وضعیت ,
Duration = مدت زمان شنیده شدن به ثانیه,
Price = هزینه ارسال,
SendDate =تاریخ ارسال به شمسی,
DeliverDate =ساعت ارسال,
TryCount = تعداد تلاش,
Vote = نظرسنجی
IsMobile =خارج شبکه,
RequestId = ,
}

در صورتیکه عملیات موفق باشد خروجی دریافت میکنید در غیر اینصورت null دریافت خواهید کرد
```
#### نمونه کد GetCampaignNumbersBySubscribeId (C#)
```csharp
string UserName = "MyUserName";
string Password = "MyPassword";
long subscibeid = 999999;

ServiceReference1.WebService3SoapClient client = new ServiceReference1.WebService3SoapClient();

var result = client.GetCampaignNumbersBySubscribeId(UserName, Password, subscibeid);

if (result != null)
{
    //خروجی
}
```
### متد لیست شماره کمپینها بر اساس کد (GetCampaignNumbersDataByIds)
از طریق این متد می توانید با استفاده از لیستی از "کد شماره کمپین" ، مشخصات آنها را دریافت نمایید.
#### پارامترهای ورودی GetCampaignNumbersDataByIds
|نام|نوع|توضیحات|
| ------------- |:-------------:| -----:|
|userName|String *|نام کاربری|
|password|String *|رمز عبور|
|campaignNumberIds|String *|لیست کد شماره کمپین ها|
#### خروجی GetCampaignNumbersDataByIds (Object)
```html
خروجی بصورت لیستی از یک کلاس با فیلدهای زیر میباشد
{
CampaignID = کد کمپین,
Id = کد شماره کمپین ,
Number =شماره,
Status =کد وضعیت ,
Duration = مدت زمان شنیده شدن به ثانیه,
Price = هزینه ارسال,
SendDate =تاریخ ارسال به شمسی,
DeliverDate =ساعت ارسال,
TryCount = تعداد تلاش,
Vote = نظرسنجی
IsMobile =خارج شبکه,
RequestId = ,
}

در صورتیکه عملیات موفق باشد خروجی دریافت میکنید در غیر اینصورت null دریافت خواهید کرد
```
#### نمونه کد GetCampaignNumbersDataByIds (C#)
```csharp
string UserName = "MyUserName";
string Password = "MyPassword";
string CampaignNumberIds = "999999,88888";

ServiceReference1.WebService3SoapClient client = new ServiceReference1.WebService3SoapClient();

var result = client.GetCampaignNumbersDataByIds(UserName, Password, CampaignNumberIds);

if (result != null)
{
    //خروجی
}
```
### متد وضعیت شماره کمپین (GetCampaignNumberStatusById)
از طریق این متد می توانید با استفاده از "کد شماره کمپین" ، ,وضعیت آن را دریافت نمایید.
#### پارامترهای ورودی GetCampaignNumberStatusById
|نام|نوع|توضیحات|
| ------------- |:-------------:| -----:|
|userName|String *|نام کاربری|
|password|String *|رمز عبور|
|campaignNumberId|Long *|کد شماره کمپین|
#### خروجی GetCampaignNumberStatusById (Object)
```html
result > 0 عملیات موفقیت آمیز و معادل کد وضعیت شماره میباشد
در غیر این صورت null یا کد خطای احراز میباشد
```
#### نمونه کد GetCampaignNumberStatusById (C#)
```csharp
string UserName = "MyUserName";
string Password = "MyPassword";
long CampaignNumberId = 999999;

ServiceReference1.WebService3SoapClient client = new ServiceReference1.WebService3SoapClient();

var result = client.GetCampaignNumberStatusById(UserName, Password, CampaignNumberId);

if (result != null)
{
    //خروجی
}
```
### متد لیست وضعیت شماره کمپین (GetCampaignNumbersStatusByIds)
از طریق این متد می توانید با استفاده از لیستی از "کد شماره کمپین" ، وضعیت آنها را دریافت نمایید.
#### پارامترهای ورودی GetCampaignNumbersStatusByIds
|نام|نوع|توضیحات|
| ------------- |:-------------:| -----:|
|userName|String *|نام کاربری|
|password|String *|رمز عبور|
|campaignNumberIds|String *|لیست کد شماره کمپین ها|
#### خروجی GetCampaignNumbersStatusByIds (Object)
```html
در صورت موفقیت آمیز و آرایه ای به تعداد کد درخواستی به ترتیب از کد وضعیت شماره میباشد

در صورت ناموفق آرایه با طول یک
 وعدد -10 به معنای اینکه حداکثر تعداد کدها 100 میباشد
و عدد -20به معنای خطای سرور میباشد
و اگر null باشد مه معنای خطای احراز است
```
#### نمونه کد GetCampaignNumbersStatusByIds (C#)
```csharp
string UserName = "MyUserName";
string Password = "MyPassword";
string CampaignNumberIds = "999999,88888";

ServiceReference1.WebService3SoapClient client = new ServiceReference1.WebService3SoapClient();

var result = client.GetCampaignNumbersStatusByIds(UserName, Password, CampaignNumberIds);

if (result != null)
{
    //خروجی
}
```
### متد ایجاد کمپین بر اساس مکان (CreateCampaignGIS)
از طریق این متد می توانید با وارد کردن کد مکان و کد فایل صوتی و شماره و زمان شروع و پایان ، پیام های خود را به شماره یا شماره های انتخابی ارسال نمایید.
#### پارامترهای ورودی CreateCampaignGIS
|نام|نوع|توضیحات|
| ------------- |:-------------:| -----:|
|userName|String *|نام کاربری|
|password|String *|رمز عبور|
|title|String *|عنوان|
|numbers|String *|لیست شماره ها که با , ازهم جدا شده اند|
|GISCroods|String *||
|maxTryCount|Int *|حداکثر تعداد تلاش برای هر شماره|
|minuteBetweenTries|Int *|مدت تاخیر بین تلاش ها بر اساس دقیقه|
|startDate|String *|2022-11-11 تاریخ شروع ارسال لیست به میلادی. مانند:|
|startTime|String *|ساعت شروع ارسال لیست. مانند: 18:11|
|endDate|String *|2022-11-11 تاریخ خاتمه ارسال لیست به میلادی. مانند:|
|endTime|String *|ساعت خاتمه ارسال لیست. مانند: 18:11|
|messageId|Int *|کد صوت|
|removeInvalids|Bool |حذف شماره های اشتباه و تکراری بصورت خودکار|
|serverId|Int |کد سرور (خودکار=0)|
|autoStart|Bool |شروع خودکار پس از ایجاد|
|vote|Bool |فعال سازی امکان نظرسنجی بر روی تماس ها|
#### خروجی CreateCampaignGIS (Long)
```html
result > 0 عملیات موفق آمیز بوده و کد کمپین "campaignId" را نشان میدهد

کد خطاها
-30 امکان شروع لیست ارسال وجود ندارد
-50 جهت شروع ارسال اعتبار کافی نیست
-60 کد فایل صوتی انتخاب شده صحیح نیست
-70 مقدار حداکثر تلاش غیرمجاز میباشد
-71 مدت ضبط صدا غیرمجاز میباشد
-72 عدم مجوز ضبط صدا
-73 ناحیه انتخابی در نقشه موجود نمیباشد
-74 خطا در دریافت شماره های ناحیه انتخابی
-75 مقادیر بطور کلی صحیح نمیباشد
```
### متد تولید صوت آواخوان (متد قدیمی) (GenerateTTS)
از طریق این متد می توانید متن مورد نظر خود را بصورت فایل صوتی در گالری اصوات خود ذخیره نمایید.
#### پارامترهای ورودی GenerateTTS
|نام|نوع|توضیحات|
| ------------- |:-------------:| -----:|
|userName|String *|نام کاربری|
|password|String *|رمز عبور|
|speaker|String *|انتخاب گوینده - male برای گوینده آقا - female برای گوینده خانم|
|text|String *|متن|
|title|String *|عنوان|
|CallFromMobile|String |شماره همراه که در انتهای صوت اضافه میشود (پیش فرض خالی باشد)|
#### خروجی GenerateTTS (Int)
```html
result > 0 عملیات موفق آمیز بوده و کد فایل صوتی "messageId" را نشان میدهد

کد خطاها
result = -6 طول متن بسیار کوتاه (کمتر از 3 ثانیه)
result = -5 طول متن بیش از حد مجاز (1000 کاراکتر)
result = -3 عدم اعتبار کافی برای تولید صوت
result = -4 خطا در تولید فایل صوتی
result = -2 خطا در تولید فایل صوتی
```
#### نمونه کد GenerateTTS (C#)
```csharp
string UserName = "MyUserName";
string Password = "MyPassword";

string speaker = "male";
string title = "صوت جدید";
string text = "این یک صوت ایجاد شده آزمایشی توسط آواخوان می باشد";
string CallFromMobile = string.Empty;

ServiceReference1.WebService3SoapClient client = new ServiceReference1.WebService3SoapClient();

var result = client.GenerateTTS(UserName, Password, speaker, text, title, CallFromMobile);

if (result >= 0)
{
    //خروجی
}
```
### متد آپلود فایل صوتی بصورت Base64 (UploadMessageBase64)
از طریق این متد می توانید فایل صوتی خود را که بصورت Base64 کدگذاری شده است در گالری اصوات خود آپلود نمایید.
#### پارامترهای ورودی UploadMessageBase64
|نام|نوع|توضیحات|
| ------------- |:-------------:| -----:|
|userName|String *|نام کاربری|
|password|String *|رمز عبور|
|title|String *|عنوان|
|base64|String *|فایل که بصورت Base64 کد گذاری شده باشد|
|Persist|Bool |ماندگار|
|CallFromMobile|String |شماره همراه که در انتهای صوت اضافه میشود (پیش فرض خالی باشد)|
#### خروجی UploadMessageBase64 (Int)
```html
result > 0 عملیات موفق آمیز بوده و کد فایل صوتی "messageId" را نشان میدهد

کد خطاها
-10 فرمت فایل نامناسب
-20 خطا در تبدیل
-30 خطا در تبدیل
-500 خطای سرور
```
### متد دانلود فایل صوتی (DownloadMessage)
از طریق این متد می توانید با استفاده از "کد صوت" ، فایل صوتی خود را دانلود نمایید.
#### پارامترهای ورودی DownloadMessage
|نام|نوع|توضیحات|
| ------------- |:-------------:| -----:|
|userName|String *|نام کاربری|
|password|String *|رمز عبور|
|messageId|Int *|کد صوت|
#### خروجی DownloadMessage (Object)
```html
خروجی بصورت آرایه ای از byte میباشد

در صورتیکه عملیات موفق باشد خروجی دریافت میکنید در غیر اینصورت null دریافت خواهید کرد
```
#### نمونه کد DownloadMessage (C#)
```csharp
string UserName = "MyUserName";
string Password = "MyPassword";
int messageId = 999999;

ServiceReference1.WebService3SoapClient client = new ServiceReference1.WebService3SoapClient();

var result = client.DownloadMessage(UserName, Password, messageId);

if (result != null)
{
    //خروجی
}
```
### متد حذف فایل صوتی (DeleteMessage)
از طریق این متد می توانید با استفاده از "کد صوت" ، فایل صوتی خود را از گالری اصوات حذف نمایید.
#### پارامترهای ورودی DeleteMessage
|نام|نوع|توضیحات|
| ------------- |:-------------:| -----:|
|userName|String *|نام کاربری|
|password|String *|رمز عبور|
|messageId|Int *|کد صوت|
#### خروجی DeleteMessage (Bool)
```html
result = true عملیات موفق آمیز بوده و فایل صوتی مورد نظر خود را حذف نموده اید

result = false عملیات ناموفق
```
#### نمونه کد DeleteMessage (C#)
```csharp
string UserName = "MyUserName";
string Password = "MyPassword";
int messageId = 999999;

ServiceReference1.WebService3SoapClient client = new ServiceReference1.WebService3SoapClient();

var result = client.DeleteMessage(UserName, Password, messageId);

if (result ==true)
{
    //خروجی
}
```
### متد مشخصات فایل صوتی (GetMessage)
از طریق این متد می توانید با استفاده از "کد صوت" ، مشخصات کامل فایل صوتی خود را دریافت نمایید.
#### پارامترهای ورودی GetMessage
|نام|نوع|توضیحات|
| ------------- |:-------------:| -----:|
|userName|String *|نام کاربری|
|password|String *|رمز عبور|
|messageId|Int *|کد صوت|
#### خروجی GetMessage (Object)
```html
خروجی بصورت یک کلاس با فیلدهای زیر میباشد
{
Id = کد فایل صوتی,
Title = عنوان,
Lenght =طول فایل صوتی به ثانیه,
IsDefault = پیش فرض,
ShowInGallery = نمایش در گالری,
Confirmed = وضعیت تایید,
CreateDate = تاریخ زمان ایجاد به میلادی
}

وضعیت تایید صوت
0=در انتظار تایید
1=تایید شده
2=رد شده
3=در انتظار تائید تلفنی

در صورتیکه عملیات موفق باشد خروجی دریافت میکنید در غیر اینصورت null دریافت خواهید کرد
```
#### نمونه کد GetMessage (C#)
```csharp
string UserName = "MyUserName";
string Password = "MyPassword";
int messageId = 999999;

ServiceReference1.WebService3SoapClient client = new ServiceReference1.WebService3SoapClient();

var result = client.GetMessage(UserName, Password, messageId);

if (result != null)
{
    //خروجی
}
```
### متد لیست مشخصات فایل صوتی (GetMessages)
از طریق این متد می توانید لیست همه اصوات گالری خود را با مشخصات کامل دریافت نمایید.
#### پارامترهای ورودی GetMessages
|نام|نوع|توضیحات|
| ------------- |:-------------:| -----:|
|userName|String *|نام کاربری|
|password|String *|رمز عبور|
#### خروجی GetMessages (Object)
```html
خروجی بصورت لیستی از کلاس با فیلدهای زیر میباشد
{
Id = کد فایل صوتی,
Title = عنوان,
Lenght =طول فایل صوتی به ثانیه,
IsDefault = پیش فرض,
ShowInGallery = نمایش در گالری,
Confirmed = وضعیت تایید,
CreateDate = تاریخ زمان ایجاد به میلادی
}

وضعیت تایید صوت
0=در انتظار تایید
1=تایید شده
2=رد شده
3=در انتظار تائید تلفنی

در صورتیکه عملیات موفق باشد خروجی دریافت میکنید در غیر اینصورت null دریافت خواهید کرد
```
#### نمونه کد GetMessages (C#)
```csharp
string UserName = "MyUserName";
string Password = "MyPassword";

ServiceReference1.WebService3SoapClient client = new ServiceReference1.WebService3SoapClient();

var result = client.GetMessages(UserName, Password);

if (result != null)
{
    //خروجی
}
```
### متد لیست فایلهای صوتی بر اساس صفحه بندی (GetMessagesByPage)
از طریق این متد می توانید لیست همه اصوات گالری خود را بصورت صفحه بندی با مشخصات کامل دریافت نمایید.
#### پارامترهای ورودی GetMessagesByPage
|نام|نوع|توضیحات|
| ------------- |:-------------:| -----:|
|userName|String *|نام کاربری|
|password|String *|رمز عبور|
|showPersist|Bool *|نمایش فقط ماندگار|
|count|Int *|تعداد|
|currentpage|Int *||
|order|String *|ترتیب|
#### خروجی GetMessagesByPage (Object)
```html
خروجی بصورت لیستی از کلاس با فیلدهای زیر میباشد
{
Id = کد فایل صوتی,
Title = عنوان,
Lenght =طول فایل صوتی به ثانیه,
IsDefault = پیش فرض,
ShowInGallery = نمایش در گالری,
Confirmed = وضعیت تایید,
CreateDate = تاریخ زمان ایجاد به میلادی
}

وضعیت تایید صوت
0=در انتظار تایید
1=تایید شده
2=رد شده
3=در انتظار تائید تلفنی

در صورتیکه عملیات موفق باشد خروجی دریافت میکنید در غیر اینصورت null دریافت خواهید کرد
```
#### نمونه کد GetMessagesByPage (C#)
```csharp
string UserName = "MyUserName";
string Password = "MyPassword";
bool showPersist = true;
int count = 50;
int currentpage = 0;
string order = "desc";

ServiceReference1.WebService3SoapClient client = new ServiceReference1.WebService3SoapClient();

var result = client.GetMessagesByPage(UserName, Password, showPersist, count, currentpage, order);

if (result != null)
{
    //خروجی
}
```
### متد دریافت تعرفه (GetPrices)
از طریق این متد می توانید کامل تعرفه خود را دریافت نمایید .
#### پارامترهای ورودی GetPrices
|نام|نوع|توضیحات|
| ------------- |:-------------:| -----:|
|userName|String *|نام کاربری|
|password|String *|رمز عبور|
#### خروجی GetPrices (Object)
```html
خروجی بصورت یک کلاس با فیلدهای زیر میباشد
{
Id = کد گروه قیمت,
InternalSecondPrice = تعرفه هر ثانیه داخل شبکه,
ExternalSecondPrice = تعرفه هر ثانیه خارج سبکه,
PulseDuration = مدت زمان هرپالس به ثانیه,
PulseOffRate = ,
AddClientRate = مبلغ افزودن هر کاربر به ثانیه,
AddResellerRate = مبلغ افزودن هر نماینده به ثانیه,
LBSSecondRate = ,
GPSSecondRate = ,
PoolSecondRate =,
AddClientPrice = مبلغ افزودن هر کاربر به ریال,
AddResellerPrice = مبلغ افزودن هر نماینده به ریال,
LBSSecondPrice =,
GPSSecondPrice =,
PoolSecondPrice =,
TTSSecondPrice =,
MaximumDebt =,
ResellerId = کدنماینده,
Title = عنوان گروه قیمت
}

در صورتیکه عملیات موفق باشد خروجی دریافت میکنید در غیر اینصورت null دریافت خواهید کرد
```
#### نمونه کد GetPrices (C#)
```csharp
string UserName = "MyUserName";
string Password = "MyPassword";

ServiceReference1.WebService3SoapClient client = new ServiceReference1.WebService3SoapClient();

var result = client.GetPrices(UserName, Password);

if (result != null)
{
    //خروجی
}
```
### متد مشخصات حساب کاربری (GetProfile)
از طریق این متد می توانید مشخصات حساب کاربری خود را دریافت نمایید .
#### پارامترهای ورودی GetProfile
|نام|نوع|توضیحات|
| ------------- |:-------------:| -----:|
|userName|String *|نام کاربری|
|password|String *|رمز عبور|
#### خروجی GetProfile (Object)
```html
خروجی بصورت یک کلاس با فیلدهای زیر میباشد
{
Result=0
UserName=نام کاربری
FullName=نام کامل
Mobile=موبایل
ExpireDate=تاریخ انقضاء به شمسی
}

در صورتیکه عملیات موفق باشد خروجی دریافت میکنید در غیر اینصورت Result کد خطای احراز میباشد
```
#### نمونه کد GetProfile (C#)
```csharp
string UserName = "MyUserName";
string Password = "MyPassword";

ServiceReference1.WebService3SoapClient client = new ServiceReference1.WebService3SoapClient();

var result = client.GetProfile(UserName, Password);

if (result != null)
{
    //خروجی
}
```
### متد دریافت تاریخ انقضاء (GetExpirationDate)
از طریق این متد می توانید تاریخ انقضاء حساب کاربری خود بصورت شمسی را دریافت نمایید .
#### پارامترهای ورودی GetExpirationDate
|نام|نوع|توضیحات|
| ------------- |:-------------:| -----:|
|userName|String *|نام کاربری|
|password|String *|رمز عبور|
#### خروجی GetExpirationDate (String)
```html
خروجی تاریخ انقضای پنل بصورت شمسی میباشد
اگر پنل دائمی باشد تاریخ ثابت 1500/01/01 دریافت خواهید کرد

در غیر اینصورت کد خطای احراز میباشد
```
#### نمونه کد GetExpirationDate (C#)
```csharp
string UserName = "MyUserName";
string Password = "MyPassword";

ServiceReference1.WebService3SoapClient client = new ServiceReference1.WebService3SoapClient();

var result = client.GetExpirationDate(UserName, Password);

if (result != null)
{
    //خروجی
}
```
### متد مجموع هزینه در بازه زمانی (GetTotalCostByDate)
از طریق این متد می توانید مجموع هزینه ارسال های کمپین خود را در بازه زمانی مشخص بصورت مبلغ به ریال دریافت نمایید .
#### پارامترهای ورودی GetTotalCostByDate
|نام|نوع|توضیحات|
| ------------- |:-------------:| -----:|
|userName|String *|نام کاربری|
|password|String *|رمز عبور|
|fromDate|String *|از تاریخ|
|toDate|String *|تا تاریخ|
#### خروجی GetTotalCostByDate (Decimal)
```html
result >= 0 عملیات موفق آمیز بوده و مبلغ ریالی مجموع هزینه ارسال را نشان میدهد

در غیر اینصورت کد خطای احراز میباشد
```
#### نمونه کد GetTotalCostByDate (C#)
```csharp
string UserName = "MyUserName";
string Password = "MyPassword";
string fromDate = "2022-09-01";
string toDate = "2022-09-10";

ServiceReference1.WebService3SoapClient client = new ServiceReference1.WebService3SoapClient();

var result = client.GetTotalCostByDate(UserName, Password, fromDate, toDate);

if (result >= 0)
{
    //خروجی
}
```
### متد لیست تراکنش های اعتباری (GetTransactions)
از طریق این متد می توانید لیست 1000 تراکنش اعتباری اخیر را دریافت نمایید
#### پارامترهای ورودی GetTransactions
|نام|نوع|توضیحات|
| ------------- |:-------------:| -----:|
|userName|String *|نام کاربری|
|password|String *|رمز عبور|
#### خروجی GetTransactions (Object)
```html
خروجی بصورت لیستی از کلاس با فیلدهای زیر میباشد
{
Amount=مقدار به ثانیه
CreateDate=تاریخ زمان به میلادی
Description=متن توضیحات
}

در صورتیکه عملیات موفق باشد خروجی دریافت میکنید در غیر اینصورت null دریافت خواهید کرد
```
#### نمونه کد GetTransactions (C#)
```csharp
string UserName = "MyUserName";
string Password = "MyPassword";

ServiceReference1.WebService3SoapClient client = new ServiceReference1.WebService3SoapClient();

var result = client.GetTransactions(UserName, Password);

if (result != null)
{
    //خروجی
}
```
### متد ایجاد تماس امن (MakeSecureCall)
از طریق این متد میتوان با دو شماره تلفن بدون مشخص بدون شماره آنها تماس برقرار کرد
#### پارامترهای ورودی MakeSecureCall
|نام|نوع|توضیحات|
| ------------- |:-------------:| -----:|
|userName|String *|نام کاربری|
|password|String *|رمز عبور|
|srcNumber|String *|شماره تلفن مبداء|
|dstNumber|String *|شماره تلفن مقصد|
|holdMessageId|Int *||
|serverId|Int *|کد سرور (خودکار=0)|
#### خروجی MakeSecureCall (Int)
```html
result > 0 عملیات موفق آمیز بوده و کد کمپین "campaignId" را نشان میدهد

کد خطاها
0 تماس امن غیرفعال میباشد
-100 شماره مبدا یا مقصد اشتباه میباشد
-101 شماره مبدا و مقصد یکسان میباشد
-5 عدم مجوز تماس امن
-2 کد سرور اشتباه میباشد
-3 عدم موجودی حداقلی (علی الحساب) 10000 ریال
```
#### نمونه کد MakeSecureCall (C#)
```csharp
string UserName = "MyUserName";
string Password = "MyPassword";
string srcNumber = "09120000000";
string dstNumber = "09150000000";
int holdMessageId = 0;
int serverId = 0;

ServiceReference1.WebService3SoapClient client = new ServiceReference1.WebService3SoapClient();

var campaignId = client.MakeSecureCall(UserName, Password, srcNumber, dstNumber, holdMessageId, serverId);

if (campaignId > 0)
{
    //خروجی
}
```
### متد ارسال OCV (SendOCV)
اعتبار سنجی با یک کلیک
#### پارامترهای ورودی SendOCV
|نام|نوع|توضیحات|
| ------------- |:-------------:| -----:|
|userName|String *|نام کاربری|
|password|String *|رمز عبور|
|number|String *|شماره همراه یا ثابت|
|callbackUrl|String *||
|serverid|Int |کد سرور (خودکار=0)|
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
string UserName = "MyUserName";
string Password = "MyPassword";
string number = "09120000000";
string callBackUrl = "http://mysite.com/OCVCallBack?number=09120000000";
int serverId = 0;

ServiceReference1.WebService3SoapClient client = new ServiceReference1.WebService3SoapClient();

var result = client.SendOCV(UserName, Password, number, callBackUrl, serverId);

if (result != null)
{
    //خروجی
}
```
