
This repo contains my class notes from the UNIPD MAPDB course on Advanced Data Science Tools

The repo is split into two sections

1) MAPDB Class Notes
2) Project: Sea Turtle Face Detection with Distributed Machine Learning on Cloud Veneto


# Class notes
This implementation utilized Docker to set up containers of various systems

1) MySQl
2) Spark
3) DASK 
4) MongoDB
5) Kafka


# Project: Sea Turtle Face Detection with Distributed Machine Learning

<img width="1112" alt="image" src="https://user-images.githubusercontent.com/61107719/196725351-81bb9622-20e8-47f1-b06e-0a45ad94ea69.png">

The main goal of this project was to experiment with different options when trying to distribute ML training between workers on a cluster running on Cloud Veneto. 
<img width="375" alt="image" src="https://user-images.githubusercontent.com/61107719/196738255-7133ce05-12a8-41ee-a57b-fbe90f7ddc4a.png">
dask.org  
Dask is a Python based API that facilitates distributed computing. It can link to a cluster of multiple workers and one scheduler computer. It provides multiple workflows to achieve distributed storing and manipulation of data. It is the main tool used in this project.

One option explored was using Dask delayed to distribute a TensorFlow CNN. The other option explored was using a scikit learn MLP classifier. 

## Computational Time Line Comparison
Dask Delayed Distribution of a Tensorflow CNN (5 epochs)
![image](https://user-images.githubusercontent.com/61107719/196728264-29fb43c6-d1c2-4a93-83ff-43017f1951c6.png)

Dask Distribution of a scikit learn MLP classifier (1 epoch)
![image](https://user-images.githubusercontent.com/61107719/196729074-a719cab6-2836-4dd9-a614-609bef9f2b83.png)

Each row in the images above represents a worker on the cluster. The red color indicates that information is being transferred from another worker. The other colors represent a task pering performed by the worker. Time is on the x-axis. You can see that when Dask distributes the MLP classifier it does not train in parallel. Each worker will train on the data it has collected and then transfer the weights to the next worker to continue training. This strategy will allow the system to break through memory limitations. Although this system does not help reduce training time. The custom Dask delayed distribution of the TensorFlow CNN is a stark contrast. It will train in parallel. This could potentially lead to reduced training times for large datasets. The data is also split between the workers allowing them to break memory limitations. For the small dataset of 4000 198x198 images, the parallel training of the Dask delayed CNN did not cause faster training times than the unparallel supported MLP. One epoch took 10min for the CNN and only 1 second for the MLP. This is partially because the MLP is a simpler algorithm that was fed further processed HOG versions of the images. The CNN is a more complex algorithm with more weights to alter and is fed the full-color images. It is also partially due to the MLP being Dask supported and thus optimised. Dask themselves admit that whilst Dask delayed is flexible it is also slow.
