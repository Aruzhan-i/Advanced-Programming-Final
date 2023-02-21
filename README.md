# Final Project Report

[Aruzhan-i/Advanced-Programming-Final](https://github.com/Aruzhan-i/Advanced-Programming-Final)

[Video](https://youtu.be/AsZ0iEmncTc)

# Introduction

Estimating age and determining the gender from a photo is one of the important tasks of computer vision. It is used in many areas of business and science and there are a lot of prepared datasets on the Internet. The biggest and most popular among them is UTKFace that will be used in this project. The goal of the project: create models for estimating age and gender from pictures and deploy it to the website.

## Literature Review

In the [article](https://iopscience.iop.org/article/10.1088/1742-6596/2084/1/012028/pdf) from Journal of Physics Mustapha et al. have tried using Convolutional Neural Networks for age group classification problem. In this study the dataset All-Age Face was used as well as pre-trained model FaceNet, which showed the 85% of accuracy. Also, they have reviewed the work of other article by Gong et al. that used debiasing adverial network called DebFace and it showed better results with 94% of accuracy. Below, you can see the simplified overall description of the experiment from the article. There’s a part for detecting a face, cropping it and then using the image for training and classifying, which we will not consider in this project. 

![Untitled](Final%20Project%20Report%20c5368453f43d434d9f9f9928eb934fb6/Untitled.png)

![Untitled](Final%20Project%20Report%20c5368453f43d434d9f9f9928eb934fb6/Untitled%201.png)

Below there’s a structure of CNN model used in the project. The study used a triplet loss that exactly matches what proposed work wants to achieve in terms of face classification.

![Untitled](Final%20Project%20Report%20c5368453f43d434d9f9f9928eb934fb6/Untitled%202.png)

Another [study](http://dx.doi.org/10.3390/app122312432) proposed another type of network for identifying gender and age:

![Untitled](Final%20Project%20Report%20c5368453f43d434d9f9f9928eb934fb6/Untitled%203.png)

![Untitled](Final%20Project%20Report%20c5368453f43d434d9f9f9928eb934fb6/Untitled%204.png)

As you can see here the face is not detected but represented in various ways and then, first, gender is detected and depending on the gender, age group is identified. The study used CNN structure as well as RF(Random Forest), creating a hybrid model. Below there’s the results of accuracy of the model on two different datasets Adience and Morph-II as well as comparison with another different models.

![Untitled](Final%20Project%20Report%20c5368453f43d434d9f9f9928eb934fb6/Untitled%205.png)

![Untitled](Final%20Project%20Report%20c5368453f43d434d9f9f9928eb934fb6/Untitled%206.png)

## Current work

As you can see, the mentioned studies used face detection, CNN architectures and tried hybrid models, the latter showed overall better resluts. In this project, the two models for detecting age(not classifying, but estimating) and gender will be trained regardless of each other. In this project, the dataset from kaggle UTKFace was used, and due to memory limitations of google colab(unfortunately, there was not any favourable way to work with this dataset without saving it in RAM, which caused problems with colab), the model was first trained in kaggle. 

# Data

UTKFace dataset is a large-scale face dataset with long age span (range from 0 to 116 years old). The dataset consists of over 20,000 face images with annotations of age, gender, and ethnicity. Due to memory limitations only 8000 of images were used for this project. The images cover large variation in pose, facial expression, illumination, occlusion, resolution, etc.

![Untitled](Final%20Project%20Report%20c5368453f43d434d9f9f9928eb934fb6/Untitled%207.png)

We analyzed the datased and plotted the age distribution:

![Untitled](Final%20Project%20Report%20c5368453f43d434d9f9f9928eb934fb6/Untitled%208.png)

As you can see, there’s too many children betweenn 0 and 4. The model could overift to this representation. For this reason, we get only leave one third of this data.

![Untitled](Final%20Project%20Report%20c5368453f43d434d9f9f9928eb934fb6/Untitled%209.png)

Now we see that there are only few images of people over 80. The model would underfit for them, so it is better to get rid of them, so we would have a model, which can predict the ages of people under 80.

![Untitled](Final%20Project%20Report%20c5368453f43d434d9f9f9928eb934fb6/Untitled%2010.png)

The distribution looks better now!

![Untitled](Final%20Project%20Report%20c5368453f43d434d9f9f9928eb934fb6/Untitled%2011.png)

The distribution of genders looks fine with slight skew to females(1-female, 0-male).

# Model

We have basically the same datasets for both detecting age and gender, except the loss for age is MSE, whereas for gender Binary Crossentropy, because for the first we are estimating an age and for the second we are classifying; and the activation output for age is ReLu and for gender Sigmoid. Below you can see the structure of the first and second models.

![Untitled](Final%20Project%20Report%20c5368453f43d434d9f9f9928eb934fb6/Untitled%2012.png)

# Results

![Untitled](Final%20Project%20Report%20c5368453f43d434d9f9f9928eb934fb6/Untitled%2013.png)

The result of training our model in estimating age. Here the loss is went down to 220-300, which is quite a good result for this dataset.

![Untitled](Final%20Project%20Report%20c5368453f43d434d9f9f9928eb934fb6/Untitled%2014.png)

Below there’s accuracy and loss of the model for gender detection. The model’s results are quite average.

![Untitled](Final%20Project%20Report%20c5368453f43d434d9f9f9928eb934fb6/Untitled%2015.png)

Here you can see the evaluation of the model on validation dataset.

![Untitled](Final%20Project%20Report%20c5368453f43d434d9f9f9928eb934fb6/Untitled%2016.png)

![Untitled](Final%20Project%20Report%20c5368453f43d434d9f9f9928eb934fb6/Untitled%2017.png)

![Untitled](Final%20Project%20Report%20c5368453f43d434d9f9f9928eb934fb6/Untitled%2018.png)

The Male precision is lower than Female, whereas recall is higher.

Below you can see the correctly identified images from web application, which has been generated with gradio.

![Untitled](Final%20Project%20Report%20c5368453f43d434d9f9f9928eb934fb6/Untitled%2019.png)

![Untitled](Final%20Project%20Report%20c5368453f43d434d9f9f9928eb934fb6/Untitled%2020.png)

# Discussion

The Results of the Models are quite average, the next steps would be implementing more complex structures like hybird using not only CNN but random forest, also it is probably better no use the co-dependent models for detecting gender and age.

# Literature

Liao, Hai-bin & Yuan, Li & Wu, Mou & Zhong, Liangji & Jin, Guonian & Xiong, Neal. (2022). Face Gender and Age Classification Based on Multi-Task, Multi-Instance and Multi-Scale Learning. Applied Sciences. 12. 12432. [10.3390/app122312432](https://www.researchgate.net/publication/366019010_Face_Gender_and_Age_Classification_Based_on_Multi-Task_Multi-Instance_and_Multi-Scale_Learning).

Muhammad Firdaus Mustapha et al 2021 J. Phys.: Conf. Ser. 2084 012028. [10.1088/1742-6596/2084/1/012028](https://www.notion.so/Gaining-a-solid-understanding-of-Pandas-series-8c5336243ebb43609dcef31fd7788a18)

Age and Gender Detection. [Github](https://github.com/smahesh29/Gender-and-Age-Detection)

Age and Gender Detection Using Deep Learning. [Article](https://www.analyticsvidhya.com/blog/2021/07/age-and-gender-detection-using-deep-learning/)

[GeeksForGeeks](https://www.geeksforgeeks.org/age-detection-using-deep-learning-in-opencv/).
