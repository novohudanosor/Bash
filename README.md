# Bash
Скрипт на Bash
Написать скрипт для CRON, который раз в час будет формировать письмо и отправлять на заданную почту.
  Необходимая информация в письме:

Список IP адресов с (наибольшим количеством запросов) с указанием количества запросов с момента последнего
запуска скрипта;
Список запрашиваемых URL (с наибольшим количеством запросов) с указанием количества запросов с момента
последнего запуска скрипта;
Ошибки веб-сервера/приложения с момента последнего запуска скрипта;
Список всех кодов HTTP ответа с указанием их количества с момента последнего запуска скрипта;
Скрипт должен предотвращать одновременный запуск нескольких копий до его завершения;
В письме должен быть прописан обрабатываемый временной диапазон.
