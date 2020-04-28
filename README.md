# Python Cheat Sheet

## Packages

### Installing a package
```
$ pip3 install numpy
```

### Importing a package
```
import numpy
import numpy as np
```

### Importing a function from a package

Not recommended because it's less clear where the function comes from. Better to import the entire package.

```
from numpy import array
from numpy import array as my_array 
```

## Arrays

### Subsetting using numpy
```
>>> import numpy as np
>>> x = [5, 10, 15]
>>> x = np.array([5, 10, 15])
>>> x > 8
array([False,  True,  True])
>>> x[x > 8]
array([10, 15])
```
