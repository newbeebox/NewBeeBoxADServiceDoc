# 新手村广告联盟 - 广告代码调用指南

## 概述

本文档介绍新手村广告联盟提供的三种广告代码调用方式。每种方式适用于不同的使用场景，开发者可根据实际需求选择合适的调用方式。

### 广告服务器信息

| 项目 | 值 |
|------|-----|
| 广告服务器域名 | ad.newbeebox.com |
| 支持协议 | HTTP / HTTPS |

### 三种调用方式对比

| 特性 | 异步JS标签 | JavaScript标签 | iFrame标签 |
|------|-----------|---------------|-----------|
| 加载方式 | 异步加载 | 同步加载 | 独立框架 |
| 页面性能影响 | 低 | 中 | 低 |
| 样式隔离 | 否 | 否 | 是 |
| SEO友好 | 是 | 是 | 否 |
| 兼容性 | 现代浏览器 | 所有浏览器 | 所有浏览器 |
| 推荐场景 | 首选方案 | 传统网站 | 需要隔离样式 |

---

## 调用方式一：异步JS标签 (Asynchronous JS Tag)

### 简介

异步JS标签是**推荐的首选方案**。采用 HTML5 的 `async` 属性异步加载广告脚本，不会阻塞页面渲染，对页面加载性能影响最小。

### 工作原理

1. 浏览器解析到 `<ins>` 标签时，创建广告占位符
2. 异步加载 `asyncjs.php` 脚本
3. 脚本执行后，根据 `data-revive-zoneid` 获取对应广告位的素材
4. 将广告内容填充到 `<ins>` 标签位置

### 代码结构

```html
<!-- 新手村广告联盟 Asynchronous JS Tag - Generated with Revive Adserver v6.0.4 -->
<ins data-revive-zoneid="[广告位ID]" data-revive-id="[Revive实例ID]"></ins>
<script async src="//ad.newbeebox.com/delivery/asyncjs.php"></script>
```

### 参数说明

| 参数 | 说明 | 示例值 |
|------|------|--------|
| `data-revive-zoneid` | 广告位ID，从后台获取 | `2` |
| `data-revive-id` | Revive Adserver 实例唯一标识 | `c20d9063ce27edd4c793a4fbeca73d54` |

### 完整示例

#### 示例1：单个广告位

```html
<!DOCTYPE html>
<html lang="zh-CN">
<head>
    <meta charset="UTF-8">
    <title>异步广告示例</title>
</head>
<body>
    <h1>网站内容</h1>

    <!-- 广告位 -->
    <div class="ad-container">
        <!-- 新手村广告联盟 Asynchronous JS Tag -->
        <ins data-revive-zoneid="2" data-revive-id="c20d9063ce27edd4c793a4fbeca73d54"></ins>
        <script async src="//ad.newbeebox.com/delivery/asyncjs.php"></script>
    </div>

    <p>更多网站内容...</p>
</body>
</html>
```

#### 示例2：同一页面多个广告位

```html
<!DOCTYPE html>
<html lang="zh-CN">
<head>
    <meta charset="UTF-8">
    <title>多广告位示例</title>
</head>
<body>
    <!-- 顶部横幅广告 (Zone 1) -->
    <div class="header-ad">
        <ins data-revive-zoneid="1" data-revive-id="c20d9063ce27edd4c793a4fbeca73d54"></ins>
    </div>

    <main>
        <h1>文章标题</h1>
        <p>文章内容...</p>

        <!-- 文章中部广告 (Zone 2) -->
        <div class="content-ad">
            <ins data-revive-zoneid="2" data-revive-id="c20d9063ce27edd4c793a4fbeca73d54"></ins>
        </div>

        <p>更多内容...</p>
    </main>

    <!-- 侧边栏广告 (Zone 3) -->
    <aside>
        <ins data-revive-zoneid="3" data-revive-id="c20d9063ce27edd4c793a4fbeca73d54"></ins>
    </aside>

    <!-- 只需引入一次脚本，会自动处理页面上所有 ins 标签 -->
    <script async src="//ad.newbeebox.com/delivery/asyncjs.php"></script>
</body>
</html>
```

### 优点

- 不阻塞页面加载，性能最优
- 代码简洁，易于维护
- 同一页面多个广告位只需引入一次脚本
- 自动处理 HTTP/HTTPS 协议

### 缺点

- 需要浏览器支持 `async` 属性（IE10+）
- 广告加载时机不确定，可能出现布局抖动

### 注意事项

1. **脚本只需引入一次**：同一页面有多个广告位时，`asyncjs.php` 脚本只需在页面底部引入一次
2. **协议自适应**：使用 `//` 开头的 URL，会自动匹配当前页面协议
3. **广告容器样式**：建议为广告容器设置固定尺寸，避免加载时布局抖动

```css
.ad-container {
    min-height: 250px;  /* 根据广告尺寸设置 */
    min-width: 300px;
}
```

---

## 调用方式二：JavaScript标签 (JavaScript Tag)

### 简介

JavaScript标签是传统的广告调用方式，使用 `document.write` 同步加载广告内容。兼容性最好，支持所有浏览器，包括老旧浏览器。

### 工作原理

1. 浏览器解析到脚本时，暂停页面渲染
2. 动态构建广告请求 URL，包含随机数、页面地址等参数
3. 使用 `document.write` 插入广告脚本
4. 广告脚本执行，输出广告内容
5. 如果 JavaScript 被禁用，显示 `<noscript>` 中的静态图片广告

### 代码结构

```html
<!-- 新手村广告联盟 Javascript Tag -->
<script type='text/javascript'><!--//<![CDATA[
   var m3_u = (location.protocol=='https:'?'https://ad.newbeebox.com/delivery/ajs.php':'http://ad.newbeebox.com/delivery/ajs.php');
   var m3_r = Math.floor(Math.random()*99999999999);
   if (!document.MAX_used) document.MAX_used = ',';
   document.write ("<scr"+"ipt type='text/javascript' src='"+m3_u);
   document.write ("?zoneid=[广告位ID]");
   document.write ('&amp;cb=' + m3_r);
   if (document.MAX_used != ',') document.write ("&amp;exclude=" + document.MAX_used);
   document.write (document.charset ? '&amp;charset='+document.charset : (document.characterSet ? '&amp;charset='+document.characterSet : ''));
   document.write ("&amp;loc=" + encodeURIComponent(window.location));
   if (document.referrer) document.write ("&amp;referer=" + encodeURIComponent(document.referrer));
   if (document.context) document.write ("&context=" + encodeURIComponent(document.context));
   document.write ("'><\/scr"+"ipt>");
//]]>--></script>
<noscript>
    <a href='https://ad.newbeebox.com/delivery/ck.php?n=[唯一标识]&amp;cb=INSERT_RANDOM_NUMBER_HERE' target='_blank'>
        <img src='https://ad.newbeebox.com/delivery/avw.php?zoneid=[广告位ID]&amp;cb=INSERT_RANDOM_NUMBER_HERE&amp;n=[唯一标识]' border='0' alt='' />
    </a>
</noscript>
```

### 参数说明

| 参数 | 说明 | 示例值 |
|------|------|--------|
| `zoneid` | 广告位ID | `2` |
| `cb` | 缓存破坏随机数 | 自动生成 |
| `loc` | 当前页面URL | 自动获取 |
| `referer` | 来源页面URL | 自动获取 |
| `charset` | 页面字符集 | 自动检测 |
| `exclude` | 排除已展示的广告 | 自动处理 |
| `n` | noscript 回退标识 | `a7999eff` |

### 完整示例

#### 示例1：基础用法

```html
<!DOCTYPE html>
<html lang="zh-CN">
<head>
    <meta charset="UTF-8">
    <title>JavaScript广告示例</title>
</head>
<body>
    <h1>网站内容</h1>

    <!-- 广告位 -->
    <div class="ad-container">
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
        //]]>--></script>
        <noscript>
            <a href='https://ad.newbeebox.com/delivery/ck.php?n=a7999eff&amp;cb=123456789' target='_blank'>
                <img src='https://ad.newbeebox.com/delivery/avw.php?zoneid=2&amp;cb=123456789&amp;n=a7999eff' border='0' alt='' />
            </a>
        </noscript>
    </div>

    <p>更多网站内容...</p>
</body>
</html>
```

#### 示例2：处理 noscript 随机数

在生产环境中，需要将 `INSERT_RANDOM_NUMBER_HERE` 替换为实际的随机数：

**服务端处理（PHP示例）：**

```php
<?php
$random = mt_rand(100000000, 999999999);
$adCode = <<<HTML
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
    <a href='https://ad.newbeebox.com/delivery/ck.php?n=a7999eff&amp;cb={$random}' target='_blank'>
        <img src='https://ad.newbeebox.com/delivery/avw.php?zoneid=2&amp;cb={$random}&amp;n=a7999eff' border='0' alt='' />
    </a>
</noscript>
HTML;
echo $adCode;
?>
```

**前端处理（JavaScript示例）：**

```html
<script>
// 在页面加载后替换 noscript 中的随机数占位符
document.addEventListener('DOMContentLoaded', function() {
    var noscripts = document.querySelectorAll('noscript');
    noscripts.forEach(function(ns) {
        var content = ns.innerHTML;
        if (content.indexOf('INSERT_RANDOM_NUMBER_HERE') > -1) {
            var random = Math.floor(Math.random() * 999999999);
            ns.innerHTML = content.replace(/INSERT_RANDOM_NUMBER_HERE/g, random);
        }
    });
});
</script>
```

### 优点

- 兼容所有浏览器，包括 IE6+
- 内置 noscript 回退方案
- 自动收集页面信息（URL、来源、字符集）
- 支持广告排重（避免同一广告重复展示）

### 缺点

- 使用 `document.write`，会阻塞页面渲染
- 代码较长，不够简洁
- 现代浏览器可能在某些情况下阻止 `document.write`

### 注意事项

1. **随机数占位符**：`INSERT_RANDOM_NUMBER_HERE` 需要替换为实际随机数
2. **页面位置**：建议放在页面靠后位置，减少对首屏加载的影响
3. **CDATA 注释**：保留 `<!--//<![CDATA[` 和 `//]]>-->` 注释，确保 XHTML 兼容性

---

## 调用方式三：iFrame标签 (iFrame Tag)

### 简介

iFrame标签将广告内容加载到独立的内嵌框架中，实现完全的样式隔离。适用于需要防止广告样式与页面样式冲突的场景。

### 工作原理

1. 浏览器创建 iframe 元素
2. iframe 加载 `afr.php`，返回完整的广告 HTML 页面
3. 广告在独立的文档环境中渲染
4. 如果 iframe 不支持，显示回退的图片广告链接

### 代码结构

```html
<!-- 新手村广告联盟 iFrame Tag -->
<iframe
    id='[唯一ID]'
    name='[唯一名称]'
    src='https://ad.newbeebox.com/delivery/afr.php?zoneid=[广告位ID]&amp;cb=[随机数]'
    frameborder='0'
    scrolling='no'
    width='[宽度]'
    height='[高度]'
    allow='autoplay'>
    <!-- 回退内容 -->
    <a href='https://ad.newbeebox.com/delivery/ck.php?n=[标识]&amp;cb=[随机数]' target='_blank'>
        <img src='https://ad.newbeebox.com/delivery/avw.php?zoneid=[广告位ID]&amp;cb=[随机数]&amp;n=[标识]' border='0' alt='' />
    </a>
</iframe>
```

### 参数说明

| 参数 | 说明 | 示例值 |
|------|------|--------|
| `id` / `name` | iframe 唯一标识 | `ab25cde1` |
| `zoneid` | 广告位ID | `2` |
| `cb` | 缓存破坏随机数 | `INSERT_RANDOM_NUMBER_HERE` |
| `width` | 广告宽度（像素） | `600` |
| `height` | 广告高度（像素） | `300` |
| `frameborder` | 边框宽度 | `0` |
| `scrolling` | 是否显示滚动条 | `no` |
| `allow` | 允许的功能 | `autoplay` |

### 完整示例

#### 示例1：基础用法

```html
<!DOCTYPE html>
<html lang="zh-CN">
<head>
    <meta charset="UTF-8">
    <title>iFrame广告示例</title>
</head>
<body>
    <h1>网站内容</h1>

    <!-- 广告位 (600x300) -->
    <div class="ad-container">
        <!-- 新手村广告联盟 iFrame - Generated with Revive Adserver v6.0.4 -->
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
    </div>

    <p>更多网站内容...</p>
</body>
</html>
```

#### 示例2：动态生成随机数

```html
<!DOCTYPE html>
<html lang="zh-CN">
<head>
    <meta charset="UTF-8">
    <title>动态iFrame广告示例</title>
</head>
<body>
    <h1>网站内容</h1>

    <!-- 广告容器 -->
    <div id="ad-container"></div>

    <script>
    (function() {
        var cb = Math.floor(Math.random() * 999999999);
        var container = document.getElementById('ad-container');

        var iframe = document.createElement('iframe');
        iframe.id = 'ad-frame-' + cb;
        iframe.name = 'ad-frame-' + cb;
        iframe.src = 'https://ad.newbeebox.com/delivery/afr.php?zoneid=2&cb=' + cb;
        iframe.width = '600';
        iframe.height = '300';
        iframe.frameBorder = '0';
        iframe.scrolling = 'no';
        iframe.allow = 'autoplay';

        // 回退内容
        iframe.innerHTML = '<a href="https://ad.newbeebox.com/delivery/ck.php?n=a36e66f1&cb=' + cb + '" target="_blank">' +
            '<img src="https://ad.newbeebox.com/delivery/avw.php?zoneid=2&cb=' + cb + '&n=a36e66f1" border="0" alt="" />' +
            '</a>';

        container.appendChild(iframe);
    })();
    </script>
</body>
</html>
```

#### 示例3：响应式 iFrame

```html
<!DOCTYPE html>
<html lang="zh-CN">
<head>
    <meta charset="UTF-8">
    <title>响应式iFrame广告</title>
    <style>
        .ad-wrapper {
            position: relative;
            width: 100%;
            max-width: 600px;
            padding-bottom: 50%; /* 宽高比 2:1 (300/600) */
            height: 0;
            overflow: hidden;
        }
        .ad-wrapper iframe {
            position: absolute;
            top: 0;
            left: 0;
            width: 100%;
            height: 100%;
            border: 0;
        }
    </style>
</head>
<body>
    <h1>响应式广告示例</h1>

    <div class="ad-wrapper">
        <iframe
            id='responsive-ad'
            name='responsive-ad'
            src='https://ad.newbeebox.com/delivery/afr.php?zoneid=2&amp;cb=123456789'
            frameborder='0'
            scrolling='no'
            allow='autoplay'>
        </iframe>
    </div>
</body>
</html>
```

### 优点

- 完全的样式隔离，广告样式不会影响页面
- 页面样式也不会影响广告展示
- 广告崩溃不会影响主页面
- 支持视频广告自动播放（需要 `allow='autoplay'`）

### 缺点

- SEO 不友好，搜索引擎无法索引 iframe 内容
- 需要预先知道广告尺寸
- 跨域限制，无法通过 JavaScript 访问 iframe 内容
- 某些广告拦截器会阻止 iframe 广告

### 注意事项

1. **尺寸匹配**：`width` 和 `height` 需要与广告位设置的尺寸一致
2. **随机数**：务必替换 `INSERT_RANDOM_NUMBER_HERE` 为实际随机数
3. **HTTPS**：生产环境建议使用 HTTPS 协议
4. **allow 属性**：如果广告包含视频，需要添加 `allow='autoplay'`

---

## 最佳实践

### 1. 调用方式选择建议

```
┌─────────────────────────────────────────────────────────────┐
│                     选择广告调用方式                          │
├─────────────────────────────────────────────────────────────┤
│                                                             │
│  是否需要支持 IE9 及以下浏览器？                              │
│       │                                                     │
│       ├── 是 ──→ 使用 JavaScript标签 (方式二)                │
│       │                                                     │
│       └── 否 ──→ 是否需要样式隔离？                          │
│                    │                                        │
│                    ├── 是 ──→ 使用 iFrame标签 (方式三)       │
│                    │                                        │
│                    └── 否 ──→ 使用 异步JS标签 (方式一) ★推荐  │
│                                                             │
└─────────────────────────────────────────────────────────────┘
```

### 2. 性能优化建议

```html
<!-- 1. 异步加载：将广告代码放在页面底部 -->
<body>
    <!-- 页面主要内容 -->
    <main>...</main>

    <!-- 广告代码放在底部 -->
    <div class="ad-container">
        <ins data-revive-zoneid="2" data-revive-id="c20d9063ce27edd4c793a4fbeca73d54"></ins>
    </div>
    <script async src="//ad.newbeebox.com/delivery/asyncjs.php"></script>
</body>

<!-- 2. 预连接：提前建立与广告服务器的连接 -->
<head>
    <link rel="preconnect" href="https://ad.newbeebox.com">
    <link rel="dns-prefetch" href="https://ad.newbeebox.com">
</head>

<!-- 3. 设置容器尺寸：避免布局抖动 -->
<style>
.ad-container {
    width: 300px;
    height: 250px;
    background: #f5f5f5; /* 加载中的背景色 */
}
</style>
```

### 3. 常见问题排查

| 问题 | 可能原因 | 解决方案 |
|------|---------|---------|
| 广告不显示 | Zone ID 错误 | 检查后台广告位配置 |
| 显示空白 | 广告位未关联素材 | 在后台为广告位关联活动 |
| 控制台报错 | 协议不匹配 | 统一使用 `//` 或 `https://` |
| 样式错乱 | CSS 冲突 | 使用 iFrame 方式隔离 |
| 被拦截 | 广告拦截器 | 提示用户关闭拦截器 |

---

## 版本历史

| 版本 | 日期 | 说明 |
|------|------|------|
| 1.0 | 2026-01-13 | 初始版本 |
