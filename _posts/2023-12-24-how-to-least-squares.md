---
title: How to least squares and <i>why</i>?
date: 2023-12-24 16:40:33 +/- 0001
categories: [Math, Statistics, Beginner]
tags: [optimization]     # TAG names should always be lowercase

layout: post

math: true
number_equations: true
toc: true
mermaid: true
plotly: true
---

---

> A lot of the main points of this post are **WRONG**. I will try to write a better version in the future. In the meantime, please read with caution.
{: .prompt-tip }

## Introduction

This post will show step-by-step when, how and why to use the [Least squares method](https://en.wikipedia.org/wiki/Least_squares). We will analyze the revenue of a company over the past 20 years and try to predict its future trajectory. This will lead us to a discussion of the [exponential least squares](https://mathworld.wolfram.com/LeastSquaresFittingExponential.html) fit. Let's start off with something simple.

## Idealized case

Imagine that you are given a set of revenue data for company *A*. The data is shown in the table below.

| Year | Revenue ($M) | Year | Revenue ($M) | Year | Revenue ($M) | Year | Revenue ($M) |
| ---- | ------------ | ---- | ------------ | ---- | ------------ | ---- | ------------ |
| 2000 | 100          | 2005 | 201          | 2010 | 405          | 2015 | 814          |
| 2001 | 115          | 2006 | 231          | 2011 | 465          | 2016 | 936          |
| 2002 | 132          | 2007 | 266          | 2012 | 535          | 2017 | 1076         |
| 2003 | 152          | 2008 | 306          | 2013 | 615          | 2018 | 1238         |
| 2004 | 175          | 2009 | 352          | 2014 | 708          | 2019 | 1423         |

On a plot it looks like:
<div id="idealized-plot">
</div>

<script type="text/javascript">
  deferPlotly(function () {
    plotName = document.getElementById('idealized-plot');

    N = 20;
    startX = 2000;
    startY = 100;
    it = Array.from({length: N}, (_, i) => i);
    var data = [{
      x: it.map(x => startX + x),
      y: it.map(x => startY * (1.15)**x),
      type: 'scatter'
    }];

    var layout = {
      title: 'Revenue for ideal <i>A</i> company',
      showlegend: false,
      xaxis: {
        title: 'Year',
        tickmargin: 10
      },
      yaxis: {
        title: 'Revenue ($M)',
        tickmargin: 10
      }
    };

    var config = getPlotlyConfig();
    config['staticPlot'] = true;
    config['scrollZoom'] = false;

    return Plotly.newPlot(plotName, data, layout, config);
  });
</script> 

If the company revenue was growing linearly, then the revenue could fit on a line, however, it doesn't seem to be the case. The revenue seems to be growing faster than linearly. This does make sense, because you expect revenue growth to be dependent on the revenue from the previous year, and this might be a fraction, which we could call $g$ - for growth. If you know that over the past 20 years the company has been growing by $g$ then you can try to predict what will be the revenue next year, and the year after that, and so on. So let's try to find $g$.

Now if the revenue is $R_n$ as the revenue in year $2000+n$ (e.g. 2010) then we could write that:

\begin{equation}
  R_{n+1} = R_{n}  + g \cdot R_{n} = R_{n} \cdot (1 + g)
  \label{eq:revenue_const}
\end{equation}

If the growth rate is constant when transitioning from one year to the next, then we can write the revenue for year $n$ as:

$$  R_{n} = R_{n-1} \cdot (1 + g) = R_{n-2} \cdot (1 + g)^2 = \dots = R_{0} \cdot (1 + g)^{n} $$

Thus we get:

\begin{equation}
  R_{n} = R_{0} \cdot (1 + g)^{n}
  \label{eq:revenue_exp}
\end{equation}

Since we are taking $(1 + g)$ to the power $n$ we say that $R_n$ is growing exponentially. We will come back to this later...

For now, let's get back to \eqref{eq:revenue_const}, since we can rewrite it to get $g$:

\begin{equation}
  R_{n+1} = R_{n} \cdot (1 + g) \Rightarrow g = \frac{R_{n+1}}{R_{n}} - 1 
  \label{eq:growth_ratio}
\end{equation}

Now we can calculate $g$ for each year (only shown up till 2009):

| Year | Revenue ($M) | $g\[\%\]$ | Year | Revenue ($M) | $g\[\%\]$ |
| ---- | ------------ | -----     | ---- | ------------ | -----     |
| 2000 | 100          |           | 2005 | 201          | 15%       |
| 2001 | 115          | 15%       | 2006 | 231          | 15%       |
| 2002 | 132          | 15%       | 2007 | 266          | 15%       |
| 2003 | 152          | 15%       | 2008 | 306          | 15%       |
| 2004 | 175          | 15%       | 2009 | 352          | 15%       |

Great! It seems like we are done because we have found a constant value of $g\[\%\]=15\%$ which perfectly matches the data. Since this value held in the past we can assume that it will hold in the future as well.

## Realistic case

However, this data set is a bit idealized... in real life the revenue might not grow at a constant rate from year to year. It might increase or decrease based on the economy, management, political situation and so on. To account for this we can define $g_n$ as the precent growth from year $n$ to year $n+1$. Then we can improve \eqref{eq:growth_ratio} to get:

\begin{equation}
  g_{\textcolor{red}{n}} = \frac{R_{n+1}}{R_{n}} - 1
  \label{eq:revenue_var}
\end{equation}

Thus now we can evaluate a more realistic company *B* that has the following revenue data:

| Year | Revenue ($M) | $g_n\[\%\]$ | Year | Revenue ($M) | $g_n\[\%\]$ | Year | Revenue ($M) | $g_n\[\%\]$ | Year | Revenue ($M) | $g_n\[\%\]$ |
| ---- | ------------ | -----   | ---- | ------------ | -----   | ---- | ------------ | -----   | ---- | ------------ | -----   |
| 2000 | 100          |         | 2005 | 207          | 20%     | 2010 | 397          | 16%     | 2015 | 723          | 12%     |
| 2001 | 115          | 15%     | 2006 | 197          | -5%     | 2011 | 464          | 17%     | 2016 | 846          | 17%     |
| 2002 | 143          | 24%     | 2007 | 234          | 19%     | 2012 | 553          | 19%     | 2017 | 1024         | 21%     |
| 2003 | 154          | 8%      | 2008 | 274          | 17%     | 2013 | 680          | 23%     | 2018 | 1137         | 11%     |
| 2004 | 172          | 12%     | 2009 | 342          | 25%     | 2014 | 646          | -5%     | 2019 | 1080         | -8%     |


<div id="real-plot">
</div>

<script type="text/javascript">
  /* Want to share data between different plots */
  pl_N = 20;
  pl_startX = 2000;
  pl_startY = 100;
  pl_it = Array.from({length: pl_N}, (_, i) => i);

  pl_growth = [15, 24,  8, 12, 20,  -5, 19, 17, 25, 16, 17, 19, 23, -5, 12, 17, 21, 11, -5];
  var pl_revenue = [pl_startY];
  pl_growth.forEach((x, i) => {
    pl_revenue[i+1] = pl_revenue[i] *(1 + pl_growth[i]/100);
  });
 
  deferPlotly(function () {
    plotName = document.getElementById('real-plot');

    var data = [{
      x: pl_it.map(x => pl_startX + x),
      y: pl_revenue,
      type: 'scatter'
    }];

    var layout = {
      title: 'Revenue for real <i>B</i> company',
      showlegend: false,
      xaxis: {
        title: 'Year',
        tickmargin: 10
      },
      yaxis: {
        title: 'Revenue ($M)',
        tickmargin: 10
      }
    };

    var config = getPlotlyConfig();
    config['staticPlot'] = true;
    config['scrollZoom'] = false;

    return Plotly.newPlot(plotName, data, layout, config);
  });
</script> 

On the plot you can see that last year company *B* took a loss of \\$60M which is almost it's total revenue in 2000.
With all this volatility we would still like to be able to determine how the company has done as a whole in the last 20 years and determine what is its likely future trajectory, thus we want a $g$ that is constant and best fits the data. One way to try to achieve this is called Compound Annual Growth Rate (CAGR).

If we used the formula from [Wikipedia](https://en.wikipedia.org/wiki/Compound_annual_growth_rate) we could write:

\begin{equation}
  g_{CAGR} = CAGR(2000, 2019) = \left(\frac{R_{0}}{R_{19}}\right)^{\frac{1}{2019-2000}} - 1
  \label{eq:cagr}
\end{equation}

Where $g_{CAGR}$ is the estimate of growth $g$ using CAGR, $R_{0}$ is the revenue in 2000 and $R_{19}$ is the revenue in 2019.

This gives us a $g_{CAGR}\[\%\] = 13\%$, but is it the best estimate? We can see on the revenue plot that company *B* took a large hit last year and CAGR is very sensitive to the most recent data. Maybe we can find an estimate that minimizes the amount of variation (noise) in the data? Yes we can, by using the least squares method.

The next problem we have is that least squares method tries to fit a straight line to the data, but our data bends upwards and is not a straight line! So what can we do? We can take advantage of the logarithm function!

## Logarithm crash course

Here is a crash course on the [$\log()$](https://en.wikipedia.org/wiki/Logarithm) function and in particular on the natural logarithm. The natural logarithm is the inverse of the exponential function. In other words:

\begin{equation}
  x = e^{\log(x)}
  \label{eq:exp}
\end{equation}

Where $e=2.71\dots$ is the [Euler number](https://en.wikipedia.org/wiki/E_(mathematical_constant)). 

Another way to say \eqref{eq:exp} is that if you take the logarithm of $x$, and raise $e$ to that value you get back $x$. Or if $y=\log(x)$ then $x=e^y$.

Using \eqref{eq:exp} we can write:

$$e^{\log(x\cdot y)} = x \cdot y = e^{\log(x)} \cdot e^{\log(y)} = e^{\log(x) + \log(y)} \Rightarrow e^{\log(x\cdot y)} = e^{\log(x) + \log(y)}$$

Thus, because the exponent is a one-to-one function we can write:

$$\log(x \cdot y) = \log(x) + \log(y)$$

This means that $\log(c \cdot x^n) = \log(c) + \log(x) \cdot n$, so if we had constant growth our equation \eqref{eq:revenue_const} would look like:

\begin{equation}
  R_{n} = R_{0} \cdot (1 + g)^{n} \Rightarrow \log(R_{n}) = \log(R_{0}) + \log(1 + g) \cdot n 
\end{equation}

We can define constants $a = \log(1 + g)$ and $b = \log(R_{0})$, thus the $\log()$ function has transformed our exponential euqation into a linear equation:

\begin{equation}
  \log(R_{n}) = b + a \cdot n = a \cdot n + b
  \label{eq:revenue_log}
\end{equation}

## Using least squares method

A quick recap of where we are:
1. We want to find a $g\%$ that best fits annual increase of revenue for company *B* to predict future revenue
2. We tried using CAGR to estimate $g$ but it only looks at the start and end points, which might not be representative
3. Since the year-to-year growth is proportional $\Rightarrow$ we get an exponential function (see \eqref{eq:revenue_exp})
4. We can use the logarithm function to turn the exponential function into a linear function (see \eqref{eq:revenue_log})

And now we will use the least squares method to find the best fit for the linear function.

Let's start with plotting the logarithm of the revenue for company *B* (you will get a similar picture if you plot the data in a [Logarithmic scale](https://en.wikipedia.org/wiki/Logarithmic_scale)):

<div id="log-plot">
</div>

<script type="text/javascript">

  deferPlotly(function () {
    plotName = document.getElementById('log-plot');

 
    var data = [{
      x: it.map(x => pl_startX + x),
      y: pl_revenue.map(x => Math.log(x)),
      type: 'scatter'
    }];

    var layout = {
      title: 'Revenue for real <i>B</i> company',
      showlegend: false,
      xaxis: {
        title: 'Year',
        tickmargin: 10
      },
      yaxis: {
        title: 'log(Revenue ($M))',
        tickmargin: 10
      }
    };

    var config = getPlotlyConfig();
    config['staticPlot'] = true;
    config['scrollZoom'] = false;

    return Plotly.newPlot(plotName, data, layout, config);
  });
</script> 

After taking the logarithm, the data does in fact look like it's on a line. That's just what we expected :).

So what does it even mean to "fit" a function to data? Well we want to choose a function from a class of functions that is "best" in some way. In our case, we want to fit a line so we want to fit a function of the form:

$$f(x) = a \cdot x + b$$

Where $a,b\in \mathbb{R}$ are real numbers.

We have our data points:

$$ y_i = \log(R_i)$$

For example $y_0 = \log(100) \approx 4.6$ and $y_1 = \log(115) \approx 4.7$.

For us $x_i$ will be the year, so $x_0 = 2000$ and $x_1 = 2001$. Now for each linear function (choice of $a$ and $b$) we can calculate the value of $f(x_i)$ for each $i$. Since we have two unknowns ($a$ and $b$) we can choose any two indexes (e.g. 0 and 1 or 0 and 19) and then solve for $a$ and $b$:

$$
\begin{cases}
  f(x_0) = a \cdot x_0 + b= y_0 \\
  f(x_1) = a \cdot x_1 + b= y_1
\end{cases}
$$

Since we know $x_0$, $x_1$, $y_0$, $y_1$ we can solve for $a$ and $b$ and get: $a=0.1$ and $b=-195.4$. However, since this looks only at two points it has the same problems as CAGR. In fact if we took $i=0$ and $i=19$ we would just get CAGR. So what else can we do?

For each point $i$ we can calculate the distance between the function estimate and the values of the points:

$$d(a,b,i) = \mid f(x_i) - y_i \mid = \mid a \cdot x_i + b - y_i \mid$$

As written the distance depends on $a$ and $b$. Since the distance is always $\geq 0$, we can sum all the distances and we also do some "magic" by squaring the distance before hand (hence "least squares"). Thus we get a "loss" function $L(a,b)$ defined as:

\begin{equation}
  L(a,b)= d(a,b, 0)^2 + d(a,b,1)^2 + \dots + d(a,b,19)^2 = \sum_{i=0}^{19} \mid a \cdot x_i + b - y_i \mid ^2
  \label{eq:loss_func_abs}
\end{equation}

Beyond the scope of this post, (see [Simple linear regression](https://en.wikipedia.org/wiki/Simple_linear_regression)) there are simple ways in using which we can exactly determine the values of $a$ and $b$ that minimize $L(a,b)$. Let's call these minimal values $\hat{a}$ and $\hat{b}$. We will now provide the explicit formulas for them.

If we define $\bar{x}$ to be the mean of $x_i$ and $\bar{y}$ to be the mean of $y_i$:

$$
  \begin{cases}
    \bar{x} = \dfrac{\sum_{i=0}^n x_i}{n} \\\\
    \bar{y} = \dfrac{\sum_{i=0}^n y_i}{n}
  \end{cases}
$$

Then we can write out the values of $\hat{a}$ and $\hat{b}$ as:

\begin{equation}
  \begin{cases}
    \hat{a} = \dfrac{\sum_{i=0}^{n}(x_i - \bar{x})(y_i - \bar{y})}{\sum_{i=0}^n(x_i - \bar{x})^2} \\\\\\
    \hat{b} = \bar{y} - \hat{a} \cdot \bar{x}
  \end{cases}
  \label{eq:least_squeares}
\end{equation}

For us $n=19$.

This procedure of obtaining them is called least squares method or linear regression and the equation \eqref{eq:least_squeares} is included in most statistical software packages. Here are examples of using it in [Excel](https://www.ablebits.com/office-addins-blog/linear-regression-analysis-excel/) or [Online](https://www.statskingdom.com/linear-regression-calculator.html).

We can now plot all of these functions on top of the data:

<div id="log-all-plot">
</div>

<script type="text/javascript">

  deferPlotly(function () {
    plotName = document.getElementById('log-all-plot');

    r = pl_revenue;
    CAGR = (r[pl_N-1]/r[0])**(1/(pl_N-1)) - 1;

    var data = [{
      x: it.map(x => pl_startX + x),
      y: pl_revenue.map(x => Math.log(x)),
      type: 'scatter',
      name: 'Data'
    }, {
      x: it.map(x => pl_startX + x),
      y: it.map(x => 0.1328 * (pl_startX + x) -260.9434),
      type: 'scatter',
      line: {
        width: 0.5
      },
      name: 'least squares fit'
    }, {
      x: it.map(x => pl_startX + x),
      y: it.map(x => Math.log((1 + CAGR)**(x)*pl_startY)),
      type: 'scatter',
      line: {
        width: 0.5
      },
      name: 'CAGR fit'
    }, {
      x: it.map(x => pl_startX + x),
      y: it.map(x => 0.1 * (pl_startX + x) - 195.4),
      type: 'scatter',
      line: {
        width: 0.5
      },
      name: 'i=0, i=1 fit'
    }];

    var layout = {
      title: 'Revenue for real <i>B</i> company',
      showlegend: true,
      xaxis: {
        title: 'Year',
        tickmargin: 10
      },
      yaxis: {
        title: 'log(Revenue ($M))',
        tickmargin: 10
      }
    };

    var config = getPlotlyConfig();
    config['staticPlot'] = false;
    config['scrollZoom'] = false;

    return Plotly.newPlot(plotName, data, layout, config);
  });
</script> 

You can see that least squares gave us the closest fit, then CAGR, and the fit based on $i=0$ and $i=1$ gave us the worst fit.

Now we exponentiate all the values to get the actual revenue:

<div id="all-plot">
</div>

<script type="text/javascript">

  pl_CAGR = (pl_revenue[pl_N-1]/pl_revenue[0])**(1/(pl_N-1)) - 1;

  deferPlotly(function () {
    plotName = document.getElementById('all-plot');

    var data = [{
      x: it.map(x => pl_startX + x),
      y: pl_revenue.map(x => x),
      type: 'scatter',
      name: 'Data'
    }, {
      x: it.map(x => pl_startX + x),
      y: it.map(x => Math.exp(0.1328 * (pl_startX + x) -260.9434)),
      type: 'scatter',
      line: {
        width: 0.5
      },
      name: `least squares fit`
    }, {
      x: it.map(x => pl_startX + x),
      y: it.map(x => (1 + pl_CAGR)**(x)*pl_startY),
      type: 'scatter',
      line: {
        width: 0.5
      },
      name: `CAGR fit`
    }, {
      x: it.map(x => pl_startX + x),
      y: it.map(x => Math.exp(0.1 * (pl_startX + x) - 195.4)),
      type: 'scatter',
      line: {
        width: 0.5
      },
      name: 'i=0, i=1 fit'
    }];

    var layout = {
      title: 'Revenue for real <i>B</i> company',
      showlegend: true,
      xaxis: {
        title: 'Year',
        tickmargin: 10
      },
      yaxis: {
        title: 'Revenue ($M)',
        tickmargin: 10
      }
    };

    var config = getPlotlyConfig();
    config['staticPlot'] = false;
    config['scrollZoom'] = false;

    return Plotly.newPlot(plotName, data, layout, config);
  });
</script> 

Now we can see how much the actual revenues differ based on the different fits.

The predicted values are for exponential functions in accordance with \eqref{eq:revenue_exp}. Thus when we create a model we define both $g$ and $R_0$ (revenue in 2000). Summarizing the fits in the table below we get:

| Name          | Symbol       | $g\[\%\]$  | $R_0$  | $R_{19}$ |
| ------------- | ------------ | ------ | ------ | -------- |
| least squares | $g_{LS}\%$   | 13.28% | 105.3  | 1312     |
| CAGR          | $g_{CAGR}\%$ | 13.34% | 100    | 1080     |
| i=0, i=1      | $g_{01}\%$   | 10.00% | 100    | 665      |

You might notice something very weird, namely that even though $g_{LS} < g_{CAGR}$ the predicted value of $R_{19}$ (revenue in 2019) for least squares is higher than for CAGR. Why is that? The reason for this is that for least squares the value $R_0$ is higher than actual ($105$ vs $100$). In a sense this can be expected, because the revenue value in year 2000 is also random and does not represent the performance of the company across the 20 years. In other words, we do not want the first or last year for which we analyze the data to be more important, but want to fit an "average" case.

If we were to give more importance to any data point we could end up with results that vary depending on the time frame we chose for generating the model. We can illustrate this point best by cherry-picking the dates for which we calculate CAGR. Here are some examples:
 
| Start year | End year | Years ($n$)   | $CAGR\[\%\]$ | $R_0$  | $R_{n}$  |
| ---------- | -------- | ------------- | -------- | ------ | -------- |
| 2000       | 2019     | 19            | 13.34%   | 100    | 1080     |
| 2013       | 2019     | 6             | 10.8%    | 680    | 1080     |
| 2006       | 2014     | 8             | 16.0%    | 197    | 646      |
 
You can see that depending on the dates chosen we can end up with a $\pm 3$ percentage points spread. This volatility of CAGR (and other methods that look only at 2 points) is the reason why we use least squares method. It is more independent of the time period we chose to generate predictions from.

The least squares is also useful when designing physics/engineering experiment. By designing the experiment to search for a value that changes lineraly with another you can collect a lot of data and perform a fit on the measurment you are intereseted in. This allows you to reduce measurement error. In fact if you get to choose how much data you get and if you know something about the noise in your system, then you can bound how much variation in your fit ($\hat{a}$ and $\hat{b}$) you will get. This is a very powerful tool.

## Conclusion

I hope you, dear reader, have enjoyed this post and have had some benefit from it.
