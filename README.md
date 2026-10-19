Практическое занятие №4



Тема: маршрутизация с chi. CRUD-сервис «Список задач».

Выполнил: Нерсисян Арам Овикович.

Группа: ЭФМО-01-26.

Вуз: РТУ МИРЭА.

Дата выполнения: 19.10.2026.



Описание



HTTP API на Go с роутером chi v5.5.0.

Поддерживает создание, просмотр, изменение и удаление задач.

Данные хранятся в памяти и очищаются при перезапуске сервера.

Подключены логирование, CORS, RequestID и Recoverer.



Требования



Go 1.27.2 или новее.

Команды ниже предназначены для PowerShell.



Структура проекта



main.go — создание роутера и запуск сервера.

internal/task/model.go — модель задачи.

internal/task/repo.go — хранилище в памяти.

internal/task/handler.go — обработчики CRUD и JSON-ответы.

pkg/middleware/logger.go — логирование запросов.

pkg/middleware/cors.go — заголовки CORS и обработка OPTIONS.

go.mod и go.sum — описание модуля и зависимостей.



Запуск из корневой папки проекта



go mod download

go run .



Адрес сервера: http://localhost:8080.

Для остановки нажмите Ctrl+C в окне сервера.



Маршруты



GET /health — проверка работоспособности, текст OK.

GET /api/tasks — список задач.

POST /api/tasks — создание задачи.

GET /api/tasks/{id} — получение одной задачи.

PUT /api/tasks/{id} — изменение названия и признака выполнения.

DELETE /api/tasks/{id} — удаление задачи.



Проверка API



Выполняйте запросы в другом окне PowerShell.

Пример рассчитан на новый запуск сервера и задачу с ID 1.



Проверка сервера:



curl.exe -i http://localhost:8080/health



Создание:



'{"title":"Learn chi"}' | curl.exe -i -X POST http://localhost:8080/api/tasks -H "Content-Type: application/json" --data-binary "@-"



Список:



curl.exe -i http://localhost:8080/api/tasks



Получение по ID:



curl.exe -i http://localhost:8080/api/tasks/1



Изменение:



'{"title":"Learn chi deeply","done":true}' | curl.exe -i -X PUT http://localhost:8080/api/tasks/1 -H "Content-Type: application/json" --data-binary "@-"



Удаление:



curl.exe -i -X DELETE http://localhost:8080/api/tasks/1



Проверка после удаления:



curl.exe -i http://localhost:8080/api/tasks/1



Коды ответа



200 OK — успешное чтение или изменение.

201 Created — задача создана.

204 No Content — задача удалена, тело ответа отсутствует.

400 Bad Request — неверный ID, JSON или пустое название.

404 Not Found — задача не найдена.



JSON-ошибки имеют формат {"error":"сообщение"}.

Поля created\_at и updated\_at содержат время в UTC.

При изменении задачи created\_at сохраняется, updated\_at обновляется.



CORS и логирование



Разрешены запросы с любого Origin.

Методы: GET, POST, PUT, DELETE, OPTIONS.

Заголовки: Content-Type, Authorization.

Предварительный запрос OPTIONS получает 204 без тела.

В терминал выводятся метод, путь и длительность запроса.



Форматирование и проверка



go fmt ./...

go vet ./...



Сборка



go build -o pz4-todo.exe .



После остановки ранее запущенного сервера:



.\\pz4-todo.exe

