---
title: K-means与KNN
date: 2023-02-17 00:02:02
tags: 
  - gradient descent
  - kmeans
  - knn
  - machine learning
---

## K-means


```python
import numpy as np
import matplotlib.pyplot as plt

def kmeans(x, k, norm=2):
    # n samples
    n = x.shape[0]
    if n < k:
        raise "len(x) < k"
    y = np.ones(n) * -1
    centroid_idx = np.linspace(0, n - 1, int(n / (n / k)), dtype=int)
    centroid = x[centroid_idx, :]

    updated = -1
    distance = np.zeros(k)
    epoch = 0
    while updated != 0 and epoch < 100:
        updated = 0
        samples = np.zeros(k)
        centroid_new = centroid.copy() * 0
        for i in range(n):
            for j in range(k):
                distance[j] = np.linalg.norm([x[i] - centroid[j]], norm)
                
            cluster_id = np.argmin(distance)
            if y[i] != cluster_id:
                updated += 1
#                 print(f'epoch={epoch} y[{i}]={y[i]} => {cluster_id} d={distance}')

            y[i] = cluster_id
            centroid_new[cluster_id] = centroid_new[cluster_id] + x[i]
            samples[cluster_id] += 1

        # update centroid
        for j in range(k):
            centroid[j] = centroid_new[j] / samples[j]

        epoch += 1
    return centroid, y
    
k = 4
n = 30
sample = np.random.rand(n, 2)
centroid, y = kmeans(sample, k, 2)

plt.title('origin')
plt.scatter(sample[:,0], sample[:,1])
plt.show()

plt.title('classified')
for i in range(k):
    mask = y[:] == i
    classified = sample[mask,:]
    plt.scatter(classified[:,0], classified[:,1], label=f'class {i}')

for i in range(k):
    plt.scatter(centroid[i, 0], centroid[i, 1], label=f'centroid {i}')

plt.legend()
plt.show()



```


    
![png](/img/kmeans_files/kmeans_2_0.png)
    



    
![png](/img/kmeans_files/kmeans_2_1.png)
    


## KNN


```python
import math
import heapq

def weight_inverse(w):
    return 1 / w

def weight_one(w):
    return 1;

def weight_gaussian(dist, a=1, b=0, c=0.3):
    return a * math.e ** (-(dist - b) ** 2 / (2 * c ** 2))

def knn(x, dataset, labels, k, norm=2, weight_fn=weight_gaussian):
    # n samples
    n = dataset.shape[0]
    # distance
    d = []
    for i in range(n):
        heapq.heappush(d, (np.linalg.norm([x - dataset[i]], norm), i))
        
    topk = np.array([np.asarray(heapq.heappop(d)) for i in range(k)])
    summation = {}
    for item in topk:
        cls = y[int(item[1])]
        if not cls in summation:
            summation[cls] = 0
        summation[cls] += weight_fn(item[0])

    cls = None
    score = 0
    for item in summation:
        if summation[item] > score:
            cls = item
            score = summation[item]
#     print(f'topk: {summation} class: {cls} score: {score}')
    return cls

x = np.random.rand(2)
cls_wo = knn(x, sample, y, k, weight_fn=weight_one)
cls_wo = int(cls_wo)

cls_wi = knn(x, sample, y, k, weight_fn=weight_inverse)
cls_wi = int(cls_wi)

cls_wg = knn(x, sample, y, k, weight_fn=weight_gaussian)
cls_wg = int(cls_wg)

plt.title('KNN')
for i in range(k):
    mask = y[:] == i
    classified = sample[mask,:]
    plt.scatter(classified[:,0], classified[:,1], label=f'class {i}')

plt.scatter(x[0], x[1], label=f'input')
plt.annotate(f'cls o: {cls_wo}\ncls_i: {cls_wi}\ncls_g: {cls_wg}', (x[0], x[1]))
plt.legend()
plt.show()

```


    
![png](/img/kmeans_files/kmeans_4_0.png)
    

