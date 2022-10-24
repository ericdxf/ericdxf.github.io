- 为什么需要LocalDate、LocalTime、LocalDateTime【java8新提供的类】
- java8新的时间API的使用方式，包括创建、格式化、解析、计算、修改

### 为什么需要LocalDate、LocalTime、LocalDateTime

Date如果不格式化，打印出的日期可读性差

```java
Tue Sep 10 09:34:04 CST 2019
```

SimpleDateFormat中的calendar对象是共享变量，并且这个共享变量没有做线程安全控制。当多个线程同时使用相同的SimpleDateFormat对象容易导致线程不安全问题。

### 一起使用java8全新的日期和时间API

#####  创建LocalDate 只获取某年某月

```java
//获取当前年月日
LocalDate localDate = LocalDate.now();
System.out.println("当前的年月日:"+localDate);
//构造指定的年月日
LocalDate localDate1 = LocalDate.of(2019, 9, 10);
System.out.println("指定的年月日:"+localDate1);
```

当前的年月日:2020-01-13
指定的年月日:2019-09-10

##### 获取年、月、日、星期几

```java
LocalDate localDate = LocalDate.now();
int year = localDate.getYear();
int year1 = localDate.get(ChronoField.YEAR);
System.out.println("当前年:"+year);
System.out.println("当前年:"+year1);
Month month = localDate.getMonth();
int month1 = localDate.get(ChronoField.MONTH_OF_YEAR);
System.out.println("当前月:"+month);
System.out.println("当前月:"+month1);
int day = localDate.getDayOfMonth();
int day1 = localDate.get(ChronoField.DAY_OF_MONTH);
System.out.println("当前天:"+day);
System.out.println("当前天:"+day1);
DayOfWeek dayOfWeek = localDate.getDayOfWeek();
int dayOfWeek1 = localDate.get(ChronoField.DAY_OF_WEEK);
System.out.println("当前星期:"+dayOfWeek);
System.out.println("当前星期:"+dayOfWeek1);
```

当前年:2020
当前年:2020
当前月:JANUARY
当前月:1
当前天:13
当前天:13
当前星期:MONDAY
当前星期:1

##### 获取时分秒

```java
//创建LocalTime
LocalTime localTime = LocalTime.of(13, 51, 10);
LocalTime localTime1 = LocalTime.now();
System.out.println("当前时间："+localTime);
System.out.println("当前时间："+localTime1);

//获取小时
int hour = localTime.getHour();
int hour1 = localTime.get(ChronoField.HOUR_OF_DAY);
System.out.println("当前小时："+hour);
System.out.println("当前小时："+hour1);
//获取分
int minute = localTime.getMinute();
int minute1 = localTime.get(ChronoField.MINUTE_OF_HOUR);
System.out.println("当前分钟："+minute);
System.out.println("当前分钟："+minute1);
//获取秒
int second = localTime.getMinute();
int second1 = localTime.get(ChronoField.SECOND_OF_MINUTE);
System.out.println("当前秒："+second);
System.out.println("当前秒："+second1);
```

当前时间：13:51:10
当前时间：10:08:13.687
当前小时：13
当前小时：13
当前分钟：51
当前分钟：51
当前秒：51
当前秒：10

##### LocalDateTime

- 创建LocalDateTime

```java
LocalDateTime localDateTime = LocalDateTime.now();
LocalDateTime localDateTime1 = LocalDateTime.of(2019, Month.SEPTEMBER, 10, 14, 46, 56);
LocalDateTime localDateTime2 = LocalDateTime.of(localDate, localTime);
LocalDateTime localDateTime3 = localDate.atTime(localTime);
LocalDateTime localDateTime4 = localTime.atDate(localDate);
```

- 还有一些对时间的操作api 相当于之前的旧API简单了很多，和SimpleDateFormat相比，DateTimeFormatter是线程安全的

```java
// 格式化LocalDateTime对象
LocalDateTime localDateTime = LocalDateTime.now();
DateTimeFormatter dateTimeFormatter = DateTimeFormatter.ofPattern("yyyy-MM-dd HH:mm:ss");
System.out.println("当前时间：" + dateTimeFormatter.format(localDateTime));

// 字符串转LocalDateTime对象
String dateTime = "2022-10-24 13:37:00";
LocalDateTime localDateTime1 = LocalDateTime.parse(dateTime, dateTimeFormatter);
```

- LocalDateTime类也提供了丰富的加减计算方法：

```java
// 加减计算
LocalDateTime localDateTime = LocalDateTime.now();
LocalDateTime l1 = localDateTime.plus(10, ChronoUnit.HOURS);
LocalDateTime l2 = localDateTime.plusYears(1);
LocalDateTime l3 = localDateTime.plusMonths(10);
LocalDateTime l4 = localDateTime.plusHours(3);
LocalDateTime l5 = localDateTime.minusHours(2);
```

- 获取时间戳和大小比较

```java
// 秒
System.out.println(LocalDateTime.now().toEpochSecond(ZoneOffset.of("+8"))); //1659344769  10位
// 毫秒
System.out.println(LocalDateTime.now().toInstant(ZoneOffset.of("+8")).toEpochMilli()); //1659344769023 13位

// 大小比较
LocalDateTime local = LocalDateTime.now();
LocalDateTime insert = LocalDateTime.parse("2022-01-12 12:25:45", DateTimeFormatter.ofPattern("yyyy-MM-dd HH:mm:ss"));

System.out.println(local.isBefore(insert)); false
System.out.println(local.isAfter(insert));  true
System.out.println(local.isEqual(insert));   false
```

