# NetBox Device Type Library

## About this Library

Эта библиотека предназначена для заполнения типов устройств в [NetBox](https://github.com/netbox-community/netbox). Она содержит набор определений типов устройств, выраженных в формате YAML и упорядоченных по производителю. Каждый файл представляет отдельный тип физического устройства (например, марку и модель). Эти определения можно загрузить в NetBox вместо создания определений новых типов устройств вручную.


Если вы хотите внести свой вклад в эту библиотеку, пожалуйста, прочитайте нашу [руководство по содействию](CONTRIBUTING.md) перед отправкой контента.

**Примечание. С марта 2023 года Netbox-Device-Type-Library-Import был перенесен в организацию сообщества NetBox. Мы будем работать над тем, чтобы в ближайшее время обеспечить полную поддержку.**
Если вы хотите автоматизировать импорт этих файлов шаблонов типов устройств, существует скрипт Python на базе сообщества NetBox Community ~~**community based**~~, который будет проверять наличие дубликатов, 
позволит вам выборочно импортировать поставщиков и т. д., доступный здесь [netbox-community/Device-Type-Library-Import](https://github.com/netbox-community/Device-Type-Library-Import).
 ~~** Примечание:** Это никаким официальным образом не связано с NetBox, и вы не получите здесь поддержки.~~

## Определения типа устройства

Каждое определение **должно** включать, как минимум, следующие поля:

- `manufacturer`: Название производителя, выпускающего данный тип устройства.
  - Тип: Строка
- `model`: Номер модели, соответствующий типу устройства. Он должен быть уникальным для каждого производителя.
  - Тип: Строка
- `slug`: Удобное для использования в URL-адресе представление номера модели. Как и номер модели, оно должно быть уникальным для 
каждого производителя. Перед всеми слитками должно быть указано название производителя с тире, пожалуйста, смотрите пример ниже.
  - Тип: Строка
  - Шаблон: `"^[-a-zA-Z0-9_]+$"`. Должны совпадать со следующими символами: `-`, `_`, прописными или строчными буквами от `a` до `z`, цифрами от `0` до `9`.

:test_tube: Пример:

  ```yaml
  manufacturer: Dell
  model: PowerEdge R6515
  slug: dell-poweredge-r6515
  ```

**При необходимости** могут быть объявлены следующие поля:

- `part_number`: Альтернативное представление номера модели (например, артикул). (**По умолчанию: Нет**)
  - Тип: Строка
  - :test_tube: Пример: `part_number: D109-C3`
- `u_height`: Высота типа устройства в стойках. Поддерживаются значения с шагом 0,5U. (**По умолчанию: 1**)
  - Тип: число (минимум `0`, кратное `0,5`)
  - :test_tube: Пример: `u_height: 12.5`
- `is_full_depth`: Логическое значение, указывающее, используется ли для данного типа устройства как передняя, так и задняя поверхности стойки. (** По умолчанию: true**)
  - Тип: Логический
  - :test_tube: Пример: `is_full_depth: false`
- `airflow`: Описание схемы воздушного потока для устройства. (**По умолчанию: Нет**)
  - Тип: Строка
  - Опции:
    - `front-to-rear`
    - `rear-to-front`
    - `left-to-right`
    - `right-to-left`
    - `side-to-rear`  
    - `passive`
  - :test_tube: Пример: `airflow: side-to-rear`
- `front_image`: Указывает, что у этого устройства есть изображение  спереди в папке [elevation-images] (elevation-images/). (**По умолчанию: None(Нет)**)
  - ПРИМЕЧАНИЕ: Для папки с изображениями высот требуется то же имя папки, что и для этого устройства. Имя файла также должно совпадать с <VALUE_IN_SLUG>.front.<acceptable_format>
  - Тип: Логический
  - :test_tube: Пример: `front_image: True`
- `rear_image`: Указывает, что у этого устройства есть изображение  сзади в папке [elevation-images] (elevation-images/). (**По умолчанию: Нет**)
  - ПРИМЕЧАНИЕ: Для папки с изображениями высот требуется то же имя папки, что и для этого устройства. Имя файла также должно совпадать с <VALUE_IN_SLUG>.rear.<acceptable_format>
  - Тип: Логический
  - :test_tube: Пример: `rear_image: True`
- `subdevice_role`: Указывает, что это `parent`(родительское) или `child`(дочернее) устройство. (**По умолчанию: None(Нет)**)
  - Тип: Строка
  - Опции:
    - `parent`
    - `child`
  - :test_tube: Пример: `subdevice_role: parent`
- `comments`: Строковое поле, позволяющее добавлять комментарии к устройству. (**По умолчанию: Нет**)
  - Тип: Строка
  - :test_tube: Пример: `comments: This is a comment that will appear on all NetBox devices of this type`
- `is_powered`: Логическое значение, указывающее, не потребляет ли питание устройство данного типа. В основном используется в качестве обходного пути для проверки работоспособности устройств, не являющихся устройствами (например, комплектов для монтажа в стойку для настольных устройств) (** По умолчанию: True**)
  - Тип: Логический
  - :test_tube: Пример: `is_powered: false`
- `weight`: Число, представляющее числовое значение веса. Должно быть кратно 0,01 (2 знака после запятой). (**По умолчанию: Нет**)
  - Тип: Номер
  - Значение: должно быть кратно 0,01
- `weight_unit`: Строка, определяющая единицу измерения. Это должно быть одно из поддерживаемых значений. (**По умолчанию: Нет**)
  - Тип: Строка
  - Значение: Перечисленные ниже параметры
    - kg
    - g
    - lb
    - oz
  - :test_tube: Пример:

    ```yaml
    weight: 12.21
    weight_unit: lb
    ```

Для получения более подробной информации об этих и перечисленных ниже атрибутах, пожалуйста, обратитесь к
[определениям схемы](schema/) и [Определениям компонентов](#component-definitions), приведенным ниже.

### Определения компонентов

Ниже перечислены допустимые типы компонентов. Для каждого типа компонента необходимо указать список шаблонов отдельных компонентов, которые будут добавлены.

- [console-ports](#console-ports "Availible in NetBox 2 and later")
- [console-server-ports](#console-server-ports "Availible in NetBox 2.2 and later")
- [power-ports](#power-ports "Availible in NetBox 1.7 and later")
- [power-outlets](#power-outlets "Availible in NetBox 2 and later")
- [interfaces](#interfaces "Availible in all versions of NetBox")
- [front-ports](#front-ports "Availible in NetBox 2.5 and later")
- [rear-ports](#rear-ports "Availible in NetBox 2.5 and later")
- [module-bays](#module-bays "Availible in NetBox 3.2 and later")
- [device-bays](#device-bays "Availible in all versions of NetBox")
- [inventory-items](#inventory-items "Availible in NetBox 3.2 and later")

Ниже перечислены доступные поля для каждого типа компонента.

#### Console Ports

**[Документация](https://docs.netbox.dev/en/stable/models/dcim/consoleport/)**

Консольный порт обеспечивает подключение к физической консоли устройства. Обычно он используется для временного доступа пользователя, физически находящегося рядом с устройством, или для удаленного внеполосного доступа, предоставляемого через сетевой консольный сервер.

- `name`: Имя
- `label`: Метка
- `type`: тип записи slug (массив)
- `poe`: Обеспечивает ли этот порт доступ к POE? (Логическое значение)

#### Console Server Ports

**[Документация](https://docs.netbox.dev/en/stable/models/dcim/consoleserverport/)**

Консольный сервер - это устройство, которое обеспечивает удаленный доступ к локальным консолям подключенных устройств. Обычно они используются для обеспечения удаленного внеполосного доступа к сетевым устройствам и, как правило, для подключения к консольным портам.

- `name`: Имя
- `label`: Метка
- `type`: тип записи slug (массив)

#### Power Ports

**[Документация](https://docs.netbox.dev/en/stable/models/dcim/powerport/)**

Порт питания - это компонент устройства, который получает питание от какого-либо внешнего источника (например, от сетевой розетки в шкафу) и, как правило, представляет собой внутренний источник питания устройства.

- `name`: Имя
- `label`: Метка
- `type`: тип записи slug (массив)
- `maximum_draw`: Максимальная потребляемая мощность порта в ваттах (опционально)
- `allocated_draw`: Потребляемая мощность порта, Вт (опционально)

#### Power Outlets

**[Документация](https://docs.netbox.dev/en/stable/models/dcim/poweroutlet/)**

Розетки питания представляют собой розетки на блоке распределения питания (PDU) или другом устройстве, которое подает питание на зависимые устройства. Каждому порту питания может быть присвоен физический тип, и он может быть связан с определенным участком питания (где используется трехфазное питание) и/или с определенным входным портом питания. Эта связь может быть использована для моделирования распределения мощности внутри устройства.

- `name`: Имя
- `label`: Метка
- `type`: тип записи slug (массив)
- `power_port`: Название порта питания на устройстве, от которого питается данная розетка (необязательно)
- `feed_leg`: Фаза (ответвление) питания, к которой подключена данная розетка; A, B или C (опционально)

#### Interfaces

**[Документация](https://docs.netbox.dev/en/stable/models/dcim/interface/)**

Iинтерфейсы в NetBox представляют собой сетевые интерфейсы, используемые для обмена данными с подключенными устройствами. В современных сетях чаще всего используется Ethernet, но поддерживаются и другие типы. Интерфейсам могут быть назначены IP-адреса и VLAN.

- `name`: Имя
- `label`: Метка
- `type`: Тип интерфейса slug (массив)
- `mgmt_only`: Логическое значение, указывающее, используется ли этот интерфейс только для целей управления (по умолчанию: false).

#### Front Ports

**[Документация](https://docs.netbox.dev/en/stable/models/dcim/frontport/)**

Передние порты - это сквозные порты, которые представляют собой физические кабельные соединения, составляющие часть более длинного пути. Например, порты на передней панели коммутационной панели UTP могут быть смоделированы в NetBox как передние порты. Каждому порту присваивается физический тип, и он должен быть сопоставлен с определенным задним портом на одном и том же устройстве. Один задний порт может быть сопоставлен с несколькими передними портами, используя числовые позиции для указания конкретного расположения каждого из них.

- `name`: Имя
- `label`: Метка
- `type`: тип записи slug (массив)
- `rear_port`: Название заднего порта на данном устройстве, к которому подключен передний порт
- `rear_port_position`: Соответствующее положение на подключенном заднем порту (по умолчанию: 1)

#### Rear Ports

**[Документация](https://docs.netbox.dev/en/stable/models/dcim/rearport/)**

Как и передние порты, задние порты являются сквозными и представляют собой продолжение пути от одного кабеля к другому. Каждый задний порт определяется своим физическим типом и количеством позиций: задние порты с несколькими позициями могут быть сопоставлены с несколькими передними портами. Это может быть полезно для моделирования случаев, когда несколько трактов используют общий кабель (например, шесть дискретных двухжильных оптоволоконных соединений используют 12-жильный кабель MPO).

- `name`: Имя
- `label`: Метка
- `type`: тип записи slug (массив)
- `positions`: Количество передних портов, которые могут быть подключены к этому заднему порту (по умолчанию: 1)
- `poe`: Обеспечивает ли этот порт доступ к POE? (Логическое значение)

#### Module Bays

**[Документация](https://docs.netbox.dev/en/stable/models/dcim/modulebay/)**

Отсеки для модулей представляют собой пространство или слот внутри устройства, в который может быть установлен модуль, заменяемый в полевых условиях. Распространенным примером является коммутатор на базе шасси, такой как Cisco Nexus 9000 или Juniper EX9200. Модули, в свою очередь, содержат дополнительные компоненты, которые становятся доступными для родительского устройства.

- `name`: Имя
- `label`: Метка
- `position`: Буквенно-цифровое положение, в котором этот модульный отсек расположен на родительском устройстве. При создании компонентов модуля строка "{module}" в названии компонента будет заменена на `position` модульного отсека. Более подробную информацию смотрите в [NetBox Documentation](https://docs.netbox.dev/en/stable/models/dcim/moduletype/#automatic-component-renaming).

#### Device Bays

**[Документация](https://docs.netbox.dev/en/stable/models/dcim/devicebay/)**

Отсеки для устройств представляют собой пространство или слот в родительском устройстве, в котором может быть установлено дочернее устройство. Например, в родительском корпусе размером 2U могут размещаться четыре отдельных блейд-сервера. Шасси будет отображаться на уровне стойки как устройство размером 2U с четырьмя отсеками для устройств, а каждый сервер в нем будет определяться как устройство размером 0U, установленное в одном из отсеков для устройств. Дочерние устройства не отображаются на уровнях стойки и не учитываются как потребляющие устройства в стойке.

Дочерние устройства сами по себе являются первоклассными устройствами: это полностью независимые управляемые объекты, которые не имеют общего уровня управления с родительским устройством. Как и обычные устройства, дочерние устройства имеют свою собственную платформу (ОС), роли, теги и компоненты. Интерфейсы LAG не могут группировать интерфейсы, принадлежащие разным дочерним устройствам.

- `name`: Имя
- `label`: Метка

#### Inventory Items

**[Документация](https://docs.netbox.dev/en/stable/models/dcim/inventoryitem/)**

Инвентарные позиции представляют собой аппаратные компоненты, установленные в устройстве, такие как блок питания, центральный процессор или сетевая плата. Они предназначены в первую очередь для целей инвентаризации.

Элементы инвентаря являются иерархическими по своей природе, так что любой отдельный элемент может быть назначен родительским для других элементов. Например, элемент инвентаря может быть создан для представления линейной карточки, в которой содержится несколько оптических элементов SFP, каждый из которых существует как дочерний элемент в устройстве. Элемент инвентаря также может быть связан с определенным компонентом в том же устройстве. Например, вы можете захотеть связать трансивер с интерфейсом.

- `name`: Имя
- `label`: Метка
- `manufacturer`: Название производителя, который производит данное изделие
- `part_id`: Идентификатор ID детали, присвоенный производителем

## Data Validation / Commit Quality Checks

There are two ways this repo focuses on keeping quality device-type definitions:

- **Предварительные проверки** - Необязательно, но **настоятельно рекомендуется**, для помощи в выявлении простых вопросов перед совершением.(Trainling-Whitespace, фиксера, Check-yaml, Yamlfmt, Yamllint)
  - Установка
    - Виртуальная среда маршрута
      - Рекомендуется создать виртуальную Env для вашего репо (`python3 -m venv venv`)
      - Установите необходимые пакеты PIP (`pip install -r requirements.txt`)
      - Продолжить до "Install `pre-commit` Hooks"
    - `pre-commit` Только маршрут
      - [Install pre-commit](https://pre-commit.com/#install) (`pip install pre-commit`)
    - Установите `pre-commi' крючки
      - Чтобы установить сценарий перед коммитацией: `pre-commit install`
  - Использование и полезные команды `pre-commit
    - After staging your files with `git`, to run the pre-commit script on changed files: `pre-commit run`
    - To run the pre-commit script on all files: `pre-commit run --all`
    - To uninstall the pre-commit script: `pre-commit uninstall`
  - Learn more about [pre-commit](https://pre-commit.com/)
- **GitHub Actions** - Automatically run before a PR can be merged. Repeats yamllint & validates against NetBox Device-Type Schema.
