### Запуск
<img width="1367" height="87" alt="image" src="https://github.com/user-attachments/assets/0115e18c-8149-43b9-b3ac-a416fe0d38d6" />
<img width="1136" height="322" alt="image" src="https://github.com/user-attachments/assets/e7e427b0-90e9-4512-a101-7510584815fb" />

### test_topic:
<img width="1134" height="547" alt="image" src="https://github.com/user-attachments/assets/6a9d6490-cd90-4def-828f-279ec35e2be9" />

8 партиций позволяют одновременно писать в 8 разных логов и читать из них

Каждая партиция сразу на трёх разных брокерах. Если один (или даже два) брокера выйдут из строя, данные не потеряются, и запись/чтение смогут продолжиться через оставшиеся реплики.

ISR - Количество реплик, которые полностью синхронизированы с лидером (8 * 3 = 24)

### НТ
<img width="1360" height="89" alt="image" src="https://github.com/user-attachments/assets/94047138-5d27-4b00-963b-5a4a60a926b8" />

Пропускная способность: ~343 тыс.

Средняя задержка: 8.73 мс

p95: 95% сообщений доставлено ≤55 мс


### Отказоустойчивость
<img width="180" height="181" alt="image" src="https://github.com/user-attachments/assets/b8e63e49-cbbc-4818-82bc-7dfb1baebab0" />
<img width="1114" height="556" alt="image" src="https://github.com/user-attachments/assets/a7ae36e0-49db-4514-b589-2daa9b045d66" />

Сначала все работает без ошибок

#### Остановка первого брокера
<img width="1134" height="542" alt="image" src="https://github.com/user-attachments/assets/4ebdef4f-9779-47af-9ea3-664edc37c0ef" />

ISR стал 16/24 - как раз из-за потери одного брокера. Но сообщения продолжают доставляться


#### Остановка второго брокера
<img width="665" height="119" alt="image" src="https://github.com/user-attachments/assets/ee0cdc2e-9eb5-4fc2-8ddf-16aea0cf6d6b" />

После остановки второго брокера кворум записи потерян, из-за чего и происходит ошибка

UI Kafka начинает тормозить при попытке получить ответ от топиков

Producer не может получить подтверждения от необходимого количества реплик, что приводит к таймаутам.

#### Восстановление брокеров
После востановления ошибка пропала, ISR снова 24/24
<img width="1123" height="547" alt="image" src="https://github.com/user-attachments/assets/0b799685-e97d-4f27-86af-c6e4ea9f638c" />


