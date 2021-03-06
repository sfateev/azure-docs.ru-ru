---
title: 'Azure Monitor для решения SAP: обзор и архитектура | Документация Майкрософт'
description: В этой статье содержатся ответы на часто задаваемые вопросы о Azure Monitor для решений SAP.
author: rdeltcheva
ms.service: virtual-machines
ms.topic: article
ms.date: 06/30/2020
ms.author: radeltch
ms.reviewer: cynthn
ms.openlocfilehash: d9730324b2557c8f0bb203f7badbd00e0e7e704e
ms.sourcegitcommit: 83610f637914f09d2a87b98ae7a6ae92122a02f1
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/13/2020
ms.locfileid: "91994250"
---
# <a name="azure-monitor-for-sap-solutions-preview"></a>Azure Monitor для решений SAP (Предварительная версия)

## <a name="overview"></a>Обзор

Azure Monitor для решений SAP — это встроенный в Azure продукт для мониторинга клиентов с помощью SAP ландшафтов в Azure. Этот продукт работает с [SAP на виртуальных машинах Azure](./hana-get-started.md) и [SAP в крупных экземплярах Azure](./hana-overview-architecture.md).
Используя Azure Monitor для решений SAP, клиенты могут получать данные телеметрии из инфраструктуры и баз данных Azure в одном централизованном расположении и визуально сопоставлять данные телеметрии для более быстрого устранения неполадок.

Azure Monitor для решений SAP предлагается в Azure Marketplace. Она предоставляет простой и интуитивно понятный процесс установки и занимает всего несколько щелчков мышью для развертывания ресурса для Azure Monitor для решений SAP (известных как **ресурс SAP Monitor**).

Клиенты могут отслеживать различные компоненты ландшафта SAP, такие как виртуальные машины Azure, кластер с высоким уровнем доступности, SAP HANA базу данных и т. д., добавляя соответствующий **поставщик** для этого компонента.

Поддерживаемая инфраструктура:

- Виртуальная машина Azure
- Крупные экземпляры Azure

Поддерживаемые базы данных:
- База данных SAP HANA
- Microsoft SQL Server

Azure Monitor для решений SAP использует возможности существующих [Azure Monitor](../../../azure-monitor/overview.md) возможностей, таких как log Analytics и [книги](../../../azure-monitor/platform/workbooks-overview.md) , для предоставления дополнительных возможностей мониторинга. Клиенты могут создавать [пользовательские визуализации](../../../azure-monitor/platform/workbooks-overview.md#getting-started) , изменяя книги по умолчанию, предоставляемые Azure Monitor для решений SAP, создавать [пользовательские запросы](../../../azure-monitor/log-query/get-started-portal.md) и создавать [настраиваемые оповещения](../../../azure-monitor/learn/tutorial-response.md) с помощью log Analytics рабочей области Azure, использовать [гибкий срок хранения](../../../azure-monitor/platform/manage-cost-storage.md#change-the-data-retention-period) и подключать данные мониторинга к своей системе билетов.

## <a name="what-data-does-azure-monitor-for-sap-solutions-collect"></a>Какие данные собираются Azure Monitor для решений SAP?

Сбор данных в Azure Monitor для решений SAP зависит от поставщиков, настроенных клиентами. Во время общедоступной предварительной версии собираются следующие данные.

Данные телеметрии кластера Pacemaker с высокой доступностью:
- Состояние узла, ресурса и устройства SBD
- Ограничения расположения Pacemaker
- Голоса и состояние кольца кворума
- [Прочие](https://github.com/ClusterLabs/ha_cluster_exporter/blob/master/doc/metrics.md)

SAP HANA телеметрии:
- Загрузка ЦП, памяти, диска и сети
- Репликация системы HANA (HSR)
- Резервное копирование HANA
- Состояние узла HANA
- Сервер индекса и роли сервера имен

Телеметрию Microsoft SQL Server:
- ЦП, память, использование диска
- Hostname, имя экземпляра SQL, идентификатор системы SAP
- Пакетные запросы, компиляции и ожидаемый срок жизни страницы со временем
- 10 самых ресурсоемких инструкций SQL с течением времени
- 12 самых крупных таблиц в системе SAP
- Проблемы, записанные в журналы ошибок SQL Server
- Блокировка процессов и статистика ожидания SQL с течением времени

## <a name="data-sharing-with-microsoft"></a>Совместное использование данных с Майкрософт

Azure Monitor для решений SAP собирает системные метаданные, чтобы обеспечить улучшенную поддержку наших клиентов SAP в Azure. Никакие персональные данные и EUII не собираются.
Клиенты могут включить совместное использование данных в Майкрософт во время создания Azure Monitor для ресурса решений SAP, выбрав в раскрывающемся списке пункт *общий доступ* .
Настоятельно рекомендуется, чтобы клиенты поддерживали совместное использование данных, так как они предоставляют группам поддержки и инженеров корпорации Майкрософт дополнительные сведения о клиентской среде и обеспечивают улучшенную поддержку наших критически важных SAP на клиентах Azure.

## <a name="architecture-overview"></a>Обзор архитектуры

На приведенной ниже схеме объясняется, как Azure Monitor для решений SAP собирает данные телеметрии из SAP HANA базы данных. Архитектура не зависит от того, развернут ли SAP HANA на виртуальных машинах Azure или больших экземплярах Azure.

![Azure Monitor архитектуры решений SAP](./media/azure-monitor-sap/azure-monitor-architecture.png)

Основные компоненты архитектуры:
- Портал Azure — начальная точка для клиентов. Клиенты могут переходить в Marketplace в портал Azure и обнаруживать Azure Monitor для решений SAP.
- Azure Monitor для ресурса решений SAP — целевое место для клиентов, которое позволяет просматривать данные телеметрии мониторинга.
- Управляемая группа ресурсов — развертывается автоматически как часть Azure Monitor для развертывания ресурсов решений SAP. Ресурсы, развернутые в управляемой группе ресурсов, находятся в коллекции телеметрии. Основные ресурсы развернуты и их назначение:
   - Виртуальная машина Azure — также известная как *Виртуальная машина сборщика*. Это Standard_B2ms виртуальная машина. Основной целью этой виртуальной машины является размещение *полезных данных мониторинга*. Полезные данные мониторинга — это логика сбора данных телеметрии из исходных систем и передача собранных данные в платформу мониторинга. На приведенной выше схеме полезные данные мониторинга содержат логику для подключения к SAP HANA базе данных через порт SQL.
   - [Azure Key Vault](../../../key-vault/general/basic-concepts.md): этот ресурс развертывается для безопасного хранения учетных данных SAP HANA и для хранения сведений о [поставщиках](./azure-monitor-providers.md).
   - Рабочая область Log Analytics: место назначения, где находятся данные телеметрии.
      - Визуализация построена на основе телеметрии в Log Analytics с помощью [книг Azure](../../../azure-monitor/platform/workbooks-overview.md). Пользователи могут настраивать визуализацию. Клиенты также могут закреплять свои книги или отдельные визуализации в книгах на панели мониторинга Azure для возможности автообновления с наименьшей степенью детализации 30 минут.
      - Клиенты могут использовать существующую рабочую область в той же подписке, что и ресурс SAP Monitor, выбрав этот параметр во время развертывания.
      - Клиенты могут использовать язык запросов Kusto (ККЛ) для выполнения [запросов](../../../azure-monitor/log-query/log-query-overview.md) к необработанным таблицам в log Analytics рабочей области. Просмотрите *пользовательские журналы*.

> [!Note]
> Клиенты несут ответственность за исправление и обслуживание виртуальной машины, развернутой в управляемой группе ресурсов.

> [!Tip]
> Клиенты могут использовать существующую рабочую область Log Analytics для сбора данных телеметрии, если она развернута в той же подписке Azure, что и ресурс для Azure Monitor для решений SAP.

### <a name="architecture-highlights"></a>Основные особенности архитектуры

Ниже приведены основные основные особенности архитектуры.
 - С **несколькими экземплярами** клиенты могут создавать мониторы для нескольких экземпляров определенного типа компонента (например, Hana DB, Ha Cluster, Microsoft SQL Server) по нескольким идентификаторам безопасности SAP в виртуальной сети с одним ресурсом Azure Monitor для решений SAP.
 - С **несколькими поставщиками** . на приведенной выше схеме архитектуры в качестве примера показан поставщик SAP HANA. Аналогичным образом клиенты могут настроить дополнительные поставщики для соответствующих компонентов (например, база данных HANA, высокодоступный кластер, Microsoft SQL Server) для получения данных из этих компонентов.
 - **Открытый исходный** код Azure Monitor для решений SAP доступен на сайте [GitHub](https://github.com/Azure/AzureMonitorForSAPSolutions). Клиенты могут обратиться к коду поставщика и получить дополнительные сведения о продукте, участвовать или поделиться отзывами.
 - **Расширяемая платформа запросов** — SQL-запросы для сбора данных телеметрии записываются в формате [JSON](https://github.com/Azure/AzureMonitorForSAPSolutions/blob/master/sapmon/content/SapHana.json). Дополнительные запросы SQL для сбора дополнительных данных телеметрии можно легко добавить. Клиенты могут запрашивать определенные данные телеметрии для добавления в Azure Monitor для решений SAP. для этого оставьте отзыв по ссылке в конце документа или обратитесь к группе своей учетной записи.

## <a name="pricing"></a>Цены
Azure Monitor для решений SAP — это бесплатный продукт (без оплаты лицензии). Клиенты несут ответственность за оплату расходов на базовые компоненты в управляемой группе ресурсов.

## <a name="next-steps"></a>Дальнейшие действия

Узнайте о поставщиках и создайте свои первые Azure Monitor для ресурса по решениям SAP.
 - Дополнительные сведения о [поставщиках](./azure-monitor-providers.md)
 - [Развертывание Azure Monitor для решений SAP с помощью Azure PowerShell](azure-monitor-sap-quickstart-powershell.md)
 - У вас возникли вопросы по Azure Monitor для решений SAP? Проверка раздела " [вопросы и ответы](./azure-monitor-faq.md) "
