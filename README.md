# Gazeek's blog

A simple mathy oriented blog based on [Chirpy Jekyll Theme](https://github.com/cotes2020/jekyll-theme-chirpy#documentation)

The main difference is support for plotly with template that follows the light/dark theme of **Chirpy**.

## Using plotly


We need to first turn on plotly in [Front Matter](https://jekyllrb.com/docs/front-matter/)
```yaml
---
plotly: true
---
```

Then the example is adapted from [plotly documentation](https://plotly.com/javascript/getting-started/). 

We start by creating the appropriate location for the plot, can be as simple as:

```html
<div id="tester">
</div>
```

By default the template will be handled by the website to match the light/dark theme, to alter this behavior set:

```html
<div id="tester" data-plotly-skip-relayout="true">
</div>

```

Next, you need to write the actual javascript code to generate your plot:

```html
<script type="text/javascript">

  TESTER = document.getElementById('tester');

  // mousewheel or two-finger scroll zooms the plot

  var trace1 = {
    x:['2020-10-04', '2021-11-04', '2023-12-04'],
    y: [90, 40, 60],
    type: 'scatter'
  };

  var data = [trace1];

  var layout = {
    title: 'Scroll and Zoom',
    showlegend: false
  };

  const config = getPlotlyConfig();

  Plotly.newPlot(TESTER, data, layout, config);

</script> 
```

The function `getPlotlyConfig()` returns a "good default" config for the plots.
I was hoping that I could use `Plotly.setPlotConfig()` to set good defaults, however, it seems it is used for some other purpose.


## Credits

I would like to really thank @cotes2020 the creator of [Chirpy Jekyll Theme](https://github.com/cotes2020/jekyll-theme-chirpy) because it has been super easy to get started with his theme and edit it for my purposes.

## License

This project is published under [MIT License][license].