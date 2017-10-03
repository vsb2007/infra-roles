[![Build Status](https://travis-ci.org/vsb2007/infra-roles-db.svg?branch=master)](https://travis-ci.org/vsb2007/infra-roles-db)

### Infra roles: DB
# Check notification in chat

Steps:

#генерируем ключ для подключения по SSH
```
ssh-keygen -t rsa -f google_compute_engine -C 'travis' -q -N ''
```
#Создаем ключ в метадате проекта infra в GCP
```
pbcopy < google_compute_engine.pub # OSX specific command
```
#Должен быть предварительно создан сервисный аккаунт и скачаны креды (credentials.json)
#Должна быть предварительно подключена данная репа в тревисе
```
travis encrypt GCE_SERVICE_ACCOUNT_EMAIL='ci-test@infra-179032.iam.gserviceaccount.com' --add
travis encrypt GCE_CREDENTIALS_FILE="$(pwd)/credentials.json" --add
travis encrypt GCE_PROJECT_ID='infra-179032' --add
```

#шифруем файлы
```
tar cvf secrets.tar credentials.json google_compute_engine

travis login

travis encrypt-file secrets.tar --add
```
#пушим и проверяем изменения
```
git commit -m 'Added Travis integration'
git push
```