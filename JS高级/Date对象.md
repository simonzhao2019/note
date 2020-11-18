#### 1、Date.setHours()

将日期当中的时分秒，设置为你指定的数

```
dateObj.setHours(hoursValue[, minutesValue[, secondsValue[, msValue]]])
例如：
const event = new Date('August 19, 1975 23:15:30');
event.setHours(20);

console.log(event);
// expected output: Tue Aug 19 1975 20:15:30 GMT+0200 (CEST)  你传入的是20，时间变成了20
```

#### 2、new Date()

创建一个新Date对象，返回的是国际标准时间，不是时间戳。如果直接使用Date()将返回一个字符串，而非 `Date` 对象。

```
const date1 = new Date('December 17, 1995 03:24:00');
// Sun Dec 17 1995 03:24:00 GMT...
```



