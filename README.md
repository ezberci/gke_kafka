## Pushing docker images to container registery



In order to deploy pods to the [Google Kubernetes Engine](https://cloud.google.com/kubernetes-engine) first you need to push your docker images to the [Google Container Registry](https://cloud.google.com/container-registry)

    docker pull zookeeper:3.6.1
    docker tag zookeeper:3.6.1 gcr.io/blog-261010/zookeeper:3.6.1
    docker push gcr.io/blog-261010/zookeeper:3.6.1

    docker pull wurstmeister/kafka:2.12-2.5.0
    docker tag wurstmeister/kafka:2.12-2.5.0 gcr.io/blog-261010/wurstmeister/kafka:2.12-2.5.0
    docker push gcr.io/blog-261010/wurstmeister/kafka:2.12-2.5.0
    
##Deploying pods and services

    kubectl apply -f ./kafka/deployments/kafka-deployment.yaml
    kubectl apply -f ./kafka/deployments/zookeeper-deployment.yaml
    kubectl apply -f ./kafka/services/zookeeper-service.yaml
    kubectl apply -f ./kafka/services/kafka-service.yaml


##Managing topics
Get your kafka pod name using

    kubectl get pods


Then connect to kafka pod using 

    kubectl exec -it kafka-pod-name -- /bin/bash


Kafka producer/consumer example as follows 

    /opt/kafka/bin/kafka-topics.sh --list --zookeeper zookeeper:2181
    /opt/kafka/bin/kafka-console-producer.sh --bootstrap-server localhost:9092 --topic testTopic
    /opt/kafka/bin/kafka-console-consumer.sh --bootstrap-server localhost:9092 --from-beginning --topic testTopic

If you want to add another topic to kafka you can update _KAFKA_CREATE_TOPICS_ section of the [kafka-deployment.yaml](./deployments/kafka-deployment.yaml)


