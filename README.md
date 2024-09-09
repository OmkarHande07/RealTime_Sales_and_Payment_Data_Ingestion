# RealTime_Sales_and_Payment_Data_Ingestion

**Designed a Data Pipeline to Ingest Sales Order and Payment Data in Realtime into a NoSQL Database.**

# Tech Stack:
1. GCP Pub/Sub : Used as a messaging middleware for real-time processing.
2. Python Producer / Consumer Script
3. DLQ (Dead Letter Queue) : Used to handle message failure by forwarding undelivered messages to a dead-letter topic.
4. Docker
5. Cassandra : Used as a NoSQL Database. 

**Steps to design pipeline:**

1. To use GCP first make sure gcp account is authenticated from terminal.
2. Follow the setup and requirenments (input file) file instructions.
3. Install following Python packages : pip3 install google-cloud-pubsub , pip3 install cassandra-driver
4. In GCP console search for Pub/Sub-> Topics-> Create-> create 3 topic with name orders_data, payment_data, dlq_payments_data.

   ![image](https://github.com/user-attachments/assets/09dd5934-2071-450e-a747-c4ea11ccf914)

5. Now from terminal run the docker-compose-cassandra.yml (input file) file using docker compose -f docker-compose-cassandra.yml up -d command this will  start the docker containers for cassandra.

   ![image](https://github.com/user-attachments/assets/a3c98761-1448-4073-b7e1-9a136507acac)

6. From Docker Desktop open the container which is created -> Exec -> type cqlsh (it will cassandra query language) helps in write queries for cassandra.
7. Now Create key space (same as schema in Redshift) and then table syntax is given in input files.

 ![image](https://github.com/user-attachments/assets/a84a222a-6c9e-46e3-bbd1-c0ec845146e2)

8. In all given python scripts update the project Id and topic names.
9. Start 4 instances of CMD -> run orders_consumer.py script first so that consumer will be up but since no records are published their will be no records on consumer side.
10. similarly run ingest_in_fact_table.py script.
11. now run the orders_producer.py script which will produce records and ingest them into the cassandra in realtime. the consumer will be consuming those records and showing the message. You can also execute select command in docker tu check records inserted into cassandra table.
12. similarly run payments_producer.py script now check the cassandra table again the records will be updated with the payments data also where the record match happens.

 ![image](https://github.com/user-attachments/assets/69d694d8-276c-4a2a-8196-b1cf5cd73165)
![image](https://github.com/user-attachments/assets/0c0bb21f-a7d3-44a3-bcf2-2c889310a47c)
![image](https://github.com/user-attachments/assets/13c99af2-e98f-4408-8f4b-6bcf9d31e0ca)
![image](https://github.com/user-attachments/assets/38beec03-9396-44f3-b8fa-32d73f6f9fc3)



13. Only those records will be updated with payment data which were produced by orders_producer.py script and rest records will be thrown to dlq.

 ![image](https://github.com/user-attachments/assets/0e39aeeb-b330-4ece-8e68-36b87bc976b0)
14. To check dlq records:
    go to  Pub/Sub topics->dlq_payments_data topic->in messages-> in step2 select the message from drop down-> click on PULL-> here all those records will be displayed where match doesn't happen.
    
    ![image](https://github.com/user-attachments/assets/5ce0e3fb-674c-46f0-9eee-b2260a0f7c98)



**[dlq helps in failure and recovery or re-ingestion or data re-processing which helps in streaming related usecases]**



	



