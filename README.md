در Compose برای نگه‌داشتن و مدیریت «وضعیت» (state) روی صفحه، از state و remember استفاده می‌شود تا با هر بار رندر شدن دوباره UI، مقدارها از بین نروند و بتوانیم به تغییرات کاربر واکنش نشان دهیم. اگر از آن‌ها استفاده نشود، هر بار که کامپوزبل دوباره ساخته می‌شود، همه متغیرها به حالت اولیه برمی‌گردند و UI ثابت و بی‌فایده می‌ماند.[3][7]

## مقدمه خیلی ساده

- استیت «State» یعنی وضعیت فعلی یک چیز در UI؛ مثلا عدد روی یک دکمه‌ی شمارنده، روشن/خاموش بودن یک سوییچ، انتخاب‌شدن یک آیتم، و غیره.[13]
- در Compose هر بار که داده عوض می‌شود، تابع‌های @Composable دوباره اجرا می‌شوند و UI را از روی state جدید می‌سازند.[6]
- برای اینکه این وضعیت وسط راه گم نشود، از ترکیب remember + mutableStateOf استفاده می‌کنیم.[7]

## state چیست؟

خیلی ساده: state = متغیری که وقتی عوض می‌شود، UI خودش آپدیت می‌شود.[13]

مثال مفهومی (بدون کد):  
- یک شمارنده روی صفحه داریم که عدد 0 را نشان می‌دهد.  
- وقتی روی دکمه «+» می‌زنیم، عدد به 1 و 2 و … تغییر می‌کند.  
- اگر این عدد را به‌صورت یک state نگه داریم، هر بار که عدد عوض شود، متن روی صفحه هم خودکار عوض می‌شود.[7]

در Compose معمولا از این الگو استفاده می‌شود:  
- var count by remember { mutableStateOf(0) }  
- این یعنی: «یک state به اسم count دارم، مقدار اولیه‌اش 0 است، و می‌خواهم بین رندرها حفظ شود.»[7]

## remember چیست؟

به‌شدت ساده: remember یعنی «این مقدار را یادت بماند، دفعه‌ی بعد که دوباره این تابع اجرا شد، از همان مقدار قبلی استفاده کن، نه از اول.»[7]

بدون remember:  
- هر بار که Composable دوباره اجرا می‌شود، متغیرهای معمولی دوباره از اول ساخته می‌شوند.  
- پس count همیشه برمی‌گردد به 0 و UI عملا تغییر واقعی را نمی‌بیند.[9]

با remember:  
- مقدار را داخل «Composition» ذخیره می‌کند تا با هر بار اجرای دوباره‌ی Composable، مقدار قبلی را برگرداند، نه مقدار اولیه.[5][7]
- وقتی state تغییر کند، همان قسمت UI که از آن state استفاده می‌کند دوباره رسم می‌شود.[5]

## چرا به state و remember احتیاج داریم؟

چند دلیل ساده و دم‌دستی:  

- UI داینامیک: بدون state، صفحه‌ات فقط یک تصویر ثابت است؛ با کلیک کردن هیچ چیز واقعی تغییر نمی‌کند.[3]
- حفظ مقادیر بین رندرها: Compose خیلی زیاد و خودکار «recompose» می‌کند؛ remember کمک می‌کند count، isChecked، selectedIndex و… وسط این بازسازی‌ها گم نشوند.[7]
- سینک بودن داده و UI: وقتی state عوض می‌شود، UI هم خودکار آپدیت می‌شود؛ نیازی نیست دستی بگویی «این ویو را ریفرش کن».[3]

به زبان خیلی ساده:  
- state = «جایی که وضعیت را نگه می‌دارم و اگر عوض شد، UI آپدیت می‌شود.»[13]
- remember = «کاری کن این state هر بار از اول ساخته نشود و مقدارش یادش بماند.»[7]

## مثال ۱: شمارنده با دکمه

ایده‌ی ذهنی (بدون TextField و بدون وارد کردن ورودی متنی):  

- یک دکمه روی صفحه داریم که وسطش عدد count را نشان می‌دهد.  
- مقدار اولیه count = 0.  
- هر بار روی دکمه می‌زنیم، count یک واحد زیاد می‌شود.  
- state: همان count است.  
- remember: کمک می‌کند count با هر کلیک صفر نشود.[7]

اسکلت کد (برای تصور ذهنی، ساده‌سازی شده):  

- در یک Composable به اسم CounterScreen:  
  - یک متغیر count تعریف می‌کنی با remember { mutableStateOf(0) }.  
  - یک Button می‌گذاری، onClick آن count++ می‌کند.  
  - داخل Button یک Text که مقدار count را نمایش می‌دهد.[5][7]

اگر remember را برداری و فقط mutableStateOf(0) بگذاری، count هر بار به 0 برمی‌گردد و دکمه عملا همیشه 0 را نشان می‌دهد.[9]

## مثال ۲: تغییر رنگ با کلیک

ایده‌ی ساده:  
- یک Box یا Card داریم.  
- روی آن کلیک می‌کنیم، رنگش بین دو رنگ جابه‌جا می‌شود (مثلا قرمز/سبز).[3]

state اینجا می‌تواند یک Boolean باشد: isOn = true/false.[13]

سناریو:  
- var isOn by remember { mutableStateOf(false) }  
- اگر isOn == true → رنگ سبز.  
- اگر isOn == false → رنگ قرمز.  
- وقتی روی Box کلیک می‌کنیم، isOn = !isOn.  
- تغییر isOn باعث می‌شود Box با رنگ جدید دوباره رندر شود.[7]

## مثال ۳: انتخاب آیتم (لیست ساده بدون TextField)

سناریو ذهنی:  
- یک ستون از سه Text داریم: «آیتم ۱»، «آیتم ۲»، «آیتم ۳».  
- روی هر کدام کلیک کنیم، آن آیتم «انتخاب شده» می‌شود و مثلا رنگ متنش آبی می‌شود.[3]

state:  
- selectedIndex که مثلا عدد 0، 1 یا 2 است.  
- var selectedIndex by remember { mutableStateOf(0) }.  

منطق:  
- وقتی روی آیتم i کلیک می‌کنیم، selectedIndex = i.  
- در Text هر آیتم، اگر index == selectedIndex باشد، رنگش آبی، وگرنه خاکستری است.[7]

## تمرین‌ها (بدون TextField)

در همه تمرین‌ها فرض کن از الگوی کلی زیر استفاده می‌کنی:  
- var something by remember { mutableStateOf(initialValue) }  
- وقتی event رخ می‌دهد (onClick و …)، مقدار را تغییر می‌دهی.  
- UI با توجه به مقدار جدید رسم می‌شود.[7]

### تمرین ۱: شمارنده منفی/مثبت

هدف: تمرین شمارنده و شرط ساده.  

مسئله:  
- یک دکمه روی صفحه بگذار که عدد count را نشان دهد.  
- هر بار روی دکمه کلیک می‌شود، count یک واحد کم شود (count--).  
- اگر count کمتر از 0 شد، رنگ متن قرمز باشد، اگر 0 یا بیشتر بود، رنگ سبز باشد.[3]

سوال‌ها:  
1) چه stateهایی لازم داریم؟  
2) مقدار اولیه هر state چه باشد؟  
3) چه شرطی روی رنگ می‌نویسی؟  

پاسخ پیشنهادی (ذهنی و ساده):  
- فقط یک state: count از نوع Int.  
- var count by remember { mutableStateOf(0) }.  
- onClick دکمه: count--.  
- رنگ متن: اگر count < 0 → Red، در غیر این صورت → Green.[7]

### تمرین ۲: سوییچ نور روز/شب

هدف: تمرین Boolean state.  

مسئله:  
- یک Box تمام عرض صفحه را بکش.  
- اگر حالت «روز» فعال بود، رنگ پس‌زمینه Box سفید و متن وسط آن «Day Mode» باشد.  
- اگر حالت «شب» بود، رنگ پس‌زمینه مشکی و متن «Night Mode» باشد.  
- با یک کلیک روی Box، حالت بین روز/شب جابه‌جا شود.[3]

سوال‌ها:  
1) چه stateی لازم داریم؟  
2) مقدار اولیه چه باشد؟  
3) چطور متن و رنگ را از روی state بسازی؟  

پاسخ پیشنهادی (ذهنی):  
- یک state از نوع Boolean: isDay.  
- var isDay by remember { mutableStateOf(true) }.  
- اگر isDay == true → رنگ پس‌زمینه Color.White و متن "Day Mode".  
- اگر isDay == false → رنگ پس‌زمینه Color.Black و متن "Night Mode".  
- در Modifier.clickable روی Box، isDay = !isDay.[7]

### تمرین ۳: شمارنده با حد بالا

هدف: ترکیب شرط با شمارنده.  

مسئله:  
- یک شمارنده با دکمه بساز که از 0 شروع شود.  
- هر بار روی دکمه کلیک می‌شود، count++ شود، ولی اگر count به 5 رسید، دیگر زیاد نشود (روی 5 بماند).[3]
- وقتی count به 5 رسید، رنگ دکمه عوض شود (مثلا نارنجی شود تا بفهمیم به حد رسیده‌ایم).  

سوال‌ها:  
1) state چیست؟  
2) چطور جلوی بیشتر شدن از 5 را می‌گیری؟  
3) چطور رنگ دکمه را از روی count تعیین می‌کنی؟  

پاسخ پیشنهادی (ذهنی):  
- state: همان count از نوع Int.  
- var count by remember { mutableStateOf(0) }.  
- در onClick: اگر count < 5 بود، count++، اگر نه، کاری نکن.  
- رنگ دکمه: اگر count == 5 → Color(مثلا Orange)، وگرنه یک رنگ پیش‌فرض.[7]

### تمرین ۴: انتخاب تب (Tab) ساده

هدف: تمرین state عددی و چند UI متفاوت.  

مسئله:  
- سه Text در یک Row: «تب ۱»، «تب ۲»، «تب ۳».  
- زیر این Row، یک Box که محتوا را نشان دهد.  
- اگر تب ۱ انتخاب شده بود، Box متن «صفحه ۱» را نشان دهد و رنگ پس‌زمینه آبی باشد.  
- اگر تب ۲، رنگ سبز و متن «صفحه ۲».  
- اگر تب ۳، رنگ زرد و متن «صفحه ۳».  
- روی هر Text که کلیک شود، همان تب فعال شود.[3]

پاسخ پیشنهادی (ذهنی):  
- یک state: selectedTabIndex از نوع Int با مقدار اولیه 0.  
- var selectedTabIndex by remember { mutableStateOf(0) }.  
- هر Text در Row یک index دارد (0، 1، 2) و با کلیک روی آن selectedTabIndex = index می‌شود.  
- زیر Row، با یک when(selectedTabIndex) { 0 -> ... 1 -> ... 2 -> ... } رنگ و متن Box را تعیین می‌کنی.[7


در ادامه یک جزوه ساده و جامع در مورد Jetpack Compose با تمرکز بر state و remember و همراه با مثال‌ها و تمرین‌های کدشده (بدون TextField) آورده شده است. تمامی کدها و توضیحات خیلی ساده و قابل فهم نوشته شده‌اند.[5][10]

***


### چرا state و remember مهم هستند؟

- در Compose هر بار که UI باید آپدیت شود، تابع‌های @Composable دوباره اجرا می‌شوند.
- اگر از state و remember استفاده نکنیم، متغیرها هر بار به حالت اولیه بر می‌گردند و تغییرات کاربر از بین می‌رود.
- state: متغیری که اگر عوض شود، UI آپدیت می‌شود.
- remember: کمک می‌کند مقدار state بین رندرها حفظ شود.[10]

***

### مثال‌ها و کدها

#### مثال ۱: شمارنده با دکمه

```kotlin
@Composable
fun CounterExample() {
    var count by remember { mutableStateOf(0) }
    Button(onClick = { count++ }) {
        Text("Count: $count")
    }
}
```
- هر بار کلیک، مقدار count یک واحد افزایش می‌یابد.[10]

***

#### مثال ۲: تغییر رنگ با کلیک

```kotlin
@Composable
fun ColorToggleExample() {
    var isOn by remember { mutableStateOf(false) }
    Box(
        modifier = Modifier
            .fillMaxSize()
            .background(if (isOn) Color.Green else Color.Red)
            .clickable { isOn = !isOn },
        contentAlignment = Alignment.Center
    ) {
        Text(if (isOn) "ON" else "OFF")
    }
}
```
- با کلیک، رنگ بین سبز و قرمز تغییر می‌کند.[10]

***

#### مثال ۳: انتخاب آیتم

```kotlin
@Composable
fun ItemSelectorExample() {
    var selectedIndex by remember { mutableStateOf(0) }
    Column {
        repeat(3) { index ->
            Text(
                text = "Item $index",
                modifier = Modifier
                    .clickable { selectedIndex = index }
                    .background(if (index == selectedIndex) Color.Blue else Color.Gray)
            )
        }
    }
}
```
- با کلیک روی هر آیتم، رنگ آن آبی می‌شود.[10]

***

### تمرین‌ها و پاسخ‌ها

#### تمرین ۱: شمارنده منفی/مثبت

```kotlin
@Composable
fun CounterNegativePositive() {
    var count by remember { mutableStateOf(0) }
    Button(
        onClick = { count-- },
        colors = ButtonDefaults.buttonColors(
            backgroundColor = if (count < 0) Color.Red else Color.Green
        )
    ) {
        Text("Count: $count")
    }
}
```
- با هر کلیک، مقدار count کاهش می‌یابد و رنگ دکمه بسته به مقدار عوض می‌شود.[10]

***

#### تمرین ۲: سوییچ نور روز/شب

```kotlin
@Composable
fun DayNightToggle() {
    var isDay by remember { mutableStateOf(true) }
    Box(
        modifier = Modifier
            .fillMaxSize()
            .background(if (isDay) Color.White else Color.Black)
            .clickable { isDay = !isDay },
        contentAlignment = Alignment.Center
    ) {
        Text(if (isDay) "Day Mode" else "Night Mode")
    }
}
```
- با کلیک، حالت بین روز و شب تغییر می‌کند.[10]

***

#### تمرین ۳: شمارنده با حد بالا

```kotlin
@Composable
fun CounterWithLimit() {
    var count by remember { mutableStateOf(0) }
    Button(
        onClick = { if (count < 5) count++ },
        colors = ButtonDefaults.buttonColors(
            backgroundColor = if (count == 5) Color.Orange else Color.Blue
        )
    ) {
        Text("Count: $count")
    }
}
```
- شمارنده فقط تا 5 افزایش می‌یابد و وقتی به 5 می‌رسد، رنگ دکمه نارنجی می‌شود.[10]

***

#### تمرین ۴: انتخاب تب (Tab) ساده

```kotlin
@Composable
fun TabSelectorExample() {
    var selectedTabIndex by remember { mutableStateOf(0) }
    Column {
        Row {
            repeat(3) { index ->
                Text(
                    text = "Tab $index",
                    modifier = Modifier
                        .clickable { selectedTabIndex = index }
                        .background(if (index == selectedTabIndex) Color.Blue else Color.Gray)
                )
            }
        }
        Box(
            modifier = Modifier
                .fillMaxWidth()
                .height(100.dp)
                .background(
                    when (selectedTabIndex) {
                        0 -> Color.Blue
                        1 -> Color.Green
                        2 -> Color.Yellow
                        else -> Color.Gray
                    }
                ),
            contentAlignment = Alignment.Center
        ) {
            Text("Page ${selectedTabIndex + 1}")
        }
    }
}
```
- با کلیک روی هر تب، محتوای زیر عوض می‌شود و رنگ تب انتخاب‌شده آبی می‌شود.[10]
