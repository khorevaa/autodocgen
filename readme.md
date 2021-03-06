# AutodocGen

Инструмент генерации документации на основании информации файлов исходных кодов конфигурации 1С:Предприятие.

## Об инструменте

Инструмент позволяет

- автоматически сформировать документацию на основании исходных файлов конфигурации 1С:Предприяти
- выполнить проверку на возможность корректного разбора информации, выступающей исходными данными для документации
- опубликовать сгенерированную документациию

## Установка

Процесс установки стандартен

- либо `opm install autodocgen`
- либо, если в opm приложения почему-то нет, то скачать архив, распаковать и выполнить (для windows) `installlocalhost.bat`

## Использование

Основные возможности есть в справке. Отдельно стоит обратить внимание на конфигурационный файл.

Конфигурационный файл соответствует структуре единого конфигурационного файла, ниже приведен пример

    {
        "GLOBAL": {
            "КаталогИсходныхФайлов": "src\\configuration",
            "version": "1.0"
        },
        "AutodocGen":{
            "НастройкиConluence": {
                "АдресСервера":"https://my-confluence.myhost.ru",
                "Пользователь":"user",
                "Пароль":"password",
                "Пространство":"key",
                "КорневаяСтраница":"Имя кореневой страницы в пространстве key",
                "ПутьКШаблонам": "",
                "АнализироватьТолькоПотомковПодсистемы": "МояКорневаяПодсистема"
            },
            "НастройкиHTML": {
                "ПутьКШаблонам": "",
                "КаталогПубликации": "./doc",
                "АнализироватьТолькоПотомковПодсистемы": "МояКорневаяПодсистема"
            },
            "ПоследнийОбработанныйКоммит": ""
        }
    }

Располагать конфигурационный файл стоит в корне репозитория под именем `v8config.json`

## Поддерживаемые варианты генерации автодокументации

Как видно из пример конфигурационного файла, поддерживаются 2 формата (ключ `-format`)

- `confluence` - генерирование страниц в указанном пространстве [confluence](https://ru.atlassian.com/software/confluence)
- `html` - генерация структуры каталогов в соответствии с подсистемами и файлов-страниц в каталогах.

Для добавления новых стоит воспользоваться шаблоном [src/Классы/ШаблонГенераторДокументации.os-template](src/Классы/ШаблонГенераторДокументации.os-template)

## Подготовка конфигурации

Для генерирования документации, конфигурация должна соответствовать требованиям

- Все модули должны иметь определенную структуру областей (в соответствии с требованиями 1С)
- В документацию добавляются только экспортные методы, находящиеся в разделе `ПрограммныйИнтерфейс`
- Описание методов должно соответствовать требованиям оформления кода
- Поддерживаются общие модуи и модули менеджеров объектов
- Все модули / объекты, которые попадают под правила автодокументирования должны располагаться в соответствующих подсистемах. Принятая структура:

```

    Подсистемы конфигурации
    |
    +-- КорневаяПодсистема (не выводится в интерфейс пользователя)
        |
        +-- Раздел
            |
            +-- Подсистема

```