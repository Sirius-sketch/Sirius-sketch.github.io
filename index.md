---
layout: default
title: 首页
---

<div id="lunar-countdown" style="padding: 20px; background-color: #f9f9f9; border-radius: 10px; text-align: center; margin: 20px 0; font-family: sans-serif;">
  <h3 style="margin: 0; color: #333;">🎂 农历生日倒计时</h3>
  <p style="font-size: 1.2em; color: #666;">
    离下一次生日（农历十月二十二）还有：
    <span id="day-count" style="font-size: 2em; color: #d9534f; font-weight: bold;">--</span> 天
  </p>
</div>

<script src="https://cdn.jsdelivr.net/npm/lunar-javascript/lunar.js"></script>

<script>
  (function() {
    // ================= 配置区域 =================
    // 在这里修改你的农历生日（数字）
    var myLunarMonth = 10;  // 农历月份
    var myLunarDay = 22;    // 农历日期
    // ===========================================

    function getDaysUntilBirthday() {
      var now = new Date();
      var solar = Solar.fromDate(now);
      var currentYear = solar.getLunar().getYear();
      
      // 1. 先尝试获取今年的农历生日对应的阳历日期
      var birthdayLunar = Lunar.fromYmd(currentYear, myLunarMonth, myLunarDay);
      var birthdaySolar = birthdayLunar.getSolar();
      var birthdayDate = new Date(birthdaySolar.getYear(), birthdaySolar.getMonth() - 1, birthdaySolar.getDay());

      // 2. 如果今年的农历生日已经过了（或者是今天），就计算明年的
      // 注意：这里简单判断时间戳。如果想包含“今天”，逻辑可以微调
      if (now.getTime() > birthdayDate.getTime() + (24 * 60 * 60 * 1000)) {
        // 获取下一年（注意：农历年+1，需要重新计算对应的阳历）
        // 这里的逻辑稍微复杂，因为单纯年份+1可能不准确，直接推算下一个农历年
        var nextLunarYear = currentYear + 1;
        birthdayLunar = Lunar.fromYmd(nextLunarYear, myLunarMonth, myLunarDay);
        birthdaySolar = birthdayLunar.getSolar();
        birthdayDate = new Date(birthdaySolar.getYear(), birthdaySolar.getMonth() - 1, birthdaySolar.getDay());
      }

      // 3. 计算差距
      var diffTime = Math.abs(birthdayDate - now);
      var diffDays = Math.ceil(diffTime / (1000 * 60 * 60 * 24)); 
      
      // 如果计算出来是0，说明就是今天（视具体需求，这里向上取整通常会显示1或者0）
      return diffDays; 
    }

    // 运行并在页面显示
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
