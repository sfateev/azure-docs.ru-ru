---
title: Мониторинг Key Vault с помощью Azure Monitor для Key Vault | Документация Майкрософт
description: В этой статье описывается Azure Monitor для хранилищ Key Vault.
services: azure-monitor
ms.topic: conceptual
author: mrbullwinkle
ms.author: mbullwin
ms.date: 09/10/2020
ms.openlocfilehash: 4b91a9a73035b3add309e72ce544375520cf279e
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/09/2020
ms.locfileid: "91278623"
---
# <a name="monitoring-your-key-vault-service-with-azure-monitor-for-key-vault"></a>Мониторинг службы хранилища ключей с помощью Azure Monitor для Key Vault
Azure Monitor для Key Vault обеспечивает полный мониторинг хранилищ ключей, обеспечивая единое представление запросов Key Vault, производительности, сбоев и задержки.
Эта статья поможет вам понять, как подключить и настроить интерфейс Azure Monitor для Key Vault.

## <a name="introduction-to-azure-monitor-for-key-vault"></a>Введение в Azure Monitor для Key Vault

Прежде чем переходить к настройке интерфейса, необходимо понять, как он представляет и визуализирует информацию.
-    **Представление в масштабе**— моментальный снимок производительности, основанный на скорости обработки запросов, детализации сбоев и общих сведениях об операциях и задержке.
-   **Детализация анализа** — подробный анализ определенного хранилища ключей.
-    **Настраиваемые параметры** — место, где можно выбрать нужные метрики, изменить или установить пороговые значения, соответствующие вашим ограничениям, и сохранить собственную книгу. Диаграммы из книги можно закрепить на панелях мониторинга Azure.

Azure Monitor для Key Vault объединяет журналы и метрики в единое решение мониторинга. Доступ к данным мониторинга на основе метрик могут получить все пользователи, однако при включении визуализаций на основе журналов необходимо будет также [включить ведение журнала Azure Key Vault](../../key-vault/general/logging.md).

## <a name="view-from-azure-monitor"></a>Просмотр данных в Azure Monitor

В Azure Monitor можно просматривать сведения о запросах, задержках и сбоях из нескольких хранилищ ключей в подписке, а также выявлять проблемы производительности и сценарии регулирования.

Чтобы просмотреть сведения об использовании и операциях хранилища ключей во всех подписках, выполните следующие действия.

1. Войдите на [портал Azure](https://portal.azure.com/).

2. Выберите **монитор** в области слева в портал Azure и в разделе Аналитика выберите **хранилища ключей**.

![Снимок экрана с обзором нескольких диаграмм](./media/key-vaults-insights-overview/overview.png)

## <a name="overview-workbook"></a>Книга "Обзор"

В книге "Обзор" для выбранной подписки отображаются интерактивные метрики хранилищ ключей, сгруппированных в пределах подписки. Результаты можно фильтровать по параметрам, выбранным в следующих раскрывающихся списках.

* Подписки — список включает только подписки с хранилищами ключей.

* Хранилища ключей — по умолчанию предварительно выбираются не более 5 хранилищ ключей. Если вы выберете в селекторе области все или несколько хранилищ ключей, система выдаст до 200 хранилищ ключей. Например, если вы выберете в общей сложности 573 хранилища ключей в трех выбранных подписках, будут показаны только 200 хранилищ.

* Диапазон времени — по умолчанию отображаются данные за последние 24 часа с учетом выбранных параметров.

На плитке счетчика под раскрывающимся списком отображается общее количество хранилищ ключей в выбранных подписках и число выбранных. Для столбцов книги предусмотрены условные тепловые карты с цветовыми обозначениями, которые сообщают о запросах, сбоях и метриках задержки. Самый темный цвет соответствует наибольшему значению, а самый светлый — наименьшему.

## <a name="failures-workbook"></a>Книга "Сбои"

Выберите **Сбои** в верхней части страницы, после чего откроется вкладка "Сбои". Здесь отображаются попадания в API, частота в динамике, а также количество определенных кодов отклика.

![Снимок экрана с книгой "Сбои"](./media/key-vaults-insights-overview/failures.png)

Для столбцов книги предусмотрены условные тепловые карты с цветовыми обозначениями, в которых попадания в API обозначаются синим цветом. Самый темный цвет соответствует наибольшему значению, а самый светлый — наименьшему.

В книге отображаются успешные попытки (коды состояния 2xx), ошибки проверки подлинности (коды состояния 401/403), регулирование (код состояния 429) и другие сбои (коды состояния 4xx).

Чтобы лучше понять, что представляет каждый код состояния, рекомендуем ознакомиться с документацией по [кодам состояния и ответов в Azure Key Vault](../../key-vault/general/authentication-requests-and-responses.md).

## <a name="view-from-a-key-vault-resource"></a>Просмотр из ресурса Key Vault

Чтобы получить доступ к Azure Monitor для Key Vault непосредственно из Key Vault

1. На портале Azure выберите Key Vault.

2. В списке выберите хранилище ключей. В разделе Мониторинг выберите аналитика.

В эти представления также можно перейти, выбрав имя ресурса хранилища ключей в книге на уровне Azure Monitor.

![Снимок экрана с представлением ресурса хранилища ключей](./media/key-vaults-insights-overview/key-vault-resource-view.png)

В книге **Обзор** для хранилища ключей отображаются несколько метрик производительности, позволяющие быстро получить доступ к следующему.

- Интерактивные диаграммы производительности, отображающие наиболее важные сведения о транзакциях, задержках и доступности хранилища ключей.

- Метрики и плитки состояния, демонстрирующие доступность службы, общее число транзакций для ресурса хранилища ключей и общую задержку.

При выборе любой из других вкладок в представлении **Сбои** или **Операции** открываются соответствующие книги.

![Снимок экрана с представлением сбоев](./media/key-vaults-insights-overview/resource-failures.png)

В книге сбоев детализируются результаты всех запросов хранилища ключей за выбранный промежуток времени и приводится классификация успешных операций (2xx), ошибок проверки подлинности (401/403), регулирования (429) и других сбоев.

![Снимок экрана с представлением операций](./media/key-vaults-insights-overview/operations.png)

Книга операций позволяет пользователям углубиться в подробные сведения обо всех транзакциях, которые можно фильтровать по состоянию результата, используя плитки верхнего уровня.

![Снимок экрана с представлением операций](./media/key-vaults-insights-overview/info.png)

Пользователи также могут выбирать представления по конкретным типам транзакций в верхней таблице, которая динамически обновляет нижнюю таблицу, где можно просматривать полные сведения об операциях во всплывающей контекстной области.

>[!NOTE]
> Обратите внимание на то, что для просмотра этой книги необходимо включить параметры диагностики. Дополнительные сведения о включении параметра диагностики см. в статье [Ведения журналов в Azure Key Vault](../../key-vault/general/logging.md).

## <a name="pin-and-export"></a>Закрепление и экспорт

Вы можете закрепить любой раздел метрик на панели мониторинга Azure, щелкнув значок канцелярской кнопки в правом верхнем углу раздела.

Книги сбоев или обзора подписок и хранилищ ключей позволяют экспортировать результаты в таблицу Excel, щелкнув значок загрузки слева от значка канцелярской кнопки.

![Снимок экрана с выбранным значком закрепления](./media/key-vaults-insights-overview/pin.png)

## <a name="customize-azure-monitor-for-key-vault"></a>Настройка Azure Monitor для Key Vault

В этом разделе описываются распространенные сценарии редактирования книги для выполнения настроек, необходимых для решения ваших задач по анализу данных.
*  Настройка книги на постоянный выбор определенной подписки или хранилища (хранилищ) ключей
* Изменение метрик в сетке
* Изменение порога запросов
* Изменение цветовых обозначений

Чтобы начать настройку, включите режим редактирования, нажав кнопку **Настроить** на верхней панели инструментов.

![Снимок экрана с кнопкой настройки](./media/key-vaults-insights-overview/customize.png)

Настройки сохраняются в пользовательской книге, чтобы конфигурация по умолчанию в опубликованной книге не перезаписывалась. Книги хранятся в группе ресурсов либо в закрытом разделе "Мои отчеты", либо в разделе "Общие отчеты", доступном всем пользователям с доступом к группе ресурсов. После сохранения пользовательской книги перейдите в коллекцию книг и запустите эту книгу.

![Снимок экрана с коллекцией книг](./media/key-vaults-insights-overview/gallery.png)

### <a name="specifying-a-subscription-or-key-vault"></a>Указание подписки или хранилища ключей

Вы можете настроить книги сбоев или обзора подписок и хранилищ ключей на выбор определенных подписок или хранилищ ключей при каждом запуске. Для этого выполните следующие действия.

1. Выберите **монитор** на портале, а затем в области слева выберите **хранилища ключей** .
2. В книге **Обзор** на панели команд выберите **Изменить**.
3. В раскрывающемся списке **Подписки** выберите одну или несколько подписок, которые вы хотите использовать по умолчанию. Помните, что книга поддерживает в общей сложности до 10 подписок.
4. В раскрывающемся списке **Хранилища ключей** выберите одну или несколько учетных записей, которые вы хотите использовать по умолчанию. Помните, что книга поддерживает в общей сложности до 200 учетных записей хранения.
5. Выберите **Сохранить как** на панели команд, чтобы сохранить копию книги с вашими настройками, а затем нажмите кнопку **Редактирование завершено**, чтобы вернуться в режим чтения.

## <a name="troubleshooting"></a>Устранение неполадок

Общие рекомендации по устранению неполадок см. в соответствующей [статье об устранении неполадок](troubleshoot-workbooks.md)на основе книги.

Этот раздел поможет вам в диагностике и устранении неполадок некоторых распространенных проблем, которые могут возникнуть при использовании Azure Monitor для Key Vault. Чтобы найти информацию о конкретной проблеме, просмотрите список ниже.

### <a name="resolving-performance-issues-or-failures"></a>Устранение проблем с производительностью или сбоев

Дополнительные сведения об устранении проблем, связанных с хранилищем ключей, которые можно определить с помощью Azure Monitor для Key Vault, см. в [документации по Azure Key Vault](../../key-vault/index.yml).

### <a name="why-can-i-only-see-200-key-vaults"></a>Почему можно видеть только хранилище ключей 200

Выбирать и просматривать можно не больше 200 хранилищ ключей. Независимо от количества выбранных подписок выбрать больше 200 хранилищ ключей невозможно.

### <a name="why-dont-i-see-all-my-subscriptions-in-the-subscription-picker"></a>Почему в средстве выбора подписки не отображаются все подписки

Мы видим только те подписки, которые содержат хранилища ключей, выбранные в фильтре подписок, который был выбран в разделе "Каталог + подписка" в заголовке портала Azure.

![Снимок экрана с фильтром подписок](./media/key-vaults-insights-overview/Subscriptions.png)

### <a name="i-want-to-make-changes-or-add-additional-visualizations-to-key-vault-insights-how-do-i-do-so"></a>Я хочу внести изменения или добавить дополнительные визуализации для Key Vault Insights, как это сделать.

Чтобы внести изменения, выберите режим редактирования и внесите изменения в книгу, после чего вы сможете сохранить измененную книгу как новую книгу, привязанную к определенной подписке и группе ресурсов.

### <a name="what-is-the-time-grain-once-we-pin-any-part-of-the-workbooks"></a>Что такое промежуток времени после закрепления любой части книг

Мы используем автоматический временной интервал, поэтому он зависит от выбранного диапазона времени.

### <a name="what-is-the-time-range-when-any-part-of-the-workbook-is-pinned"></a>Каков диапазон времени, когда какая-либо часть книги закреплена

Диапазон времени будет зависеть от параметров панели мониторинга.

### <a name="what-if-i-want-to-see-other-data-or-make-my-own-visualizations-how-can-i-make-changes-to-the-key-vault-insights"></a>Что делать, если нужно просмотреть другие данные или создать собственные визуализации? Как внести изменения в Key Vault Insights

Вы можете внести изменения в существующую книгу, используя режим редактирования, а затем сохранить измененную книгу как новую книгу, содержащую все изменения.

## <a name="next-steps"></a>Дальнейшие действия

Узнайте, в каких ситуациях применяются книги, как создавать и настраивать отчеты и многое другое, изучив статью [Создание интерактивных отчетов с использованием книг Azure Monitor](../platform/workbooks-overview.md).
