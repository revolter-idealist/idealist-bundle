# Idealist Bundle

Технически это бандл для Симфони который конвертирует тексты размеченные MarkDown в более простой и удобный в восприятии и использовании вид. Основная цель - упростить по максимуму путь идеи от мысли к печати на бумаге и рапространению в обществе.

В целом, этого бандла достаточно для развертывания полноценной точки доступа к текстам на любом проекте Symfony или Laravel - любом другом проекте использующем компоненты Симфони. Работа продолжается. Для подключения к проекту, добавьте зависимость через composer.json и импортируйте или настройте роуты для текстов.

`composer require revolter/idealist-bundle`

Тексты подключаются из самостоятельных репозиториев - каждая идея в своем репозитории, свой проект. Бандл соотносит название текста и роута.

