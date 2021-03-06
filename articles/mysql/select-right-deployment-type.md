---
title: Выбор правильного типа развертывания — база данных Azure для MySQL
description: В этой статье описываются факторы, которые следует учитывать перед развертыванием базы данных Azure для MySQL в качестве инфраструктуры как службы (IaaS) или платформы как услуги (PaaS).
author: savjani
ms.author: pariks
ms.service: mysql
ms.topic: conceptual
ms.date: 08/26/2020
ms.openlocfilehash: a1b66528bee63fb123271e4277e122603ced2e75
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/09/2020
ms.locfileid: "90906517"
---
# <a name="choose-the-right-mysql-server-option-in-azure"></a>Выбор правильного сервера MySQL в Azure

В Azure рабочие нагрузки сервера MySQL могут работать в размещенной инфраструктуре виртуальной машины как услуга (IaaS) или в качестве службы (PaaS). PaaS имеет несколько вариантов развертывания, и в каждом варианте развертывания существуют уровни служб. Выбор варианта IaaS или PaaS в первую очередь зависит от того, хотите ли вы управлять базой данных, применять исправления и создавать резервные копии или же вы хотите делегировать эти операции Azure.

При принятии решения примите во внимание следующие два варианта:

- **База данных Azure для MySQL**. Этот вариант является полностью управляемым ядром СУБД MySQL на основе стабильной версии выпуска MySQL Community Edition. Эта реляционная база данных как услуга (DBaaS), размещенная на облачной платформе Azure, попадает в категорию PaaS в отрасли.

  С помощью управляемого экземпляра MySQL в Azure можно использовать встроенные функции, визуализациям автоматизированную установку исправлений, высокий уровень доступности, автоматическое резервное копирование, Эластичное масштабирование, безопасность корпоративного уровня, соответствие требованиям и управление, мониторинг и оповещения. в противном случае требуется расширенная конфигурация, если MySQL Server находится локально или на виртуальной машине Azure. При использовании MySQL как услуги вы будете платить по мере использования с возможностями увеличения или уменьшения масштаба для большего контроля без прерывания. 
  
  [База данных Azure для MySQL](overview.md), реализованная на базе MySQL Community Edition, доступна в двух режимах развертывания:
    - [Single Server](single-server-overview.md) — это полностью управляемая служба базы данных с минимальными требованиями к настройке базы данных. Платформа отдельного сервера рассчитана на обработку большинства функций управления базами данных, как, например, установка исправлений, резервное копирование, обеспечение высокого уровня доступности и безопасности с минимальным вмешательством пользователя дли их настройки и управления. Эта архитектура оптимизирована для обеспечения уровня доступности 99,99 % при использовании одной зоны доступности. Отдельные серверы лучше всего подходят для приложений, оптимизированных для работы в облаке, которым требуется автоматическая установка исправлений без подробного управления расписанием установки или параметрами конфигурации MySQL. 
    
    - [Гибкий сервер (Предварительная версия)](flexible-server/overview.md) — это полностью управляемая служба базы данных, которая обеспечивает более детализированный контроль и гибкость функций управления базами данных и параметров конфигурации. Как правило, служба обеспечивает большую гибкость и настройку конфигурации сервера по сравнению с развертыванием одиночного сервера в зависимости от требований пользователя. Гибкая серверная архитектура позволяет пользователям выбрать высокий уровень доступности в рамках одной зоны доступности и в нескольких зонах доступности. Гибкие серверы также предоставляют лучшие средства оптимизации затрат с возможностью запуска и отключения сервера и пакетных номеров SKU, идеально подходят для рабочих нагрузок, для которых не требуется непрерывная полная расчетная емкость. 
    Гибкие серверы лучше всего подходят для:
     
      - Разработка приложений требует лучшего контроля и настройки модуля MySQL.
      - высокий уровень доступности с избыточностью в пределах зоны;
      - управляемые периоды обслуживания.

- **MySQL на виртуальных машинах Azure**. Этот вариант относится к категории отрасли IaaS. С помощью этой службы можно запустить MySQL Server внутри управляемой виртуальной машины на облачной платформе Azure. На виртуальной машине можно установить все последние версии и выпуски MySQL.

## <a name="comparing-the-mysql-deployment-options-in-azure"></a>Сравнение вариантов развертывания MySQL в Azure

Основные различия между этими вариантами показаны в приведенной ниже таблице.


| attribute          | База данных Azure для MySQL<br/>Одиночный сервер |База данных Azure для MySQL<br/>Гибкий сервер  |MySQL на виртуальных машинах Azure                      |
|:-------------------|:-------------------------------------------|:---------------------------------------------|:---------------------------------------|
| Поддержка версии MySQL | 5,6, 5,7 & 8,0| 5.7 | Любая версия|
| Масштабирование вычислительных ресурсов | Поддерживается (масштабирование от базового уровня и к базовому не поддерживается)| Поддерживается | Поддерживается|
| Объем памяти | от 5 гиб до 16 тиб| от 5 гиб до 16 тиб | 32 гиб 32 767 гиб|
| Масштабирование хранилища в сети | Поддерживается| Поддерживается| Не поддерживается|
| Автоматическое масштабирование хранилища | Поддерживается| Не поддерживается в предварительной версии| Не поддерживается|
| Сетевое подключение | — Общедоступные конечные точки с брандмауэром сервера.<br/> — Частный доступ с поддержкой закрытых ссылок.|— Общедоступные конечные точки с брандмауэром сервера.<br/> — Частный доступ с интеграцией виртуальной сети.| — Общедоступные конечные точки с брандмауэром сервера.<br/> — Частный доступ с поддержкой закрытых ссылок.|
| Соглашение об уровне обслуживания | Соглашение об уровне обслуживания 99,99% доступности |Нет соглашения об уровне обслуживания в предварительной версии| 99,99% использования Зоны доступности|
| Установка исправлений операционной системы| Автоматически  | Автоматически с пользовательским контролем периода обслуживания | Управляется конечными пользователями |
| Исправление MySQL     | Автоматически  | Автоматически с пользовательским контролем периода обслуживания | Управляется конечными пользователями |
| Высокий уровень доступности | Встроенная Высокая доступность в пределах одной зоны доступности| Встроенная Высокая доступность в пределах и между зонами доступности | Настраиваемое управление с использованием кластеризации, репликация и т. д.|
| Избыточность в пределах зоны | Не поддерживается | Поддерживается | Поддерживается|
| Гибридные сценарии | Поддерживается с [репликация входных данных](https://docs.microsoft.com/azure/mysql/concepts-data-in-replication)| Недоступно в предварительной версии | Управляется конечными пользователями |
| Реплики чтения | Поддерживается| Поддерживается | Управляется конечными пользователями |
| Резервное копирование | Автоматизировано с периодом хранения 7-35 дней | Автоматизировано с периодом хранения 1-35 дней | Управляется конечными пользователями |
| Мониторинг операций базы данных | Поддерживается | Поддерживается | Управляется конечными пользователями |
| Аварийное восстановление | Поддерживается с геоизбыточным хранилищем резервных копий и репликами чтения в разных регионах | Не поддерживается в предварительной версии| Специализированные управляемые с помощью технологий репликации |
| Сведения о производительности запросов | Поддерживается | Недоступно в предварительной версии| Управляется конечными пользователями |
| Цены на зарезервированные экземпляры виртуальных машин Azure | Поддерживается | Недоступно в предварительной версии | Поддерживается |
| Аутентификация Azure AD | Поддерживается | Недоступно в предварительной версии | Не поддерживается|
| Шифрование неактивных данных | Поддерживается управляемыми клиентами ключами | Поддерживается с ключами, управляемыми службой | Не поддерживается|
| SSL/TLS; | Включено по умолчанию с поддержкой TLS v 1.2, 1,1 и 1,0 | Принудительно применяется с TLS v 1.2 | Поддерживается с помощью TLS версии 1.2, 1,1 и 1,0 | 
| Управление препарком | Поддерживается в Azure CLI, PowerShell, RESTFUL и Azure Resource Manager | Поддерживается в Azure CLI, PowerShell, RESTFUL и Azure Resource Manager  | Поддерживается для виртуальных машин с Azure CLI, PowerShell, RESTFUL и Azure Resource Manager |


## <a name="business-motivations-for-choosing-paas-or-iaas"></a>Бизнес-мотивация для выбора PaaS или IaaS

Существует несколько факторов, которые могут повлиять на выбор PaaS или IaaS для размещения баз данных MySQL.

### <a name="cost"></a>Cost

Снижение затрат часто является основным фактором, определяющим лучшее решение для размещения баз данных. Это верно независимо от того, являетесь ли вы начинающим разработчиком, стесненным в деньгах, либо группой в солидной организации с ограниченным бюджетом. В этом разделе описываются основы выставления счетов и лицензирования в Azure, которые применяются к базе данных Azure для MySQL и MySQL на виртуальных машинах Azure.

#### <a name="billing"></a>Выставление счетов

В настоящее время база данных Azure для MySQL доступна в виде службы на нескольких уровнях с различными ценами на ресурсы. Счета за все ресурсы выставляются ежечасно по фиксированной ставке. Последние сведения о поддерживаемых в настоящее время уровнях служб, размерах вычислений и объемах хранения см. на [странице с ценами](https://azure.microsoft.com/pricing/details/mysql/). Вы можете динамически изменять уровни служб и размеры вычислений в соответствии с различными потребностями в пропускной способности приложения. Счета выставляются за исходящий интернет-трафик по обычным [тарифам на передачу данных](https://azure.microsoft.com/pricing/details/data-transfers/).

С помощью базы данных Azure для MySQL Корпорация Майкрософт автоматически настраивает и обновляет программное обеспечение для баз данных. Эти автоматизированные действия снижают затраты на администрирование. Кроме того, база данных Azure для MySQL имеет возможности [автоматического резервного копирования](https://docs.microsoft.com/azure/mysql/concepts-backup) . Эти функции помогают значительно экономить средства, особенно при наличии большого количества баз данных. В свою очередь, с помощью MySQL на виртуальных машинах Azure можно выбрать и запустить любую версию MySQL. Независимо от используемой версии MySQL вы платите за подготовленную виртуальную машину, стоимость хранилища, связанные с данными, резервное копирование, мониторинг данных и хранение журналов, а также затраты на использование конкретного типа лицензии MySQL (если они есть).

База данных Azure для MySQL предоставляет встроенную высокую доступность для любого рода прерываний на уровне узла, сохраняя при этом гарантию соглашения об уровне обслуживания 99,99% для службы. Однако для обеспечения высокой доступности базы данных в виртуальных машинах используются такие параметры высокого уровня доступности, как [репликация MySQL](https://dev.mysql.com/doc/refman/8.0/en/replication.html) , доступные в базе данных MySQL. Использование параметра высокой доступности не дает дополнительных гарантий уровня обслуживания. Однако это позволяет достичь более чем 99,99 % доступности базы данных за дополнительную плату и за счет повышения затрат на администрирование.

Дополнительную информацию см. в следующих статьях:
* [Цены на базу данных Azure для MySQL](https://azure.microsoft.com/pricing/details/mysql/)
* [Цены на виртуальные машины Azure](https://azure.microsoft.com/pricing/details/virtual-machines/)
* [Калькулятор цен Azure](https://azure.microsoft.com/pricing/calculator/)

### <a name="administration"></a>Администрирование

Для многих компаний решение о переходе в облачную службу позволит уменьшить не только расходы, но и сложность администрирования системы. 

С помощью IaaS корпорация Майкрософт выполняет следующие действия:

- Управляет базовой инфраструктурой.
- Обеспечивает автоматическую установку исправлений для базового оборудования и ОС.
  
С помощью PaaS корпорация Майкрософт выполняет следующие действия:

- Управляет базовой инфраструктурой.
- Автоматически устанавливает исправления для базового оборудования, ОС и ядра СУБД.
- Управляет высокой доступностью базы данных.
- Автоматически выполняет резервное копирование и реплицирует все данные для аварийного восстановления.
- По умолчанию шифрует неактивные и перемещаемые данные.
- Отслеживает сервер и предоставляет функции для анализа производительности запросов и рекомендации по производительности.

В следующем списке приведены рекомендации по администрированию для каждого параметра.

* С помощью базы данных Azure для MySQL можно продолжить Администрирование базы данных. При этом больше не требуется управлять ядром СУБД, операционной системой или оборудованием. Ниже перечислены примеры элементов, которые можно продолжить администрировать.

  - Базы данных
  - Вход
  - Настройка индексов
  - Настройка запросов
  - Аудит
  - Безопасность

  Кроме того, при настройке высокой доступности для другого центра обработки данных требуется минимальная (или вовсе не требуется) конфигурация и администрирование.

* С помощью MySQL на виртуальных машинах Azure вы имеете полный контроль над операционной системой и конфигурацией экземпляра сервера MySQL. В рамках виртуальной машины вы определяете, когда следует обновлять операционную систему и программное обеспечение базы данных, а также какие исправления применять. Вы также определяете, когда устанавливать дополнительное программное обеспечение, например антивирусное приложение. Некоторые предлагаемые функции автоматизации позволяют значительно упростить процессы исправления, резервного копирования и обеспечения высокой доступности. Вы можете контролировать размер виртуальной машины, количество дисков и их конфигурации хранения. Дополнительные сведения см. в статье [Размеры виртуальных машин в Azure](https://docs.microsoft.com/azure/virtual-machines/windows/sizes).

### <a name="time-to-move-to-azure"></a>Пора переходить на Azure

* База данных Azure для MySQL — это подходящее решение для приложений, разработанных в облаке, когда производительность разработки и быстрый доступ к рынкам для новых решений очень важны. Благодаря функциональным возможностям, схожим с возможностями администратора базы данных, она идеально подходит для разработчиков облачных служб, так как позволяет уменьшить необходимость управления базовой операционной системой и базой данных.

* Если вы хотите избежать времени и расходов на приобретение нового локального оборудования, MySQL на виртуальных машинах Azure является верным решением для приложений, требующих детального контроля и настройки модуля MySQL, которые не поддерживаются службой или не требуют доступа к базовой ОС. Это решение также подходит для переноса существующих локальных приложений и баз данных в Azure без изменений, в случаях, когда база данных Azure для MySQL плохо подходит.

Поскольку нет необходимости изменять уровни представления, приложения и данных, вы экономите время и бюджеты при изменении архитектуры существующего решения. Вместо этого вы можете сосредоточиться на переносе всех ваших решений в Azure и устранении некоторых оптимизаций производительности, которые могут потребоваться для платформы Azure.

## <a name="next-steps"></a>Дальнейшие шаги

* См. [цены на базу данных Azure для MySQL](https://azure.microsoft.com/pricing/details/MySQL/).
* Приступить к работе, [создав первый сервер](https://docs.microsoft.com/azure/MySQL/quickstart-create-MySQL-server-database-using-azure-portal).
