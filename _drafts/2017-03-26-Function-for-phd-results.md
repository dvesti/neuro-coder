---
layout: single
title:  "I wrote a function to provide results for my PhD thesis"
date:   2017-03-01 16:16:01 +0100
categories: PhD
tags: R python javascript Ruby C# fun 
---

After struggling with my EEG analyses for quite some time, I got an idea to represent my efforts in multiple programming languages. This is to at lease showcase, that I learned something, if not analysing data properly.

```r

```


```python
from enum import Enum
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
    if(data.type == DataType.CRAP) print("You suck")
    if(data.type == DataType.WHAT) print("If you were inteligent, this would make sense. But it doesn't.")
    if(data.type == DataType.EFFORT) print("There there")
    return None

data = DataClass('path to data')
provide_results(data)
```

```Matlab

```