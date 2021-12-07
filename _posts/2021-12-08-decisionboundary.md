---
layout: single
title:  "TIL-2021-12-08-Decision boundary를 그려보자"
categories:

- TIL

tags: [python, visualization]

author_profile: true
typora-copy-images-to: ../images/2021-12-08
---

scikit learn과 matplot을 활용하여 decision boundary를 그려보았다.

- classification 모델이 학습되고 난 뒤 학습이 잘 이루어졌는지 직관적으로 확인하고 싶을 때가 있다.
- clf 예측값을 2차원 상의 공간(meshgrid)에 `plt.contourf` 함수로 시각화 하면 된다.
- 3차원 이상의 입력 데이터에 대한 decision boundary를 확인하고 싶으면 tsne를 통해 차원 축소를 한 뒤에 같은 과정을 해주면 된다.

# random 2D 데이터 생성


```python
import numpy as np
import matplotlib.pyplot as plt

# 샘플수 : 500
sample_size = 500
num_feats = 2

"""
다양한 평균,분산을 갖는 데이터 샘플링
"""
def normal_sampling(mu1, sd1, mu2, sd2, nrow):
    x1 = np.random.normal(mu1, sd1, nrow)
    x2 = np.random.normal(mu2, sd2, nrow)
    return np.vstack([x1,x2])

sample_size = 500
cluster_num = 3

# 각 row는 다른 평균,분산을 갖는 클러스터
X = np.hstack([
    normal_sampling(0, 1, 0, 1, sample_size),
    normal_sampling(2, 1, 2, 1, sample_size), 
    normal_sampling(3, 1, 7, 1, sample_size),
    normal_sampling(8, 1, 4, 1, sample_size),
    normal_sampling(6, 1, 5, 1, sample_size),
    normal_sampling(6, 3, 0, 1, sample_size),
    normal_sampling(0, 3, 6, 2, sample_size)
]).T # (500,2)

Y = []
for i in range(0, X.shape[0]//sample_size):
    Y+=[i for j in range(0, sample_size)]
Y = np.array(Y)
```

# MLPClassifier 학습


```python
from sklearn.neural_network import MLPClassifier
clf = MLPClassifier(hidden_layer_sizes=[50,50,10], activation='relu')
clf.fit(X, Y)
print("Score: {}".format(clf.score(X, Y)))
```

    Score: 0.8422857142857143


# Decision boundary 시각화


```python
num_grid = 500

u = np.linspace(X[:, 0].min(), X[:,0].max(), num_grid)
v = np.linspace(X[:, 1].min(), X[:,1].max(), num_grid)

# 입력 데이터에 대한 메쉬그리드 구함
uu,vv = np.meshgrid(u,v)

# 메쉬그리드 영역 전체에 대해 분류
z = clf.predict(np.hstack([ uu.reshape(-1, 1), vv.reshape(-1, 1) ])).reshape(num_grid, num_grid)

# contour plot 그리면 끝!
_, ax = plt.subplots(figsize=(15,6))
ax.scatter(X[:,0], X[:,1], c=Y, cmap=plt.cm.rainbow, alpha=0.3)
ax.contourf(uu, vv, z, cmap=plt.cm.rainbow, alpha=0.3)
```




    <matplotlib.contour.QuadContourSet at 0x7fdbdcc4c5f8>

![output_5_1](/images/2021-12-08/output_5_1.png)
