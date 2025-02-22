# advanced_swing_duration

این کد یک اسکریپت نوشته شده با زبان Pine Script در نسخه 6 است که برای استفاده در پلتفرم **TradingView** طراحی شده است. هدف اصلی این اسکریپت، شبیه‌سازی و نمایش اطلاعات مربوط به محدوده‌های قیمتی، نقاط نوسانی (Swing Points)، و محاسبات زمانی در قالب یک جدول است. در ادامه توضیحات کد به زبان فارسی ارائه شده است:

---

### توضیح بخش‌های مختلف کد:

#### 1. **تعریف اندیکاتور**
```pinescript
indicator('Integrated Simulation with Table', overlay = true, max_bars_back = 499)
```
- این خط اندیکاتور را با نام **Integrated Simulation with Table** تعریف می‌کند.
- `overlay = true` به این معنی است که اندیکاتور روی نمودار قیمت نمایش داده می‌شود.
- `max_bars_back = 499` مشخص می‌کند که اندیکاتور می‌تواند تا 499 کندل گذشته را تجزیه و تحلیل کند.

---

#### 2. **تعریف آرایه‌ها**
```pinescript
var array<float> t_upper = array.new_float(10)
var array<float> t_lower = array.new_float(100)
var array<float> t_external = array.new_float(500)
```
- سه آرایه تعریف شده‌اند:
  - `t_upper`: برای ذخیره مقادیر محدوده داخلی بالایی (1 تا 10).
  - `t_lower`: برای ذخیره مقادیر محدوده داخلی پایینی (1 تا 100).
  - `t_external`: برای ذخیره مقادیر محدوده خارجی (100 تا 240 و 0 تا -260).

---

#### 3. **مقداردهی اولیه آرایه‌ها**
```pinescript
f_init_arrays() =>
    ...
if barstate.islast
    f_init_arrays()
```
- تابع `f_init_arrays` برای مقداردهی اولیه آرایه‌ها استفاده می‌شود:
  - مقادیر محدوده بالایی (`t_upper`) به صورت خطی از 1 تا 10 تنظیم می‌شوند.
  - مقادیر محدوده پایینی (`t_lower`) به صورت خطی از 1 تا 100 تنظیم می‌شوند.
  - مقادیر محدوده خارجی (`t_external`) در دو بخش مقداردهی می‌شوند:
    - از 100 تا 240.
    - از 0 تا -260.

این مقداردهی تنها در آخرین کندل (`barstate.islast`) انجام می‌شود.

---

#### 4. **ایجاد جدول برای نمایش اطلاعات**
```pinescript
var table resultsTable = table.new(position.bottom_right, 50, 50, border_color = color.white, bgcolor = color.new(color.black, 70))
```
- یک جدول با 50 سطر و 50 ستون در گوشه پایین سمت راست نمودار ایجاد می‌شود.
- رنگ پس‌زمینه جدول نیمه‌شفاف (70%) و مشکی است.

---

#### 5. **محاسبه نقاط نوسانی (Swing Points)**
```pinescript
if high > hh[1] and high > high[3]
    ...
if low < ll[1] and low < low[3]
    ...
```
- نقاط نوسانی بالا (`swingHigh`) و پایین (`swingLow`) شناسایی می‌شوند:
  - اگر قیمت بالاتر از بالاترین قیمت قبلی باشد، به‌عنوان یک نوسان بالا در نظر گرفته می‌شود.
  - اگر قیمت پایین‌تر از پایین‌ترین قیمت قبلی باشد، به‌عنوان یک نوسان پایین در نظر گرفته می‌شود.
- خطوط و برچسب‌هایی (Labels) برای نمایش این نقاط روی نمودار رسم می‌شوند.

---

#### 6. **محاسبه محدوده (Range) و نقطه تعادل (Equilibrium)**
```pinescript
RANGE_CALC = math.max(swingHigh, swingLow) - math.min(swingHigh, swingLow)
EQUILIBRIUM = (math.max(swingHigh, swingLow) + math.min(swingHigh, swingLow)) / 2
```
- `RANGE_CALC`: محدوده قیمتی (فاصله بین بالاترین و پایین‌ترین نقاط نوسانی).
- `EQUILIBRIUM`: نقطه تعادل محدوده (میانگین بالاترین و پایین‌ترین نقاط نوسانی).

---

#### 7. **نمایش اطلاعات در جدول**
```pinescript
table.cell(rangeTable, column = 0, row = 0, text = 'Range Value', ...)
table.cell(rangeTable, column = 1, row = 0, text = str.tostring(RANGE_CALC), ...)
```
-
