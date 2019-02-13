# Matplotlib Plots (plt.) verses Subplots (ax.) commands.
I often find myself tirelessly searching for comparable commands when I want to switch between matplotlib.pyplot plotting commands and matplotlib.pyplot.subplots commands. 

Using python:

```python

import matplotlib.pyplot as plt

# IN EACH OF THESE EXAMPLES, THE SAME COMMAND WORKS TO 
# SAVE THE FIGURE:  plt.savefig('figurename.png')
# SAME GOES FOR     plt.show()


# EXAMPLE 1 - subplots
fig, ax = plt.subplots(figsize=(11,5))
ax.plot(x, y, color='red', 
        linestyle='--', lw=2, alpha=0.6)
ax.text(0.2, 0.95, 
        'mean temp: %i$\degree$F'%y.mean(),
        fontsize = 18,
        horizontalalignment='center',
        verticalalignment='center', 
        transform=ax.transAxes)
ax.tick_params(axis='x', labelrotation=10)
ax.set_xlim(x.min(), x.max())
ax.set_ylim(20, 75)
#fig.savefig("figure.png")
plt.show()



# EXAMPLE 2 - traditional single plot
plt.figure(figsize=(11,5))
plt.plot(x, y, color='red', 
         linestyle='--', lw=2, alpha=0.6)
# NOTICE figure locations changed from 0.2, 0.95 to 0.3, 0.85
# these are figure locations where EXAMPLE 1 was axes locations. 
# I had to update these to get the text inside the plot!!
plt.figtext(0.3, 0.85, 
            'mean temp: %i$\degree$F'%y.mean(),
            fontsize = 18,
            horizontalalignment='center',
            verticalalignment='center')
plt.xticks(rotation=10)  # xticks identifies x axis.
plt.xlim(x.min(), x.max())
plt.ylim(20, 75)
plt.show()



# EXAMPLE 3
# set fig = to plot and use it to transform plt.text to the figure.
fig = plt.figure(figsize=(11,5))
plt.plot(x, y, color='red', linestyle='--', 
         lw=2, alpha=0.6)
# If you don't use transform=fig.transFigure, the plt.text command
# will plot text according to the x- and y-axes coordinates. 
plt.text(0.3, 0.85,
        'mean temp: %i$\degree$F'%y.mean(),
        fontsize = 18,
        horizontalalignment='center',
        verticalalignment='center', 
        transform=fig.transFigure) # transform plt.text to the figure.
plt.xticks(rotation=10)  # xticks identifies x axis.
plt.xlim(x.min(), x.max())
plt.ylim(20, 75)
plt.show()



# EXAMPLE 4
# set fig = plt.figure to use fig.text 
fig = plt.figure(figsize=(11,5))
plt.plot(x, y, color='red', linestyle='--', 
         lw=2, alpha=0.6)
# If using fig.text, don't need transform to fig.
fig.text(0.3, 0.85,
        'mean temp: %i$\degree$F'%y.mean(),
        fontsize = 18,
        horizontalalignment='center',
        verticalalignment='center') 
        #transform=fig.transFigure)    # don't need anymore
plt.xticks(rotation=10)  # xticks identifies x axis.
plt.xlim(x.min(), x.max())
plt.ylim(20, 75)
plt.show()
```
In the examples above, I first show the process of using subplots. However, the plot I am wanting is a single plot. Using subplots still works. It's probably a good idea to use subplots often as to become more comfortable with the different plotting commands. If no subplot layout is specified in `fig, ax = plt.subplots()`, then only a single axes `ax` container is made. If multiple plots are specified, such as `fig, ax = plt.subplots(3,2)`, then three rows and 2 columns of plots are made and each have their own axes. Thus, the `ax` container is an numpy ndarray of shape (3, 2), having 6 individual axes in the following layout:
``` 
ax[0,0]  ax[0,1]
ax[1,0]  ax[1,1]
ax[2,0]  ax[2,1]
```
With subplots, the plotting commands would look something like this:
```python
ax[0,1].plot(x, y, color='red', 
        linestyle='--', lw=2, alpha=0.6)

ax[2,1].plot(x, y, color='red', 
        linestyle='--', lw=2, alpha=0.6)
```
if you were adding plots to the top right and bottom right subplots in the layout. 


From EXAMPLE 1 to EXAMPLES 2-4, notice the commands now start with `plt` instead of `ax`. These are the traditional `matplotlib.pyplot` commands and the `ax` plotting was built off of these. 
Unfortunately not EVERY command is exactly the same. Some of the differences between them are subtle, while others are more severe. For example, the basic plot command is identical:
```python
plt.plot(x,y)
ax.plot(x,y)
```
setting axes limits are fairly subtle:
```python
plt.xlim(xmin,xmax)
ax.set_xlim(xmin,xmax)
```
but adjusting the xtick parameters is different enough to cause a headache:
```python
plt.xticks(rotation=10)
ax.tick_params(axis='x', labelrotation=10)
```
Although something I didn't show so far is that there is actually an identical `plt` command -- `plt.tick_params(axis='x', labelrotation=10)`, however most basic tutorials don't use it and opt for the `plt.xticks` command instead. **It is important to know that the plotting commands that WE FIND to be significantly differnet between `ax` and `plt` are due to our use of matplotlib `plt` shortcuts! The same shortcuts not offered under subplots. So becomming familiar with all of matplotlib's `plt` commands is important.**


**And last but not least (actually the most important thing you should take away from this) is the `ax.set` property batch setter.**
Using `ax.set` can save some time. 
Here is an example of how to use it:
```python
fig, ax = plt.subplots(figsize=(11,5))
ax.plot(x, y, color='red', 
        linestyle='--', lw=3, alpha=0.6)
ax.set(xlim=(x.min(), x.max()),
       ylim=(20,75),
       xlabel='x',
       ylabel='y',
       title='title',
       facecolor='black', # only works if frame_on = True
       frame_on = False)
plt.show()
```
If you want to know what all works with `ax.set`, hit `ax.set_` and then the tab key. You can try all options listed there and see if all of them work.  

<br></br>
<br></br>
<br></br>

I a providing a table containing the comparisons of some of the more popular plotting commands (or the ones I can think of). I will try and stick to the x-axis commands, but there will be identical y-axis commands for most. I will try and update these tables in time.

#### The following table contains equivalent plotting commands between single plots and subplots.
<table>
  <thead>
    <tr>
      <th>matplotlib.pyplot</th>
      <th>matplotlib.pyplot.subplot</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>plt.figure()</td>
      <td>fig,ax = plt.subplots()</td>
    </tr>
    <tr>
      <td>plt.plot</td>
      <td>ax.plot</td>
    </tr>
    <tr>
      <td>plt.xlim</td>
      <td>ax.set_xlim</td>
    </tr>
    <tr>
      <td>plt.xlabel</td>
      <td>ax.set_xlabel</td>
    </tr>
    <tr>
      <td>plt.tick_params(axis='x', labelrotation=10)</td>
      <td>ax.tick_params(axis='x', labelrotation=10)</td>
    </tr>
    <tr>
      <td>plt.xticks(rotation=10)</td>
      <td>ax.tick_params(axis='x', labelrotation=10)</td>
    </tr>
  </tbody>
</table>


<br></br>
<br></br>
<br></br>


#### The following table contains commands that ARE NOT equivalent to eachother and can easily confuse the user. I have included an asterisk (\*) at the beginning of the command that will fail. 
<table>
  <thead>
    <tr>
      <th>matplotlib.pyplot</th>
      <th>matplotlib.pyplot.subplot</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>plt.xticks(rotation=10)</td>
      <td>*ax.set_xticks(rotation=10)</td>
    </tr>
  </tbody>
</table>


