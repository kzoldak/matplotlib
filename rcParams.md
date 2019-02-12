# Matplotlib's rcParams

## Viewing rcParams
There are a few ways to change the matplotlib defaults for its various features. But first lets take a look at them.
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

## Default rcParams
`plt.rcParamsDefault` is also a dictionary object. 
Lets check and see if our default params match the current rcParams. The code below is from a clean ipython environment. We'd expect all the rcParams to be identical to the defaults, which they are except for a few. This may have to do with the spyder environment I'm testing this in.


```python
import matplotlib.pyplot as plt

# ARE THE CURRENT RCPARAMS THE DEFAULTS?
plt.rcParamsDefault == plt.rcParams
# RETURNS FALSE. LETS TAKE A LOOK AT THEM THEN.


p1 = dict(plt.rcParamsDefault)
p2 = dict(plt.rcParams)
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
        print('plt.rcParamsDefault: %r'%p1[key])
        print('plt.rcParams:        %r'%p2[key])
        print('')
        
        
'''
DIFFERENCES:
------------
key: interactive
plt.rcParamsDefault: False
plt.rcParams:        True

key: figure.figsize
plt.rcParamsDefault: [6.4, 4.8]
plt.rcParams:        [6.0, 4.0]

key: figure.dpi
plt.rcParamsDefault: 100.0
plt.rcParams:        72.0

key: figure.subplot.bottom
plt.rcParamsDefault: 0.11
plt.rcParams:        0.125
'''
```

## Changing rcParams
Now that we've viewed the rcParams, we want to change a few of them. Here are a few ways to change these parameters.
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
The `plt.rc(group, **kwargs)` command first takes the `group` name, which is `lines` in this example. Then it is followed up with several of the arguments that are associated with that group. You can use the alias for them as well, as shown above.


## Changing Style Sheets
[See Documentation](https://matplotlib.org/tutorials/introductory/customizing.html).



## Restoring to Default rcParams
```python
plt.rcdefaults()

?plt.rcdefaults

Signature: plt.rcdefaults()
Docstring:
Restore the rc params from Matplotlib's internal default style.

Style-blacklisted rc params (defined in
`matplotlib.style.core.STYLE_BLACKLIST`) are not updated.

See Also
--------
rc_file_defaults :
    Restore the rc params from the rc file originally loaded by Matplotlib.
matplotlib.style.use :
    Use a specific style file.  Call ``style.use('default')`` to restore
    the default style.
File:      ~/anaconda3/lib/python3.7/site-packages/matplotlib/pyplot.py
Type:      function


```



## Files
The file that holds all of the rcParam commands can be found at 
`~/anaconda3/lib/python3.7/site-packages/matplotlib/__init__.py`


The file that stores all the rcParams settings can be found here:
`/Users/kimzoldak/anaconda3/lib/python3.7/site-packages/matplotlib/rcsetup.py`
These can be changed within this file so that certain features are present every time you use matplotlib. 


The rcParams are read from the matplotlibrc file, which is found at:`~/anaconda3/lib/python3.7/site-packages/matplotlib/mpl-data/matplotlibrc`
This file is all commented out. It holds the ultimate defaults for the rcParams. 



Under `def rcdefaults():`
```python
        from .style.core import STYLE_BLACKLIST
        rcParams.clear()
        rcParams.update({k: v for k, v in rcParamsDefault.items()
                         if k not in STYLE_BLACKLIST})
```
