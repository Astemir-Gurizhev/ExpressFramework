# Framework like express for node.js

## 1	Поднять сервер:	
	1	Подключить библиотеку http.
	2	Создать порт для запуска сервера.
	3	Создать сервер с помощью createServer((req, res)).
	4	Создать слушатель на нашем порту.
	5	Установить зависимость nodemon для обновления данных при их сохранении.
	6	Установить нужные заголовки для корректного отображения русского языка:
	7	Указать заголовок "Content-Type": "text/html; charset=utf-8;".
	8	Отправить HTML разметку в браузер.
	9	Настроить сервер для работы с API:
	10	Изменить заголовок на заголовок для JSON.
	11	Добавить проверку для разных вариантов запросов, например, /users и /posts.
	12	Результатом запроса должен быть объект, который мы преобразуем в строку с помощью JSON.stringify.
		
## 2	Реализация класса Router	
	1	Создать класс Router с конструктором, принимающим пустой объект endpoints.
	2	Добавить метод request с параметрами method='GET', path и handler:
	3	В методе request проверить существование endpoints[path].
	4	Если endpoints[path] не существует, заполнить его пустым объектом.
	5	Вынести endpoints[path] в отдельную переменную endpoint.
	6	Если endpoints[method] уже задан, выбросить ошибку с помощью throw и создать новый объект класса Error с сообщением [${method}] по адресу ${path} уже существует.
	7	Поместить handler в endpoints[method].
	8	Сгенерировать событие для соответствующего ключа-значения ([${path}:${method}]). Внутри обработчика этого события вызвать handler(req, res). Для генерации события можно использовать emitter.
	9	Добавить обертки для разных вызовов метода request будущего роутера GET, POST, PUT, DELETE.
		
## 3	Проверка работы GET запросов для users и posts с использованием класса Router	
	1	Создать экземпляр класса Router.
	2	Добавить GET запрос для /users:
	3	Подписаться на событие GET с помощью метода router.GET('/users', handler).
	4	Внутри обработчика запроса вернуть обычное сообщение на клиент.
	5	Добавить второй endpoint для /posts:
	6	Подписаться на событие GET с помощью метода router.GET('/posts', handler).
	7	Сгенерировать событие при создании сервера:
	8	При генерации события обязательно передать параметры req и res.
	9	Проверить некорректный адрес:
	10	Проверить, что при обращении к некорректному адресу, сервер возвращает ошибку.
		
## 4	Вынести Router в отдельный блок (файл Router.js):	
	1	Создаем папку framework и в ней создаем файл Router.js.
	2	Переносим код класса Router в этот файл и сразу экспортируем его.
	3	Создаем глобальный экземпляр эмиттера внутри обертки для класса Router.
		
## 5	Создание обложки для класса Router и реализации класса Application:	
	1	Создать файл Application.js в папке framework.
	2	Экспортировать модуль events http.
	3	Создать и экспортировать класс Application с использованием объекта EventEmitter внутри конструктора.
	4	Добавить метод _createServer для создания сервера, который возвращает emitter.
	5	Немного изменить emitter, чтобы он запускал внутренний emitter класса.
	6	Разместить маску в отдельный метод [${path}]:[${method}].
	7	Добавить метод addRoute(router) для добавления роутеров, так как у приложения может быть несколько роутеров.
	8	Получить ключи у эндпоинтов (пути) и выцепить соответствующий эндпоинт для удобства с помощью цикла forEach.
	9	Получить handler'ы и подписаться на события с помощью цикла forEach.
	10	Подписаться на событие emitter.on.
	11	Заменить все строки с путем и методом на _getRouteMask.
	12	Создать метод запуска сервера listen, в котором вызывать у сервера метод listen.
		
## 6	Переделываем index.js с имеющимися модулями Router.js и Application.js:	
	1	Импортируем модули Router.js и Application.js.
	2	Создаем экземпляр класса Router.
	3	Создаем экземпляр класса Application.
	4	Добавляем два запроса для /users и /posts.
	5	Добавляем роутер в приложение.
	6	Запускаем слушатель на указанном порту.
		
## 7	Декомпозиция endpoint'ов	
	1	Создайте папку src.
	2	В папке src создайте файл user-router.js.
	3	Импортируйте в него модуль Router.
	4	Создайте экземпляр роутера router.
	5	Создайте переменную users, которая будет содержать список объектов с id и name пользователей.
	6	Используйте метод router.get() для отправки списка пользователей на сервер.
	7	Используйте второй метод router.post(), который будет добавлять пользователей на сервер в будущем.
	8	Добавьте заголовки в методы, чтобы указать формат файла, с которым они работают.
	9	Экспортируйте роутер.
		
## 8	Переделываем index.js с имеющимся user-router.js	
	1	Создайте папку src.
	2	В папке src создайте файл user-router.js и реализуйте в нем необходимую логику для работы с пользователями.
	3	В файле index.js импортируйте класс Application и модуль user-router.js.
	4	Создайте экземпляр класса Application.
	5	Установите порт для сервера.
	6	Создайте экземпляр роутера userRouter из модуля user-router.js.
	7	Добавьте роутер userRouter в приложение.
	8	Запустите слушатель на указанном порту.
		
## 9	Делаем middleware. Метод для парсинга JSON и проставление заголовков:	
	1	Создадим файл parseJson.js.
	2	Реализуем экспортируемую функцию, которая принимает req и res.
	3	Внутри функции отправим данные data с помощью res.send.
	4	Кроме того, помимо отправки data в виде строки, нужно отправить заголовки.
	5	В классе Application реализуем добавление middleware:
	6	Добавим массив this.middleware.
	7	Добавим метод use, который будет добавлять middleware в наш массив.
	8	Добавим обработку middleware в addRoute перед вызовом обработчика (handler).
	9	Вернемся обратно в index.js и подключим middleware для парсинга JSON.
	10	Вызовем метод use у app и передадим в него функцию в качестве ссылки.
	11	Уберем прописанные заголовки в эндпоинтах, достаточно оставить только send.
		
## 10	POST запросы:	
	1	Перейдите к файлу user-router.js.
	2	Добавьте метод post, в котором будет обрабатываться тело запроса (req.body).
	3	Добавьте полученные данные к уже существующим пользователям.
	4	Отправьте обновленный массив пользователей в ответ.
	5	Для получения тела запроса в классе Application в методе _createServer используйте req.on.
	6	Создайте переменную body и добавляйте к ней каждый chunk.
	7	Повесьте слушатель на событие "end", которое возникает после прочтения всех chunk.
	8	Если тело запроса не пустое, распарсите его с помощью JSON.parse.
	9	Перенесите проверку на emitted внутрь проверки на пустое тело запроса.
		
## 11	Работа с query параметрами	
	1	Создайте файл с названием parseUrl.js.
	2	В этом файле реализуйте экспортируемую функцию, которая будет обрабатывать URL из запроса.
	3	Преобразуйте URL из запроса в объект класса URL.
	4	Добавьте модуль parseUrl.js в index.js и запустите его с помощью app.use.
	5	Получите полный маршрут с помощью замыкания (вместо /users нам нужно localhost:5000/users).
	6	Дополните parseUrl дополнительной функцией baseUrl, которую вызывайте перед основной функцией.
	7	При создании объекта URL используйте URL(req.url, baseUrl).
	8	В index.js передайте основной адрес (например, http://localhost:5000) в функцию.
	9	В логах вы получите объект, в котором есть searchParams: URLSearchParams {}.
	10	В методе _createServer в событии req.on при создании эмиттера передайте не req.url, а req.pathname.
	11	Перенесите вызов middleware в addRouter. Переместите его в _createServer перед созданием эмиттера.
	12	Перейдите к файлу parseUrl.js и присвойте req.pathname новое значение parsedUrl.pathname.
		
## 12	Обработка параметров запроса	
	1	Создайте файл с названием parseUrl.js.
	2	В этом файле реализуйте экспортируемую функцию, в которой будут обрабатываться параметры запроса из URL.
	3	Используйте класс URLSearchParams для получения параметров запроса из объекта parsedUrl.
	4	Создайте переменную params и инициализируйте ее пустым объектом.
	5	Пройдитесь циклом forEach по параметрам parsedUrl.searchParams.
	6	Используйте специфическую реализацию цикла forEach для parsedUrl.searchParams: parsedUrl.searchParams.forEach((value, key) => params[key] = value).
	7	Присвойте новые параметры из запроса req.params.
	8	Добавьте вывод логов для запросов.
	9	Добавьте фильтрацию по id, если он присутствует в запросе, и возвращайте пользователя с соответствующим id.
		
## 13	Сделать декомпозицию обработчиков endpoints	
	1	Создайте файл user-controller.js.
	2	Перенесите в него функции users, getUsers и createUser.
	3	Измените router.get на getUsers и router.post на createUser.
	4	Экспортируйте эти функции из user-controller.js.
	5	Перейдите в файл user-router.js.
	6	Импортируйте controller.
	7	В качестве обработчиков передайте соответствующие функции из контроллера.