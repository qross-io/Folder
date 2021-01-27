# Calendar 多功能日历组件

**Calendar** 可以实现一月、多月和整年日历功能，支持日期单选、周选和范围选择，可通过数据接口显示农历和特殊工作日、休息日，还可以与其他组件进行组合应用。日历组件也适配移动端。

## 下载

[点击下载 Calendar 组件及相关依赖文件](http://www.qross.cn/download?file=calendar.zip)

## 使用

Calendar 组件及其依赖库或组件列表如下：

* [root.js 基本库](/root.js/root.md)
* [Animation 动画库 root.animation.js](/root.js/animation.md)
* [DateTime 日期时间类 root.datetime.js](/root.js/datetime.md)
* Calendar 组件默认样式 `root.calendar.css`
* 需要两个`iconfont`图标，在`root.calendar.iconfont.css`中。
* 如果要显示农历，还需要自己做一个农历数据接口，农历数据文件为`calendar.csv`

引用代码示例如下，可放在页面的任何地方，建议放在 HEAD 标签中：

```html
<script type="text/javascript" src="/root.js"></script>
<script type="text/javascript" src="/root.animation.js"></script>
<script type="text/javascript" src="/root.datetime.js"></script>
<script type="text/javascript" src="/root.calendar.js"></script>
<link href="/root.calendar.css" rel="stylesheet" type="text/css" />
<link href="/root.calendar.iconfont.css" rel="stylesheet" type="text/css" />
```

如果需要显示农历和特殊工作日/休息日，还需要把日历数据放到数据库中，实现一个`GET`接口，核心语句如下：

```sql
SELECT concat(solar_year, '-', IF(solar_month < 10, '0', ''), solar_month, '-', IF(solar_day < 10, '0', ''), solar_day) AS solar, lunar_day, solar_term, festival, workday FROM qross_calendar WHERE solar_year IN (#{year}) -> TO NESTED MAP 'solar';
```

数据结构为：

```json
{
    "2020-09-20": {
        "lunar_day": "初四",
        "solar_term": "",
        "festival": "",
        "workday": -1
    },
    ...
}
```

特殊工作日和休息日的管理设置参见 [Master 管理平台](http://m.qross.cn)中的“系统” -> “Keeper 监控” -> “工作日历”。

## 示例

<script type="text/javascript" src="@/root.animation.js"></script>
<script type="text/javascript" src="@/root.datetime.js"></script>
<script type="text/javascript" src="@/root.calendar.js"></script>
<script type="text/javascript" src="@/root.clock.js"></script>
<script type="text/javascript" src="@/root.popup.js"></script>
<link href="@/root.popup.css" rel="stylesheet" type="text/css" />
<link href="@/root.calendar.css" rel="stylesheet" type="text/css" />
<link href="@/root.calendar.iconfont.css" rel="stylesheet" type="text/css" />
<link href="@/root.clock.css" rel="stylesheet" type="text/css" />
<style type="text/css">
input { font-size: 1rem; }
</style>

和其他的组件一件，日历组件也通过自定义 HTML 标签实现，一般不需要写 Javascript 代码。唯一可能写代码的地方只有 **事件**，以下配合示例进行说明：

### 单月单选模式

```html
<calendar id="calendar1"></calendar>
```

注意是 `<calendar></calendar>` 而不是 `<calendar />`，虽然标签之间没有任何东西。

<calendar id="calendar1"></calendar>

日历中各个部分的文字如星期的文字均可设置，见下面属性说明。

### INPUT 单选模式

```html
<input type="calendar" name="calendar2" week-names="一,二,三,四,五,六,日" />
```

注意`type`属性一定要设置为`calendar`。

<input type="calendar" name="calendar2" week-names="一,二,三,四,五,六,日" />

各个部分的样式如日期的字体大小均可自行设置，见下面的样式属性说明。

### 双月周选模式

```html
<calendar id="calendar3" mode="week" week-names="一,二,三,四,五,六,日" month-names="一月,二月,三月,四月,五月,六月,七月,八月,九月,十月,十一月,十二月" months="2"></calendar>
```

<calendar id="calendar3" mode="week" week-names="一,二,三,四,五,六,日" month-names="一月,二月,三月,四月,五月,六月,七月,八月,九月,十月,十一月,十二月" this-week-text="本周" months="2"></calendar>

可通过日历属性获取已选中周的各项值，如第几周、开始日期、结束日期等。

### INPUT 周选模式

```html
<input type="calendar" name="calendar4" size="30" mode="week" />
```

<input type="calendar" id="calendar4" size="25" mode="week" months="2" /> &nbsp; &nbsp; 选中的周：<code id="SelectedWeek"></code> &nbsp; 开始日期：<code id="WeekStartDate"></code> &nbsp; 结束日期：<code id="WeekEndDate"></code>

<script type="text/javascript">
$listen('calendar4').on('WeekSelected', function(year, weekOfYear, startDate, endDate) {
    let input = document.querySelector('#calendar4');
    $x('#SelectedWeek').html(this.week);
    $x('#WeekStartDate').html(startDate);
    $x('#WeekEndDate').html(endDate);
});
</script>

可以通过事件参数或Calendar选择器获取选中的周、开始日期和结束日期。如

```javascript
$listen('calendar4').on('WeekSelected', function(year, weekOfYear, startDate, endDate) {
    let input = document.querySelector('#calendar4');
    $x('#SelectedWeek').html(this.week); //返回选中的年和周，格式 yyyy-ww
    $x('#WeekStartDate').html(input.getAttribute('start')); //返回选中周的开始日期，格式 yyyy-MM-dd
    $x('#WeekEndDate').html(endDate); //返回选中周的结束日期，格式 yyyy-MM-dd
    //以上三种方式都可以得到选中的值，详见下面的属性说明
});
```

### 三月多选模式

```html
<calendar id="calendar5" mode="range" days-of-other-month="hidden" months="3"></calendar>
```

<calendar id="calendar5" mode="range" days-of-other-month="hidden" months="3"></calendar>

### INPUT 多选模式

```html
<input type="calendar" name="calendar6" size="40" mode="range" months="2" days-of-other-month="hidden" init="0" />
```

<input type="calendar" name="calendar6" size="40" mode="range" months="2" days-of-other-month="hidden" init="0" />

可以通过 Calendar 的`startDate`和`endDate`属性获取选中的开始日期和结束日期。如

```javascript
$calendar('calendar6').startDate; //返回选中的开始日期，格式`yyyy-MM-dd`
$calendar('calendar6').endDate; //返回选中的结束日期，格式`yyyy-MM-dd`
```

### 整年模式

```html
<calendar id="calendar7" mode="none" lunar="yes" holiday="yes" title-format="yyyy年"
 days-of-other-month="hidden" week-names="一,二,三,四,五,六,日" this-year-text="今年"
  month-names="一月,二月,三月,四月,五月,六月,七月,八月,九月,十月,十一月,十二月" corner-names="休,班" 
  extension-api="/api/system/calendar?year=" months="12"></calendar>
```
<br/>
<div>
<calendar id="calendar7" mode="none" lunar="yes" holiday="yes" title-format="yyyy年"
 days-of-other-month="hidden" week-names="一,二,三,四,五,六,日" this-year-text="今年"
  month-names="一月,二月,三月,四月,五月,六月,七月,八月,九月,十月,十一月,十二月" corner-names="休,班" 
  extension-api="/api/system/calendar?year=" months="12"></calendar>
</div>

### 与其他组件组合

可以和 [Popup 组件](/root.js/popup.md)和 [Clock 组件](/root.js/clock.md)一起合成一个日期时间选择控件。

<input id="DateTime" type="text" size="30" placeholder="yyyy-MM-dd HH:mm:00" /><a id="DateTimePicker_OpenButton" href="javascript:void(0)" style="margin-left: -24px"><i class="iconfont icon-calendar"></i></a>

<div id="DateTimePicker" popup="yes" class="popup-autosize" display="sidebar" position="right">
    <div id="DateTimePicker_CloseButton" class="popup-close-button"><i class="iconfont icon-quxiao"></i></div>
    <div class="popup-bar"><i class="iconfont icon-calendar"></i> &nbsp; <span id="DateTimePickerTitle"></span></div>
    <div class="popup-title">请分别选择日期和时间</div>
    <calendar id="Calendar" lunar="yes" corner-names="休,班" week-title="周" week-names="一,二,三,四,五,六,日" month-names="一月,二月,三月,四月,五月,六月,七月,八月,九月,十月,十一月,十二月" this-month-text="本月" today-text="今天" head-format="yyyy年M月" holiday="yes" extension-api="/api/system/calendar?year=" date="today"></calendar>
    <div class="space10"></div>
    <clock id="Clock" hour-interval="1" minute-interval="1" second-interval="0" option-frame-side="upside" value="HH:mm:00"></clock>
    <div id="DateTimePickerTip" class="space40 error" style="display: flex; justify-content: center; align-items: center;">&nbsp;</div>
    <div class="popup-button"><input id="DateTimePicker_ConfirmButton" type="button" value=" OK " class="normal-button prime w80" /> &nbsp; &nbsp; &nbsp; <input id="DateTimePicker_CancelButton" type="button" value=" Cancel " class="normal-button minor w80" /></div>
</div>

<script type="text/javascript">
$listen('DateTimePicker').on('confirm', function() {
    $x('#DateTime').value($calendar('Calendar').date + ' ' + $clock('Clock').time);
});
</script>

代码如下：

```html
<script type="text/javascript" src="/root.clock.js"></script>
<script type="text/javascript" src="/root.popup.js"></script>
<link href="/root.clock.css" rel="stylesheet" type="text/css" />
<link href="/root.popup.css" rel="stylesheet" type="text/css" />

<input id="DateTime" type="text" size="30" placeholder="yyyy-MM-dd HH:mm:00" /><a id="DateTimePicker_OpenButton" href="javascript:void(0)" style="margin-left: -24px"><i class="iconfont icon-calendar"></i></a>

<div id="DateTimePicker" popup="yes" class="popup-autosize" display="sidebar" position="right">
    <div id="DateTimePicker_CloseButton" class="popup-close-button"><i class="iconfont icon-quxiao"></i></div>
    <div class="popup-bar"><i class="iconfont icon-calendar"></i> &nbsp; <span id="DateTimePickerTitle"></span></div>
    <div class="popup-title">请分别选择日期和时间</div>
    <calendar id="Calendar" lunar="yes" corner-names="休,班" week-title="周" week-names="一,二,三,四,五,六,日" month-names="一月,二月,三月,四月,五月,六月,七月,八月,九月,十月,十一月,十二月" this-month-text="本月" today-text="今天" head-format="yyyy年M月" holiday="yes" extension-api="/api/system/calendar?year=" date="today"></calendar>
    <div class="space10"></div>
    <clock id="Clock" hour-interval="1" minute-interval="1" second-interval="0" option-frame-side="upside" value="HH:mm:00"></clock>
    <div id="DateTimePickerTip" class="space40 error" style="display: flex; justify-content: center; align-items: center;">&nbsp;</div>
    <div class="popup-button"><input id="DateTimePicker_ConfirmButton" type="button" value=" OK " class="normal-button prime w80" /> &nbsp; &nbsp; &nbsp; <input id="DateTimePicker_CancelButton" type="button" value=" Cancel " class="normal-button minor w80" /></div>
</div>

<script type="text/javascript">
$listen('DateTimePicker').on('confirm', function() {
    $x('#DateTime').value($calendar('Calendar').date + ' ' + $clock('Clock').time);
});
</script>
```

## 标签和类

### 标签属性

标签属性可以在自定义标签`<calendar>`上进行设置。

#### 基本设置

* `id`或`name` Calendar在页面上的唯一名称。
* `mode` Calendar日期的选择模式，可选值`day`、`week`、`range`、`none`，分别代表单选、周选、日期范围选择和仅事件模式，属性值不区分大小写。
* `months` 设置显示几个月的日历，默认值为`1`，如果设置为`12`，则显示整年日历。
* `init` 在多月模式下，初始月份的偏移值。例如在显示`3`个月的日历时，值为`-1`时默认显示上月、本月和下月，值为`0`显示本月、下月和下下月，值为`-2`显示上上月、上月和本月。
* `first-day-of-week` 每周的第一天是周几，默认周一，可选任意表示星期值，如`MON`、`SUNDAY`、`星期二`、`周三`、`日`、`5`等，如果是数字的话，`0`表示周日。
* `days-of-other-month` 是否在日历中显示其他月份的日期，可选值`visible`或`hidden`，默认为`visible`。每月日历都是`6`行`7`列，除当月的日期外，还会有部分上月和下月的日期，这个属性设置其他月的日期是否显示。

* `quick-switch` 是否启用快速切换，月历时点击月标题可快速切换年和月，年历时点击年标题可以快速切换年，默认为`true`。当可总年数小于`4`时或月数小于`months+3`时，这个属性会自动关闭。
* `lunar` 是否显示农历，默认`false`，需要扩展接口支持。作者没有使用农历算法进行转换，那个代码量要远远超过这个组件的代码本身。
* `holiday` 是否显示特殊工作日和特殊休息日（如法定节假日），默认`false`，需要扩展接口支持。
* `extension-api` 扩展接口地址，仅支持`GET`方式获取。接口数据已在压缩包中。

* `icon` 在`INPUT`模式下，会在输入框右侧显示一个图标，这个属性用来自定义图片地址或`iconfont`类名，如`/images/calendar.png`或`icon-calendar`。
* `align` 在`INPUT`模式下，日历弹出框的对齐方式，可选`LEFT`或`RIGHT`，即与输入框左侧对齐还是右侧对齐。没有居中对齐选项，真的不好看。

#### 初始值

* `min-date` 设置允许选择的最小日期，格式`yyyy-MM-dd`，支持`today`关键字，默认值为`1900-01-01`。
* `max-date` 设置允许选择的最大日期，格式`yyyy-MM-dd`，支持`today`关键字，默认值为`2100-12-31`。
* `date` 在单选模式下，默认选中哪一天，格式`yyyy-MM-dd`，支持`today`关键字。
* `week` 在周选模式下，默认选中哪一周，格式`yyyy-ww`，`ww`代表周数，支持`this-week`关键词。
* `start-date` 在范围选择模式下，默认选中的开始日期，格式`yyyy-MM-dd`，支持`today`关键字。
* `end-date` 在范围选择模式下，默认选中的结束日期，格式`yyyy-MM-dd`，支持`today`关键字。

#### 格式属性

* `title-format` 整年模式下，设置年份标题的格式，如`yyyy年`，默认为空，即不显示。
* `head-format` 单月或多月模式下，月历头的格式，默认`yyyy-MM`，如可以设置为`yyyy年M月`。
* `week-title` 周选模式下，会有一列显示第几周，这个属性用来设置这一列的标题，默认值为`Week`，如可以设置为`周`。
* `week-names` 星期的表头名称，默认值为`Mon,Tue,Wed,Thu,Fri,Sat,Sun`，如可以设置为`一、二、三、四、五、六、日`。
* `month-names` 年历模式下，每个月的月份名称，默认值为`January,February,March,April,May,June,July,August,September,October,November,December`，如可以设置为`一月,二月,三月,四月,五月,六月,七月,八月,九月,十月,十一月,十二月`。
* `corner-names` 特殊工作日和特殊休息日（如法定节假日）的名称，默认值为`W,R`，如可以设置为`班,假`。

#### 样式属性

样式属性一般不需要设置，可直接修改calendar.css中对应的样式即可。考虑到可能存在某些极端的情况，Calendar提供了这些可设置样式的属性。或者要修改样式时可以根据说明去找。

* `frame-class` 整体日历框架的样式，默认值`-calendar`
* `title-class` 整个日历的标题框`DIV`样式，一般用于年历模式， 默认值`-calendar-title`
* `title-text-class` （年）标题文字样式， 默认值`-calendar-title-text`
* `head-class` 月历头样式，包含导航按钮和日历标题(年月)， 默认值`-calendar-head`
* `nav-class` 导航`TABLE` 框体样式，默认值`-calendar-nav`
* `nav-month-button-class` 月导航按钮（上一月/下一月）样式，默认值`-calendar-nav-button`
* `nav-year-button-class` 年导航按钮（上一年/下一年）样式，默认值`-calendar-nav-year-button`
* `head-title-class` 日历标题（年和月）样式，默认值`-calendar-head-title`
* `body-class` 日历主体`DIV`样式，默认值`-calendar-body`
* `content-class` 日历内容`TABLE`样式'，默认值`-calendar-content`
* `corner-class` 日历日期单元格的角标`DIV`样式（即特殊工作日和休息日），默认值`-calendar-corner`
* `day-class` 日期数字`DIV`样式，默认值`-calendar-day`
* `lunar-class` 农历文字样式，默认值`-calendar-lunar`
* `today-class` “今天”单元格`TD`样式，默认值`-calendar-today`
* `holiday-class` 休息日单元格`TD`样式，默认值`-calendar-holiday`
* `workdayClass` 工作日单元格`TD`样式，默认值`-calendar-workday`
* `focus-day-Class` 选中日期单元格`TD`样式，默认值`-calendar-focus-day`
* `disabled-day-class` 可选择日期区间外的日期样式，默认值`-calendar-disabled-day`
* `days-of-other-month-class` 月历中其他月的日期`TD`样式 `-calendar-days-of-other-month`
* `focus-start-day-class` 范围选择模式下选中的开始日期样式，默认值`-calendar-focus-start-day`
* `focus-end-day-class`范围选择模式下选中的结束日期样式，默认值也是'-calendar-focus-start-day'
* `selection-day-class`范围选择模式下选中的开始日期和结束日期之间的日期样式`-calendar-selection-day`
* `week-head-class` 周序号的表头`TH`样式，默认值`-calendar-week-head`
* `week-name-class` 周名称`TH`样式，默认值`-calendar-week-name`
* `week-row-class` 周选模式下默认的行`TR`样式，默认值`-calendar-week-row`
* `week-focus-row-class` 周选模式下选中行`TR`样式，默认值`-calendar-week-focus-row`
* `week-of-year-class` 周选模式下周数字`TD`样式，默认值`-calendar-week-of-year`
* `week-of-year-focus-class` 周选模式下选中状态下的周数字`TD`样式，默认值`-calendar-week-of-year-focus`
* `foot-class` 月历底部的`DIV`样式，默认值`-calendar-foot`
* `foot-button-class` 月历底部按钮`BUTTON`样式，如“今天”、“本月”等，默认值`-calendar-foot-button`
* `switch-frame-class` 快速切换年或月框体`DIV`样式，默认值`-calendar-switch-frame`
* `switch-year-option-class` 年快速切换框体中的年样式，默认值`-calendar-switch-year-option`
* `switch-this-year-option-class` 年快速切换框体中的今年样式，默认值`-calendar-switch-this-year-option`
* `switch-year-checked-option-class` 年快速切换框体中的选中年样式，默认值`-calendar-switch-year-checked-option`
* `switch-year-head-class` 月快速切换框体中的年份样式，默认值`-calendar-switch-year-head`
* `switch-month-option-class` 月快速切换框体中的月份选项样式，默认值`-calendar-switch-month-option`
* `switch-this-month-option-class` 月快速切换框体中的本月项样式，默认值`-calendar-switch-this-month-option`
* `switch-month-checked-option-class` 月快速切换框体中的选中月样式，默认值`-calendar-switch-month-checked-option`

样式属性的名称建议使用中横线连字符分隔的格式，不过去掉连字符也没什么问题。在使用Javascript操作这个Calendar对象时，可以通过`camel`格式属性进行调用 ，如`focus-day-class`对应属性`focusDayClass`，但是组件没有提供属性设置功能，如`calendar.focusDayClass = 'other-class-name'`是无效的。

### 选择器

因为`<calendar>`属于自定义标签，不能通过原生的选择器如`document.querySelector`来操作这个标签。Calendar对象提供了两种格式的选择器：

* `$calendar('name')` 这个选择器返回`Calendar`对象本身，然后可以访问`Caldendar`对象的属性、方法和事件。因为页面的加载顺序是`页面加载完成` > `解析Calendar标签` > `通过$calendar选择`，所以`$calendar`选择器要求在页面加载完成并且标签解析之后才能调用 ，一般放在`$finish`函数里面。如
    ```javascript
    $finish(function() {
        $calendar('name').date = '2020-09-21';
    });
    ```
* `$listen('name')` 这个选择器主要用于为 Calendar 对象设置事件。
    ```javascript
    $listen('calendar1').on('DaySelected', function(day) { ... });
    ```
    当然也可以通过上一个选择器添加事件。
    ```javascript
    $finish(function() {
        $calendar('calendar1').on('DaySelected', function(day) { ... });
        $calendar('calendar1').onDaySelected = function(day) { ... };
    });
    ```
    二者的区别是`$listen`无需要考虑页面是否加载完成，而`$calendar`必须在标签解析之后才能调用。

### 属性

在标签属性中已经介绍了很多属性，这些标签属性除了可以进行预设置外，都可以通过 Calendar 对象进行调用，不过标签属性在 Calenar 对象中大部分都是只读的。另外还有一些不是标签属性的 Calendar 对象属性。

几个重要的属性有：

* `date` 单选模式下获取或设置日历的选中日期，格式`yyyy-MM-dd`，可写
* `startDate` 范围选择模式下获取或设置选中的开始日期，格式`yyyy-MM-dd`，可写
* `endDate` 范围选择模式下获取或设置选中的结束日期，格式`yyyy-MM-dd`，可写
* `week` 周选模式下获取或设置日历的选中周，格式`yyyy-ww`，可写。注意在周选模式下，`startDate`和`endDate`可以获取值，表示选中周的开始日期和结束日期。
* `year` 当年日历中第一个月的年份，可写
* `month` 当前日历中第一个月的月份，可写
* `minYear` 最小可选择的年份，默认为`1900`，只读
* `maxYear` 最大可选择的年份，默认为`2100`，只读
* `minMonth` 最小可选择的月份，默认为`1900-01`，只读
* `maxMonth` 最大可选择的月份，默认为`2100-12`，只读

其他只读的需要在标签中进行设置且只读的属性有：

* `name`或`id` 获取日历在页面上的唯一名称
* `mode` 获取日历的选择模式，返回值有`DAY`、`WEEK`、`RANGE`和`NONE`
* `months` 获取日历同时可以显示几个月
* `firstDayOfWeek` 获取日历的第一天是周几
* `daysOfOtherMonth` 获取日历中其他月份的日期是否在显示，返回值有`VISIBLE`和`HIDDEN`
* `minDate` 获取可选择的最小日期，格式`yyyy-MM-dd`，默认值为`1900-01-01`
* `maxDate` 获取可选择的最大日期，格式`yyyy-MM-dd`，默认值为`2100-12-31`
* `weekTitle` 周选模式下周顺序号那一列的表头名称，默认值`Week`
* `weekNames` 周名称列表，默认值`['Mon', 'Tue', 'Wed', 'Thu', 'Fri', 'Sat', 'Sun']`
* `monthNames` 月名称列表，年历的快速切换中使用，默认值`['January', 'February', 'March', 'April', 'May', 'June', 'July', 'August', 'September', 'October', 'November', 'December'],`
* `cornerNames` 工作日和休息日角标名称，默认值为`['R', 'W']`
* `todayText` “今天”的文字，默认为`Today`
* `thisWeekText` “本周”的文字，默认为`This Week`
* `thisMonthText` “本月”的文字，默认为`This Month`
* `thisYearText` “今年”的文字，默认为`This Year`
* `quickSwitch` 是否在显示快速切换，默认值为`true`
* `lunar` 是否在显示显示农历，默认值为`false`
* `holiday` 是否在显示特殊工作日和节假日角标，默认值为`false`
* `extensionApi` 扩展Api，用以读取农历和特殊工作日/休息日

另有所有的样式属性也都是只读的，不再一一列出。

### 方法

在Calendar对象中，私有方法居多，可用的方法比较少。

* `getCell(date)` 根据日期（格式`yyyy-MM-dd`）获取日历单元格，返回CalendarCell对象，见下面的说明。注意一定是可见的单元格。
* `yearSwitchEnabled()` 年模式下年快速切换是否可用，可用年数小于`4`时自动关闭。
* `monthSwitchEnabled()` 单月或多月模式下月快速切换是否可用，可用月数小于`months+3`时自动关闭，`months`为当前日历中可显示的月数。

### 事件

事件可以捕获用户对Calendar的操作，基本上是除了`INPUT`模式外必须有的编程工作。

* `onNavPrevMonth` 当月历导航到上一个月时触发，可传递参数`prevMonth`，参数格式为`yyyy-MM`。
* `onNavNextMonth` 当月历导航到下一个月时触发，可传递参数`nextMonth`，参数格式为`yyyy-MM`。
* `onNavPrevYear` 当年历导航到上一年时触发，可传递参数`prevYear`，格式为年份数字。
* `onNavNextYear` 当年历导航到下一年时触发，可传递参数`nextYear`，格式为年份数字。
* `onDaySelected` 单选模式下选中某一天时触发，可传递参数`selectedDay`，格式为`yyyy-MM-dd`。
* `onDayClick` 非选择模式下点击某一天时触发，可传递参数`day`，参数类型为`CalendarCell`。
* `onWeekSelected` 周选模式下选中某一周时触发，可传递`4`个参数：`year`, `weekOfYear`, `startDate`, `endDate`，其中`year`和`weekOfYear`是整数，`startDate`和`endDate`格式为`yyyy-MM-dd`，分别表示选中周的开始日期和结束日期。
* `onRangeStartSelected` 范围选择模式下，选中开始日期时触发，传递两个参数`startDate`、`endDate`。当没有结束时间选中时`endDate`为空。
* `onRangeStartCanceled` 范围选择模式下，取消选中开始日期时触发，传递参数`endDate`，当没有结束日期选中时`endDate`为空。
* `onRangeEndSelected` 范围选择模式下，选中结束日期时触发，传递两个参数`startDate`、`endDate`。
* `onRangeEndCanceled` 范围选择模式下，取消选中结束日期时触发，传递一个参数`startDate`
* `onRangeSelected` 范围选择模式下，选择了开始日期并选中了结束日期时触发，传递两个参数`startDate`、`endDate`
* `onRangeCanceled` 范围选择模式下，取消选择开始日期和结束日期时触发，无参数传递

事件使用示例：

```javascript
$listen('calendar1').on('WeekSelected', function(year, weekOfYear, startDate, endDate) {
    console.log(this.week);
});
```

在事件函数中，`this`关键字指向当前 Calendar 对象

### CalendarCell 类

**CalendarCell** 类主要用于操作每个日期项，比如设置角标、设置样式等，这个类的成员主要是属性。上面介绍过，通过 Calendar 对象的`getCell(date)`方法获取某个日期单元格，然后可以访问或设置这个单元格的各种属性。

* `date` 获取当前单元格的日期时间，DateTime类型，只读
* `day` 获取或设置当前单元格的日期数字，可写
* `corner` 获取或设置当前单元格的角标，可写
* `lunar` 获取或设置当前单元格的农历，可写
* `className` 获取或设置当前单元格的样式，可写
* `dayClass` 获取或设置当前单元格日期数字的样式，可写
* `cornerClass` 获取或设置当前单元格的角标样式，可写
* `lunarClass` 获取或设置当前单元格的农历样式，可写

通过这个类可以为日历添加更多的功能。

---
参考链接

* [root.js 基础库](/root.js/root.md)
* [Animation 动画库](/root.js/root.md)
* [DateTime 日期时间类](/root.js/datetime.md)
* [Clock 文字时钟组件](/root.js/clock.md)
* [Popup 弹出对话框组件](/root.js/popup.md)