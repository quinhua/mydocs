
> Chart.xkcd是一个图表库，绘制“粗略”，“卡通”或“手绘”样式的图表。

# 安装

### CDN

```js

 <script src="https://cdn.jsdelivr.net/npm/chart.xkcd@1.1/dist/chart.xkcd.min.js"></script>
```

### npm
```js
npm i chart.xkcd
```

# 折线图
```javascript
<svg class="line-chart"></svg>
<script src="https://cdn.jsdelivr.net/npm/chart.xkcd@1.1/dist/chart.xkcd.min.js"></script>
<script>
  const svg = document.querySelector('.line-chart')
  const lineChart = new chartXkcd.Line(svg, {
    title: 'Monthly income of an indie developer', // optional
    xLabel: 'Month', // optional
    yLabel: '$ Dollors', // optional
    data: {
      labels: ['1', '2', '3', '4', '5', '6', '7', '8', '9', '10'],
      datasets: [{
        label: 'Plan',
        data: [30, 70, 200, 300, 500, 800, 1500, 2900, 5000, 8000],
      }, {
        label: 'Reality',
        data: [0, 1, 30, 70, 80, 100, 50, 80, 40, 150],
      }],
    },
    options: { // optional
      yTickCount: 3,
      legendPosition: chartXkcd.config.positionType.upLeft
    }
  });
</script>
```
<iframe height="300" style="width: 100%;" scrolling="no" title="Untitled" src="https://codepen.io/quinhua/embed/jOxwwrw?default-tab=html%2Cresult" frameborder="no" loading="lazy" allowtransparency="true" allowfullscreen="true">
  See the Pen <a href="https://codepen.io/quinhua/pen/jOxwwrw">
  Untitled</a> by qianhuiya (<a href="https://codepen.io/quinhua">@quinhua</a>)
  on <a href="https://codepen.io">CodePen</a>.
</iframe>

# XY 控制图
```js
<svg class="xy-chart"></svg>
<script src="https://cdn.jsdelivr.net/npm/chart.xkcd@1.1/dist/chart.xkcd.min.js"></script>
<script>
  const svg = document.querySelector('.xy-chart');
  new chartXkcd.XY(svg, {
    title: 'Pokemon farms', //optional
    xLabel: 'Coodinate', //optional
    yLabel: 'Count', //optional
    data: {
      datasets: [{
        label: 'Pikachu',
        data: [{ x: 3, y: 10 }, { x: 4, y: 122 }, { x: 10, y: 100 }, { x: 1, y: 2 }, { x: 2, y: 4 }],
      }, {
        label: 'Squirtle',
        data: [{ x: 3, y: 122 }, { x: 4, y: 212 }, { x: -3, y: 100 }, { x: 1, y: 1 }, { x: 1.5, y: 12 }],
      }],
    },
    options: { //optional
      xTickCount: 5,
      yTickCount: 5,
      legendPosition: chartXkcd.config.positionType.upRight,
      showLine: false,
      timeFormat: undefined,
      dotSize: 1,
    },
  });
</script>
```
<iframe height="300" style="width: 100%;" scrolling="no" title="chart.xkcd-XY控制图" src="https://codepen.io/quinhua/embed/ZEoyypx?default-tab=html%2Cresult" frameborder="no" loading="lazy" allowtransparency="true" allowfullscreen="true">
  See the Pen <a href="https://codepen.io/quinhua/pen/ZEoyypx">
  chart.xkcd-XY控制图</a> by qianhuiya (<a href="https://codepen.io/quinhua">@quinhua</a>)
  on <a href="https://codepen.io">CodePen</a>.
</iframe>

# 条形图
```javascript
<svg class="bar-chart"></svg>
<script src="https://cdn.jsdelivr.net/npm/chart.xkcd@1.1/dist/chart.xkcd.min.js"></script>
<script>
  const svg = document.querySelector('.bar-chart')
  
const barChart = new chartXkcd.Bar(svg, {
  title: 'github stars VS patron number', // optional
  // xLabel: '', // optional
  // yLabel: '', // optional
  data: {
    labels: ['github stars', 'patrons'],
    datasets: [{
      data: [100, 2],
    }],
  },
  options: { // optional
    yTickCount: 2,
  },
});

</script>
```
<iframe height="300" style="width: 100%;" scrolling="no" title="chart.xkcd-条形图" src="https://codepen.io/quinhua/embed/PoejjbN?default-tab=html%2Cresult" frameborder="no" loading="lazy" allowtransparency="true" allowfullscreen="true">
  See the Pen <a href="https://codepen.io/quinhua/pen/PoejjbN">
  chart.xkcd-条形图</a> by qianhuiya (<a href="https://codepen.io/quinhua">@quinhua</a>)
  on <a href="https://codepen.io">CodePen</a>.
</iframe>

# 饼图
```javascript
<svg class="pie-chart"></svg>
<script src="https://cdn.jsdelivr.net/npm/chart.xkcd@1.1/dist/chart.xkcd.min.js"></script>
<script>
  const svg = document.querySelector('.pie-chart');
  const pieChart = new chartXkcd.Pie(svg, {
    title: 'What Tim made of', // optional
    data: {
      labels: ['a', 'b', 'e', 'f', 'g'],
      datasets: [{
        data: [500, 200, 80, 90, 100],
      }],
    },
    options: { // optional
      innerRadius: 0.5,
      legendPosition: chartXkcd.config.positionType.upRight,
    },
  });
</script>
```
<iframe height="300" style="width: 100%;" scrolling="no" title="Untitled" src="https://codepen.io/quinhua/embed/NWMggpj?default-tab=html%2Cresult" frameborder="no" loading="lazy" allowtransparency="true" allowfullscreen="true">
  See the Pen <a href="https://codepen.io/quinhua/pen/NWMggpj">
  Untitled</a> by qianhuiya (<a href="https://codepen.io/quinhua">@quinhua</a>)
  on <a href="https://codepen.io">CodePen</a>.
</iframe>

# 雷达图
```javascript
<svg class="radar-chart"></svg>
<script src="https://cdn.jsdelivr.net/npm/chart.xkcd@1.1/dist/chart.xkcd.min.js"></script>
<script>
  const svg = document.querySelector('.radar-chart');
  
  const radarChart = new chartXkcd.Radar(svg, {
    title: 'Letters in random words',
    data: {
      labels: ['c', 'h', 'a', 'r', 't'],
      datasets: [{
        label: 'ccharrrt',
        data: [2, 1, 1, 3, 1],
      }, {
        label: 'chhaart',
        data: [1, 2, 2, 1, 1],
      }],
    },
    options: {
      showLegend: true,
      dotSize: 0.8,
      showLabels: true,
      legendPosition: chartXkcd.config.positionType.upRight,
      // unxkcdify: true,
    },
  });
</script>
```
<iframe height="300" style="width: 100%;" scrolling="no" title="chart.xkcd-雷达图" src="https://codepen.io/quinhua/embed/QWrggvM?default-tab=html%2Cresult" frameborder="no" loading="lazy" allowtransparency="true" allowfullscreen="true">
  See the Pen <a href="https://codepen.io/quinhua/pen/QWrggvM">
  chart.xkcd-雷达图</a> by qianhuiya (<a href="https://codepen.io/quinhua">@quinhua</a>)
  on <a href="https://codepen.io">CodePen</a>.
</iframe>