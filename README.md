自己修改版本volantis主题
> ...表示省略

## 增加文章首行缩进功能

> 在文章meta信息加上 `indent: false` 即可关闭

layout/_partial/article.ejs

from

```ejs
<article itemscope itemtype="http://schema.org/Article" class="article post white-box reveal md ...
```

to

```ejs
<article itemscope itemtype="http://schema.org/Article" class="<%- page.indent == true ? 'cus-indent' : '' %> article post white-box reveal md ...
```

## 增加文章时效性提醒

> 在文章meta信息加上 `dateout: true` 即可显示

layout/_partial/article.ejs

在

```ejs
<div id="layoutHelper-page-plugins"></div>
```

的上一行新增

```ejs
  <%
  let warningDay = 180;
  let errorDay = 365;
  // 获取文章的发布日期
  let pubTime = new Date(post.date);
  // 获取文章更新日期
  let lastUpdate = new Date(post.updated);
  let currentDate = new Date();
  // 发布日期时间差
  let timeDifference = currentDate - pubTime;
  // 更新日期时间差
  let timeDifference2 = currentDate - lastUpdate;
  // 将毫秒数转换为天数
  let interval = Math.floor(timeDifference / (1000 * 60 * 60 * 24));
  let updateInterval = Math.floor(timeDifference2 / (1000 * 60 * 60 * 24));
  let dateOutShow = post.dateout == true
  %>
  <%if(dateOutShow != false) {%>
  <%if(interval > warningDay && updateInterval <= 7) {%>
    <div class="note info">
      <p style="font-weight: bold;">文章时效性提示</p>
      <p>本文发布于 <%-interval%> 天前，已于 <%-updateInterval%> 天前更新。</p>
    </div>
  <%} else if (interval > warningDay && interval < errorDay) {%>
    <div class="note warning">
      <p style="font-weight: bold;">文章时效性提示</p>
      <p>本文发布于 <%-interval%> 天前，且 <%-updateInterval%> 天未更新，部分信息可能已有发展或改变！</p>
    </div>
  <% } else if (interval >= errorDay && (updateInterval >= warningDay && updateInterval < errorDay)) { %>
    <div class="note warning">
      <p style="font-weight: bold;">文章时效性提示</p>
      <p>本文发布于 <%-interval%> 天前，且 <%-updateInterval%> 天未更新，部分信息可能已有发展或改变！</p>
    </div>
  <% } else if (interval >= errorDay && updateInterval >= errorDay) { %>
    <div class="note radiation red">
      <p style="font-weight: bold;">文章时效性提示</p>
      <p>本文发布于 <%-interval%> 天前，且 <%-updateInterval%> 天未更新，部分信息可能已有发展或改变！</p>
    </div>
  <%}}%>
```