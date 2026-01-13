# 新手村广告联盟 - 广告代码调用指南

## 概述

本指南说明如何在网站中接入新手村广告联盟的广告代码。系统提供三种调用方式，您可根据需求选择。

| 调用方式 | 特点 | 推荐场景 |
|---------|------|---------|
| 异步JS标签 | 加载快，不阻塞页面 | **首选方案** |
| JavaScript标签 | 兼容老旧浏览器 | 需要支持 IE9 以下 |
| iFrame标签 | 样式完全隔离 | 防止样式冲突 |

---

## 调用方式一：异步JS标签

### 代码

```html
<!-- 新手村广告联盟 Asynchronous JS Tag - Generated with Revive Adserver v6.0.4 -->
<ins data-revive-zoneid="2" data-revive-id="c20d9063ce27edd4c793a4fbeca73d54"></ins>
<script async src="//ad.newbeebox.com/delivery/asyncjs.php"></script>
```

### 使用方法

将代码直接复制粘贴到网页中需要展示广告的位置即可。

```html
<!DOCTYPE html>
<html>
<head>
    <title>我的网站</title>
</head>
<body>
    <h1>网站内容</h1>

    <!-- 把广告代码放在这里 -->
    <ins data-revive-zoneid="2" data-revive-id="c20d9063ce27edd4c793a4fbeca73d54"></ins>
    <script async src="//ad.newbeebox.com/delivery/asyncjs.php"></script>

    <p>更多内容...</p>
</body>
</html>
```

### 多个广告位

如果同一页面有多个广告位，`<script>` 标签只需引入一次，放在页面底部：

```html
<body>
    <!-- 顶部广告 -->
    <ins data-revive-zoneid="1" data-revive-id="c20d9063ce27edd4c793a4fbeca73d54"></ins>

    <h1>网站内容</h1>

    <!-- 中部广告 -->
    <ins data-revive-zoneid="2" data-revive-id="c20d9063ce27edd4c793a4fbeca73d54"></ins>

    <p>更多内容...</p>

    <!-- 底部广告 -->
    <ins data-revive-zoneid="3" data-revive-id="c20d9063ce27edd4c793a4fbeca73d54"></ins>

    <!-- 脚本只需引入一次 -->
    <script async src="//ad.newbeebox.com/delivery/asyncjs.php"></script>
</body>
```

---

## 调用方式二：JavaScript标签

### 代码

```html
<!-- 新手村广告联盟 Javascript - Generated with Revive Adserver v6.0.4 -->
<script type='text/javascript'><!--//<![CDATA[
   var m3_u = (location.protocol=='https:'?'https://ad.newbeebox.com/delivery/ajs.php':'http://ad.newbeebox.com/delivery/ajs.php');
   var m3_r = Math.floor(Math.random()*99999999999);
   if (!document.MAX_used) document.MAX_used = ',';
   document.write ("<scr"+"ipt type='text/javascript' src='"+m3_u);
   document.write ("?zoneid=2");
   document.write ('&amp;cb=' + m3_r);
   if (document.MAX_used != ',') document.write ("&amp;exclude=" + document.MAX_used);
   document.write (document.charset ? '&amp;charset='+document.charset : (document.characterSet ? '&amp;charset='+document.characterSet : ''));
   document.write ("&amp;loc=" + encodeURIComponent(window.location));
   if (document.referrer) document.write ("&amp;referer=" + encodeURIComponent(document.referrer));
   if (document.context) document.write ("&context=" + encodeURIComponent(document.context));
   document.write ("'><\/scr"+"ipt>");
//]]>--></script><noscript><a href='https://ad.newbeebox.com/delivery/ck.php?n=a7999eff&amp;cb=INSERT_RANDOM_NUMBER_HERE' target='_blank'><img src='https://ad.newbeebox.com/delivery/avw.php?zoneid=2&amp;cb=INSERT_RANDOM_NUMBER_HERE&amp;n=a7999eff' border='0' alt='' /></a></noscript>
```

### 使用方法

**步骤1**：将 `INSERT_RANDOM_NUMBER_HERE` 替换为随机数字

```html
<!-- 替换前 -->
cb=INSERT_RANDOM_NUMBER_HERE

<!-- 替换后（使用任意数字） -->
cb=123456789
```

**步骤2**：将处理后的代码复制到网页中

```html
<!DOCTYPE html>
<html>
<head>
    <title>我的网站</title>
</head>
<body>
    <h1>网站内容</h1>

    <!-- 把广告代码放在这里 -->
    <script type='text/javascript'><!--//<![CDATA[
       var m3_u = (location.protocol=='https:'?'https://ad.newbeebox.com/delivery/ajs.php':'http://ad.newbeebox.com/delivery/ajs.php');
       var m3_r = Math.floor(Math.random()*99999999999);
       if (!document.MAX_used) document.MAX_used = ',';
       document.write ("<scr"+"ipt type='text/javascript' src='"+m3_u);
       document.write ("?zoneid=2");
       document.write ('&amp;cb=' + m3_r);
       if (document.MAX_used != ',') document.write ("&amp;exclude=" + document.MAX_used);
       document.write (document.charset ? '&amp;charset='+document.charset : (document.characterSet ? '&amp;charset='+document.characterSet : ''));
       document.write ("&amp;loc=" + encodeURIComponent(window.location));
       if (document.referrer) document.write ("&amp;referer=" + encodeURIComponent(document.referrer));
       if (document.context) document.write ("&context=" + encodeURIComponent(document.context));
       document.write ("'><\/scr"+"ipt>");
    //]]>--></script>
    <noscript>
        <a href='https://ad.newbeebox.com/delivery/ck.php?n=a7999eff&amp;cb=123456789' target='_blank'>
            <img src='https://ad.newbeebox.com/delivery/avw.php?zoneid=2&amp;cb=123456789&amp;n=a7999eff' border='0' alt='' />
        </a>
    </noscript>

    <p>更多内容...</p>
</body>
</html>
```

### 动态生成随机数（可选）

如果使用服务端语言，可以动态生成随机数：

**PHP：**
```php
<?php $cb = mt_rand(100000000, 999999999); ?>
<noscript>
    <a href='https://ad.newbeebox.com/delivery/ck.php?n=a7999eff&amp;cb=<?php echo $cb; ?>' target='_blank'>
        <img src='https://ad.newbeebox.com/delivery/avw.php?zoneid=2&amp;cb=<?php echo $cb; ?>&amp;n=a7999eff' border='0' alt='' />
    </a>
</noscript>
```

---

## 调用方式三：iFrame标签

### 代码

```html
<!-- 新手村广告联盟 iFrame - Generated with Revive Adserver v6.0.4 -->
<iframe id='ab25cde1' name='ab25cde1' src='https://ad.newbeebox.com/delivery/afr.php?zoneid=2&amp;cb=INSERT_RANDOM_NUMBER_HERE' frameborder='0' scrolling='no' width='600' height='300' allow='autoplay'><a href='https://ad.newbeebox.com/delivery/ck.php?n=a36e66f1&amp;cb=INSERT_RANDOM_NUMBER_HERE' target='_blank'><img src='https://ad.newbeebox.com/delivery/avw.php?zoneid=2&amp;cb=INSERT_RANDOM_NUMBER_HERE&amp;n=a36e66f1' border='0' alt='' /></a></iframe>
```

### 使用方法

**步骤1**：将所有 `INSERT_RANDOM_NUMBER_HERE` 替换为相同的随机数字

```html
<!-- 替换前 -->
cb=INSERT_RANDOM_NUMBER_HERE

<!-- 替换后（三处都要替换为相同的数字） -->
cb=987654321
```

**步骤2**：根据广告位实际尺寸修改 `width` 和 `height`

**步骤3**：将处理后的代码复制到网页中

```html
<!DOCTYPE html>
<html>
<head>
    <title>我的网站</title>
</head>
<body>
    <h1>网站内容</h1>

    <!-- 把广告代码放在这里 -->
    <iframe
        id='ab25cde1'
        name='ab25cde1'
        src='https://ad.newbeebox.com/delivery/afr.php?zoneid=2&amp;cb=987654321'
        frameborder='0'
        scrolling='no'
        width='600'
        height='300'
        allow='autoplay'>
        <a href='https://ad.newbeebox.com/delivery/ck.php?n=a36e66f1&amp;cb=987654321' target='_blank'>
            <img src='https://ad.newbeebox.com/delivery/avw.php?zoneid=2&amp;cb=987654321&amp;n=a36e66f1' border='0' alt='' />
        </a>
    </iframe>

    <p>更多内容...</p>
</body>
</html>
```

---

## 常见问题

### 广告不显示怎么办？

1. 检查广告位是否已在后台配置素材
2. 检查浏览器是否安装了广告拦截插件
3. 检查代码是否完整复制

### 应该选择哪种调用方式？

- **一般情况**：选择方式一（异步JS标签）
- **需要兼容IE8及以下**：选择方式二（JavaScript标签）
- **广告样式与网站冲突**：选择方式三（iFrame标签）

### INSERT_RANDOM_NUMBER_HERE 必须替换吗？

是的，方式二和方式三中必须将 `INSERT_RANDOM_NUMBER_HERE` 替换为数字，否则广告统计可能不准确。可以使用任意数字，如 `123456789`。

---

*文档版本：1.0 | 更新日期：2026-01-13*
