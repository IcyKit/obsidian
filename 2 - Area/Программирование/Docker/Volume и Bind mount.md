# Volume и Bind mount

docker volume ls — вывести список вольюмов

docker volume create <название> — создать вольюм

docker volume rm <название> — удалить вольюм

docker volume prune — удалить вольюмы, которые не используются контейнерами

---

## Bind mount:

docker run -v <полный_путь_на_хосте>:<полный_путь_в_контейнере> <образ>

## Volume:

docker run -v <название_вольюма>:<полный_путь_в_контейнере> <образ>

## Readonly режим

docker run -v <полный_путь_на_хосте>:<полный_путь_в_контейнере>:ro <образ>