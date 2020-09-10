---
layout: post
title: 'Distributed Machine Learning on Edge Devices'
# author: '<a href="https://mru.ca/amannejad">Yasaman Amannejad</a>, <a href="http://arash-afshar.github.io/">Arash Afshar</a>'
fullview: false
comments: false
---

Sensors and smart devices are continuously collecting massive amounts of data. Today's state-of-the-art machine learning (ML) techniques are typically trained using cloud platforms, leveraging the elastic scalability of the cloud. For such processing, data from various sources need to be transferred to a cloud server. However, this can cause privacy concerns for users and also creates overhead on the network. Sharing life logging photos and videos from cell phones and wearable devices can cause privacy concerns and network overheas. To overcome these challenges, the data processing can be done on devices where data is generated. In this blog post, we discuss as a distributed learning solution suitable for such data processings on edge devices.

<hr>
**The need for distributed data processing -** Learning from distributed data over edge devices has gained increasing interest in the recent years. This increase in popularity is because of the following reasons. ***First***, there is a need for processing massive amount of data that is continuously being generated through mobile phones, wearable devices, autonomous vehicles. Transferring all the data to a remote cloud server is not always feasible due to limiting factors such as network bandwidth of these devices and the long propagation delays that can incur unacceptable latency. ***Second***, the private nature of these data, e.g., life-logging videos and recorded phone calls, causes privacy concerns for sharing the data with cloud servers. ***Finally***, the transfer of such massive data to a cloud server for processing can burden the backbone networks especially for applications with unstructured data, e.g., image and video analytics. These factors along with the improvements in the storage and computation capacity of these edge devices are shifting the data processing and model training of ML applications from cloud servers to edge devices. 

<!-- **Example -**  -->
Consider large number of user devices, such as mobile phones, each with personal collections of photos or autonomous vehicles with images captured by cameras. If all the distributed data on these devices are accessible in a central place, one can obtain a high-performance ML model that has been trained on an extremely large dataset. However, it is not desirable for clients to share their data due to privacy concerns.

 **One possible solution -**  The need for distributed ML on edge devices has lead to  a  growing  interest  in  *federated  learning  (FL)*  as  an intersection of ML and edge computing fields. FL is to provide a framework for distributed ML on edge devices. In this framework, clients do not share their private data with the remote server, e.g., with a cloud server. Instead, clients train their own local models from their own data. These local models are then shared with a central server to aggregate them and build a global model. This way, using the FL framework clients can build a model from their collective data without sharing their own local data with other clients or with a remote server.


<!-- In general, there are two main entities in FL process: the data owners, i.e., *client*, and the global model owner, i.e., *server*. Let *Clients = {client_1, client_2,..., client_n}* denote the set of *n* clients, each of which has a dataset *D_i*, *i* in *n*, stored on their personal devices, e.g., cellphones. In classical approaches, all clients send their data to the server and the server trains a conventional model, *model*, in central fashion using all data in D. However, as described earlier, this is not possible for all applications due to privacy concerns and the network bandwidth limitations of the edge computing devices. In FL, clients do not share their private data with the remote server, instead, they build a local model, *model_i*, using their own data, *D_i*, and share the parameters of their trained model with the FL server. The server collects all local model parameters and aggregates them to build a global model, *model_federated*. The trained model is sent back to clients for further enhancements. This process continues for *r* rounds of training.  -->


A typical training process is shown in the following figure and it includes four steps:

1. Model broadcast: The server defines the structure of the model to be trained by all clients and decides the hyper-parameters of the local and global models, e.g., the optimizer or the learning rate of a neural network. Next, the server broadcasts the model and parameters to clients. In the first round, the broadcasted model is not trained. In the following rounds, the server broadcasts the model aggregated from the local training of clients.
  
2. Local model training: Clients participating in the training process download the model and train the model based on their local data. The objective of each client during its local training is to find optimal model parameters that minimize the model loss based on client's local data. 
    
3. Model parameter updates: When each client finishes its training, they share their trained model with the FL server. 
    
4. Global model aggregation: When the participating clients share their models with the server, the server aggregates them into a global model. The server's objective is to minimize the global loss function.



{: .center-image}
![The main steps in FL training process]({{ site.BASE_PATH }}/assets/media/FL-process-1.png "The main steps in FL training process")


In FL clients do not share their data with the server.  Clients only share their local model parameters; this significantly reduces the network overhead compared to when the data moves over the network. The computation that was previously performed on a powerful server is distributed to many less powerful devices such as mobile phones or on-board units in vehicles. In each round, the parameters are exchanged twice, once when clients send their local models to the server, and once when they download the aggregated model from the server. This is done for all participating clients. The communication cost is a function of model size, the number of rounds and the number of participating clients. This cost can be compared with the cost of exchanging all client data records with a central server. The cost of exchanging data, specially for unstructured data such as image or video records can be very large. This motivates the use of FL as a distributed ML technique that results in higher privacy for clients and lower communication overhead.


In our recently published <a href="https://drive.google.com/file/d/1hN3QjdYxMF5uwuMdT1kI7C5ezWzDJLRW/view?usp=sharing" target="_blank">paper</a>, we study the performance of a deep learning model trained in distributed fashion under different parameter settings and compare it's performance with the same model when trained in central and traditional fashion. My co-authors in this paper are *Saba F. Lameh*, *Wade Noble*, and <a href="http://arash-afshar.github.io/">*Arash Afshar*</a>.

{: .center-image}
<!-- <a href="https://drive.google.com/file/d/1hN3QjdYxMF5uwuMdT1kI7C5ezWzDJLRW/view?usp=sharing" target="_blank"> -->
![Analysis of FL models]({{ site.BASE_PATH }}/assets/media/paper_IDSTA2020.png "Analysis of Federated Learning Models")
<!-- </a> -->
