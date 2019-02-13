# Matplotlib's rcParams


## Viewing rcParams
Use `plt.rcParams` to view the rcParams set within your currently active matplotlib environment. If you havent changed any of these (will show you how to change time in a moment), then they should *mostly* match the defaults. If you change any of these parameters within your active python session, they will be cleared when you close and the defaults will be launched at every new python/matplotlib session. 

```python
import matplotlib.pyplot as plt


plt.rcParams  # set up like a dictionary 

# LETS STORE IT FOR EASY ACCESS, IT HAS SAME STRUCTURE AS A DICTIONARY
params = dict(plt.rcParams)

# GET A LIST OF THE KEYS ONLY
params.keys()

# GET A LIST OF THE VALUES ONLY
params.values()

# OR TO VIEW BOTH 
params
# or simply
plt.rcParams
```
These rcParams can be changed, but we will discuss viewing the rcParam defaults first.


## Default rcParams
Using `plt.rcParamsDefault` allows you to view the default params. 
`plt.rcParamsDefault` is also a dictionary object, just like `plt.rcParams`.
Lets check and see if our default params match our current session's rcParams at the beginning of a new python/matplotlib session. 
```python
In [1]: import matplotlib.pyplot as plt

In [2]: p1 = plt.rcParams 

In [3]: p1 = dict(plt.rcParams)

In [4]: p2 = dict(plt.rcParamsDefault)

In [5]: p1 == p2
Out[5]: True
```
This was done in ipython. If we do this same thing in Spyder, we get False. Spyder was designed to change some of these parameters at launch. 

SPYDER:
```python
import matplotlib.pyplot as plt
p1 = dict(plt.rcParams)
p2 = dict(plt.rcParamsDefault)

p1 == p2  # False
len(p1) == len(p2) # True
p1.keys() == p2.keys() # True

# keys in both are same.
# so then the values are different.
# which ones are different?

# since they have the same keys, only need
# one key identifier
for key in p1.keys():
    # if values don't match, print key
    if p1[key] != p2[key]:
        print('key: %s'%key)
        print('plt.rcParams:          %r'%p1[key])
        print('plt.rcParamsDefault:   %r'%p2[key])
        print('')
        
        
'''
DIFFERENCES:
------------
key: interactive
plt.rcParams:          True
plt.rcParamsDefault:   False

key: figure.figsize
plt.rcParams:          [6.0, 4.0]
plt.rcParamsDefault:   [6.4, 4.8]

key: figure.dpi
plt.rcParams:          72.0
plt.rcParamsDefault:   100.0

key: figure.subplot.bottom
plt.rcParams:          0.125
plt.rcParamsDefault:   0.11
'''
```
We found the same is true within Jupyter Notebook. If we run the same code as above in Jupyter, we find the following rcParams differed from their defaults:
```
interactive
figure.figsize
figure.dpi
figure.facecolor
figure.edgecolor
figure.subplot.bottom
```
So your IDE can change these rcParams when they launch. 


## Changing rcParams
Now that we've viewed the rcParams, we want to change a few of them. Here are a few ways to do this:
```python
plt.rcParams['lines.linewidth'] = 3.0
plt.rcParams['lines.linestyle'] = '--'
plt.rcParams['lines.color'] = 'green'

# OR THE EQUIVALENT
plt.rc('lines', linewidth=3, linestyle='--', color='green')
# OR
plt.rc('lines', lw=3, ls='--', c='green')  # with aliases

# USING A DICTIONARY -- YOU CAN USE EITHER VERSION OF pars2chng
pars2chng = {'linewidth' : 5,
             'linestyle' : '-.',
             'color'   : 'orange'}

pars2chng = dict(linewidth = 5,
                 linestyle = '-.',
                 color   = 'orange')

plt.rc('lines', **pars2chng )  # pass in the font dict as kwargs
```
The command is `plt.rc(group, **kwargs)`, which first takes the `group` name (in our example that would be `lines`) followed by a few of the group's attributes (in our example those are `linewidth`, `linestyle`, and `color`) in argument-assignment form. You can use the plotting alias for those that have them, as shown in the code above.

## Changing rcParams in the setup file
The defaults launched with every new matplotlib session are stored in the `~/anaconda3/lib/python3.7/site-packages/matplotlib/rcsetup.py` file. 
These can be edited within the file if you wish to have certain style features become defaults for every new matplotlib session. For example, I hate the new style of having the x- and y-ticks being located inside of the plot. To make this a permanent change, I can update both the 'xtick.direction' and 'ytick.direction' to be 'in' instead of 'out'. 


## Changing Style Sheets
If you wish to make a lot of changes, you can make your own style sheet of rcParams and store in in the following directory: `/Users/kimzoldak/anaconda3/lib/python3.7/site-packages/matplotlib/mpl-data/stylelib/`. You can view some of these to get an idea of how it is done. You should also see the documentation [here](https://matplotlib.org/tutorials/introductory/customizing.html) on this topic before doing this. 


## Restoring to Default rcParams
Using `plt.rcdefaults()` will restore all the rcParams back to their default values, as seen in the `~/anaconda3/lib/python3.7/site-packages/matplotlib/rcsetup.py` file. Any edits you've made to this file will be preserved, no worries!

Again, using `plt.rcParamsDefault` allows you to view the default params. Using `plt.rcdefaults()` will restore them within your current python/matplotlib session.




## Files
- The file that holds all of the rcParam commands can be found at 
`~/anaconda3/lib/python3.7/site-packages/matplotlib/__init__.py`


- The file that stores all the rcParams settings can be found here:
`~/anaconda3/lib/python3.7/site-packages/matplotlib/rcsetup.py`
These can be changed within this file so that certain features are present every time you use matplotlib. 


- The rcParams are read from the matplotlibrc file, which is found at:`~/anaconda3/lib/python3.7/site-packages/matplotlib/mpl-data/matplotlibrc`
This file is all commented out. It holds the ultimate defaults for the rcParams. 

- Blacklisting params so that restoring params to defaults will not change certain params within the `rcsetup.py` file.  `~/anaconda3/lib/python3.7/site-packages/matplotlib/style/core.py`.  
Within this file: 
```
# A list of rcParams that should not be applied from styles
STYLE_BLACKLIST = {
    'interactive', 'backend', 'backend.qt4', 'webagg.port', 'webagg.address',
    'webagg.port_retries', 'webagg.open_in_browser', 'backend_fallback',
    'toolbar', 'timezone', 'datapath', 'figure.max_open_warning',
    'savefig.directory', 'tk.window_focus', 'docstring.hardcopy'}
```

