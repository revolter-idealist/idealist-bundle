# Idealist Bundle

Технически это бандл для Симфони который конвертирует тексты размеченные MarkDown в более простой и удобный в восприятии и использовании вид. Основная цель - упростить по максимуму путь идеи от мысли к печати на бумаге и рапространению в обществе.

В целом, этого бандла достаточно для развертывания полноценной точки доступа к текстам на любом проекте Symfony или Laravel - любом другом проекте использующем компоненты Симфони. Работа продолжается. Для подключения к проекту, добавьте зависимость через composer.json и импортируйте или настройте роуты для текстов.

## Установка и настройка

`composer require "revolter/idealist-bundle:dev-master"`
или
`composer require revolter/idealist-bundle`

Затем добавьте инициализацию бандла в `app/AppKernel.php`
`new Revolter\IdealistBundle\RevolterIdealistBundle(),`

После установки код бандла будет в одноименной папке вендоров вашего проекта. Обычно это папка `vendor` в корне. И весь путь тогда `vendor/revolter/idealist-bundle`. Там будет два файла, которые необходимо будет скопировать отдельно.
* base.html.twig - основной шаблон страницы проекта. Ссылается на файл счетчиков в коде
    * counters.html.twig - файл для счетчиков статистики

Вариантов три. Рекоммендуется сделать ссылку на `base.html.twig` и скопировать, а затем отредактировать `counters.html.twig` добавив в него свои счетчики статистики. 
* Если счетчики не нужны, делайте ссылки на оба файла
* Если нужен свой дизайн, копируйте оба

Для примера, если ваш проект на Symfony вам нужно скопировать оба файла в папку `app/Resources/views`. Таким образом вы сможете отредактировать стандартный дизайн проекта как посчитаете нужным. И добавить свои счетчики статистики, да всё что угодно. Бандл отвечает только за генерацию текста/контента и генерирование текста для печати, других форматов. При копировании/изменении `base.html.twig` усложнится возможность обновления. 

Настройка требует указания стандартных параметров для бандла, например текст по умолчанию(index_idea), который будет открыт при обращении к главной странице. Параметры указываются в *parameters.yml* вашего проекта.
```
    index_file: README # Имя файла текста
    index_idea: revolter-social-project # Текст по умолчанию
    index_name: info # Ссылка по умолчанию
    path.web: '%kernel.root_dir%/../web' # Папка веб
    path.idea: '' # Папка идеи
    path.texts: '%path.web%/texts' # Папка текстов
``` 

## Стили, картинки, скрипты

Так как бандл подразумевает изменение дизайна, стили и фоновые картинки, как и шаблоны их использующие, не устанавливаются автоматически. Оформление находится в папке `Resources/public` бандла.

* `php app/console assets:install --symlink` - эта команда автоматически копирует или делает ссылки на файлы ресурсов. Опция `--symlink` делает простую ссылку на ресурсы вместо копирования файлов. Таким образом, можно быть спокойным при обновлении бандла, так как в иных случаях, вам необходимо обновлять ранее скопированные файлы. При установке ссылки, копировать файлы заново после обновления нет необходимости.
    * В шаблонах путь к `css` файлам генерируется автоматически исходя из настроек *assets* вашего проекта. Вы можете настроить эту часть конфигурации проекта, либо изменить способ доступа к стилям, либо захардкодить пути стилей в шаблонах.
* Вы можете скопировать ее содержимое руками в веб корень проекта, обычно это папка `web`. Таким образом у вас добавится как минимум папка `web/static` со стилями и фонами для шаблонов. Вы сможете редактировать стили, и вам нужно будет поправить в шаблоне ссылку на `css` файл, но будет уже проблематично обновляться потом.


## Добавление текстов

Тексты подключаются из самостоятельных репозиториев - каждая идея в своем репозитории, свой проект. Бандл соотносит название текста и роута. Тексты необходимо расположить в папке `texts` корневой публичной папке доступной из веб. В проектах симфони это папка `web` вашего проекта.

Есть как минимум три способа, как подключить тексты.
* Самое простое, скопировать файлы в нужные папки. Один минус, обновление невозможно автоматизировать, и придется обновлять вручную каждый раз. Это будет основной причиной потери актуальности текстов на вашем сайте.
* Командой `git clone <repo> <web/texts/name-repo>` клонировать репозиторий `<repo>` в нужное место `<web/texts/name-repo>` где *web/texts* папка для текстов, а *name-repo* это название текста/репозитория. Например `git clone https://github.com/revolter-idealist/distributed-community web/texts/distributed-community` для текстов о распределенных Сообществах. 
    * Для обновления нужно будет перейти в папку с текстом и выполнить команду `git pull` - новый текст загрузится/обновится автоматически. 
    * Плюс в том, что вы также сможете работать с текстом самостоятельно прямо в этой папке, и отправлять изменения на сервер.
    * Минус - нужно выполнять команды для каждого текста. Верояно будет конфликт, если ваш основной проект также под контролем версий.
* Добавить текст как подмодуль к вашему проекту `git submodule add <repo> <web/texts/name-repo>`. Команда почти идентична предыдущей, но дает несколько преимуществ.
    * Можно обновить все тексты одной командой `git submodule update`
    * Без конфликтов с корневым репозиторием проекта. Подмодули дополняют его.

Лучше всего добавлять тексты именно как подмодули. Тогда легко работать с текстом из его папки прямо в вашем проекте. Если вы только загружаете тексты, легко будет обновить их все до актуальных состояний одной командой. После публикации своего проекта в таком случае, его легко можно склонировать и развернуть всего одной командой весте с подмодулями добавленных ранее текстов. Это требует знание трех команд гита, но более простого способа я еще не нашел.

## FAQ - Вопрос/Ответ

*Почему папка текстов в публичном доступе?* - Сама публичность будет зависеть от настроек вашего сервера/хостинга. Желательно всёже сделать папку текстов доступной. Тогда можно будет
* Получить доступ к исходникам/текстам напрямую
* Можно будет скопировать/склонировать репозитории текстов

*Не легко как-то с подмодулями* - Подмодули дают преимущество при создании цельного проекта текстов и его последующего клонирования в распределенной системе одной командой. Другого способа добиться этой цели мне пока не известно.
