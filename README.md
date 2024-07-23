# Management of Physical Datasets B

The repo is split into two sections, 

1) MAPDB Class Notes
2) Project: Sea Turtle Face Detection with Distributed Machine Learning on Cloud Veneto

For the [MAPDA VHDL FPGA project click here](https://github.com/jjackson1994/MAPD)

# Class notes
This implementation utilized Docker to set up containers of various systems

1) [MySQL](https://github.com/jjackson1994/MAPDB/tree/main/Class_Problems/mysql/notebooks) 
2) [Spark](https://github.com/jjackson1994/MAPDB/tree/main/Class_Problems/spark/notebooks)
3) [DASK](https://github.com/jjackson1994/MAPDB/tree/main/Class_Problems/dask/notebooks)
4) [MongoDB](https://github.com/jjackson1994/MAPDB/tree/main/Class_Problems/mongo/notebooks)
5) [Kafka](https://github.com/jjackson1994/MAPDB/tree/main/Class_Problems/kafka/notebooks)

# Project: Sea Turtle Face Detection with Distributed Machine Learning

<img width="1112" alt="image" src="https://user-images.githubusercontent.com/61107719/196725351-81bb9622-20e8-47f1-b06e-0a45ad94ea69.png">

[Project Notebook](https://github.com/jjackson1994/MAPDB/blob/main/Dask_Distributed_ML_Project/Dask_distributed_ML_Turtle_Project%20.ipynb)

The primary objective of this project was to experiment with different options when trying to distribute ML training between workers on a cluster running on Cloud Veneto.  
<img width="375" alt="image" src="https://user-images.githubusercontent.com/61107719/196738255-7133ce05-12a8-41ee-a57b-fbe90f7ddc4a.png">
 
Dask is a Python-based API that facilitates distributed computing. It can connect to a cluster consisting of multiple workers and a single scheduler computer. It provides various workflows for distributed storage and manipulation of data. Dask is the main tool used in this project.

One option explored was using Dask Delayed to distribute a TensorFlow CNN. The other option explored was using a scikit-learn MLP classifier.

## Computational Time Line Comparison
Dask Delayed Distribution of a Tensorflow CNN (5 epochs)
![image](https://user-images.githubusercontent.com/61107719/196728264-29fb43c6-d1c2-4a93-83ff-43017f1951c6.png)

Dask Distribution of a scikit learn MLP classifier (1 epoch)
![image](https://user-images.githubusercontent.com/61107719/196729074-a719cab6-2836-4dd9-a614-609bef9f2b83.png)


Each row in the images above represents a worker in the cluster. The red color indicates that information is being transferred from another worker, while the other colors represent tasks being performed by the worker. The x-axis denotes time.

When Dask distributes the MLP classifier, it does not train in parallel. Each worker trains on the data it has collected and then transfers the weights to the next worker to continue training. This strategy helps the system overcome memory limitations but does not reduce training time.

In contrast, the custom Dask delayed distribution of the TensorFlow CNN trains in parallel, potentially reducing training times for large datasets. The data is split between the workers, allowing them to handle memory limitations effectively. However, for the small dataset of 4,000 198x198 images, the parallel training of the Dask delayed CNN did not result in faster training times compared to the non-parallel MLP. One epoch took 10 minutes for the CNN, whereas it took only 1 second for the MLP. This discrepancy is partly because the MLP is a simpler algorithm trained on preprocessed HOG versions of the images, while the CNN is a more complex algorithm with more weights to adjust, using the full-color images. Additionally, the MLP benefits from being Dask-supported and thus optimized. Dask acknowledges that while Dask delayed is flexible, it can also be slow.
