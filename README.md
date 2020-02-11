#Основная система

###Клонируем ссылку на репозиторий

`git clone https://git.involta.ru/univer/Inuniver.ru.git`

###Устанавливаем проект:

`composer install`

###Убедиться, что в корне есть директория src с правами на чтение и запись.
###Если отстутсвует

`mkdir src`

###Убедиться, что в /src/ есть папки: ckeditor, dpocourses, files_backup, lib, logotips, news, other, personaldata, tmp, usrs с правами на чтение и запись. Если нет, то создать:

`cd src`
		
`mkdir ckeditor dpocourses files_backup lib logotips news other personaldata tmp usrs`

###Создать папку logs в /applications/myuni/ и /applications/webinars/ с правами на запись

`mkdir application/myuni/logs`
`mkdir application/webinars/logs`

###Убедиться, что /applications/myuni/cache достуна для чтения и записи и существует, если нет, то создать

`mkdir application/cache`

###Создать папку i18n в /applications/myuni/cache/ с правами на чтение и запись

`mkdir application/myuni/cache/i18n`

###Проверить создан ли файл /applications/myuni/config/database.php по аналогии с /applications/myuni/config/database.default.php, если нет, то создать на основании файла /applications/myuni/config/database.default.php:

`cd applications/myuni/config`
`cp database.default.php database.php`

###Проверить создан ли файл applications/config.json, если нет, то создать его по аналогии с файлом applications/config.default.json

`cd applications`
`cp config.default.json config.json`

###Настроить права(выполнять в корневой папке):

`chown -R www-data:www-data src applications/myuni/logs applications/webinars/logs applications/myuni/cache`

#Установка базы данных

`sudo -u postgres psql`
	
`CREATE ROLE "dolero-user" WITH LOGIN PASSWORD 'PASS_word';`
###где dolero-user - ваше имя пользователя PASS_word - пароль для пользователя (по умолчанию postgres - пользователь, "" - пароль) если пользователь был создан не по умолчанию, изменить данные в файле application/config.json на нужные

`sudo -u postgres createdb -O dolero-user dolero-base`
###где dolero-user - имя пользователя dolero-base - имя БД

`sudo systemctl restart postgresql`

`psql -h localhost -U dolero-user -d dolero-base -f DDL_inuniver.sql`

`psql -h localhost -U dolero-user -d dolero-base -f DDL_webinars.sql`

`psql -h localhost -U dolero-user -d dolero-base -f DUMP_inuniver.sql`
		
`psql -h localhost -U dolero-user -d dolero-base -f DUMP_webinars.sql`

###После заполнения выполнить миграцию.  Для этого просто нужно перейти по ссылке http://inuniver.ru/migrate (inuniver.ru - название нужного домена)

###Войти под администратором и перейти на страницу /localization/phrases и нажать кнопку «Обновить кеш» 

###Загрузить файл с логотипом университета в /media/imgs/ . Файл должен иметь название logo_100.png

#Установка и настройка nodejs
		
`sudo apt install nodejs`

`sudo apt install npm`
	
###В директории /nodejs/config создать файлы config.js и database.js (по аналогии с config.default.js и database.default.js)

`cp config.default.js config.js`

`cp database.default.js database.js`

###Устанавливаем foreverjs

`npm i forever -g`

###В директории /nodejs/ выполнить:  
		
`forever start index.js`

`forever start mobile_notifications.js`

###Добавить в крон задания(домен заменить на нужный):
		
`crontab -e`

###https://inuniver.ru/cron/cleantmpfolder?key=866a8a5205b9e6cdd08be349d03fc8b7b8c3c12c (запускать раз в 10 минут)

`*/10 * * * * https://inuniver.ru/cron/cleantmpfolder?key=866a8a5205b9e6cdd08be349d03fc8b7b8c3c12c`
			
###https://inuniver.ru/cron/setLastVisitCourseAlerts?key=866a8a5205b9e6cdd08be349d03fc8b7b8c3c12c (запускать раз в 2 часа)

`*/10 * * * * https://inuniver.ru/cron/setLastVisitCourseAlerts?key=866a8a5205b9e6cdd08be349d03fc8b7b8c3c12c`			

###https://inuniver.ru/cron/transferStudents?key=866a8a5205b9e6cdd08be349d03fc8b7b8c3c12c (запускать 1 раз в год 1 августа в любое время)
	
`* * 1 8 * https://inuniver.ru/cron/transferStudents?key=866a8a5205b9e6cdd08be349d03fc8b7b8c3c12c`
	
###https://inuniver.ru/cron/sendingMessages?key=866a8a5205b9e6cdd08be349d03fc8b7b8c3c12c (запускать раз в 5 минут)
		
`*/5 * * * * https://inuniver.ru/cron/sendingMessages?key=866a8a5205b9e6cdd08be349d03fc8b7b8c3c12c`

###https://inuniver.ru/cron/setExamAlerts?key=866a8a5205b9e6cdd08be349d03fc8b7b8c3c12c (запускать раз в день ночью)

`30 22 * * * https://inuniver.ru/cron/sendingMessages?key=866a8a5205b9e6cdd08be349d03fc8b7b8c3c12c`

#Установка Big Blue Button
		
###Изменить подключение к (свойства $secret и $apiUrl) BBB в файлах: 

###/applications/myuni/libraries/My_webinars.php  

###/applications/webinars/libraries/Webinars_api.php

#Учетная запись администратора

`admin@inuniver.ru`

`5cd2f0dm3`






