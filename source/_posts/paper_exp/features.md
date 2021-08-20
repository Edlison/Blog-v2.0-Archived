---
title: 特征工程
categories:
- Paper
tags:
- features
---

# 特征工程

## SIFT

尺度不变特征转换(Scale-invariant feature transform).

```python
# @Author  : Edlison
# @Date    : 5/30/21 23:26

import cv2
import numpy as np
from matplotlib import pyplot as plt

# Load
imgname = 'Image.jpeg'
sift = cv2.SIFT_create()
img = cv2.imread(imgname)

# Calculate
gray = cv2.cvtColor(img.numpy(), cv2.COLOR_BGR2GRAY)
kp, des = sift.detectAndCompute(img, None)

# Draw sift
img1 = cv2.drawKeypoints(gray, kp, img, color=(255, 0, 0))

plt.imshow(img1)
plt.title('after')
plt.show()
```

调用cv2中的SIFT算法。

1. 读取图片，cv读取通道默认是BGR，需要手动转换为RGB或其他。
2. 计算keypoints
3. 在原图上画出keypoints，可以选择keypoint的color或者flag。



## LBP

局部二值模型（Local Binary Pattern）.

是一种用来描述图像局部纹理特征的算子，它具有**旋转不变性**和**灰度不变性**的特点。

```python
import numpy as np
import cv2
import torch
from PIL import Image
from pylab import*


class LBP:
    def __init__(self):
        super()
        
    #将图像载入，并转化为灰度图，获取图像灰度图的像素信息
    def describe(self,image):
        image_array=np.array(Image.open(image).convert('L'))
        return image_array
    
    #图像的LBP原始特征计算算法：将图像指定位置的像素与周围8个像素比较
    #比中心像素大的点赋值为1，比中心像素小的赋值为0，返回得到的二进制序列
    def calute_basic_lbp(self,image_array,i,j):
        sum=[]
        if image_array[i-1,j-1]>image_array[i,j]:
            sum.append(1)
        else:
            sum.append(0)
        if image_array[i-1,j]>image_array[i,j]:
            sum.append(1)
        else:
            sum.append(0)
        if image_array[i-1,j+1]>image_array[i,j]:
            sum.append(1)
        else:
            sum.append(0)
        if image_array[i,j-1]>image_array[i,j]:
            sum.append(1)
        else:
            sum.append(0)
        if image_array[i,j+1]>image_array[i,j]:
            sum.append(1)
        else:
            sum.append(0)
        if image_array[i+1,j-1]>image_array[i,j]:
            sum.append(1)
        else:
            sum.append(0)
        if image_array[i+1,j]>image_array[i,j]:
            sum.append(1)
        else:
            sum.append(0)
        if image_array[i+1,j+1]>image_array[i,j]:
            sum.append(1)
        else:
            sum.append(0)
        return sum
    
    #获取二进制序列进行不断环形旋转得到新的二进制序列的最小十进制值
    def get_min_for_revolve(self,arr): 
        values=[]
        circle=arr
        circle.extend(arr)
        for i in range(0,8):
            j=0
            sum=0
            bit_num=0
            while j<8:
                sum+=circle[i+j]<<bit_num
                bit_num+=1
                j+=1
            values.append(sum)
        return min(values)

    #获取值r的二进制中1的位数
    def calc_sum(self,r):
        num=0
        while(r):
            r&=(r-1)
            num+=1
        return num

    #获取图像的LBP原始模式特征
    def lbp_basic(self,image_array):
        basic_array=np.zeros(image_array.shape, np.uint8)
        width=image_array.shape[0]
        height=image_array.shape[1]
        for i in range(1,width-1):
            for j in range(1,height-1):
                sum=self.calute_basic_lbp(image_array,i,j)
                bit_num=0
                result=0
                for s in sum:
                    result+=s<<bit_num
                    bit_num+=1
                basic_array[i,j]=result
        return basic_array

    #显示图像
    def show_image(self,image_array):
        plt.imshow(image_array)
        plt.show()


if __name__ == '__main__':
    image = '../Image.jpeg'
    lbp=LBP()
    img=lbp.describe(image)
    #获取图像原始LBP特征，并显示其统计直方图与特征图像
    basic_array=lbp.lbp_basic(img)
    lbp.show_image(basic_array)

```



## GLCM

灰度共生矩阵（Gray-Level Co-occurrence Matrix）,

一种描述图像局部区域或整体区域的某像素与相邻像素或一定距离内的像素的灰度关系的矩阵。

```python
# coding: utf-8

import numpy as np
import matplotlib.pyplot as plt
import cv2
import torch
from skimage import data


def main():
    pass


def fast_glcm(img, vmin=0, vmax=255, nbit=8, kernel_size=5):
    mi, ma = vmin, vmax
    ks = kernel_size
    h,w = img.shape

    # digitize
    bins = np.linspace(mi, ma+1, nbit+1)
    gl1 = np.digitize(img, bins) - 1
    gl2 = np.append(gl1[:,1:], gl1[:,-1:], axis=1)

    # make glcm
    glcm = np.zeros((nbit, nbit, h, w), dtype=np.uint8)
    for i in range(nbit):
        for j in range(nbit):
            mask = ((gl1==i) & (gl2==j))
            glcm[i,j, mask] = 1

    kernel = np.ones((ks, ks), dtype=np.uint8)
    for i in range(nbit):
        for j in range(nbit):
            glcm[i,j] = cv2.filter2D(glcm[i,j], -1, kernel)

    glcm = glcm.astype(np.float32)
    return glcm


def fast_glcm_mean(img, vmin=0, vmax=255, nbit=8, ks=5):
    '''
    calc glcm mean
    '''
    h,w = img.shape
    glcm = fast_glcm(img, vmin, vmax, nbit, ks)
    mean = np.zeros((h,w), dtype=np.float32)
    for i in range(nbit):
        for j in range(nbit):
            mean += glcm[i,j] * i / (nbit)**2

    return mean


def fast_glcm_std(img, vmin=0, vmax=255, nbit=8, ks=5):
    '''
    calc glcm std
    '''
    h,w = img.shape
    glcm = fast_glcm(img, vmin, vmax, nbit, ks)
    mean = np.zeros((h,w), dtype=np.float32)
    for i in range(nbit):
        for j in range(nbit):
            mean += glcm[i,j] * i / (nbit)**2

    std2 = np.zeros((h,w), dtype=np.float32)
    for i in range(nbit):
        for j in range(nbit):
            std2 += (glcm[i,j] * i - mean)**2

    std = np.sqrt(std2)
    return std


def fast_glcm_contrast(img, vmin=0, vmax=255, nbit=8, ks=5):
    '''
    calc glcm contrast
    '''
    h,w = img.shape
    glcm = fast_glcm(img, vmin, vmax, nbit, ks)
    cont = np.zeros((h,w), dtype=np.float32)
    for i in range(nbit):
        for j in range(nbit):
            cont += glcm[i,j] * (i-j)**2

    return cont


def fast_glcm_dissimilarity(img, vmin=0, vmax=255, nbit=8, ks=5):
    '''
    calc glcm dissimilarity
    '''
    h,w = img.shape
    glcm = fast_glcm(img, vmin, vmax, nbit, ks)
    diss = np.zeros((h,w), dtype=np.float32)
    for i in range(nbit):
        for j in range(nbit):
            diss += glcm[i,j] * np.abs(i-j)

    return diss


def fast_glcm_homogeneity(img, vmin=0, vmax=255, nbit=8, ks=5):
    '''
    calc glcm homogeneity
    '''
    h,w = img.shape
    glcm = fast_glcm(img, vmin, vmax, nbit, ks)
    homo = np.zeros((h,w), dtype=np.float32)
    for i in range(nbit):
        for j in range(nbit):
            homo += glcm[i,j] / (1.+(i-j)**2)

    return homo


def fast_glcm_ASM(img, vmin=0, vmax=255, nbit=8, ks=5):
    '''
    calc glcm asm, energy
    '''
    h,w = img.shape
    glcm = fast_glcm(img, vmin, vmax, nbit, ks)
    asm = np.zeros((h,w), dtype=np.float32)
    for i in range(nbit):
        for j in range(nbit):
            asm  += glcm[i,j]**2

    ene = np.sqrt(asm)
    return asm, ene


def fast_glcm_max(img, vmin=0, vmax=255, nbit=8, ks=5):
    '''
    calc glcm max
    '''
    glcm = fast_glcm(img, vmin, vmax, nbit, ks)
    max_  = np.max(glcm, axis=(0,1))
    return max_


def fast_glcm_entropy(img, vmin=0, vmax=255, nbit=8, ks=5):
    '''
    calc glcm entropy
    '''
    glcm = fast_glcm(img, vmin, vmax, nbit, ks)
    pnorm = glcm / np.sum(glcm, axis=(0,1)) + 1./ks**2
    ent  = np.sum(-pnorm * np.log(pnorm), axis=(0,1))
    return ent


if __name__ == '__main__':
    nbit = 8
    ks = 5
    mi, ma = 0, 255

    tar_f = '../../data/MNIST/processed/training.pt'
    imgs, labels = torch.load(tar_f)
    img = imgs[2]

    img = fast_glcm_mean(img, vmin=mi, vmax=ma, nbit=nbit, ks=ks)
    plt.imshow(img)
    plt.title('after')
    plt.show()
```



## GIST

一种宏观意义的场景特征描述。

只识别’大街上有一些行人’这个场景，无需知道图像中在那些位置有多少人，或者有其他什么对象。

GIST特征向量可以一定程度表征这种宏观场景特征。