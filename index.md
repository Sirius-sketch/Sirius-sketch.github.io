---
layout: default
title: 首页
---

<div id="lunar-countdown" style="padding: 20px; background-color: #f9f9f9; border-radius: 10px; text-align: center; margin: 20px 0; font-family: sans-serif;">
  <h3 style="margin: 0; color: #333;">🎂 咪咪生日倒计时</h3>
  <p style="font-size: 1.2em; color: #666; margin-bottom: 5px;">
    离亲爱的咪咪下一次生日还有：
    <span id="day-count" style="font-size: 2em; color: #d9534f; font-weight: bold;">--</span> 天
  </p>
  
  <p style="color: #1E90FF; font-size: 1.1em; font-weight: bold; margin-top: 10px;">
    永远年轻的好妈妈，天天开心！Happy forever！🎉
  </p>
</div>

<script src="https://cdn.jsdelivr.net/npm/lunar-javascript/lunar.js"></script>

<script>
  (function() {
    // ================= 配置区域 =================
    // 修改为：农历十月二十三
    var myLunarMonth = 10;  // 农历月份
    var myLunarDay = 23;    // 农历日期
    // ===========================================

    function getDaysUntilBirthday() {
      var now = new Date();
      // 获取当前农历年份
      var solar = Solar.fromDate(now);
      var currentLunarYear = solar.getLunar().getYear();
      
      // 1. 获取“今年”该农历生日对应的阳历日期
      var birthdayLunar = Lunar.fromYmd(currentLunarYear, myLunarMonth, myLunarDay);
      var birthdaySolar = birthdayLunar.getSolar();
      var birthdayDate = new Date(birthdaySolar.getYear(), birthdaySolar.getMonth() - 1, birthdaySolar.getDay());

      // 为了防止“今天就是生日”时被误判为明年，我们将比较时间设为当天的23:59:59之后
      // 或者简单地比较：如果 当前时间 > 今年的生日日期（已过），则算明年
      // 注意：这里需要把 birthdayDate 设为当天的结束，或者只比较日期部分。
      // 为简单起见，如果 birthdayDate 在“昨天”或更早，就算明年。
      
      // 设置生日当天的 23:59:59 用于比较，确保生日当天显示 0 天而不是明年
      birthdayDate.setHours(23, 59, 59, 999);

      if (now.getTime() > birthdayDate.getTime()) {
        // 如果今年的生日已经过完了（比如昨天刚过），则计算明年的农历生日
        var nextLunarYear = currentLunarYear + 1;
        birthdayLunar = Lunar.fromYmd(nextLunarYear, myLunarMonth, myLunarDay);
        birthdaySolar = birthdayLunar.getSolar();
        // 重置为当年的阳历日期对象
        birthdayDate = new Date(birthdaySolar.getYear(), birthdaySolar.getMonth() - 1, birthdaySolar.getDay());
        // 重置时间为0点，方便计算天数差
        birthdayDate.setHours(0, 0, 0, 0);
      } else {
        // 如果还没过（或者就是今天），重置时间为0点
        birthdayDate.setHours(0, 0, 0, 0);
      }

      // 计算当前时间（去掉时分秒干扰）
      var today = new Date();
      today.setHours(0, 0, 0, 0);

      // 计算差距
      var diffTime = birthdayDate.getTime() - today.getTime();
      var diffDays = Math.ceil(diffTime / (1000 * 60 * 60 * 24)); 
      
      return diffDays; 
    }

    try {
      var days = getDaysUntilBirthday();
      document.getElementById('day-count').innerText = days;
    } catch (e) {
      console.error("农历计算出错:", e);
      document.getElementById('lunar-countdown').innerHTML = "计算器加载中...";
    }
  })();
</script>

## notes

<ul>
  {% for post in site.posts %}
    <li>
      <a href="{{ post.url }}">{{ post.title }}</a>
      <span>{{ post.date | date: "%Y-%m-%d" }}</span>
    </li>
  {% endfor %}
</ul>
