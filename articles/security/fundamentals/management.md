---
title: Усиление защиты удаленного управления в Azure | Документация Майкрософт
description: В этой статье рассматриваются шаги по улучшению безопасности удаленного управления при администрировании Microsoft Azureных сред, включая облачные службы, виртуальные машины и пользовательские приложения.
services: security
documentationcenter: na
author: TerryLanfear
manager: rkarlin
editor: TomSh
ms.assetid: 2431feba-3364-4a63-8e66-858926061dd3
ms.service: security
ms.subservice: security-fundamentals
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 04/08/2020
ms.author: terrylan
ms.openlocfilehash: 73d82efed438d447c7af3bfc54d5c3fc22cdd819
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/09/2020
ms.locfileid: "87921934"
---
# <a name="security-management-in-azure"></a>Управление безопасностью в Azure
Подписчики Azure могут управлять своими облачными средами с помощью нескольких устройств, в том числе рабочих станций управления, ПК для разработки и даже устройств привилегированных пользователей, у которых есть разрешения на выполнение конкретных задач. В некоторых случаях функции администрирования выполняются через веб-консоли, такие как [портал Azure](https://azure.microsoft.com/features/azure-portal/). Кроме того, пользователи могут устанавливать прямое подключение к Azure из локальной системы, используя виртуальные частные сети (VPN), службы терминалов, протоколы клиентских приложений или API управления службами Azure (программным путем). Они также могут использовать присоединенные к домену или изолированные и неуправляемые конечные точки клиентов, например планшеты и смартфоны.

Хотя все эти возможности доступа и управления и предоставляют множество преимуществ, многообразие вариантов увеличивает риски в облачном развертывании. Например, может быть сложно управлять административными действиями, отслеживать их и вести их аудит. Многообразие вариантов также может создавать угрозу безопасности, так как доступ к конечным точкам клиентов, используемым для управления облачными службами, не регулируется. При использовании общедоступных или личных рабочих станций для разработки инфраструктуры и управления ею открываются новые направления непрогнозируемых угроз, например при просмотре веб-страниц (атаки на предприятие) или использовании электронной почты (социотехника и фишинг).

![Схема, показывающая различные способы подключения угроз.](./media/management/typical-management-network-topology.png)

В таких средах увеличивается вероятность атак, так как в них сложно предусмотреть политики и механизмы безопасности, которые позволят надлежащим образом управлять доступом множества различных конечных точек к интерфейсу Azure (такому как API управления службами Azure).

### <a name="remote-management-threats"></a>Угрозы при удаленном управлении
Злоумышленники часто пытаются скомпрометировать учетные данные (например, с помощью подбора пароля, фишинга и сбора учетных данных) или обманом вынудить пользователей запустить вредоносный код (например, с помощью скрытых загрузок с вредоносных веб-сайтов или вредоносных вложений в сообщениях электронной почты), чтобы получить привилегированный доступ. В облачной среде, управляемой удаленно, нарушение безопасности учетной записи создает высокий риск, так как она предоставляет доступ с любых конечных точек и в любое время.

Даже если основные учетные записи администраторов находятся под строгим контролем, с помощью учетных записей пользователей более низкого уровня можно воспользоваться уязвимостями в стратегии безопасности. Отсутствие достаточных знаний об обеспечении безопасности также может приводить к нарушениям, ведь пользователи могут случайно раскрыть или предоставить сведения о своей учетной записи.

Если рабочая станция пользователя также служит для выполнения задач администрирования, компрометации можно ожидать со многих направлений. Вирус может проникнуть в систему во время просмотра веб-сайтов, использования сторонних средств и средств с открытым исходным кодом или открытия вредоносного файла документа, который содержит программу-троян.

Как правило, атаки настольных компьютеров, которые влекут нарушения безопасности данных, осуществляют с помощью эксплойтов для браузеров, подключаемых модулей (таких как Flash, PDF и Java), а также целевого фишинга (по электронной почте). Если эти компьютеры используют для разработки других ресурсов или управления ими, у них могут быть разрешения административного уровня или уровня обслуживания, позволяющие получать доступ к активным серверам или сетевым устройствам, чтобы выполнять операции.

### <a name="operational-security-fundamentals"></a>Основы безопасности работы
Чтобы сделать управление и выполнение операций безопаснее, можно уменьшить количество возможных точек входа, снизив риск атаки клиента. Это можно сделать с помощью таких принципов безопасности, как "разделение обязанностей" и "разделение сред".

Изолируйте важные функции друг от друга, чтобы уменьшить вероятность того, что ошибка на одном уровне приведет к нарушению на другом. Примеры:

* При выполнении задач администрирования следует избегать действий, которые могут привести к нарушениям в системе безопасности (например, если администратор откроет вредоносное вложение в сообщении электронной почты, оно может заразить сервер инфраструктуры).
* Рабочую станцию, которая служит для выполнения операций, связанных с данными высокой конфиденциальности, не следует использовать в целях, связанных с высоким риском, например для просмотра страниц в Интернете.

Уменьшите площадь атак на систему, удалив ненужное программное обеспечение. Пример.

* Стандартная рабочая станция администрирования, поддержки или разработки не требует установки клиента электронной почты или других повышающих эффективность приложений, если основное предназначение устройства — управление облачными службами.

Клиентские системы, которые имеют доступ администратора к компонентам инфраструктуры, должны подчиняться наиболее строгой политике для снижения угроз безопасности. Примеры:

* Политики безопасности могут включать параметры групповой политики, которые запрещают открытый доступ к Интернету с устройства и применяют ограничительную настройку брандмауэра.
* При необходимости в прямом доступе используйте VPN-подключение по протоколу IPsec.
* Настройте отдельные домены Active Directory для управления и разработки.
* Изолируйте сетевой трафик рабочей станции управления и включите его фильтрацию.
* Используйте антивредоносное ПО.
* Реализуйте многофакторную проверку подлинности, чтобы уменьшить риск кражи учетных данных.

Консолидация ресурсов доступа и удаление неуправляемых конечных точек также упрощают управление.

### <a name="providing-security-for-azure-remote-management"></a>Обеспечение безопасности удаленного управления Azure
Azure предоставляет механизмы безопасности, предназначенные помочь администраторам облачных служб и виртуальных машин Azure. Эти механизмы включают в себя следующее:

* Проверка подлинности и [Управление доступом на основе ролей Azure (Azure RBAC)](../../role-based-access-control/role-assignments-portal.md).
* мониторинг, ведение журналов и аудит;
* сертификаты и шифрование подключений;
* портал веб-управления;
* фильтрацию сетевых пакетов.

Если настроить безопасность на стороне клиента и развернуть шлюз управления в центре обработки данных, можно ограничить административный доступ к облачным приложениям и данным, а также выполнять его мониторинг.

> [!NOTE]
> Выполнение некоторых рекомендаций в этой статье может привести к более интенсивному использованию данных, а также сетевых и вычислительных ресурсов, а значит, и к дополнительным затратам на лицензии или подписки.
>
>

## <a name="hardened-workstation-for-management"></a>Усиление защиты рабочей станции управления
Цель повышения защиты рабочей станции — оставить только самые важные функции, необходимые для работы, чтобы свести к минимуму количество возможных направлений атак. Усиление защиты системы предусматривает уменьшение количества установленных служб и приложений, ограничение работы приложений, ограничение доступа к сети только до рамок необходимого, а также постоянное своевременное обновление системы. Кроме того, при использовании рабочей станции с усиленной защитой для управления средства и действия администрирования выполняются отдельно от других пользовательских задач.

Чтобы уменьшить количество возможных направлений атак физической инфраструктуры в локальной корпоративной среде, вы можете выделить отдельные сети для управления, реализовать пропускную систему для серверных задач и настроить работу станций в защищенных областях сети. В облачной и гибридной ИТ-моделях сложнее защитить службы управления из-за отсутствия физического доступа к ИТ-ресурсам. Реализация решений по защите требует тщательной настройки программного обеспечения, а также настройки процессов обеспечения безопасности и детальных политик.

Если использовать на защищенной рабочей станции для облачного управления или разработки приложений программное обеспечение с минимальными привилегиями, это снижает риск возникновения инцидентов безопасности за счет стандартизации сред удаленного управления и разработки. Усиление защиты рабочей станции помогает предотвратить компрометацию учетных записей, используемых для управления важными облачными ресурсами, устраняя распространенные уязвимости, на которые рассчитаны вредоносные программы и эксплойты. В частности, вы можете использовать [Windows AppLocker](https://technet.microsoft.com/library/dd759117.aspx) и Hyper-V, чтобы управлять поведением клиентской системы, изолировать ее работу и уменьшить риски, в том числе со стороны почты или Интернета.

Администратор рабочей станции с усиленной защитой использует обычную учетную запись пользователя (что предотвращает выполнение действий административного уровня), а управление связанными приложениями осуществляется согласно списку разрешений. Ниже приведены основные характеристики рабочей станции с усиленной защитой:

* Активное сканирование и исправление. Разверните антивредоносное ПО, выполняйте регулярное сканирование уязвимостей и своевременно обновляйте все рабочие станции с использованием новых обновлений системы безопасности.
* Ограниченная функциональность. Удалите все ненужные приложения и отключите лишние службы (запускаемые автоматически).
* Усиление защиты сети. Применяйте правила брандмауэра Windows, чтобы разрешить использовать только допустимые IP-адреса, порты, а также URL-адреса, связанные с управлением Azure. Кроме того, заблокируйте входящие удаленные подключения к рабочей станции.
* Ограничение на выполнение. Разрешите использовать только набор предопределенных исполняемых файлов, необходимых для управления (также называется запретом по умолчанию). По умолчанию пользователям должно быть запрещено запускать программы, если они явно не заданы в списке разрешений.
* Минимальные привилегии. У пользователей рабочей станции управления не должно быть прав администратора на локальном компьютере. Таким образом они не смогут (преднамеренно или случайно) изменить конфигурацию системы или системные файлы.

Все это можно реализовать принудительно с помощью [объектов групповой политики](../../active-directory-domain-services/manage-group-policy.md) (GPO) в доменных службах Active Directory, применив эти объекты ко всем учетным записям управления в локальном домене управления.

### <a name="managing-services-applications-and-data"></a>Управление службами, приложениями и данными
Чтобы настроить облачные службы Azure, можно использовать портал Azure или API управления службами Azure, интерфейс командной строки Windows PowerShell или пользовательское приложение, которое поддерживает использование интерфейсов RESTful. К службам, которые используют эти механизмы, относятся Azure Active Directory (Azure AD), служба хранилища Azure, Веб-сайты Azure, виртуальная сеть Azure и другие.

При необходимости в приложениях, развернутых в виртуальной машине, доступны клиентские средства и интерфейсы, такие как консоль управления (MMC), консоль управления предприятием (например, Microsoft System Center или Windows Intune), или другие приложения для управления, к примеру Microsoft SQL Server Management Studio. Обычно эти средства находятся в корпоративной среде или сети клиента. Их работа может зависеть от определенных сетевых протоколов, таких как протокол удаленного рабочего стола (RDP), которые требуют прямого подключения с отслеживанием состояния. У некоторых приложений могут быть веб-интерфейсы, к которым не должен быть открыт общий доступ или доступ через Интернет.

Вы можете ограничить доступ для управления инфраструктурой и службами платформы в Azure, настроив [многофакторную проверку подлинности](/azure/active-directory/authentication/multi-factor-authentication), [сертификаты управления X.509](https://blogs.msdn.microsoft.com/azuresecurity/2015/07/13/certificate-management-in-azure-dos-and-donts/) и правила брандмауэра. Портал и API управления службами Azure требуют использования протокола TLS. Тем не менее, при развертывании служб и приложений в Azure нужно предпринять меры по защите, в зависимости от развертываемого объекта. В большинстве случаев эти механизмы можно с легкостью включить при стандартизированной конфигурации рабочей станции с усиленной защитой.

### <a name="management-gateway"></a>Шлюз управления
Чтобы полностью централизовать административный доступ и упростить мониторинг и ведение журналов, можно развернуть выделенный сервер [шлюза удаленных рабочих столов](https://technet.microsoft.com/library/dd560672) в локальной сети, подключенной к среде Azure.

Шлюз удаленных рабочих столов — это прокси-служба RDP на основе политик, которая обеспечивает принудительное выполнение требований к безопасности. Реализация этого шлюза в сочетании с защитой доступа к сети Windows Server позволяет гарантировать, что подключение смогут устанавливать только те клиенты, которые отвечают определенным критериям работоспособности системы безопасности, установленным с помощью объектов групповой политики AD DS. Дополнительно

* Подготовьте [сертификат управления Azure](https://msdn.microsoft.com/library/azure/gg551722.aspx) в шлюзе удаленных рабочих столов так, чтобы он был единственным узлом с доступом к порталу Azure.
* Присоедините шлюз удаленных рабочих столов к тому же [домену управления](https://technet.microsoft.com/library/bb727085.aspx), к которому относятся рабочие станции администратора. Это необходимо, если вы используете подключение ExpressRoute или VPN типа "сеть — сеть" с протоколом IPsec в домене, в котором настроено одностороннее отношение доверия к Azure AD, или используете учетные данные федеративно для локального экземпляра AD DS и Azure AD.
* Настройте [политику авторизации клиентских подключений](https://technet.microsoft.com/library/cc753324.aspx), чтобы шлюз удаленных рабочих столов проверял, является ли имя клиентского компьютера допустимым (присоединен ли компьютер к домену) и разрешен ли компьютеру доступ к порталу Azure.
* Используйте протокол IPsec для [VPN Azure](https://azure.microsoft.com/documentation/services/vpn-gateway/), чтобы дополнительно защитить трафик управления от перехвата данных и кражи маркеров, или изолированное интернет-подключение через [Azure ExpressRoute](https://azure.microsoft.com/documentation/services/expressroute/).
* Включите многофакторную проверку подлинности (с помощью [Многофакторной идентификации Azure](/azure/active-directory/authentication/multi-factor-authentication)) или проверку подлинности с помощью смарт-карт для администраторов, которые входят через шлюз удаленных рабочих столов.
* Настройте [ограничения IP-адресов](https://azure.microsoft.com/blog/2013/08/27/confirming-dynamic-ip-address-restrictions-in-windows-azure-web-sites/) источников или [группы безопасности сети](/azure/virtual-network/security-overview) в Azure, чтобы свести к минимуму количество разрешенных конечных точек управления.

## <a name="security-guidelines"></a>Рекомендации по безопасности
Как правило, методы повышения безопасности для рабочих станций администрирования, предназначенных для работы с облаком, аналогичны процедурам для любой локальной рабочей станции. Это, например, методы минимальной сборки и ограничения разрешений. У некоторых уникальных аспектов облачного управления больше общего с удаленным управлением или корпоративным управлением по внештатному каналу. К ним относятся использование и аудит учетных данных, удаленный доступ с повышенным уровнем безопасности, обнаружение угроз и реагирование.

### <a name="authentication"></a>Аутентификация
Вы можете настроить ограничения входа Azure, чтобы ограничить доступ к инструментам администрирования и возможность запрашивать доступ для аудита по исходным IP-адресам. Чтобы помочь Azure определить клиенты управления (рабочие станции и (или) приложения), можно настроить оба SMAPI (с помощью разработанных клиентом средств, таких как командлеты Windows PowerShell) и портал Azure для установки клиентских сертификатов управления в дополнение к сертификатам TLS/SSL. Мы также советуем использовать многофакторную проверку подлинности для доступа администраторов.

В одних приложениях или службах, развертываемых в Azure, могут быть свои механизмы проверки подлинности доступа пользователей и администраторов, тогда как другие пользуются лишь механизмом Azure AD. В зависимости от того, используете ли вы учетные данные федеративно через службы федерации Active Directory (AD FS), синхронизируете службы каталогов или обслуживаете учетные записи пользователей только в облаке, использование диспетчера [Microsoft Identity Manager](https://technet.microsoft.com/library/mt218776.aspx) (который является частью Azure AD Premium) упрощает управление жизненным циклом удостоверений ресурсов.

### <a name="connectivity"></a>Соединение
Есть несколько механизмов, которые позволяют обезопасить клиентские подключения к виртуальным сетям Azure. Два из них, [VPN типа "сеть — сеть"](https://channel9.msdn.com/series/Azure-Site-to-Site-VPN) и [VPN типа "точка — сеть"](/azure/vpn-gateway/vpn-gateway-point-to-site-create), позволяют использовать стандартные отраслевые протоколы IPsec (VPN типа "сеть — сеть") или [Secure Socket Tunneling Protocol](https://technet.microsoft.com/magazine/2007.06.cableguy.aspx) (SSTP) (VPN типа "точка — сеть") для шифрования и туннелирования. При подключении к общедоступной системе управления службами Azure, например к порталу Azure, платформа Azure требует использовать протокол HTTPS.

Изолированная рабочая станция с усиленной защитой, которая не подключается к Azure через шлюз удаленных рабочих столов, должна работать в VPN типа "точка — сеть", использующей протокол SSTP, чтобы создать начальное подключение к виртуальной сети Azure, а затем установить RDP-подключение с туннелем VPN из этой сети к отдельным виртуальным машинам.

### <a name="management-auditing-vs-policy-enforcement"></a>Аудит управления и принудительное применение политик
Как правило, чтобы обеспечить безопасность процессов управления, используют два подхода: аудит и принудительные политики. Их одновременное использование обеспечивает комплексный контроль, но в некоторых случаях это невозможно. Кроме того, отличаются степень риска, издержки и объем действий по управлению безопасностью при каждом подходе. Это связано, в частности, с уровнем доверия, установленным у отдельных пользователей и в системных архитектурах.

Мониторинг, ведение журналов и аудит выступают основой для отслеживания административных действий и формирования общих сведений о них, но вести подробный аудит всех действий не всегда целесообразно, так это создает большой объем данных. Лучшее решение — вести аудит эффективности политик управления.

Принудительное применение политик, в том числе средств ограничения доступа, обеспечивает регулирование действий администраторов программными методами и гарантирует применение всех возможных мер защиты. Ведение журналов служит подтверждением принудительного выполнения в дополнение к записям о том, кто, где и когда выполнил действие. Оно также позволяет вам проверять сведения о том, насколько администраторы соблюдают политики, в том числе перекрестно, и предоставляет доказательства их действий.

## <a name="client-configuration"></a>Настройка клиента
Мы советуем задать для рабочей станции с усиленной защитой одну из трех основных конфигураций. Главные их различия заключаются в стоимости, удобстве использования и возможностях доступа. При этом у всех конфигураций одинаковый профиль безопасности. В следующей таблице представлен краткий анализ преимуществ и рисков при каждой конфигурации. (Обратите внимание, что "Корпоративный ПК" означает стандартную конфигурацию настольного ПК, развертываемую для всех пользователей домена, независимо от ролей.)

| Конфигурация | Преимущества | Недостатки |
| --- | --- | --- |
| Изолированная рабочая станция с усиленной защитой |Строго контролируемая рабочая станция |Больше затрат на выделенные настольные компьютеры |
| - | Ниже риск эксплойтов приложений |Требуется больше действий по управлению |
| - | Четкое разделение обязанностей | - |
| Корпоративный ПК в качестве виртуальной машины |Меньше затрат на оборудование | - |
| - | Разделение ролей и приложений | - |

Важно, чтобы рабочая станция с усиленной защитой была узлом, а не гостем, и ее операционная система не была связана с оборудованием. Она должна поддерживать принцип чистого источника (также известный как принцип безопасного источника). Это значит, что больше всего необходимо увеличить защиту узла. В противном случае система, в которой находится рабочая станция с усиленной защитой (гость), будет подвержена атакам.

Вы можете дополнительно разделить административные функции для каждой рабочей станции с усиленной защитой по образам выделенных систем, чтобы в каждом из них были только инструменты и разрешения, необходимые для управления выбранными облачными приложениями и приложениями Azure, назначив конкретные локальные объекты групповой политики AD DS для выполнения необходимых задач.

В ИТ-средах без локальной инфраструктуры (например, объекты групповой политики не имеют доступа к локальному экземпляру доменных служб Active Directory, потому что все серверы находятся в облаке) такая служба, как [Microsoft Intune](https://technet.microsoft.com/library/jj676587.aspx), упрощает развертывание и обслуживание конфигураций рабочих станций.

### <a name="stand-alone-hardened-workstation-for-management"></a>Изолированная рабочая станция управления с усиленной защитой
Изолированная рабочая станция с усиленной защитой предусматривает, что у администраторов есть один ПК или ноутбук для задач администрирования и второе такое устройство для выполнения других задач. На рабочей станции для управления службами Azure не нужно устанавливать дополнительные приложения. Кроме того, использование рабочих станций, поддерживающих [доверенный платформенный модуль](https://technet.microsoft.com/library/cc766159) или аналогичную технологию шифрования на уровне оборудования, упрощает проверку подлинности устройств и предотвращение атак определенных типов. Доверенный платформенный модуль также может поддерживать полную защиту системного диска благодаря технологии [шифрования диска BitLocker](https://technet.microsoft.com/library/cc732774.aspx).

В сценарии изолированной рабочей станции с усиленной защитой (показанном ниже) в локальном экземпляре брандмауэра Windows (вместо него может быть брандмауэр стороннего клиента) настроена блокировка входящих подключений, например по протоколу RDP. Администраторы могут входить на рабочую станцию с усиленной защитой и устанавливать подключения к виртуальной сети Azure через VPN, а затем запускать RDP-сеансы с подключением к Azure. Но они не могут входить на компьютеры организации и использовать RDP для подключения к рабочей станции с усиленной защитой.

![Схема, на которой показан изолированный сценарий рабочей станции с усиленной защитой.](./media/management/stand-alone-hardened-workstation-topology.png)

### <a name="corporate-pc-as-virtual-machine"></a>Корпоративный ПК в качестве виртуальной машины
В случаях, когда нерентабельно и неудобно содержать отдельную изолированную рабочую станцию с усиленной защитой, на станции можно разместить виртуальную машину для выполнения обычных пользовательских задач.

![Схема, показывающая зафиксированную рабочую станцию, в которой размещается виртуальная машина для выполнения задач, не связанных с администрированием.](./media/management/hardened-workstation-enabled-with-hyper-v.png)

Чтобы избежать множества рисков для безопасности, которые может создавать использование одной станции для управления системами и других повседневных задач, на рабочей станции с усиленной защитой можно развернуть виртуальную машину Windows Hyper-V. Эту виртуальную машину можно использовать в качестве корпоративного ПК. Среду корпоративного ПК можно оставить изолированной от узла. Это уменьшит количество возможных направлений атак и позволит отделить повседневные действия пользователя (например, использование электронной почты) от задач администрирования, связанных с конфиденциальностью.

Виртуальная машина корпоративного ПК работает в защищенном пространстве и дает возможность использовать пользовательские приложения. Узел остается чистым источником и принудительно применяет строгие сетевые политики к корневой операционной системе (например, блокирует доступ к виртуальной машине по протоколу RDP).

## <a name="best-practices"></a>Рекомендации
При управлении приложениями и данными в Azure учитывайте дополнительные рекомендации, приведенные ниже.

### <a name="dos-and-donts"></a>Рекомендации
Не думайте, что если рабочая станция защищена, не нужно соблюдать другие общие требования к безопасности. Риск больше, так как у учетных записей администраторов обычно повышенные права доступа. В таблице ниже показаны примеры рисков и альтернативные безопасные меры.

| Не рекомендуется | Рекомендуется |
| --- | --- |
| Не отправлять учетные данные электронной почты для доступа администратора или других секретов (например, TLS/SSL или сертификаты управления). |Соблюдайте конфиденциальность: предоставляйте имена и пароли учетных записей устно (но не храните их в ящике голосовой почты), устанавливайте сертификаты клиентов и серверов удаленно (используя зашифрованный сеанс), скачивайте данные из защищенных сетевых папок и распространяйте их с помощью съемных носителей. |
| - | Поддерживайте упреждающее управление жизненным циклом сертификатов управления. |
| Не храните пароли учетных записей в незашифрованном или в нехэшированном виде в хранилищах приложений (например, в электронных таблицах, на сайтах SharePoint или в файловых ресурсах). |Установите принципы управления безопасностью и политики усиления безопасности системы и примените их к среде разработки. |
| - | Используйте правила привязки сертификатов [Enhanced Mitigation Experience Toolkit 5.5](https://technet.microsoft.com/security/jj653751), чтобы предоставить надлежащие права доступа к сайтам Azure, которые используют SSL- или TLS-сертификат. |
| Не предоставляйте администраторам одни и те же учетные записи и пароли, не используйте пароли повторно для нескольких пользовательских учетных записей или служб, особенно тех записей, которые служат для доступа к социальным сетям или выполнения других действий, не связанных c администрированием. |Выделите учетную запись Майкрософт для управления подпиской Azure, которая не будет использоваться для работы с личной электронной почтой. |
| Не отправляйте файлы конфигурации по электронной почте. |Файлы и профили конфигурации следует устанавливать из надежного источника (например, зашифрованного USB-устройства флэш-памяти). Для этого не стоит использовать механизм, который легко скомпрометировать, такой как электронная почта. |
| Не используйте простые и ненадежные пароли для входа. |Принудительно примените политики использования надежных паролей, циклы окончания срока действия паролей (изменение после первого использования), время ожидания для консолей и автоматическую блокировку учетных записей. Используйте систему управления клиентскими паролями с многофакторной проверкой подлинности при доступе к хранилищу паролей. |
| Не открывайте порты управления для доступа через Интернет. |Заблокируйте порты и IP-адреса Azure, чтобы ограничить доступ к управлению. Дополнительные сведения см. в техническом документе [Azure Network Security](https://download.microsoft.com/download/4/3/9/43902EC9-410E-4875-8800-0788BE146A3D/Windows%20Azure%20Network%20Security%20Whitepaper%20-%20FINAL.docx) (Безопасность в сети Azure). |
| - | Используйте брандмауэры, VPN и защиту доступа к сети для всех подключений, предназначенных для управления. |

## <a name="azure-operations"></a>Операции в Azure
В поддержке среды Azure участвуют инженеры по эксплуатации и технический персонал службы поддержки Майкрософт. У них есть доступ к рабочим системам Azure, которые используют [компьютеры (рабочие станции с усиленной защитой) с виртуальными машинами](#stand-alone-hardened-workstation-for-management). Эти компьютеры подготовлены для внутреннего доступа к корпоративной сети и использования соответствующих приложений (например, почты, интрасети и т. д.). На всех компьютерах рабочей станции управления есть доверенные платформенные модули, диск загрузки узла зашифрован с помощью BitLocker, и они присоединены к специальному подразделению в основном корпоративном домене Майкрософт.

Для принудительного усиления безопасности системы используется групповая политика с централизованным обновлением программного обеспечения. В результате сбора данных о событиях на рабочих станциях управления формируются журналы событий (например, безопасности и AppLocker), которые хранятся в центральном расположении и обеспечивают возможность аудита и анализа.

Кроме того, чтобы подключиться к рабочей сети Azure, используются компьютеры, выполняющие функцию "поля перехода". Это компьютеры, выделенные в сети Майкрософт, которые требуют двухфакторной проверки подлинности.

## <a name="azure-security-checklist"></a>Контрольный список безопасности Azure
Сокращение числа задач, которые могут выполнять администраторы на рабочей станции с усиленной защитой, сводит к минимуму количество возможных направлений атак в средах разработки и управления. Защитить рабочую станцию с усиленной защитой вам помогут следующие механизмы:

* Усиление безопасности Internet Explorer. Браузер Internet Explorer (или любой другой веб-браузер) — это основная точка входа вредоносного кода, так как эта программа интенсивно взаимодействует с внешними серверами. Просмотрите политики клиента и принудительно переведите браузер в защищенный режим работы, отключив надстройки и скачивание файлов и включив фильтрацию с помощью [Microsoft SmartScreen](https://technet.microsoft.com/library/jj618329.aspx). Убедитесь, что отображаются предупреждения безопасности. Воспользуйтесь преимуществами зон Интернета и создайте список доверенных сайтов, для которых вы настроили достаточный уровень усиления защиты. Заблокируйте все другие сайты и внутренние коды браузера, такие как ActiveX и Java.
* Работа с правами обычного пользователя. Работа в качестве обычного пользователя предоставляет ряд преимуществ. Самое большое из них — так сложнее украсть учетные данные администратора с помощью вредоносных программ. Кроме того, у учетной записи обычного пользователя нет привилегий высокого уровня для использования корневой операционной системы, а многие параметры конфигурации и API заблокированы по умолчанию.
* AppLocker. С помощью [AppLocker](https://technet.microsoft.com/library/ee619725.aspx) можно ограничить программы и скрипты, которые могут запускать пользователи. AppLocker можно запускать в режиме аудита или принудительного применения. По умолчанию в AppLocker есть разрешающее правило, которое позволяет пользователям с маркером администратора выполнять любой код на стороне клиента. Это правило служит для того, чтобы администраторы не блокировали себя, и применяется только к маркерам с повышенными привилегиями. См. также дополнительные сведения о целостности кода в рамках [основ безопасности](https://technet.microsoft.com/library/dd348705.aspx) Windows Server.
* Подписывание кода. Подписывание кода всех средств и скриптов, которые используют администраторы, создает управляемый механизм для развертывания политик защиты приложений. Хэш не масштабируется при быстром изменении кода, а использование путей к файлам не обеспечивает высокий уровень безопасности. Правила AppLocker следует использовать в сочетании с [политикой выполнения](https://technet.microsoft.com/library/ee176961.aspx) PowerShell, которая позволяет [выполнять](https://technet.microsoft.com/library/hh849812.aspx) только определенные подписанные скрипты и код.
* Групповая политика. Создайте глобальную политику администрирования и примените ее ко всем рабочим станциям домена, используемым для управления (и заблокируйте доступ для всех остальных станций), а также к учетным записям пользователей, прошедшим проверку подлинности на этих рабочих станциях.
* Подготовка с повышенным уровнем безопасности. Защитите базовый образ рабочей станции с усиленной защитой от незаконного получения. Предпринимайте меры по обеспечению безопасности, такие как шифрование и изолированное хранение образов, виртуальных машин и скриптов, а также ограничьте доступ к ним (вы можете использовать возврат и извлечение, которые подлежат аудиту).
* Установка исправлений. Вы можете поддерживать согласованность сборки (или использовать отдельные образы для разработки, эксплуатации и других задач администрирования), регулярно проверять наличие изменений и вредоносных программ, обновлять сборки и активировать компьютеры только при необходимости.
* Шифрование. На рабочих станциях управления должны быть установлены доверенные платформенные модули. Это повышает безопасность включения [шифрованной файловой системы](https://technet.microsoft.com/library/cc700811.aspx) (EFS) и BitLocker.
* Регулирование. Используйте объекты групповой политики AD DS, чтобы контролировать все интерфейсы администраторов Windows, например предоставление общего доступа к файлам. Включите рабочие станции управления в процессы аудита, мониторинга и ведения журналов. Отслеживайте все действия доступа и использования, которые выполняют администраторы и разработчики.

## <a name="summary"></a>Итоги
Использование рабочей станции с усиленной защитой для администрирования облачных служб, виртуальных машин и приложений Azure помогает избежать множества рисков и угроз, которые может представлять удаленное управление важной ИТ-инфраструктурой. Azure и Windows предоставляют механизмы, которые можно использовать для защиты подключений и управления ими, а также управления проверкой подлинности и поведением клиентов.

## <a name="next-steps"></a>Дальнейшие шаги
Ниже приведены ресурсы, с помощью которых можно получить общие сведения о службах Azure и связанных службах Microsoft, а также некоторые дополнения к информации, содержащейся в этом документе.

* [Securing Privileged Access](https://docs.microsoft.com/windows-server/identity/securing-privileged-access/securing-privileged-access) (Защита привилегированного доступа). Ознакомьтесь с техническими сведениями по проектированию и созданию безопасной рабочей станции администрирования для управления Azure.
* [Microsoft Trust Center](https://microsoft.com/en-us/trustcenter/cloudservices/azure). Узнайте о возможностях Azure, которые позволяют защитить структуру и рабочие нагрузки Azure, используемые на соответствующей платформе.
* [Microsoft Security Response Center](https://www.microsoft.com/msrc). Это место, куда можно сообщать об уязвимостях, в том числе о проблемах Azure. Это также можно сделать по почте, отправив сообщение по адресу [secure@microsoft.com](mailto:secure@microsoft.com).
