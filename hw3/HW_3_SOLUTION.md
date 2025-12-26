### patronictl list

<img width="699" height="153" alt="Снимок экрана 2025-12-26 в 16 05 42" src="https://github.com/user-attachments/assets/1ca2026e-69fc-4ead-b05a-2950b6b53255" />

Кластер состоит из 3 узлов, patroni3 - лидер

patroni2 и patroni1 - реплики. Они только читают данные, копируя их с лидера.

Прольем скрипт в базу, проверим, что все сработало

<img width="809" height="207" alt="Снимок экрана 2025-12-26 в 16 14 21" src="https://github.com/user-attachments/assets/a45a91d9-51f6-48c8-9406-8e57dab18eb8" />

### Стреляем
Сначала все работает по плану, записи идут

<img width="343" height="249" alt="Снимок экрана 2025-12-26 в 16 25 47" src="https://github.com/user-attachments/assets/df79f9f7-7bc1-466f-9a97-22a6e74b46ca" />

После отключения лидера посмотрим в лог одной из реплик

Видно, что реплика потеряла связь с мастером, и после таймаута, так как мастер все еще не отвечает - занял место мастера

Также можно отметить, что между потери связи с мастером и восстановлении работы прошло 25с

<img width="1216" height="606" alt="Снимок экрана 2025-12-26 в 16 25 10" src="https://github.com/user-attachments/assets/a4b9a0b8-69c8-4171-af65-4e1b12e1d004" />

В HAProxy также можно наблюдать потерю связи мастера с последующей сменой лидера

<img width="1510" height="462" alt="Снимок экрана 2025-12-26 в 16 19 34" src="https://github.com/user-attachments/assets/011e31ae-7915-460b-8693-a74f4305e06e" />

<img width="1512" height="471" alt="Снимок экрана 2025-12-26 в 16 20 16" src="https://github.com/user-attachments/assets/2cef6fad-c028-4689-aa52-a47beb4245f9" />

Если отключить HAProxy, можно увидеть следующее в логах лидера:

<img width="631" height="352" alt="image" src="https://github.com/user-attachments/assets/d9c592cf-273d-4ab3-add1-f962e6c1566b" />

Данные до БД не доходят, но сама БД продолжает жить

После отключения одного etcd бд все еще функционирует, но при отключении второго, мы видим следующее:

<img width="1202" height="204" alt="image" src="https://github.com/user-attachments/assets/a7941be8-890c-4e3e-a45e-c8d9ac9fb137" />
Поскольку 2 из 3 нод etcd отключены, кворум потерян.


