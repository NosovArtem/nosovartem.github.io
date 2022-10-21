# 				Docker remote debug 	

#### Способ первый

1) правильно залить образ в Docker.

   ​	В массив комманд передаем команду удаленного дебага.

   ```
   FROM adoptopenjdk/openjdk11:ubi
   EXPOSE 8081
   COPY /target/subscription-management-service.jar fatJar.jar
   CMD ["java", "-agentlib:jdwp=transport=dt_socket,server=y,suspend=n,address=*:5005", "-jar","fatJar.jar"]
   ```

2) подключиться по правильному ip к контейнеру в котором работает нужное приложение

   Узнает на какам ip работает наш сервис. Для этого:

      -	 sudo docker ps -a      -      узнаем "CONTAINER ID" для следующего шага.
      -	 sudo docker inspect "CONTAINER ID" | grep "IPAddress"

3) настраиваем дебаг в Intelij IDEA

   IP меняется при рестарте контейнера, придется редактировать конфиг для Remote Debug в IDEA

#### Способ второй

От первого отличается тем, что мы дополнительно открываем порт в контейнере.
И перенапровляем запрос на него.

```
FROM adoptopenjdk/openjdk11:ubi
EXPOSE 8081
EXPOSE 5005
COPY /target/subscription-management-service.jar fatJar.jar
CMD ["java", "-agentlib:jdwp=transport=dt_socket,server=y,suspend=n,address=*:5005", "-jar","fatJar.jar"]
```



```
subscription-management-service:
  image: subscription-management-service:latest
  container_name: subscription-management-service
  ports:
    - "8081:8080"
    - "5005:5005"
  depends_on:
    - broker
  environment:
    KAFKA_BROKERS: "broker:9092"
    KAFKA_SUBSCRIPTION_STATUSES_TOPIC: "subscription-statuses-topic"
    KAFKA_SUBSCRIPTION_COMMANDS_TOPIC: "subscription-commands-topic"
    KAFKA_APPLICATION_ID: "subscription-management-service"
```

