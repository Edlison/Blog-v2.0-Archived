---
title: matplotlib的使用
catagories:
- Paper
tags:
- python
- matplotlib
---

# 子图

## subplot

```python
    for i in range(100):
        plt.subplot(10, 10, i + 1)
        plt.xticks([])
        plt.yticks([])
        plt.imshow(np.vstack((multiview_data[0][i], multiview_data[1][i])), cmap=plt.gray())
    plt.show()
```



## subplots

# 出现的BUG

```python
    for i in range(100):
        plt.subplot(10, 10, i + 1)
        plt.xticks([])
        plt.yticks([])
        if labels[i] == 1:  # 需要是(i-1)才能确定我们想要的位置
            print('highlight: ', i, end=' ')
            plt.title(labels[i], fontsize=3)
            plt.imshow(np.vstack((multiview_data[0][i], multiview_data[1][i])), cmap=plt.hot())  # 但是这里显示还是第i个，不知道是什么bug
        else:
            plt.imshow(np.vstack((multiview_data[0][i], multiview_data[1][i])), cmap=plt.gray())
    plt.tight_layout()
    plt.show()
```



