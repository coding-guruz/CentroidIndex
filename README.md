# Centroid Index: Clustering evaluation Algorithm
Random swap is a kmeans variant that does not stuck in local optima and finds the optimal partitions by removing a random centroid and adding a random centroid. This strategy work pretty well. You just need to book keep the successful swaps where distortion value reduced further. 

## Example 1: Random swap relative SSE improvements over kmeans using random dataset 

```python

import numpy as np
from evaluation import CentroidIndex as ci
from sklearn.cluster import KMeans
import matplotlib.pyplot as plt

#random 2D data set
X=np.random.rand(1000,2)

# number of centroids
k=50

for i in range(5):
    km = KMeans(n_clusters=k, init='random').fit(X)
    kmp = KMeans(n_clusters=k).fit(X)
    
    
    # relative SSE improvement of kmeans++ over kmeans
    imp = 1 - kmp.inertia_/km.inertia_
    print(f"SSE improvement over k-means: {imp:.2%}")
    
    #CI: Number of mismatch cluster from both solutions(kmeans++, kmeans)
    CI = ci.CentroidIndex(km.labels_, kmp.labels_)
    print(f"Mismatch between k-means and k-means++: {CI}")
    
    #plotting the kmeans results
    for j in np.unique(km.labels_):
         plt.scatter(X[km.labels_ == j , 0] , X[km.labels_ == j , 1] , label = j)
    plt.scatter(km.cluster_centers_[:,0] , km.cluster_centers_[:,1] , s = 80, color = 'k')
    # displaying the title
    plt.title("k-means results of iteration: "+str(i))
    plt.show()
    
    #plotting the random swap results
    for j in np.unique(kmp.labels_):
         plt.scatter(X[kmp.labels_ == j , 0] , X[kmp.labels_ == j , 1] , label = j)
    plt.scatter(kmp.cluster_centers_[:,0] , kmp.cluster_centers_[:,1] , s = 80, color = 'k')
    # displaying the title
    plt.title("k-means++ results of iteration: "+str(i))
    plt.show()
```
## Output
```bash
SSE improvement over k-means: 2.96%
Mismatch between k-means and k-means++: 8
SSE improvement over k-means: 4.65%
Mismatch between k-means and k-means++: 7
SSE improvement over k-means: 0.10%
Mismatch between k-means and k-means++: 4
SSE improvement over k-means: 3.61%
Mismatch between k-means and k-means++: 8
SSE improvement over k-means: 3.09%
Mismatch between k-means and k-means++: 8
```
## Visuals
!["Kmeans results on iteration: 0"](https://github.com/gulraizchoudhary/Random-Swap-Clustering-Algorithm/blob/main/img/km_0.png)
!["Random Swap results on iteration: 0"](https://github.com/gulraizchoudhary/Random-Swap-Clustering-Algorithm/blob/main/img/rs_0.png)

!["Kmeans results on iteration: 1"](https://github.com/gulraizchoudhary/Random-Swap-Clustering-Algorithm/blob/main/img/km_1.png)
!["Random Swap results on iteration: 1"](https://github.com/gulraizchoudhary/Random-Swap-Clustering-Algorithm/blob/main/img/rs_1.png)

!["Kmeans results on iteration: 2"](https://github.com/gulraizchoudhary/Random-Swap-Clustering-Algorithm/blob/main/img/km_2.png)
!["Random Swap results on iteration: 2"](https://github.com/gulraizchoudhary/Random-Swap-Clustering-Algorithm/blob/main/img/rs_2.png)

!["Kmeans results on iteration: 3"](https://github.com/gulraizchoudhary/Random-Swap-Clustering-Algorithm/blob/main/img/km_3.png)
!["Random Swap results on iteration: 3"](https://github.com/gulraizchoudhary/Random-Swap-Clustering-Algorithm/blob/main/img/rs_3.png)

!["Kmeans results on iteration: 4"](https://github.com/gulraizchoudhary/Random-Swap-Clustering-Algorithm/blob/main/img/km_4.png)
!["Random Swap results on iteration: 4"](https://github.com/gulraizchoudhary/Random-Swap-Clustering-Algorithm/blob/main/img/rs_4.png)
## Example 2: Random swap relative SSE improvements over kmeans++ using random dataset
```python

from rs import rs
import numpy as np
from sklearn.cluster import KMeans

#random 2D data set
X=np.random.rand(1000,2)

# number of centroids
k=100
swaps = 20000

for i in range(5):
    randomSwap = rs(n_clusters=k).fit(X, swaps)

    km = KMeans(n_clusters=k, init='random').fit(X)
    
    # relative SSE improvement of random swap over kmeans
    imp = 1 - randomSwap.inertia_/km.inertia_
    print(f"SSE improvement over k-means: {imp:.2%}")
```
## Output
```bash
SSE improvement over k-means++: 1.69%
SSE improvement over k-means++: -1.98%
SSE improvement over k-means++: 0.42%
SSE improvement over k-means++: 1.18%
SSE improvement over k-means++: -1.18%
```


## Acknowledgements
Credit goes to Pasi Fränt, You may consider to read his paper for more understanding "Efciency of random swap clustering", also credit to the scikit-learn team for their excellent sklearn.cluster.KMeans class.

## License
[MIT](https://choosealicense.com/licenses/mit/)