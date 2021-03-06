---
title: Общие сведения об обеспечении безопасности в Azure Data Lake Storage 1-го поколения | Документы Майкрософт
description: Сведения о возможностях безопасности Azure Data Lake Storage 1-го поколения, включая проверку подлинности, авторизацию, сетевую изоляцию, защиту данных и аудит.
services: data-lake-store
author: twooley
ms.service: data-lake-store
ms.topic: conceptual
ms.date: 03/11/2020
ms.author: twooley
ms.openlocfilehash: 240018381a3139a6378141d78514e43ae469de5d
ms.sourcegitcommit: dbe434f45f9d0f9d298076bf8c08672ceca416c6
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/17/2020
ms.locfileid: "92146325"
---
# <a name="security-in-azure-data-lake-storage-gen1"></a>Безопасность в Azure Data Lake Storage 1-го поколения

Многие предприятия используют аналитику больших данных, чтобы получать информацию, которая поможет принимать обоснованные решения. В организациях могут быть сложные управляемые среды с растущим числом разных пользователей. Поэтому для предприятий крайне важно, чтобы ценные бизнес-данные хранились в надежном месте и определенным пользователям можно было назначать надлежащий уровень доступа к этим данным. Azure Data Lake Storage 1-го поколения разработано с учетом этих требований к безопасности. Из этой статьи вы узнаете о том, как обеспечить безопасность Data Lake Storage 1-го поколения с помощью следующих функций:

* Аутентификация
* Авторизация
* Сетевая изоляция
* Защита данных
* Аудит

## <a name="authentication-and-identity-management"></a>Проверка подлинности и управление удостоверениями

Проверка подлинности — это процесс, в ходе которого подтверждается удостоверение пользователя при его взаимодействии с Data Lake Storage 1-го поколения или любыми службами, которые подключаются к Data Lake Storage 1-го поколения. Чтобы управлять удостоверениями и выполнять проверку подлинности, Data Lake Storage 1-го поколения использует [Azure Active Directory](../active-directory/fundamentals/active-directory-whatis.md), комплексное решение для управления удостоверениями и доступом в облаке, которое упрощает управление пользователями и группами.

Каждая подписка Azure связана с одним экземпляром каталога Azure Active Directory. Доступ к учетной записи Data Lake Storage 1-го поколения могут получить только те пользователи и службы, удостоверения которых определены в службе Azure Active Directory. Доступ реализуется с помощью портала Azure, средств командной строки или клиентских приложений, созданных с помощью пакета SDK для Data Lake Storage 1-го поколения. Ниже перечислены основные преимущества использования Azure Active Directory в качестве централизованного механизма управления доступом.

* Упрощенное управление жизненным циклом удостоверений. Удостоверение пользователя или службы (удостоверение субъекта-службы) можно быстро создавать и отзывать. Для этого нужно просто удалить или отключить учетную запись в каталоге.
* Многофакторная проверка подлинности. [многофакторной проверки подлинности](../active-directory/authentication/concept-mfa-howitworks.md) обеспечивает дополнительный уровень безопасности при входе пользователей в систему и осуществлении транзакций.
* Проверка подлинности с любого клиента с помощью стандартного открытого протокола, например OAuth или OpenID.
* Федерация со службами каталога предприятия и поставщиками удостоверений в облаке.

## <a name="authorization-and-access-control"></a>Проверка подлинности и управление доступом

После того как пользователь прошел проверку подлинности в Azure Active Directory для доступа к Data Lake Storage 1-го поколения, проверка подлинности управляет разрешениями на доступ к Data Lake Storage 1-го поколения. Data Lake Storage 1-го поколения различает авторизацию для действий, связанных с учетной записью, и для действий, связанных с данными, следующим образом.

* Управление [доступом на основе ролей Azure (Azure RBAC)](../role-based-access-control/overview.md) для управления учетными записями
* ACL POSIX для доступа к данным в хранилище.

### <a name="azure-rbac-for-account-management"></a>Azure RBAC для управления учетными записями

По умолчанию для Data Lake Storage 1-го поколения определены четыре основные роли. С помощью этих ролей разрешается доступ на выполнение различных операций в учетной записи Data Lake Storage 1-го поколения с использованием портала Azure, командлетов PowerShell и REST API. Роли "Владелец" и "Участник" предоставляют доступ к различным функциям администрирования учетной записи. Пользователям, которые будут только просматривать данные управления учетной записью, можно назначить роль "Читатель".

![Роли Azure](./media/data-lake-store-security-overview/rbac-roles.png "Роли Azure")

Обратите внимание, что хотя эти роли назначаются для управления учетными записями, некоторые из них влияют на доступ к данным. Чтобы управлять доступом к операциям, которые пользователь может выполнять в файловой системе, используйте списки управления доступом. В следующей таблице приведен краткий обзор прав на управление и прав на доступ к данным для ролей по умолчанию.

| Роли | Права управления | Права доступа к данным | Объяснение |
| --- | --- | --- | --- |
| Роль не назначена |None |Управление с помощью ACL |Пользователь не может использовать портал Azure или командлеты PowerShell Azure для просмотра Data Lake Storage 1-го поколения. Он может использовать только средства командной строки. |
| Владелец |Все |Все |Владелец — это привилегированный пользователь. Он может управлять всем и имеет полный доступ к данным. |
| Читатель |Только для чтения |Управление с помощью ACL |Читатель может просматривать всю информацию об управлении учетными записями. Например, пользователь с такой ролью может видеть, кому какая роль назначена, но не может вносить изменения в эти данные. |
| Участник |Все, кроме добавления и удаления ролей |Управление с помощью ACL |Участник может управлять другими функциями учетной записи, например развертываниями, созданием оповещений и управлением ими, но не может добавлять или удалять роли. |
| Администратор доступа пользователей |Добавление и удаление ролей |Управление с помощью ACL |Администратор доступа пользователей может управлять доступом пользователей к учетным записям. |

Инструкции см. в разделе [Назначение пользователей или групп безопасности учетным записям хранения Azure Data Lake Storage 1-го поколения](data-lake-store-secure-data.md#assign-users-or-security-groups-to-data-lake-storage-gen1-accounts).

### <a name="using-acls-for-operations-on-file-systems"></a>Использование списков управления доступом для операций в файловых системах

Data Lake Storage 1-го поколения — это иерархическая файловая система (как распределенная файловая система Hadoop (HDFS)), которая поддерживает [списки управления доступом POSIX](https://hadoop.apache.org/docs/current/hadoop-project-dist/hadoop-hdfs/HdfsPermissionsGuide.html#ACLs_Access_Control_Lists). Она предоставляет владельцам, группе владельцев и другим пользователям и группам права на чтение (r), запись (w) и выполнение (x) операций в ресурсах. В Data Lake Storage 1-го поколения списки управления доступом можно включить в корневой папке, вложенных папках, а также в отдельных файлах. Дополнительные сведения о принципе работы списков управления доступом в контексте Data Lake Storage 1-го поколения см. в статье [Контроль доступа в Azure Data Lake Storage 1-го поколения](data-lake-store-access-control.md).

Чтобы определять списки управления доступом для нескольких пользователей, рекомендуется использовать [группы безопасности](../active-directory/fundamentals/active-directory-groups-create-azure-portal.md). Добавьте пользователей в группу безопасности, а затем назначьте этой группе список управления доступом для файла или папки. Это полезно, когда требуется предоставить назначенные разрешения, так как для назначенных разрешений установлено ограничение максимум 28 записей. Дополнительные сведения о защите данных, содержащихся в Data Lake Storage 1-го поколения, с помощью групп безопасности Azure Active Directory см. в разделе [Назначение пользователей или группы безопасности в виде ACL в файловой системе Data Lake Storage 1-го поколения](data-lake-store-secure-data.md#filepermissions).

![Вывод списка разрешений на доступ](./media/data-lake-store-security-overview/adl.acl.2.png "Вывод списка разрешений на доступ")

## <a name="network-isolation"></a>Сетевая изоляция

Data Lake Storage 1-го поколения позволяет управлять доступом к хранилищу данных на уровне сети. Вы можете настроить брандмауэры и определить диапазон IP-адресов для доверенных клиентов. После определения диапазона IP-адресов к Data Lake Storage 1-го поколения смогут подключаться только клиенты с IP-адресами в пределах этого диапазона.

![Параметры брандмауэра и доступ по IP-адресу](./media/data-lake-store-security-overview/firewall-ip-access.png "Параметры и IP-адрес брандмауэра")

Теги службы поддержки виртуальных сетей Azure (VNet) для Data Lake Gen 1. Тег службы представляет группу префиксов IP-адресов из определенной службы Azure. Корпорация Майкрософт управляет префиксами адресов, входящих в тег службы, и автоматически обновляет этот тег при изменении адресов. Дополнительные сведения см. в статье [Общие сведения о тегах служб Azure](../virtual-network/service-tags-overview.md).

## <a name="data-protection"></a>Защита данных

Data Lake Storage 1-го поколения защищает данные на протяжении всего жизненного цикла. Data Lake Storage 1-го поколения использует стандартный протокол TLS 1.2 для защиты данных, передаваемых по сети.

![Шифрование в Data Lake Storage 1-го поколения](./media/data-lake-store-security-overview/adls-encryption.png "Шифрование в Data Lake Storage 1-го поколения")

В Data Lake Storage 1-го поколения можно также включить шифрование данных, хранящихся в учетной записи. Шифрование данных можно как включить, так и отключить. Если включено шифрование, данные, хранящиеся в Data Lake Storage 1-го поколения, будут шифроваться перед сохранением на постоянный носитель. В этом случае Data Lake Storage 1-го поколения автоматически шифрует данные перед сохранением и расшифровывает их до извлечения. Таким образом, данные полностью прозрачны для клиента, который получает к ним доступ. Со стороны клиента не требуется изменение кода для шифрования и расшифровки данных.

Data Lake Storage 1-го поколения предоставляет два режима для управления главными ключами шифрования (MEKs), необходимыми для расшифровки любых данных, хранящихся в Data Lake Storage 1-го поколения. Вы можете позволить Data Lake Storage 1-го поколения управлять главными ключами шифрования или управлять ими самостоятельно с помощью учетной записи Azure Key Vault. Способ управления ключами можно задать во время создания учетной записи Data Lake Storage 1-го поколения. Дополнительные сведения о настройке шифрования см. в статье [Начало работы с Azure Data Lake Storage 1-го поколения с помощью портала Azure](data-lake-store-get-started-portal.md).

## <a name="activity-and-diagnostic-logs"></a>Журналы действий и диагностики

Действия, связанные с управлением учетными записями и данными, можно просматривать в журналах действий или журналах диагностики.

* Действия, связанные с управлением учетными записями, используют API-интерфейсы Azure Resource Manager и отображаются на портале Azure в журналах действий.
* Действия, связанные с данными, используют REST API WebHDFS и отображаются на портале Azure в журналах диагностики.

### <a name="activity-log"></a>Журнал действий

Чтобы обеспечить соответствие нормативным требованиям, организациям могут потребоваться журналы аудита действий по управлению учетными записями при необходимости в расследовании определенных инцидентов. В Data Lake Storage 1-го поколения реализована встроенная функция мониторинга, которая регистрирует все действия, связанные с управлением учетными записями.

В журналах аудита для управления учетными записями можно просматривать и выбирать нужные столбцы для регистрации. Кроме того, журналы действий можно экспортировать в службу хранилища Azure.

![Журнал действий](./media/data-lake-store-security-overview/activity-logs.png "Журнал действий")

Дополнительные сведения о работе с журналами действий см. в статье [Просмотр журналов действий для аудита действий с ресурсами](../azure-resource-manager/management/view-activity-logs.md).

### <a name="diagnostics-logs"></a>Раздел "Журналы диагностики"

Вы можете включить аудит доступа к данным и ведение журнала диагностики в портал Azure и отправить журналы в учетную запись хранения BLOB-объектов Azure, концентратор событий или журналы Azure Monitor.

![Раздел "Журналы диагностики"](./media/data-lake-store-security-overview/diagnostic-logs.png "Раздел "Журналы диагностики"")

Дополнительные сведения о работе с журналами диагностики в Data Lake Storage 1-го поколения см. в статье [Доступ к журналам диагностики Azure Data Lake Storage 1-го поколения](data-lake-store-diagnostic-logs.md).

## <a name="summary"></a>Сводка

Корпоративным клиентам нужна безопасная и простая в использовании облачная платформа для аналитики данных. Data Lake Storage 1-го поколения полностью отвечает этим требованиям благодаря интеграции с Azure Active Directory. Это позволяет реализовать управление удостоверениями и проверку подлинности, авторизацию с помощью списков управления доступом, сетевую изоляцию, шифрование данных при передаче и хранении, а также аудит.

Если вы хотите увидеть новые функции в Data Lake Storage 1-го поколения, оставьте свой отзыв на [форуме Data Lake Storage 1-го поколения UserVoice](https://feedback.azure.com/forums/327234-data-lake).

## <a name="see-also"></a>См. также

* [Общие сведения об Azure Data Lake Storage Gen1](data-lake-store-overview.md)
* [Начало работы с Data Lake Storage 1-го поколения](data-lake-store-get-started-portal.md)
* [Защита данных в Data Lake Storage Gen1](data-lake-store-secure-data.md)