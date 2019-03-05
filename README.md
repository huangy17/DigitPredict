# MNIST-Cassandra-Docker

### Overview
This project aimed at applying MNIST dataset to data containers, identifying values of digital photos of handwriting provided by users, returning the results to users and store them in Cassandra.

Four important things you have to know about this project: 

1. *model2.ckpt* – model2.ckpt is a trained model based on the MNIST tutorials from TensorFlow website for digit recognizing.
2. *predict_7.py* – The main python code of this project including digit recognizing, creating keyspace and inserting data into Cassandra.
3. *Dockerfile* – uses Dockerfile to assemble an image of application which predicts the correct integer from a handwritten number.
4. *docker-compose.yml* – uses the docker-compose.yml file to initial the image of Cassandra database.


### Installing Docker
At the beginning, Docker needs to be installed as driver. 

Following [Docker for Mac](https://docs.docker.com/docker-for-mac/) to install on mac .

Following [Docker for Windows](https://docs.docker.com/docker-for-windows/) to install on Windows .


## Running
Running is based on the steps:
1. Create user-defined net work on Docker
2. creating and running Cassandra image
3. creating and running digit recognizing image
4. submit digit photo on browser to predict handwriting number. 

### 1. Create user-defined net work on Docker
Use command below to define the network name as my-net which will be used by following steps. This allows containerized applications to communicate with each other easily, without accidentally opening access to the outside world. 

```$ docker network create my-net```

### 2. creating and running Cassandra image
The easiest way is to cd to the directory where this project files are located. Then run:

```$ docker-compose up -d```

to create a Cassandra image. 
After creating Cassandra database image, it is time to run the database container. You can use the cqlsh tool included in container to check your key space using the following commands:

```$ docker exec -it cassandratable cqlsh```


### 3. creating and running digit recognizing image

Now, opening a new terminal window and cd to the directory where this project files are located. Then run:

```$ docker build --tag=predict:1.0 .``` 

this command will build an application image from Dockerfile. You can change the name and tag as you want, in this example, the image is named as "predict:1.0". 

It will take you a few minutes to build an image. After building up, then run: 

```$ docker run -p 8000:8000 --network=my-net -itd --name=predict1.0 predict:1.0```

This command will run the container upon the image you created above on Port 8000. Note: you cannont change the network's name, but you can change the container's name as you want. (In this example, predict1.0 is container's name)

### 4. submit digit photo on browser to predict handwriting number. 

You can check the Port that the predict1.0 is running on by using the command: 

```$ docker ps```

in this example, predict1.0 is running on 0.0.0.0:8000

Copy and past 0.0.0.0:8000 as URL into browser, then you will see the webpage that you can submit handwriting digit. The result will show on the webpage. 

Note:  Digit photo has to be black text on a white background. 

You can check the predict result in Cassandra that you created before by using cqlsh command:

```select * from keyspacetest.mytable ;```





