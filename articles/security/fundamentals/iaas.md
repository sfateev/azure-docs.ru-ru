---
title: Рекомендации по безопасности для рабочих нагрузок IaaS в Azure | Документация Майкрософт
description: " Перенос рабочих нагрузок в Azure IaaS позволяет повторно оценить структуру "
services: security
documentationcenter: na
author: terrylanfear
manager: rkarlin
editor: TomSh
ms.assetid: 02c5b7d2-a77f-4e7f-9a1e-40247c57e7e2
ms.service: security
ms.subservice: security-fundamentals
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 10/28/2019
ms.author: terrylan
ms.openlocfilehash: 03258bf204491afce4635828b3a33a06886aca2d
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/09/2020
ms.locfileid: "87448387"
---
# <a name="security-best-practices-for-iaas-workloads-in-azure"></a>Рекомендации по безопасности для рабочих нагрузок IaaS в Azure
В этой статье описываются рекомендации по обеспечению безопасности для виртуальных машин и операционных систем.

Рекомендации основаны на общем мнении и подходят для работы с текущими возможностями и наборами функций платформы Azure. Так как со временем мнения и технологии могут меняться, эта статья будет обновляться для отражения таких изменений.

В большинстве сценариев инфраструктуры как услуги (IaaS) [виртуальные машины (ВМ) Azure](/azure/virtual-machines/) являются основной рабочей нагрузкой для организаций, использующих облачные вычисления. Этот заметно в [гибридных сценариях](https://social.technet.microsoft.com/wiki/contents/articles/18120.hybrid-cloud-infrastructure-design-considerations.aspx), в которых организациям необходимо медленно переносить рабочие нагрузки в облако. В таких случаях необходимо следовать [общим рекомендациям по обеспечению безопасности для IaaS](https://social.technet.microsoft.com/wiki/contents/articles/3808.security-considerations-for-infrastructure-as-a-service-iaas.aspx), а также выполнять рекомендации по обеспечению безопасности на всех виртуальных машинах.

## <a name="protect-vms-by-using-authentication-and-access-control"></a>Защита виртуальных машин с помощью проверки подлинности и контроля доступа
Для защиты виртуальной машины в первую очередь необходимо предоставить возможность настройки новых виртуальных машин и доступа к ним только авторизованным пользователям.

> [!NOTE]
> Чтобы повысить безопасность виртуальных машин Linux в Azure, можно выполнить интеграцию с проверкой подлинности Azure AD. При использовании [проверки подлинности Azure AD для виртуальных машин Linux](/azure/virtual-machines/linux/login-using-aad)вы централизованно контролируете и применяйте политики, разрешающие или запрещающие доступ к виртуальным машинам.
>
>

**Рекомендация**: контролируйте доступ к виртуальной машине.   
**Сведения**: используйте [политики Azure](/azure/azure-policy/azure-policy-introduction) для создания правил для ресурсов в вашей организации и разработайте настраиваемые политики. Примените эти политики к ресурсам, таким как [группы ресурсов](/azure/azure-resource-manager/resource-group-overview). Виртуальные машины, которые относятся к группе ресурсов, наследуют ее политики.

Если в организации много подписок, для эффективного управления доступом к ним, их политиками и соответствием требуется особый подход. [Группы управления Azure](/azure/azure-resource-manager/management-groups-overview) предоставляют уровень области действия подписок. Подписки упорядочиваются в группы управления (контейнеры). К этим группам применяются условия системы управления. Все подписки в группе управления автоматически наследуют условия, применяемые к группе. Группы управления обеспечивают корпоративное управление в больших масштабах независимо от типа подписки.

**Рекомендация**: сократите количество вариантов установки и развертывания виртуальных машин.   
**Сведения**: используйте шаблоны [Azure Resource Manager](/azure/azure-resource-manager/resource-group-authoring-templates), чтобы реализовывать фиксированные варианты развертывания, а также упростить процедуры анализа и учета виртуальных машин в вашей среде.

**Рекомендация**: обеспечьте безопасность привилегированного доступа.   
**Сведения**: чтобы пользователи могли получать доступ к виртуальным машинам и настраивать их, используйте [принцип минимальных прав](https://technet.microsoft.com/windows-server-docs/identity/ad-ds/plan/security-best-practices/implementing-least-privilege-administrative-models) и встроенные роли Azure.

- [Участник виртуальных машин](../../role-based-access-control/built-in-roles.md#virtual-machine-contributor) — может управлять виртуальными машинами, но не виртуальными сетями и учетными записями хранения, к которым они подключены.
- [Участник классических виртуальных машин](../../role-based-access-control/built-in-roles.md#classic-virtual-machine-contributor) — может управлять виртуальными машинами, созданными с помощью классической модели развертывания, но не может управлять виртуальными сетями и учетными записями хранения, к которым подключены виртуальные машины.
- [Администратор системы безопасности](../../role-based-access-control/built-in-roles.md#security-admin) (только в центре безопасности) может просматривать политики безопасности и состояния безопасности, изменять политики безопасности, просматривать оповещения и рекомендации, а также закрывать предупреждения и рекомендации
- [Пользователь DevTest Labs](../../role-based-access-control/built-in-roles.md#devtest-labs-user) — может просматривать все, а также подключать, запускать, перезапускать виртуальные машины и завершать их работу.

Администраторы и соадминистраторы подписок могут изменить этот параметр. В результате они станут администраторами всех виртуальных машин в подписке. Необходимо доверять всем администраторам и соадминистраторам подписки, входящим в системы виртуальных машин.

> [!NOTE]
> Мы рекомендуем объединять виртуальные машины с одинаковым жизненным циклом в одну группу ресурсов. С помощью групп ресурсов можно развертывать и отслеживать ресурсы, а также получать сводную информацию о затратах на ресурсы.
>
>

Организации, которые управляют доступом к виртуальной машине и ее настройкой, повышают общую безопасность виртуальной машины.

## <a name="use-multiple-vms-for-better-availability"></a>Использование нескольких виртуальных машин для повышения уровня доступности
Если на виртуальной машине выполняются критически важные приложения, которым требуется высокий уровень доступности, настоятельно рекомендуем использовать несколько виртуальных машин. Для повышения уровня доступности используйте [группу доступности](../../virtual-machines/windows/manage-availability.md#configure-multiple-virtual-machines-in-an-availability-set-for-redundancy) или [зоны](../../availability-zones/az-overview.md)доступности.

Группа доступности — это логическая группа, которую можно использовать в Azure, чтобы гарантировать изоляцию ресурсов виртуальной машины во время развертывания в центре обработки данных Azure. Azure гарантирует, что виртуальные машины, размещенные в группе доступности, выполняются на нескольких физических серверах, в вычислительных стойках, в пределах единиц хранения и сетевых коммутаторов. В случае сбоя оборудования или программного обеспечения в Azure затрагивается только подмножество виртуальных машин, а приложение остается доступным для клиентов. Группы доступности — это важный элемент при создании надежных облачных решений.

## <a name="protect-against-malware"></a>Защита от вредоносных программ
В целях обнаружения и удаления вирусов, шпионских и других вредоносных программ необходимо установить средства защиты от вредоносных программ. Вы можете установить [антивредоносное по Майкрософт](antimalware.md) или решение для защиты конечных точек партнера Майкрософт ([Trend Micro](https://help.deepsecurity.trendmicro.com/Welcome.html), [Broadcom](https://www.broadcom.com/products), [McAfee](https://www.mcafee.com/us/products.aspx), [Защитник Windows](https://www.microsoft.com/windows/comprehensive-security)и [System Center Endpoint Protection](/configmgr/protect/deploy-use/endpoint-protection)).

Антивредоносное ПО Майкрософт обладает такими возможностями, как обеспечение защиты в реальном времени, плановое сканирование, исправление вредоносных действий, обновление подписей, обновление модуля защиты, отправка образцов и сбор событий исключения. Для сред, размещенных отдельно от рабочей среды, вы можете использовать расширение антивредоносных программ, чтобы защитить виртуальные машины и облачные службы.

Для упрощения развертывания и использования встроенных обнаружений (оповещений и инцидентов) антивредоносное ПО Майкрософт и партнерские решения можно интегрировать с [центром безопасности Azure](../../security-center/index.yml).

**Рекомендация**: установите решение для защиты от вредоносных программ.   
**Сведения**: [установите решение от партнера Майкрософт или антивредоносное ПО Майкрософт](../../security-center/security-center-install-endpoint-protection.md).

**Рекомендация**: для отслеживания состояния установленной защиты интегрируйте свое решение для защиты от вредоносных программ с центром безопасности.   
**Сведения**: [управляйте проблемами защиты конечных точек с помощью центра безопасности](../../security-center/security-center-partner-integration.md).

## <a name="manage-your-vm-updates"></a>Управление обновлением виртуальной машины
Виртуальными машинами Azure, например всеми виртуальными машинами в локальной среде, должны управлять пользователи. Azure не передает на них обновления Windows. Управлять обновлениями виртуальной машины необходимо самостоятельно.

**Рекомендация**: поддерживайте виртуальные машины в актуальном состоянии.   
**Сведения**: используйте решение [по управлению обновлениями](../../automation/update-management/update-mgmt-overview.md) в службе автоматизации Azure, чтобы управлять обновлениями операционной системы для компьютеров Windows и Linux, развернутых в Azure, локальных средах или других поставщиках облачных услуг. Благодаря ему вы сможете быстро оценить состояние доступных обновлений на всех компьютерах агентов и управлять установкой необходимых обновлений на серверах.

Ниже перечислены конфигурации, которые используются компьютерами, управляемыми с помощью решения "Управление обновлениями", для выполнения развертываний оценок и обновлений:

- Microsoft Monitoring Agent (MMA) для Windows или Linux;
- служба настройки требуемого состояния PowerShell;
- гибридные рабочие роли Runbook службы автоматизации;
- службы Центра обновления Майкрософт или Windows Server Update Services (WSUS) для компьютеров с Windows.

Если вы используете Центр обновления Windows, оставьте включенной функцию автоматического обновления Windows.

**Рекомендация**: убедитесь, что во время развертывания созданные образы содержат последние обновления Windows.   
**Сведения**: проверьте наличие и установите все обновления Windows — это должен быть первый шаг при каждом развертывании. Она особенно важна при развертывании ваших собственных образов или образов из вашей библиотеки. Несмотря на то, что образы из Azure Marketplace автоматически обновляются по умолчанию, после выхода общедоступной версии может существовать интервал задержки (до нескольких недель).

**Рекомендация**: периодически повторно развертывайте виртуальные машины в новой версии операционной системы.   
**Сведения**: определите виртуальную машину с помощью [шаблона Azure Resource Manager](../../azure-resource-manager/templates/template-syntax.md), чтобы повторно развертывать ее в дальнейшем. Благодаря шаблону в вашем распоряжении в нужный момент будет исправленная и защищенная виртуальная машина.

**Рекомендации**. Быстрое применение обновлений для системы безопасности к виртуальным машинам.   
**Сведения**: Включите центр безопасности Azure (уровень "бесплатный" или "Стандартный"), чтобы [найти недостающие обновления для системы безопасности и применить их](../../security-center/security-center-apply-system-updates.md).

**Рекомендация**: устанавливайте последние обновления безопасности.   
**Сведения**: одними из первых рабочих нагрузок, которые клиенты перемещают в Azure, являются лаборатории и внешние системы. Если на виртуальных машинах Azure размещены приложения или службы, к которым требуется доступ через Интернет, при установке исправлений необходимо соблюдать осторожность. Не ограничивайтесь только исправлениями операционной системы. Неисправленные уязвимости в партнерских приложениях также могут привести к проблемам, которых можно было бы избежать при эффективном управлении исправлениями.

**Рекомендация**: разверните и протестируйте решение для архивации.   
**Сведения**: архивацию следует выполнять подобно любым другим операциям. Это касается систем, входящих в рабочую среду, распространяющуюся в облако.

В системах тестирования и разработки следует применять стратегии архивации, которые могут предоставить возможности восстановления, аналогичные привычным для пользователей возможностям с учетом опыта работы в локальных средах. По возможности рабочие нагрузки, перемещенные в Azure, должны интегрироваться с существующими решениями для архивации. Кроме того, для обеспечения соответствия требованиям к архивации можно использовать [службу архивации Azure](../../backup/backup-azure-vms-first-look-arm.md).

Организации, которые не применяют политики обновления программного обеспечения, больше подвержены угрозам, связанным с использованием известных уязвимостей, которые были устранены ранее. Чтобы соответствовать отраслевым нормам, компании должны показать, что они проявляют осмотрительность и используют правильные средства управления безопасностью, обеспечивая безопасность рабочей нагрузки, находящейся в облаке.

Рекомендации по обновлению программного обеспечения для традиционных центров обработки данных и Azure IaaS имеют много общего. Мы рекомендуем проанализировать текущие политики обновления программного обеспечения, чтобы добавить в них виртуальные машины, расположенные в Azure.

## <a name="manage-your-vm-security-posture"></a>Управление системой безопасности виртуальной машины
Киберугрозы эволюционируют. Для защиты виртуальных машин требуются возможности мониторинга, способные быстро выявить угрозы, предотвратить несанкционированный доступ к ресурсам, активировать оповещения и сократить число ложных срабатываний.

Для мониторинга состояния безопасности виртуальных машин под управлением [Windows](../../security-center/security-center-virtual-machine.md) и [Linux](../../security-center/security-center-linux-virtual-machine.md) можно использовать [центр безопасности Azure](../../security-center/security-center-intro.md). В центре безопасности можно защитить виртуальные машины, используя следующие возможности:

- применение параметров безопасности операционной системы в соответствии с рекомендуемыми правилами конфигурации;
- идентификация и загрузка отсутствующих обновлений системы безопасности и критических обновлений;
- развертывание рекомендаций по защите конечных точек от вредоносных программ;
- проверка шифрования диска;
- оценка и исправление уязвимостей;
- обнаружение угроз.

Центр безопасности может активно отслеживать угрозы, а потенциальные угрозы будут отображаться в разделе оповещений системы безопасности. Коррелированные угрозы будут собраны в одном представлении, которое называется "инцидент безопасности".

Центр безопасности хранит данные в [журналах Azure Monitor](/azure/log-analytics/log-analytics-overview). Журналы Azure Monitor предоставляют язык запросов и модуль аналитики, который позволяет получить представление о работе приложений и ресурсов. Данные также собираются из [Azure Monitor](../../batch/monitoring-overview.md), решений по управлению и агентов, установленных на виртуальных машинах в локальной среде или в облаке. Такие общие функции позволяют составить полную картину всей среды.

Организации, которые не обеспечивают надежную защиту для виртуальных машин, не знают о потенциальных попытках неавторизованных пользователей обойти средства управления безопасностью.

## <a name="monitor-vm-performance"></a>Мониторинг производительности виртуальной машины
Применение ресурсов не по назначению (например, когда процессы на виртуальной машине потребляют больше ресурсов, чем положено) может стать проблемой. Снижение производительности виртуальной машины может привести к прерыванию работы служб, что в свою очередь нарушает доступность, являющуюся одним из основных принципов безопасности. Это особенно важно для виртуальных машин, на которых размещаются службы IIS или другие веб-серверы, так как высокий уровень использования ЦП или памяти может указывать на атаку типа "отказ в обслуживании" (DoS). Крайне важно не только контролировать доступ к виртуальным машинам в случае возникновения проблем, но и заранее отслеживать базовые показатели производительности в периоды штатного функционирования.

Для получения сведений о работоспособности ресурсов рекомендуется использовать [Azure Monitor](/azure/monitoring-and-diagnostics/monitoring-overview-metrics). Возможности Azure Monitor:

- [Диагностические файлы журналов ресурсов](../../azure-monitor/platform/platform-logs-overview.md): отслеживание ресурсов виртуальной машины и выявление потенциальных проблем, которые могут снизить производительность и доступность.
- [Расширение системы диагностики Azure](/azure/azure-monitor/platform/diagnostics-extension-overview): использование возможностей мониторинга и диагностики на виртуальных машинах под управлением Windows. Чтобы использовать эти возможности, необходимо включить расширение в [шаблон Azure Resource Manager](/azure/virtual-machines/windows/extensions-diagnostics-template).

Организации, которые не отслеживают производительность виртуальных машин, не способны определить, являются ли определенные изменения в показателях производительности нормальными или аномальными. Если виртуальная машина потребляет больше ресурсов, чем обычно, это может свидетельствовать об атаке, совершаемой с внешнего ресурса, или о скомпрометированном процессе, выполняемом на виртуальной машине.

## <a name="encrypt-your-virtual-hard-disk-files"></a>Шифрование файлов виртуального жесткого диска
Рекомендуется шифровать виртуальные жесткие диски (VHD) для защиты загрузочного тома и томов данных в хранилище, также нужно шифровать ключи шифрования и секреты.

[Шифрование дисков Azure](../azure-security-disk-encryption-overview.md) позволяет шифровать диски виртуальных машин IaaS под управлением Windows и Linux. Для шифрования дисков Azure используются стандартные для отрасли функции ([BitLocker](https://technet.microsoft.com/library/cc732774.aspx) в Windows и [DM-Crypt](https://en.wikipedia.org/wiki/Dm-crypt) в Linux), которые обеспечивают шифрование томов дисков ОС и дисков данных. Решение интегрировано с [Azure Key Vault](https://azure.microsoft.com/documentation/services/key-vault/) , помогающее управлять ключами и секретами шифрования дисков в подписке хранилища ключей. Данное решение обеспечивает шифрование всех неактивных данных на дисках виртуальной машины в службе хранилища Azure.

Ниже приведены рекомендации по использованию службы шифрования дисков Azure.

**Рекомендация**: включайте шифрование на виртуальных машинах.   
**Сведения**: служба шифрования дисков Azure создает и записывает ключи шифрования в хранилище ключей. Для управления ключами шифрования в хранилище ключей требуется аутентификация Azure AD. Для этой цели создайте приложение Azure AD. Можно использовать аутентификацию на основе секрета клиента или [на основе сертификата клиента в Azure AD](../../active-directory/authentication/active-directory-certificate-based-authentication-get-started.md).

**Рекомендация**: используйте ключ шифрования ключей (KEK), чтобы добавить уровень безопасности для ключей шифрования. Поместите KEK в хранилище ключей.   
**Сведения**: используйте командлет [Add-азкэйваулткэй](/powershell/module/az.keyvault/add-azkeyvaultkey) , чтобы создать ключ шифрования ключа в хранилище ключей. Кроме того, KEK можно импортировать из своего локального аппаратного модуля безопасности (HSM) для управления ключами. Дополнительные сведения см. в документации по [Key Vault](../../key-vault/keys/hsm-protected-keys.md). Когда ключ шифрования ключей указан, шифрование дисков Azure использует его для упаковки секретов шифрования перед записью в Key Vault. Наличие депонированной копии этого ключа в локальном модуле HSM управления ключами обеспечивает дополнительную защиту от случайного удаления ключей.

**Рекомендация**: создайте [моментальный снимок](../../virtual-machines/windows/snapshot-copy-managed-disk.md) и (или) резервную копию перед шифрованием дисков. Резервные копии позволяют выполнять восстановление, если во время шифрования происходит непредвиденная ошибка.   
**Сведения**: для виртуальных машин с управляемыми дисками необходимо создать резервную копию до начала шифрования. После создания резервной копии можно использовать командлет **Set-азвмдискенкриптионекстенсион** для шифрования управляемых дисков, указав параметр *-skipVmBackup* . Дополнительные сведения см. в статье [Резервное копирование и восстановление зашифрованных виртуальных машин с помощью службы Azure Backup](../../backup/backup-azure-vms-encryption.md).

**Рекомендация**: чтобы убедиться, что секреты шифрования не пересекают региональные границы, шифрованию дисков Azure требуется, чтобы хранилище ключей и виртуальные машины были расположены в одном регионе.   
**Сведения**: создайте и используйте хранилище ключей, которое находится в том же регионе, что и виртуальная машина, которую нужно зашифровать.

Применяя решение по шифрованию дисков Azure, можно удовлетворять приведенные ниже потребности организации.

- При хранении виртуальные машины IaaS защищаются с помощью стандартных отраслевых технологий шифрования, которые отвечают корпоративным требованиям безопасности и соответствия.
- Запуск виртуальных машин IaaS выполняется в соответствии с ключами и политиками, которыми управляет клиент. Можно также проводить аудит их использования в хранилище ключей.

## <a name="restrict-direct-internet-connectivity"></a>Ограничение прямого подключения к Интернету
Мониторинг и ограничение прямого подключения к Интернету для виртуальной машины. Злоумышленники постоянно проверяют диапазоны IP-адресов общедоступного облака для открытых портов управления и пытаются выполнить простые атаки, такие как общие пароли и известные неисправленные уязвимости. В следующей таблице приведены рекомендации, помогающие защититься от таких атак.

**Рекомендации**. предотвращение непреднамеренной раскрытия сетевой маршрутизации и безопасности.   
**Сведения**: используйте RBAC, чтобы убедиться, что только Центральная сеть имеет разрешение на доступ к сетевым ресурсам.

**Рекомендации**. выявление и исправление общедоступных виртуальных машин, которые разрешают доступ с "любого" исходного IP-адреса.   
**Сведения**: используйте центр безопасности Azure. Центр безопасности порекомендует ограничить доступ через конечные точки с выходом в Интернет, если в любой из групп безопасности сети есть одно или несколько правил для входящих подключений, которые разрешают доступ с любого исходного IP-адреса. Центр безопасности порекомендует изменить эти правила для входящих подключений, чтобы [ограничить доступ](../../security-center/security-center-network-recommendations.md) к исходным IP-адресам, которые действительно необходимы для доступа.

**Рекомендации**: ограничение портов управления (RDP, SSH).   
**Сведения**: JIT [-доступ к виртуальной машине](../../security-center/security-center-just-in-time.md) можно использовать для блокировки входящего трафика к виртуальным машинам Azure, что снижает вероятность атак и обеспечивает простой доступ к виртуальным машинам при необходимости. Если JIT включен, центр безопасности блокирует входящий трафик на виртуальные машины Azure, создавая правило группы безопасности сети. Вы выбираете порты на виртуальной машине, для которых будет заблокирован входящий трафик. Эти порты управляются JIT-решением.

## <a name="next-steps"></a>Дальнейшие действия
Дополнительные рекомендации по обеспечению безопасности, используемые при разработке, развертывании облачных решений и управлении ими с помощью Azure, см. в статье [Рекомендации и шаблоны для обеспечения безопасности в Azure](best-practices-and-patterns.md).

Ниже приведены ресурсы, с помощью которых можно получить общие сведения о службах безопасности Azure и связанных службах Майкрософт.
* [Блог команды безопасности Azure](https://blogs.msdn.microsoft.com/azuresecurity/). Благодаря этому блогу вы будете в курсе последних новостей о безопасности Azure.
* [Центр Microsoft Security Response Center](https://technet.microsoft.com/library/dn440717.aspx) , где уязвимости системы безопасности Майкрософт, включая проблемы с Azure, можно сообщить или отправить по электронной почте secure@microsoft.com
