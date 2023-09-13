# 14.5_K8S_Aleksandr_Molokov

### Задание. При деплое приложение web-consumer не может подключиться к auth-db. Необходимо это исправить

1. Установить приложение по команде:
```shell
kubectl apply -f https://raw.githubusercontent.com/netology-code/kuber-homeworks/main/3.5/files/task.yaml
```
2. Выявить проблему и описать.
3. Исправить проблему, описать, что сделано.
4. Продемонстрировать, что проблема решена.

### Ответ

#### Готовый кластер

![кластер готов](https://github.com/ALEMOLOKOV/14.5_K8S_Aleksandr_Molokov/assets/109212419/dbd51308-1398-4c4b-b29e-046407c622c0)

#### Создание namespace web и data для деплоя приложений

![создаем ns](https://github.com/ALEMOLOKOV/14.5_K8S_Aleksandr_Molokov/assets/109212419/d9bbdccd-8d47-41d1-8afd-62d8933f916a)

#### Выявление проблемы и описание решения

После развертывания всех объектов, выяснилось, что приложение web-consumer не может связаться с приложением auth-db.

![нет доступа по имени auth-db](https://github.com/ALEMOLOKOV/14.5_K8S_Aleksandr_Molokov/assets/109212419/383ef9ad-2e48-42a7-95d0-f0adbd4db205)

Так как поды находяться в разных неймспейсах то при обращении нужно испольховать полное доменное(FQDN) имя сервика.
В заданном деплойменте приложение web-consumer обращалось по имени сервиса auth-db, в результате сервис был недоступен.
Необходимо испольховать полное имя - auth-db.data.svc.cluster.local.

#### Выдержка из статьи про namespaces

Напоследок стоит сказать, что при создании сервиса (service) в кластере Kubernetes создается соответствующая DNS-запись. Запись создается в формате <service-name>.<namespace-name>.svc.cluster.local, а это означает, что если вы используете только часть <service-name> то это позволит “достучаться” до сервиса только в пределах неймспейса. Если вам необходимо взаимодействовать с сервисом, запущенным в другом неймспейсе, следует использовать полное доменное имя (FQDN).


#### Я разбил заданный файл на 3 отдельных файла

![Deployment-web](https://github.com/ALEMOLOKOV/14.5_K8S_Aleksandr_Molokov/blob/765346c7deb1d3005df6b529ac753ea955a87e65/deployment-web.yaml)

![Deployment-db](https://github.com/ALEMOLOKOV/14.5_K8S_Aleksandr_Molokov/blob/765346c7deb1d3005df6b529ac753ea955a87e65/deployment-db.yaml)

![Service-db](https://github.com/ALEMOLOKOV/14.5_K8S_Aleksandr_Molokov/blob/765346c7deb1d3005df6b529ac753ea955a87e65/service-db.yaml)

#### Результат после внесения правок в deployment

#### Статус объектов после внесения изменений

![все готово](https://github.com/ALEMOLOKOV/14.5_K8S_Aleksandr_Molokov/assets/109212419/3b97f9cb-cbbc-471c-82b7-db562e9c8a22)

#### Проверка логов с пода для подтверждения связи

![логи с weba](https://github.com/ALEMOLOKOV/14.5_K8S_Aleksandr_Molokov/assets/109212419/a7b86861-fcc9-4c9f-8308-c49063a963e8)





