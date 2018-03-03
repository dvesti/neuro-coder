---
layout: single
title:  "I wrote a function to provide results for my PhD thesis"
date:   2017-03-01 16:16:01 +0100
categories: PhD
tags: R python javascript Ruby C# fun 
---

After struggling with my EEG analyses for quite some time, I got an idea to represent my efforts in multiple programming languages. This is to at lease showcase, that I learned something, if not analysing data properly.

```python
from enum import Enum
#got his function from supervisor, no idea what it does
from god import random_magic

class DataType(Enum):
    CRAP = 1
    WHAT = 2
    EFFORT = 3

class DataClass:
    def _init_(self, path):
        self.data = import_data(path)
        self.assign_type()

    def assign_type(self):
        self.type = random_magic(self.data) 

def provide_results(data):
    if(data.type == DataType.CRAP):
        print("You suck!")
    if(data.type == DataType.WHAT):
        print("If you were smart, this would make sense. It doesn't.")
    if(data.type == DataType.EFFORT):
        print("There, there ...")
    return None

## Load
path_to_data = "Hope is lost\Annonymised\that blond dude\answers.csv"
# I shoudl really stup using spaces in folder names
data = DataClass(path_to_data)

## Analyse
provide_results(data)
[1] "You suck"

### Make a chart
import matplotlib.pyplot as plt
from matplotlib.patches import Wedge
def make_phd_plot(data):
    # Ignoring data
    ignore_data(data)

    #plot results
    main = plt.Circle((0,0),5, fill=False)
    angle1, angle2 = 0, 180
    sub_result = Wedge((-2.75, 1.5), 1, 
        angle1, angle2, width = 0.1)
    sub_result2 = Wedge((2.75, 1.5), 1, 
        angle1, angle2, width = 0.1)

    reported_result = Wedge((0,-3), 2.5, 0, 180, color = 'black')
    reported_result2 = Wedge((0, -4), 2.7, 20, 160, color='white')

    main_application = Wedge((-7.5, 2.5), 5,
        angle1, angle2, color = 'blue', width = 0.2)
    main_application2 = Wedge((7.5, 2.5), 5, 
        angle1, angle2, color='blue', width = 0.2)

    fi, ax = plt.subplots()
    ax.set_xlim((-10, 10))
    ax.set_ylim((-10, 10))
    ax.add_artist(main)
    ax.add_artist(sub_result)
    ax.add_artist(sub_result2)
    ax.add_artist(reported_result)
    ax.add_artist(reported_result2)
    ax.add_artist(main_application)
    ax.add_artist(main_application2)

make_phd_plot(data)
```
![]({{site.baseurl}}/assets/img/2018/phd-results/cry-in-python.png)