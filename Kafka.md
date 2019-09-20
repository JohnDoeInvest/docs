# Kafka
The backend of JohnDoeInvest uses Kafka for all of the long term storage of data. Since we want to both store and retrieve data from
multiple applications we also need a way to structure the data. For this we use Avro which allows you to define a schema of how the data
you store looks like and it also allows you to have backwards compatability between schemas. Storing these schemas is handled by
[Schema Registry](https://docs.confluent.io/current/schema-registry/index.html).
