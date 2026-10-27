# Docker тапсырмасы
Орындаған: Ибадулла Ақторғын

Жобада үш образ бар: веб-сайт, PostgreSQL және MySQL.
Командаларды орындамас бұрын Docker Desktop іске қосулы болуы керек.
Контейнер атаулары және сайтқа арналған 8080 порты бос болуы керек.

## 1. PostgreSQL

Docker Hub:
https://hub.docker.com/r/torgyn1/aktorgyn-postgres

Іске қосу:
```powershell
docker run -d --name aktorgyn-pg -e POSTGRES_PASSWORD=LabPg2026 -e POSTGRES_DB=studentdb torgyn1/aktorgyn-postgres:1.0
```

Дайындығын тексеру:
```powershell
docker exec aktorgyn-pg pg_isready -U postgres -d studentdb
```

`accepting connections` шыққаннан кейін кестені көрсету:
```powershell
docker exec -e PGCLIENTENCODING=UTF8 aktorgyn-pg psql -U postgres -d studentdb -c "SELECT * FROM student;"
```

## 2. MySQL

Docker Hub:
https://hub.docker.com/r/torgyn1/aktorgyn-mysql

Іске қосу:
```powershell
docker run -d --name aktorgyn-mysql -e MYSQL_ROOT_PASSWORD=LabMysql2026 -e MYSQL_DATABASE=studentdb torgyn1/aktorgyn-mysql:1.0
```

Бастапқы баптау барысын тексеру:
```powershell
docker logs --tail 30 aktorgyn-mysql
```

Соңғы сервер үшін `ready for connections` және `port: 3306` шыққаннан кейін:
```powershell
docker exec -it aktorgyn-mysql mysql --default-character-set=utf8mb4 -u root -p studentdb -e "SELECT * FROM student;"
```

Оқу контейнерінің паролі: `LabMysql2026`.
Пароль енгізгенде таңбалар экранда көрінбейді.

Екі дерекқордағы нәтиже:
- id: 1
- full_name: Ибадулла Ақторғын

## 3. Веб-сайт

Docker Hub:
https://hub.docker.com/r/torgyn1/aktorgyn-site

Іске қосу:
```powershell
docker run -d --name aktorgyn-site -p 8080:80 torgyn1/aktorgyn-site:1.0
```

Браузерде ашу:
http://localhost:8080

Интернеттегі сайт:
https://docker-fio.onrender.com

## Кодтан образдарды жинау

Осы репозиторийдің негізгі папкасында орындау:
```powershell
docker build -t torgyn1/aktorgyn-site:1.0 .
docker build -t torgyn1/aktorgyn-postgres:1.0 ./postgres
docker build -t torgyn1/aktorgyn-mysql:1.0 ./mysql
```

Дайын образдар компьютерде болмаса, docker run оларды
Docker Hub-тан автоматты жүктейді.

Дерекқордағы бастапқы кестелер init.sql арқылы жаңа дерекқор
алғаш құрылғанда жасалады.
