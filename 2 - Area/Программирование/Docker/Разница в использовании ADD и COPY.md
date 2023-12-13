# Разница в использовании ADD и COPY

- [Тред на StackOverflow](https://www.geeksforgeeks.org/difference-between-the-copy-and-add-commands-in-a-dockerfile/)
- [Еще один тред на StackOverflow](https://stackoverflow.com/questions/24958140/what-is-the-difference-between-the-copy-and-add-commands-in-a-dockerfile)

Рассмотрим разницу в использовании на двух примерах — добавление архива и файла по ссылке. 

Dockerfile для `ADD`:

```dockerfile
FROM ubuntu:22.04

WORKDIR /zip
ADD archive.tar.xz .

WORKDIR /url
ADD https://airflow.apache.org/docs/apache-airflow/2.4.0/docker-compose.yaml .
```

Dockerfile для `COPY`:

```dockerfile
FROM ubuntu:22.04

WORKDIR /zip
COPY archive.tar.xz .

WORKDIR /url
COPY https://airflow.apache.org/docs/apache-airflow/2.4.0/docker-compose.yaml .
```
