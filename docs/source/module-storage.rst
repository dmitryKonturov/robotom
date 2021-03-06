Документация Хранилища и БД
=============================
Весь код написан на ``Python 2.7``

* Подключение к хранилищу:

	``ssh mardanov@109.234.34.140``

* В качестве БД используется:

	`MongoLab <https://mongolab.com/>`_

* Все запросы имеют следующий вид:

	**URI:** ``http://109.234.34.140:<номер порта>/storage/<к чему адресован запрос>/<сам запрос>``

* Например(функция возвращающая данные о пользователе):

	**URI:** ``http://109.234.34.140:5006/storage/users/get``

В случае ошибки будет возвращено:
	**400 - Not found**
	**404 - Incorrect format**
	**500 - Internal error**

Формат принимаемых запросов:
	Все запросы на вход принимают **json строку** и имеют тип запроса: **POST**

Все функции храняться в 3 файлах формата ``.py``:
	* main

	* pyframes

	* yfileSystem

Список функций **main.py**:

Для работы с пользователями:
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Создание пользователя
""""""""""""""""""""""""""""""""""""

**URI:** ``http://109.234.34.140:5006/storage/users/post``

На вход получает джейсон стоку формата:

	.. code-block:: python

		{
		"tag": "12345",
		"role": "admin",
		"user": "Alexander"
		}

где tag - уникальный идентификатор(в текущий момент никак не используется), role - роль пользователя, name - имя пользователя

На выход возвращает json файл с записью result: success

Просмотр данных пользователя/пользователей
""""""""""""""""""""""""""""""""""""""""""""""""""""""

**URI:** ``http://109.234.34.140:5006/storage/users/get``

На вход получает джейсон стоку, в которой указан один или несколько параметров поиска, или all(для вывода всех пользователей)(например "user": "Alexander")

На выход возвращает json файл со всеми данными о пользователе(или всех пользователях)

Обновленние данных о пользователе
""""""""""""""""""""""""""""""""""""""""""""""""""""""

**URI:** ``http://109.234.34.140:5006/storage/users/update``

На вход получает джейсон стоку, в которой указано имя пользователя(по анологии с post запросом, например "user": "Alexander", "role": "experimenter")

На выход возвращает json файл с записью updated: <имя пользователя>

Удаление пользователя
""""""""""""""""""""""""""""""""""""

**URI:** ``http://109.234.34.140:5006/storage/users/delete``

На вход получает джейсон стоку, в которой указано имя пользователя(по анологии с post запросом, например "user": "Alexander")

На выход возвращает json файл с записью deleted: <имя пользователя>


Для работы с экспериментом:
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
		
Создание эксперимента:
""""""""""""""""""""""""""""""""""""

**URI:** ``http://109.234.34.140:5006/storage/experiments/post``

На вход получает джейсон стоку формата:

	.. code-block:: python

		{
		    "experiment parameters": {
			"DARK": {
			    "count": 1,
			    "exposure": 1000
			},
			"DATA": {
			    "angle step": 36,
			    "count per step": 1,
			    "step count": 10,
			    "exposure": 6000
			},
			"advanced": false,
			"EMPTY": {
			    "count": 1,
			    "exposure": 1000
			}
		    },
		    "tags": "microsd",
		    "specimen": "microsd",
		    "experiment id": "ca91a2f2-d9ea-427d-8c80-eaf5eb0980e7",
		    "finished": false
		}

На выход возвращает json файл с записью result: success или result: experiment<id эксперимента> already exists in file system

Просмотр данных об эксперименте
""""""""""""""""""""""""""""""""""""""""""""""

**URI:** ``http://109.234.34.140:5006/storage/experiments/get``

На вход получает джейсон стоку, в которой указан <id эксперимента> или all(для вывода всех экспериментов)

На выход возвращает json файл со всеми данными об эксперименте(или все эксперементы)

Обновление данных экспперимента:
""""""""""""""""""""""""""""""""""""""""""""""
**URI:** ``http://109.234.34.140:5006/storage/experiments/put``

На вход получает джейсон стоку, в которой указаны поля, которые надо обновить

На выход возвращает json файл с записью result: success

Удаление данных эксперимента:
""""""""""""""""""""""""""""""""""""

**URI:** ``http://109.234.34.140:5006/storage/experiments/delete``

На вход получает джейсон стоку, в которой указан <id эксперимента>

На выход возвращает json файл с записью deleted: кол-во удаленных экспериментов 



Для работы с фреймами:
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Создание нового фрейма:
""""""""""""""""""""""""""""""""""""

**URI:** ``http://109.234.34.140:5006/storage/frames/post``

На вход получает джейсон стоку, в которой указан

	.. code-block:: python

		{
		    "exp_id": "c5e04c21-f912-4419-aa55-1a7f7ecadacd",
		    "frame": {
			"object": {
				"horizontal position": 0,
				"angle position": 19.99504643962848,
		      		"vertical position": 0,
				"present": true
			},
			"number": 6,
			"X-ray source": {
			    "current": 20,
			    "voltage": 40
			},
			"shutter": {
			    "open": true
			},
			"mode": "data",
			"image_data": {
			    "detector": {
				"model": "Ximea xiRAY"
			    },
			    "datetime": "22.05.2015 13:20:05",
			    "exposure": 10000
			}
		    },
		    "type": "frame"
		}

На выход возвращает json файл с записью result: success
		
Получение фрейма в виде массива чисел:
""""""""""""""""""""""""""""""""""""""""""""""""""""""""

**URI:** ``http://109.234.34.140:5006/storage/frames/get``

На вход получает джейсон стоку, в которой указан id эксперимента и id фрейма

На выход возвращает json файл со всеми данными о данном фрейме

Получение информации о фрейме:
""""""""""""""""""""""""""""""""""""

**URI:** ``http://109.234.34.140:5006/storage/frames_info/get``

На вход получает джейсон стоку, в которой указан id эксперимента и id фрейма

На выход возвращает json файл с информацией о фрейме

Возвращает фреймы в формане ``.png``:
""""""""""""""""""""""""""""""""""""""""""""""""""""""""

**URI:** ``http://109.234.34.140:5006/storage/png/get``

На вход получает джейсон стоку, в которой указан id эксперимента и id фрейма

На выход возвращает файл формата .png


В **pyfileSystem.py** функции создания и удаления экспериментов в файловой системе.
В **pyframes.py** хранятся тела функций добавления, получения, удаления из h5 архивов, а так же конвертирования в .png формат 



		















