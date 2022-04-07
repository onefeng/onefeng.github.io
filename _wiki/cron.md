---
layout: wiki
title: cron表达式
categories: cron
description: cron解析
keywords: cron相关说明
---

## cron概念

cron主要用于执行定时任务的表达式

## cron解析

## 主要构成
cron主要由5-7位构成
1. 5位从左到右依次是，分钟，小时，天，月，周
2. 6位从左到右依次是，秒，分钟，小时，天，月，周
3. 7位从左到右依次是，秒，分钟，小时，天，月，周，年


## 每个域允许的值

|域|有效值|有效字符|说明|
|----|----|----|----|
|秒|0-59|,-*/|-|
|分|0-59|,-*/|-|
|小时|0-23|,-*/|-|
|日期|1-31|,- * ? / L W C|-|
|日期|1-12|,JAN-DEC - * /|-|
|星期|1-7|,SUN-SAT - * ? / L C #|-|
|年|1970-2099|, - * /|-|

## 特殊字符的含义

|字符|含义|说明|
|----|----|----|
|*|表示匹配域的任意值|在分这个域使用 *，即表示每分钟都会触发事件。|
|/|表示起始时间开始触发，然后每隔固定时间触发一次。|例如：在分这个域使用 5/20，则意味着 5 分钟触发一次，而 25，45 等分别触发一次。|
|?|表示匹配域的任意值|只能用在日期和星期两个域，因为这两个域会相互影响。要在每月的 20 号触发调度，不管每个月的 20 号是星期几，则只能使用如下写法：13 13 15 20 * ?。其中，因为日期域已经指定了 20 号，最后一位星期域只能用 ?，不能使用 *。如果最后一位使用 *，则表示不管星期几都会触发，与日期域的 20 号相斥，此时表达式不正确。|
|-|表示匹配域的起止范围|在分这个域使用 5-20，表示从 5 分到 20 分钟每分钟触发一次。|
|,|列出枚举值|在分这个域使用 5,20，则意味着在 5 和 20 分每分钟触发一次。|
|L|表示最后，只能出现在日和星期两个域|在星期这个域使用 5L，意味着在最后的一个星期四触发。|
|W|表示有效工作日（周一到周五），只能出现在日这个域，系统将在离指定日期最近的有效工作日触发事件。|在日这个域使用 5W，如果 5 号是星期六，则将在最近的工作日星期五，即 4 号触发。如果 5 号是星期天，则在 6 号（周一）触发；如果 5 号为工作日，则就在 5 号触发。另外，W 的最近寻找不会跨过月份。|
|LW|这两个字符可以连用，表示在某个月最后一个工作日，即最后一个星期五。|-|
|#|表示每个月第几个星期几，只能出现在星期这个域|在星期这个域使用 4#2，表示某月的第二个星期三，4 表示星期三，2 表示第二个|

## 实例
- */5 * * * * ?：每隔 5 秒执行一次
- 0 */1 * * * ?：每隔 1 分钟执行一次
- 0 0 2 1 * ? *：每月 1 日的凌晨 2 点执行一次
- 0 15 10 ? * MON-FRI：周一到周五每天上午 10：15 执行作业
- 0 15 10 ? 6L 2002-2006：2002 年至 2006 年的每个月的最后一个星期五上午 10:15 执行作业
- 0 0 23 * * ?：每天 23 点执行一次
- 0 0 1 * * ?：每天凌晨 1 点执行一次
- 0 0 1 1 * ?：每月 1 日凌晨 1 点执行一次
- 0 0 23 L * ?：每月最后一天 23 点执行一次
- 0 0 1 ? * L：每周星期天凌晨 1 点执行一次
- 0 26,29,33 * * * ?：在 26 分、29 分、33 分执行一次
- 0 0 0,13,18,21 * * ?：每天的 0 点、13 点、18 点、21 点都执行一次
- 0 0 10,14,16 * * ?：每天上午 10 点，下午 2 点，4 点执行一次
- 0 0/30 9-17 * * ?：朝九晚五工作时间内每半小时执行一次
- 0 0 12 ? * WED：每个星期三中午 12 点执行一次
- 0 0 12 * * ?：每天中午 12 点触发
- 0 15 10 ? * *：每天上午 10:15 触发
- 0 15 10 * * ?：每天上午 10:15 触发
- 0 15 10 * * ? *：每天上午 10:15 触发
- 0 15 10 * * ? 2005：2005 年的每天上午 10:15 触发
- 0 * 14 * * ?：每天下午 2 点到 2:59 期间的每 1 分钟触发
- 0 0/5 14 * * ?：每天下午 2 点到 2:55 期间的每 5 分钟触发
- 0 0/5 14,18 * * ?：每天下午 2 点到 2:55 期间和下午 6 点到 6:55 期间的每 5 分钟触发
- 0 0-5 14 * * ?：每天下午 2 点到 2:05 期间的每 1 分钟触发
- 0 10,44 14 ? 3 WED：每年三月的星期三的下午 2:10 和 2:44 触发
- 0 15 10 ? * MON-FRI：周一至周五的上午 10:15 触发
- 0 15 10 15 * ?：每月 15 日上午 10:15 触发
- 0 15 10 L * ?：每月最后一日的上午 10:15 触发
- 0 15 10 ? * 6L：每月的最后一个星期五上午 10:15 触发
- 0 15 10 ? * 6L 2002-2005：2002 年至 2005 年的每月的最后一个星期五上午 10:15 触发
- 0 15 10 ? * 6#3：每月的第三个星期五上午 10:15 触发