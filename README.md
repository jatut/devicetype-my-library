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

**[Documentation](https://docs.netbox.dev/en/stable/models/dcim/consoleport/)**

Консольный порт обеспечивает подключение к физической консоли устройства. Обычно он используется для временного доступа пользователя, физически находящегося рядом с устройством, или для удаленного внеполосного доступа, предоставляемого через сетевой консольный сервер.

- `name`: Имя
- `label`: Метка
- `type`: тип записи slug (массив)
- `poe`: Обеспечивает ли этот порт доступ к POE? (Логическое значение)

#### Console Server Ports

**[Documentation](https://docs.netbox.dev/en/stable/models/dcim/consoleserverport/)**

A console server is a device which provides remote access to the local consoles of connected devices. They are typically used to provide remote out-of-band access to network devices, and generally connect to console ports.

- `name`: Имя
- `label`: Метка
- `type`: тип записи slug (массив)

#### Power Ports

**[Documentation](https://docs.netbox.dev/en/stable/models/dcim/powerport/)**

A power port is a device component which draws power from some external source (e.g. an upstream power outlet), and generally represents a power supply internal to a device.

- `name`: Имя
- `label`: Метка
- `type`: тип записи slug (массив)
- `maximum_draw`: The port's maximum power draw, in watts (optional)
- `allocated_draw`: The port's allocated power draw, in watts (optional)

#### Power Outlets

**[Documentation](https://docs.netbox.dev/en/stable/models/dcim/poweroutlet/)**

Power outlets represent the outlets on a power distribution unit (PDU) or other device that supplies power to dependent devices. Each power port may be assigned a physical type, and may be associated with a specific feed leg (where three-phase power is used) and/or a specific upstream power port. This association can be used to model the distribution of power within a device.

- `name`: Имя
- `label`: Метка
- `type`: тип записи slug (массив)
- `power_port`: The name of the power port on the device which powers this outlet (optional)
- `feed_leg`: The phase (leg) of power to which this outlet is mapped; A, B, or C (optional)

#### Interfaces

**[Documentation](https://docs.netbox.dev/en/stable/models/dcim/interface/)**

Interfaces in NetBox represent network interfaces used to exchange data with connected devices. On modern networks, these are most commonly Ethernet, but other types are supported as well. IP addresses and VLANs can be assigned to interfaces.

- `name`: Имя
- `label`: Метка
- `type`: Interface type slug (Array)
- `mgmt_only`: A boolean which indicates whether this interface is used for management purposes only (default: false)

#### Front Ports

**[Documentation](https://docs.netbox.dev/en/stable/models/dcim/frontport/)**

Front ports are pass-through ports which represent physical cable connections that comprise part of a longer path. For example, the ports on the front face of a UTP patch panel would be modeled in NetBox as front ports. Each port is assigned a physical type, and must be mapped to a specific rear port on the same device. A single rear port may be mapped to multiple front ports, using numeric positions to annotate the specific alignment of each.

- `name`: Имя
- `label`: Метка
- `type`: тип записи slug (массив)
- `rear_port`: The name of the rear port on this device to which the front port maps
- `rear_port_position`: The corresponding position on the mapped rear port (default: 1)

#### Rear Ports

**[Documentation](https://docs.netbox.dev/en/stable/models/dcim/rearport/)**

Like front ports, rear ports are pass-through ports which represent the continuation of a path from one cable to the next. Each rear port is defined with its physical type and a number of positions: Rear ports with more than one position can be mapped to multiple front ports. This can be useful for modeling instances where multiple paths share a common cable (for example, six discrete two-strand fiber connections sharing a 12-strand MPO cable).

- `name`: Имя
- `label`: Метка
- `type`: тип записи slug (массив)
- `positions`: The number of front ports that can map to this rear port (default: 1)
- `poe`: Does this port access/provide POE? (Boolean)

#### Module Bays

**[Documentation](https://docs.netbox.dev/en/stable/models/dcim/modulebay/)**

Module bays represent a space or slot within a device in which a field-replaceable module may be installed. A common example is that of a chassis-based switch such as the Cisco Nexus 9000 or Juniper EX9200. Modules in turn hold additional components that become available to the parent device.

- `name`: Имя
- `label`: Метка
- `position`: The alphanumeric position in which this module bay is situated within the parent device. When creating module components, the string `{module}` in the component name will be replaced with the module bay's `position`. See the [NetBox Documentation](https://docs.netbox.dev/en/stable/models/dcim/moduletype/#automatic-component-renaming) for more details.

#### Device Bays

**[Documentation](https://docs.netbox.dev/en/stable/models/dcim/devicebay/)**

Device bays represent a space or slot within a parent device in which a child device may be installed. For example, a 2U parent chassis might house four individual blade servers. The chassis would appear in the rack elevation as a 2U device with four device bays, and each server within it would be defined as a 0U device installed in one of the device bays. Child devices do not appear within rack elevations or count as consuming rack units.

Child devices are first-class Devices in their own right: That is, they are fully independent managed entities which don't share any control plane with the parent. Just like normal devices, child devices have their own platform (OS), role, tags, and components. LAG interfaces may not group interfaces belonging to different child devices.

- `name`: Имя
- `label`: Метка

#### Inventory Items

**[Documentation](https://docs.netbox.dev/en/stable/models/dcim/inventoryitem/)**

Inventory items represent hardware components installed within a device, such as a power supply or CPU or line card. They are intended to be used primarily for inventory purposes.

Inventory items are hierarchical in nature, such that any individual item may be designated as the parent for other items. For example, an inventory item might be created to represent a line card which houses several SFP optics, each of which exists as a child item within the device. An inventory item may also be associated with a specific component within the same device. For example, you may wish to associate a transceiver with an interface.

- `name`: Имя
- `label`: Метка
- `manufacturer`: The name of the manufacturer which produces this item
- `part_id`: The part ID assigned by the manufacturer

## Data Validation / Commit Quality Checks

There are two ways this repo focuses on keeping quality device-type definitions:

- **Pre-Commit Checks** - Optional, but **highly recommended**, for helping to identify simple issues before committing. (trailing-whitespace, end-of-file-fixer, check-yaml, yamlfmt, yamllint)
  - Installation
    - Virtual Environment Route
      - It is recommended to create a virtual env for your repo (`python3 -m venv venv`)
      - Install the required pip packages (`pip install -r requirements.txt`)
      - Continue to the "Install `pre-commit` Hooks"
    - `pre-commit` Only Route
      - [Install pre-commit](https://pre-commit.com/#install) (`pip install pre-commit`)
    - Install `pre-commit` Hooks
      - To install the pre-commit script: `pre-commit install`
  - Usage & Useful `pre-commit` Commands
    - After staging your files with `git`, to run the pre-commit script on changed files: `pre-commit run`
    - To run the pre-commit script on all files: `pre-commit run --all`
    - To uninstall the pre-commit script: `pre-commit uninstall`
  - Learn more about [pre-commit](https://pre-commit.com/)
- **GitHub Actions** - Automatically run before a PR can be merged. Repeats yamllint & validates against NetBox Device-Type Schema.
