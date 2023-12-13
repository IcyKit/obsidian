# Разница в использовании ENTRYPOINT и CMD

Несмотря на то, что и `ENTRYPOINT`, и `CMD` отвечают за запуск программы в контейнере, они используются в разных ситуациях. Особенно это касается случая, когда в одном докерфайле используются обе инструкции одновременно.  

- [Документация](https://docs.docker.com/engine/reference/builder/#understand-how-cmd-and-entrypoint-interact)
- [Тред на Stackoverflow](https://stackoverflow.com/questions/21553353/what-is-the-difference-between-cmd-and-entrypoint-in-a-dockerfile)

|                                |                            |                                    |                                                |
| ------------------------------ | -------------------------- | ---------------------------------- | ---------------------------------------------- |
|                                | **No ENTRYPOINT**          | **ENTRYPOINT exec_entry p1_entry** | **ENTRYPOINT [“exec_entry”, “p1_entry”]**      |
| **No CMD**                     | error, not allowed         | /bin/sh -c exec_entry p1_entry     | exec_entry p1_entry                            |
| **CMD [“exec_cmd”, “p1_cmd”]** | exec_cmd p1_cmd            | /bin/sh -c exec_entry p1_entry     | exec_entry p1_entry exec_cmd p1_cmd            |
| **CMD [“p1_cmd”, “p2_cmd”]**   | p1_cmd p2_cmd              | /bin/sh -c exec_entry p1_entry     | exec_entry p1_entry p1_cmd p2_cmd              |
| **CMD exec_cmd p1_cmd**        | /bin/sh -c exec_cmd p1_cmd | /bin/sh -c exec_entry p1_entry     | exec_entry p1_entry /bin/sh -c exec_cmd p1_cmd |
