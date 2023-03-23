```css
body {
            margin: 0;
            padding: 0;
        }
        .header {
            position: sticky;
            background: #fff;
            top: 0;
            font-size: 20px;
            height:50px;
            z-index: 1;
            display: flex;
            justify-content: center;
            align-items: center;
        }
        .shadow::before {
            content: '';
            box-shadow: 0 0 10px 1px #333;
            position: fixed;
            width: 100%;
        }
        .shadow::after {
            content: '';
            width: 100%;
            height: 20px;
            background: linear-gradient(to bottom, #fff 50%, transparent);
            position: absolute;
        }
        .main {
            height:1000px;
            padding:20px 10px 10px 10px;
        }
```

```html
    <div class="header">不忘初心，方得始终</div>
    <div class="shadow"></div>
    <div class="main">
        林扬生
    </div>
```
<iframe height="300" style="width: 100%;" scrolling="no" title="纯css导航滚动阴影" src="https://codepen.io/quinhua/embed/xxWBEBg?default-tab=html%2Cresult" frameborder="no" loading="lazy" allowtransparency="true" allowfullscreen="true">
  See the Pen <a href="https://codepen.io/quinhua/pen/xxWBEBg">
  纯css导航滚动阴影</a> by qianhuiya (<a href="https://codepen.io/quinhua">@quinhua</a>)
  on <a href="https://codepen.io">CodePen</a>.
</iframe>