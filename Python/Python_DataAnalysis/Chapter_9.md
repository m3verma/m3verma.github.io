---
layout: default
---

# Plotting and Visualization

Making informative visualizations (sometimes called plots) is one of the most important tasks in data analysis. It may be a part of the exploratory process—for example, to help identify outliers or needed data transformations, or as a way of generating ideas for models. matplotlib is a desktop plotting package designed for creating (mostly two-dimensional) publication-quality plots. matplotlib supports various GUI backends on all operating systems and additionally can export visualizations to all of the common vector and raster graphics formats (PDF, SVG, JPG, PNG, BMP, GIF, etc.).

## A Brief matplotlib API Primer

With matplotlib, we use the following import convention :

```python
In [11]: import matplotlib.pyplot as plt
```

After running %matplotlib notebook in Jupyter (or simply %matplotlib in IPython), we can try creating a simple plot. If everything is set up right, a line plot appear :

```python
In [12]: import numpy as np
In [13]: data = np.arange(10)
In [15]: plt.plot(data)
```
![Plot_1](https://m3verma.github.io/Python/Python_DataAnalysis/Images/Chap9_1.PNG)

### Figures and Subplots

Plots in matplotlib reside within a Figure object. You can create a new figure with plt.figure :

```python
In [16]: fig = plt.figure()
```

In IPython, an empty plot window will appear, but in Jupyter nothing will be shown until we use a few more commands. plt.figure has a number of options; notably, figsize will guarantee the figure has a certain size and aspect ratio if saved to disk. You can’t make a plot with a blank figure. You have to create one or more subplots using add_subplot :

```python
In [17]: ax1 = fig.add_subplot(2, 2, 1)
In [18]: ax2 = fig.add_subplot(2, 2, 2)
In [19]: ax3 = fig.add_subplot(2, 2, 3)
```
![Plot_2](https://m3verma.github.io/Python/Python_DataAnalysis/Images/Chap9_2.PNG)

When you issue a plotting command like plt.plot([1.5, 3.5, -2, 1.6]), matplotlib draws on the last figure and subplot used (creating one if necessary), thus hiding the figure and subplot creation.

```python
In [20]: plt.plot(np.random.randn(50).cumsum(), 'k--')
```
![Plot_3](https://m3verma.github.io/Python/Python_DataAnalysis/Images/Chap9_3.PNG)

The 'k--' is a style option instructing matplotlib to plot a black dashed line. Creating a figure with a grid of subplots is a very common task, so matplotlib includes a convenience method, plt.subplots, that creates a new figure and returns a NumPy array containing the created subplot objects. This is very useful, as the axes array can be easily indexed like a two-dimensional array; for example, axes[0, 1]. You can also indicate that subplots should have the same x- or y-axis using sharex and sharey, respectively.

```python
In [24]: fig, axes = plt.subplots(2, 3)
```

pyplot.subplots optons :

| Argument        | Description          |
|:-------------|:------------------|
| nrows | Number of rows of subplots |
| ncols | Number of columns of subplots |
| sharex | All subplots should use the same x-axis ticks (adjusting the xlim will affect all subplots) |
| sharey | All subplots should use the same y-axis ticks (adjusting the ylim will affect all subplots) |
| subplot_kw | Dict of keywords passed to add_subplot call used to create each subplot |
| **fig_kw | Additional keywords to subplots are used when creating the figure, such as plt.subplots(2, 2, figsize=(8, 6)) |

By default matplotlib leaves a certain amount of padding around the outside of the subplots and spacing between subplots. This spacing is all specified relative to the height and width of the plot, so that if you resize the plot either programmatically or manually using the GUI window, the plot will dynamically adjust itself. You can change the spacing using the subplots_adjust method on Figure objects, also available as a top-level function :

```python
subplots_adjust(left=None, bottom=None, right=None, top=None,wspace=None, hspace=None)
```

### Colors, Markers, and Line Styles

Matplotlib’s main plot function accepts arrays of x and y coordinates and optionally a string abbreviation indicating color and line style. For example, to plot x versus y with green dashes, you would execute :

```python
ax.plot(x, y, 'g--')
```

This way of specifying both color and line style in a string is provided as a convenience; in practice if you were creating plots programmatically you might prefer not to have to munge strings together to create plots with the desired style. The same plot could also have been expressed more explicitly as :

```python
ax.plot(x, y, linestyle='--', color='g')
```
Line plots can additionally have markers to highlight the actual data points. Since matplotlib creates a continuous line plot, interpolating between points, it can occasionally be unclear where the points lie. The marker can be part of the style string, which must have color followed by marker type and line style :

```python
In [30]: from numpy.random import randn
In [31]: plt.plot(randn(30).cumsum(), 'ko--')
```
![Plot_4](https://m3verma.github.io/Python/Python_DataAnalysis/Images/Chap9_4.PNG)

This could also have been written more explicitly as :

```python
plot(randn(30).cumsum(), color='k', linestyle='dashed', marker='o')
```

### Ticks, Labels, and Legends

The pyplot interface, designed for interactive use, consists of methods like xlim, xticks, and xticklabels. These control the plot range, tick locations, and tick labels, respectively. They can be used in two ways :

1. Called with no arguments returns the current parameter value (e.g., plt.xlim() returns the current x-axis plotting range)
2. Called with parameters sets the parameter value (e.g., plt.xlim([0, 10]), sets the x-axis range to 0 to 10)

To illustrate customizing the axes, We’ll create a simple figure and plot of a random walk :

```python
In [37]: fig = plt.figure()
In [38]: ax = fig.add_subplot(1, 1, 1)
In [39]: ax.plot(np.random.randn(1000).cumsum())
```

To change the x-axis ticks, it’s easiest to use set_xticks and set_xticklabels. The former instructs matplotlib where to place the ticks along the data range; by default these locations will also be the labels. But we can set any other values as the labels using set_xticklabels. Lastly, set_xlabel gives a name to the x-axis and set_title the subplot title :

```python
In [40]: ticks = ax.set_xticks([0, 250, 500, 750, 1000])
In [41]: labels = ax.set_xticklabels(['one', 'two', 'three', 'four', 'five'],
   ....:                             rotation=30, fontsize='small')
In [42]: ax.set_title('My first matplotlib plot')
In [43]: ax.set_xlabel('Stages')
```

![Plot_5](https://m3verma.github.io/Python/Python_DataAnalysis/Images/Chap9_5.PNG)

Modifying the y-axis consists of the same process, substituting y for x in the above. Legends are another critical element for identifying plot elements. There are a couple of ways to add one. The easiest is to pass the label argument when adding each piece
of the plot. Once you’ve done this, you can either call ax.legend() or plt.legend() to automatically create a legend :

```python
In [44]: from numpy.random import randn
In [45]: fig = plt.figure(); ax = fig.add_subplot(1, 1, 1)
In [46]: ax.plot(randn(1000).cumsum(), 'k', label='one')
In [49]: ax.legend(loc='best')
```

The loc tells matplotlib where to place the plot. If you aren’t picky, 'best' is a good option, as it will choose a location that is most out of the way. To exclude one or more elements from the legend, pass no label or label='_nolegend_'.

### Annotations and Drawing on a Subplot

In addition to the standard plot types, you may wish to draw your own plot annotations, which could consist of text, arrows, or other shapes. You can add annotations and text using the text, arrow, and annotate functions. text draws text at given coordinates (x, y) on the plot with optional custom styling :

```python
ax.text(x, y, 'Hello world!', family='monospace', fontsize=10)
```

Drawing shapes requires some more care. matplotlib has objects that represent many common shapes, referred to as patches. Some of these, like Rectangle and Circle, are found in matplotlib.pyplot, but the full set is located in matplotlib.patches. To add a shape to a plot, you create the patch object shp and add it to a subplot by calling ax.add_patch(shp) :

```python
fig = plt.figure()
ax = fig.add_subplot(1, 1, 1)
rect = plt.Rectangle((0.2, 0.75), 0.4, 0.15, color='k', alpha=0.3)
circ = plt.Circle((0.7, 0.2), 0.15, color='b', alpha=0.3)
pgon = plt.Polygon([[0.15, 0.15], [0.35, 0.4], [0.2, 0.6]], color='g', alpha=0.5)
ax.add_patch(rect)
ax.add_patch(circ)
ax.add_patch(pgon)
```

![Plot_6](https://m3verma.github.io/Python/Python_DataAnalysis/Images/Chap9_6.PNG)

### Saving Plots to File

You can save the active figure to file using plt.savefig. This method is equivalent to the figure object’s savefig instance method. For example, to save an SVG version of a figure, you need only type :

```python
plt.savefig('figpath.svg')
```

To get the same plot as a PNG with minimal whitespace around the plot and at 400 DPI, you would do :

```python
plt.savefig('figpath.png', dpi=400, bbox_inches='tight')
```

## Plotting with pandas and seaborn

