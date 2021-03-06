---
title: Защита инфраструктуры удостоверений Azure AD
titleSuffix: Azure Active Directory
description: В этом документе приводится список важных действий, которые должны выполнить администраторы, чтобы обеспечить безопасность организации с помощью возможностей Azure AD.
author: martincoetzer
manager: manmeetb
tags: azuread
ms.service: security
ms.subservice: security-fundamentals
ms.topic: conceptual
ms.workload: identity
ms.date: 01/29/2020
ms.author: martinco
ms.openlocfilehash: a0a11cf3bfac7d1e8fd2d117e13532e2ce49caa0
ms.sourcegitcommit: ae6e7057a00d95ed7b828fc8846e3a6281859d40
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/16/2020
ms.locfileid: "92107816"
---
# <a name="five-steps-to-securing-your-identity-infrastructure"></a>Пять шагов по защите инфраструктуры удостоверений

Если вы читаете этот документ, значит вы понимаете значимость безопасности. Скорее всего, вы уже несете ответственность за обеспечение безопасности вашей организации. Если вам нужно убедить других пользователей в важности защиты, посоветуйте им прочитать последний [отчет Microsoft Security Intelligence](https://www.microsoft.com/security/business/security-intelligence-report).

Этот документ поможет вам достичь более высокого уровня безопасности с помощью возможностей Azure Active Directory и контрольного списка из пяти шагов, предназначенных для защиты организации от кибератак.

Этот контрольный список поможет вам быстро выполнить важные рекомендуемые действия для защиты вашей организации, так как в нем объясняется, как реализовать следующие задачи.

* Укрепление учетных данных
* Уменьшение контактной зоны
* Автоматизация реагирования на угрозы
* Использование Cloud Intelligence
* Включение возможности самообслуживания для конечных пользователей

При чтении этого контрольного списка необходимо отслеживать уже выполненные функции и шаги.

> [!NOTE]
> Многие рекомендации, приведенные в этом документе, применяются только к приложениям, которые настроены для использования Azure Active Directory в качестве поставщика удостоверений. Настроив приложения для единого входа, вы получите преимущества политик учетных данных, обнаружения угроз, аудита, ведения журналов, а также другие возможности, добавляемые в приложения. [Управление приложениями Azure AD](../../active-directory/manage-apps/what-is-application-management.md) является основой, на которой основаны все эти рекомендации.

Рекомендации в этом документе сопоставляются с [оценкой безопасности удостоверений](../../active-directory/fundamentals/identity-secure-score.md), автоматизированной оценкой конфигурации безопасности удостоверений клиента Azure AD. Организации могут использовать страницу "Оценка безопасности удостоверений" на портале Azure AD, чтобы найти пробелы в текущей конфигурации безопасности и обеспечить соблюдение текущих [рекомендаций](identity-management-best-practices.md) корпорации Майкрософт по безопасности. Реализация каждой рекомендации на странице оценки безопасности увеличит ваш рейтинг и позволит вам отслеживать свой прогресс, а также поможет вам сравнить свою реализацию с реализациями других организаций аналогичного размера или отрасли.

![Оценка безопасности удостоверений](./media/steps-secure-identity/azure-ad-sec-steps0.png)

> [!NOTE]
> Для использования многих приведенных здесь функций требуется подписка Azure AD Premium. Некоторые функции предоставляются бесплатно. Дополнительные сведения см. на странице [Цены на Azure Active Directory](https://azure.microsoft.com/pricing/details/active-directory/) и в [контрольном списке для развертывания Azure AD](https://docs.microsoft.com/azure/active-directory/fundamentals/active-directory-deployment-checklist-p2).

## <a name="before-you-begin-protect-privileged-accounts-with-mfa"></a>Перед началом работы Защита привилегированных учетных записей с помощью Многофакторной идентификации

Перед выполнением задач в этом контрольном списке убедитесь, что во время чтения этого списка ваша безопасность не пострадает. Сначала необходимо защитить привилегированные учетные записи.

Злоумышленники, получившие доступ к привилегированным учетным записям, могут причинить значительный ущерб, поэтому очень важно в первую очередь защитить именно эти учетные записи. Включите и требуйте [многофакторную идентификацию Azure](../../active-directory/authentication/multi-factor-authentication.md) (MFA) для всех администраторов в организации, использующих [параметры безопасности Azure AD по умолчанию](../../active-directory/fundamentals/concept-fundamentals-security-defaults.md) или [условный доступ](../../active-directory/conditional-access/plan-conditional-access.md). Если вы еще не реализовали многофакторную идентификацию, сделайте это сейчас! Это очень важно.

Все сделано? Начнем работу по контрольному списку.

## <a name="step-1---strengthen-your-credentials"></a>Шаг 1. Укрепление учетных данных

Причиной большинства нарушений системы безопасности организации является несанкционированный доступ к учетным записям с помощью одного из множества методов, таких как распыление паролей, воспроизведение угрозы или фишинг. Дополнительные сведения об этих атаках см. в этом видео (45 мин.):
> [!VIDEO https://www.youtube.com/embed/uy0j1_t5Hd4]

### <a name="make-sure-your-organization-uses-strong-authentication"></a>В организации должна действовать строгая проверка подлинности.

Принимая во внимание частоту подбора, фишинга, кражи паролей вредоносными программами или повторное использование паролей, крайне важно защитить пароли с помощью надежных учетных данных определенного вида — [узнайте больше о многофакторной идентификации Azure](../../active-directory/authentication/multi-factor-authentication.md).

Быстро и легко включить базовый уровень безопасности удостоверений можно с помощью [параметров безопасности по умолчанию](../../active-directory/fundamentals/concept-fundamentals-security-defaults.md). Параметры безопасности по умолчанию применяют Azure MFA для всех пользователей в клиенте и блокируют вход, выполняемый по устаревшим протоколам, в масштабе клиента.

### <a name="start-banning-commonly-attacked-passwords-and-turn-off-traditional-complexity-and-expiration-rules"></a>Начните с применения запрета к часто атакуемым паролям, откажитесь от традиционных требований к сложности, отключите правила истечения срока действия.

Многие организации применяют традиционные требования к сложности (специальные символы, цифры, прописные и строчные буквы) и правила истечения срока действия пароля. [Исследование корпорации Майкрософт](https://aka.ms/passwordguidance) показало, что эти политики вынуждают пользователей выбирать легко подбираемые пароли.

Функция [динамического запрещенного пароля](../../active-directory/authentication/concept-password-ban-bad.md) в Azure Active Directory использует текущее поведение злоумышленника, чтобы запретить пользователям задавать легко угадываемые пароли. Эта возможность всегда включена при создании пользователей в облаке, но теперь также доступна для гибридных организаций при развертывании [защиты паролем Azure AD для Windows Server Active Directory](../../active-directory/authentication/concept-password-ban-bad-on-premises.md). Защита паролем Azure AD блокирует выбор этих распространенных простых паролей, и ее можно расширить до блокировки паролей, содержащих пользовательские ключевые слова, указанные вами. Например, вы можете запретить пользователям выбирать пароли, содержащие названия продуктов вашей компании или местной спортивной команды.

Корпорация Майкрософт рекомендует принять следующую современную политику паролей, основанную на [руководстве NIST](https://pages.nist.gov/800-63-3/sp800-63b.html):

1. Длина паролей должна быть не менее 8 символов. Больше не значит лучше, так как в противном случае пользователи будут выбирать прогнозируемые пароли, сохранять пароли в файлах или записывать их.
2. Следует отключить правила истечения срока действия, на основе которых пользователи могут создавать легко угадываемые пароли, такие как **Spring2019!** .
3. Следует отключить требования к составным символам и запретить пользователям выбирать часто атакуемые пароли, так как возникает вероятность прогнозируемых замен символов в паролях.

При создании удостоверений непосредственно в Azure AD воспользуйтесь [PowerShell для предотвращения истечения срока действия паролей](../../active-directory/authentication/concept-sspr-policy.md). Гибридные организации должны реализовывать эти политики с помощью [параметров групповой политики домена](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/hh994572(v%3dws.10)) или [Windows PowerShell](https://docs.microsoft.com/powershell/module/addsadministration/set-addefaultdomainpasswordpolicy).

### <a name="protect-against-leaked-credentials-and-add-resilience-against-outages"></a>Защита от утечки учетных данных и добавление устойчивости к сбоям

Если организация использует гибридное решение со сквозной проверкой подлинности или федерацией для удостоверений, необходимо включить синхронизацию хэшей паролей по следующим двум причинам.

* Отчет [Пользователи с украденными учетными данными](../../active-directory/reports-monitoring/concept-risk-events.md) в службе управления Azure AD предупреждает о парах "имя пользователя-пароль", которые были раскрыты в так называемом "темном Интернете". Невероятное количество паролей пропадает в результате фишинга, применения вредоносного ПО и повторного использования паролей на сайтах сторонних производителей, которые через какое-то время подвергаются угрозам. Корпорация Майкрософт находит многие из украденных учетных данных и сообщает о них в этом отчете, если обнаруженные сведения совпадают с учетными данными в вашей организации, но только если вы [включите синхронизацию хэшей паролей](../../active-directory/hybrid/how-to-connect-password-hash-synchronization.md)!
* В случае сбоя в локальной среде (например, при атаке от атаки через атаку) вы можете переключиться на использование [облачной аутентификации с помощью синхронизации хэшей паролей](choose-ad-authn.md). Этот метод проверки подлинности позволяет продолжить доступ к приложениям, настроенным для проверки подлинности, с Azure Active Directory, включая Microsoft 365. В этом случае ИТ-персоналу не нужно будет прибегать к личным учетным записям электронной почты для обмена данными до тех пор, пока не будет устранен локальный сбой.

Узнайте больше о том, [как работает синхронизация хэшей паролей](../../active-directory/hybrid/how-to-connect-password-hash-synchronization.md).

> [!NOTE]
> Если включить синхронизацию хэша паролей при использовании Доменных служб Azure AD, хэши Kerberos (AES 256) и при необходимости хэши NTLM (RC4, без добавления случайных данных) также зашифровываются и синхронизируются с Azure AD.

### <a name="implement-ad-fs-extranet-smart-lockout"></a>Реализация интеллектуальной блокировки экстрасети служб федерации Active Directory

Организации, настраивающие приложения для проверки подлинности непосредственно в Azure AD, пользуются [интеллектуальной блокировкой Azure AD](../../active-directory/authentication/concept-sspr-howitworks.md). Если вы используете AD FS в Windows Server 2012 R2, реализуйте [защиту блокировки экстрасетей](https://docs.microsoft.com/windows-server/identity/ad-fs/operations/configure-ad-fs-extranet-soft-lockout-protection) в AD FS. Если же AD FS используется в Windows Server 2016, реализуйте [смарт-блокировку экстрасетей](https://support.microsoft.com/help/4096478/extranet-smart-lockout-feature-in-windows-server-2016). Смарт-блокировка экстрасетей для AD FS обеспечивает защиту от атак методом подбора, направленных на AD FS, и в то же время не позволяет блокировать пользователей в Active Directory.

### <a name="take-advantage-of-intrinsically-secure-easier-to-use-credentials"></a>Использование преимуществ безопасных по сути и простых в применении учетных данных

С помощью [Windows Hello](https://docs.microsoft.com/windows/security/identity-protection/hello-for-business/hello-identity-verification) можно заменить пароли на строгую двухфакторную проверку подлинности на мобильных устройствах и компьютерах. Эта проверка состоит из нового типа учетных данных пользователя, которые надежно привязываются к устройству, и использует биометрические данные или ПИН-код.

## <a name="step-2---reduce-your-attack-surface"></a>Шаг 2. Уменьшение контактной зоны

Учитывая широкое распространение компрометации паролей, решающее значение имеет минимизация контактной зоны для атаки. Это можно сделать путем отказа от использования устаревших и менее безопасных протоколов, ограничения доступа к точкам входа и более серьезного управления административным доступом к ресурсам.

### <a name="block-legacy-authentication"></a>Блокировка устаревших методов проверки подлинности

Еще одним фактором риска для организаций являются приложения, использующие собственные устаревшие методы для проверки подлинности в Azure AD и доступа к корпоративным данным. В их число входят клиенты SMTP, POP3 и IMAP4. Устаревшие приложения проверки подлинности выполняют проверку от имени пользователя и блокируют Azure AD от проведения дополнительной оценки безопасности. Альтернативные современные варианты позволяют снизить риск угрозы безопасности, так как они поддерживают многофакторную идентификацию и условный доступ. Мы рекомендуем следующие три действия:

1. Блокировка [устаревшей проверки подлинности (если используются службы AD FS)](https://docs.microsoft.com/windows-server/identity/ad-fs/operations/access-control-policies-w2k12).
2. Настройка [SharePoint Online и Exchange Online для использования современных способов проверки подлинности](../../active-directory/conditional-access/conditional-access-for-exo-and-spo.md).
3. Если у вас есть Azure AD Premium, используйте политики условного доступа, чтобы [блокировать устаревшие методы проверки подлинности](../../active-directory/conditional-access/howto-conditional-access-policy-block-legacy.md). В противном случае используйте [параметры безопасности Azure AD по умолчанию](../../active-directory/fundamentals/concept-fundamentals-security-defaults.md).

### <a name="block-invalid-authentication-entry-points"></a>Блокировка недопустимых точек входа для проверки подлинности

Используя концепцию предполагаемого нарушения, вы должны уменьшить влияние скомпрометированных учетных данных, когда таковые появятся. Для каждого приложения в вашей среде рассмотрите применимые варианты использования: какие группы, сети, устройства и другие элементы следует авторизовать, а затем заблокируйте остальные. С помощью [условного доступа Azure AD](../../active-directory/conditional-access/overview.md) вы можете контролировать доступ авторизованных пользователей к приложениям и ресурсам на основе конкретных определенных вами условий.

### <a name="restrict-user-consent-operations"></a>Ограничение операций предоставления согласия пользователем

Важно понимать действие различных [процедур предоставления согласия для приложений в Azure AD](../../active-directory/develop/application-consent-experience.md), [типы разрешений и согласия](../../active-directory/develop/v2-permissions-and-consent.md), а также их влияние на уровень безопасности вашей организации. По умолчанию все пользователи в Azure AD могут предоставлять приложениям, которые используют платформу удостоверений Майкрософт, доступ к данным организации. Хотя разрешение предоставлять согласие пользователям позволяет им легко приобретать полезные приложения, которые интегрируются с Microsoft 365, Azure и другими службами, без должного мониторинга и использования может возникнуть риск.

Корпорация Майкрософт рекомендует ограничить согласие пользователя, чтобы помочь сократить контактную зону и снизить риск. Вы также можете использовать [политики согласия приложений (Предварительная версия)](../../active-directory/manage-apps/configure-user-consent.md) , чтобы ограничить согласие конечного пользователя только проверенными издателями и только для выбранных вами разрешений. Если согласие конечных пользователей ограничено, предыдущие разрешения на согласие по-прежнему будут учитываться, но все последующие операции предоставления согласия должны выполняться администратором. В случае ограниченных случаев согласие администратора может запрашиваться пользователями через встроенный [Рабочий процесс запроса согласия администратора](../../active-directory/manage-apps/configure-admin-consent-workflow.md) или с помощью собственных процессов поддержки. Прежде чем ограничивать согласие конечного пользователя, используйте наши [рекомендации](../../active-directory/manage-apps/manage-consent-requests.md) для планирования изменений в Организации. Для приложений, к которым вы хотите разрешить доступ всем пользователям, рассмотрите возможность [предоставления согласия от имени всех пользователей](../../active-directory/develop/v2-admin-consent.md), чтобы доступ к приложению получили также те пользователи, которые еще не дали индивидуального согласия. Если предоставлять доступ к этим приложениям всем пользователям во всех сценариях не требуется, используйте [назначения приложений](../../active-directory/manage-apps/assign-user-or-group-access-portal.md) и условный доступ, чтобы ограничить доступ пользователей к [определенным приложениям](../../active-directory/conditional-access/concept-conditional-access-cloud-apps.md).

Убедитесь, что пользователи могут запрашивать одобрение администратора для новых приложений, чтобы упростить процедуру входа, минимизировать число обращений в службу поддержки и запретить пользователям регистрироваться с использованием учетных данных, отличных от Azure AD. После того как вы отрегулируете и стабилизируете операции на предоставление согласия, администраторы должны регулярно выполнять аудит приложений и выданных разрешений.


### <a name="implement-azure-ad-privileged-identity-management"></a>Реализация Azure AD Privileged Identity Management

Другое влияние "предполагаемого нарушения" заключается в сведении к минимуму вероятности того, что скомпрометированная учетная запись может работать с привилегированной ролью.

[Azure AD Privileged Identity Management (PIM)](../../active-directory/privileged-identity-management/pim-configure.md) помогает минимизировать привилегии учетной записи за счет следующих действий:

* идентификация пользователей с назначенными административными ролями и управление ими;
* определение ролей с неиспользуемыми или избыточными привилегиями, которые следует удалить;
* установка правил для защиты привилегированных ролей с помощью многофакторной идентификации;
* установка правил, предусматривающих действие привилегированных ролей в течение периода выполнения привилегированной задачи.

Включите Azure AD PIM, просмотрите пользователей, которым назначены административные роли, и удалите ненужные учетные записи в этих ролях. Остальных привилегированных пользователей переместите из категории имеющих постоянные привилегии в категорию с возможностью получения привилегий. Наконец, установите соответствующие политики, согласно которым при возникновении необходимости доступ к привилегированным ролям осуществляется в безопасном режиме с необходимым управлением изменениями.

В ходе развертывания привилегированной учетной записи следуйте [рекомендации по созданию как минимум двух экстренных учетных записей](../../active-directory/users-groups-roles/directory-admin-roles-secure.md), чтобы сохранить права доступа к Azure AD в случае самоблокировки.

## <a name="step-3---automate-threat-response"></a>Шаг 3. Автоматизация реагирования на угрозы

В состав Azure Active Directory входит целый ряд возможностей, которые автоматически перехватывают атаки, чтобы исключить задержку между обнаружением и реагированием. Сократив время внедрения злоумышленников в среду, можно уменьшить расходы и риски. Ниже приведены рекомендуемые к выполнению конкретные действия.

### <a name="implement-user-risk-security-policy-using-azure-ad-identity-protection"></a>Реализация политики безопасности для риска пользователя с помощью защиты идентификации Azure AD

Риск пользователя означает вероятность компрометации удостоверения пользователя и вычисляется на основе [обнаружений риска пользователя](../../active-directory/identity-protection/overview.md), которые связаны с удостоверением пользователя. Политика риска пользователя — это политика условного доступа, которая оценивает уровень риска для конкретного пользователя или группы. На основании низкого, среднего и высокого уровней риска можно настроить политику для блокировки доступа или требования изменить надежный пароль с помощью многофакторной идентификации. Корпорация Майкрософт рекомендует требовать изменения надежного пароля для пользователей с высоким уровнем риска.

![Пользователи, помеченные для события риска](./media/steps-secure-identity/azure-ad-sec-steps1.png)

### <a name="implement-sign-in-risk-policy-using-azure-ad-identity-protection"></a>Реализация политики риска при входе с помощью защиты идентификации Azure AD

Риск при входе представляет собой вероятность попытки входа с использованием удостоверения, предпринимаемой кем-то другим, но не владельцем учетной записи. [Политика риска при входе в систему](../../active-directory/identity-protection/overview.md) — это политика условного доступа, которая оценивает уровень риска для конкретного пользователя или группы. На основе уровня риска (высокий, средний, низкий) можно настроить политику для блокировки доступа или принудительного выполнения многофакторной идентификации. Многофакторную идентификацию необходимо применять при входах с уровнем риска "Средний" или выше.

![Вход с анонимных IP-адресов](./media/steps-secure-identity/azure-ad-sec-steps2.png)

## <a name="step-4---utilize-cloud-intelligence"></a>Шаг 4. Использование Cloud Intelligence

Аудит и ведение журнала событий безопасности и связанные с ними оповещения являются важными составляющими эффективной стратегии защиты. Отчеты и журналы безопасности обеспечивают электронную запись подозрительных действий и помогают выявлять закономерности, которые могут свидетельствовать об успешной или неудачной попытке проникнуть в сеть извне, а также о внутренних атаках. Аудит можно использовать для отслеживания действий пользователей, соблюдения требований нормативных документов, выполнения экспертизы и многого другого. Оповещения содержат уведомления о событиях безопасности.

### <a name="monitor-azure-ad"></a>Мониторинг Azure AD

Службы и компоненты Microsoft Azure дают возможность настраивать параметры аудита и ведения журнала безопасности. Это поможет выявить недочеты в политиках и механизмах безопасности и устранить эти недочеты для предотвращения нарушений. Можно использовать функции [аудита и ведения журналов Azure](log-audit.md) и [отчеты о действиях аудита на портале Azure Active Directory](../../active-directory/reports-monitoring/concept-audit-logs.md).

### <a name="monitor-azure-ad-connect-health-in-hybrid-environments"></a>Мониторинг Azure AD Connect Health в гибридных средах

[Мониторинг ADFS с помощью Azure AD Connect Health](../../active-directory/hybrid/how-to-connect-health-adfs.md) дает более полное представление о потенциальных проблемах и обнаружении атак в имеющейся инфраструктуре ADFS. Служба Azure AD Connect Health выводит оповещения с подробными сведениями, шагами по устранению проблем и ссылками на соответствующую документацию, предоставляет аналитику использования для ряда показателей, связанных с трафиком проверки подлинности аутентификации, обеспечивает мониторинг производительности и формирование отчетов.

![Azure AD Connect Health](./media/steps-secure-identity/azure-ad-sec-steps4.png)

### <a name="monitor-azure-ad-identity-protection-events"></a>Мониторинг событий защиты идентификации Azure AD

[Защита идентификации Azure Active Directory](../../active-directory/identity-protection/overview.md) — это средство уведомления, мониторинга и отчетности, которое можно использовать для обнаружения потенциальных уязвимостей, влияющих на удостоверения вашей организации. Оно определяет обнаружения риска, такие как потерянные учетные данные, невозможное перемещение, попытки входа с зараженных устройств, анонимных IP-адресов, IP-адресов, связанных с подозрительной активностью, и из неизвестных расположений. Включите оповещения с уведомлениями для получения сообщений от пользователей, подверженных риску, и (или) сообщение электронной почты еженедельного дайджеста.

Защита идентификации Azure AD предоставляет два важных отчета, которые вы должны отслеживать ежедневно:
1. Отчеты о рискованных входах будут отображаться в действиях пользователя. Вы должны узнать, действительно ли законный владелец выполнил вход.
2. Отчеты о рискованных пользователях будут отображаться с учетными записями пользователей, которые могут быть скомпрометированы, например обнаруженные утечки учетных данных или пользователь, выполнявший вход в разных местах, что приводит к событию невозможности перехода.

![Пользователи, помеченные для события риска](./media/steps-secure-identity/azure-ad-sec-steps3.png)

### <a name="audit-apps-and-consented-permissions"></a>Аудит приложений и разрешения с предоставлением согласия

Пользователи могут быть обмануты при переходе на скомпрометированный веб-сайт или в приложения, которые получат доступ к их информации о профиле и пользовательским данным, таким как их электронная почта. Злоумышленник может использовать полученные разрешения для шифрования содержимого почтового ящика и потребовать выкуп за восстановление ваших данных почтового ящика. [Администраторы должны просматривать и проверять](https://docs.microsoft.com/office365/securitycompliance/detect-and-remediate-illicit-consent-grants) разрешения, предоставляемые пользователями, или отключать для пользователей возможность предоставлять согласие по умолчанию.

Помимо аудита разрешений, предоставленных пользователями, можно [обнаруживать рискованные или нежелательные приложения OAuth](https://docs.microsoft.com/cloud-app-security/investigate-risky-oauth) в средах премиум-класса.

## <a name="step-5---enable-end-user-self-service"></a>Шаг 5. Включение возможности самообслуживания для конечных пользователей

Вы захотите максимально сбалансировать безопасность с производительностью. Придерживаясь мысли о формировании основы для безопасности в долгосрочной перспективе, вы можете устранить разногласия в своей организации, предоставив пользователям широкие возможности работы с одновременным сохранением бдительности.

### <a name="implement-self-service-password-reset"></a>Реализация самостоятельного сброса пароля

С помощью системы [самостоятельного сброса пароля (SSPR)](../../active-directory/authentication/quickstart-sspr.md) Azure AD ИТ-администраторы могут разрешить пользователям самостоятельно сбрасывать или разблокировывать пароли и учетные записи без обращения в службу поддержки. Эта система предоставляет подробные отчеты, которые позволяют отслеживать сброс пароля пользователями, а также создает уведомления в случае несанкционированного использования или применения не по назначению.

### <a name="implement-self-service-group-and-application-access"></a>Реализация самостоятельного доступа к группам и приложениям

Azure AD предоставляет не администраторам возможность управлять доступом к ресурсам с помощью групп безопасности, групп Microsoft 365, ролей приложений и доступа к каталогам пакетов.  [Самостоятельное управление группами](../../active-directory/users-groups-roles/groups-self-service-management.md) позволяет владельцам групп управлять собственными группами без назначения административной роли. Пользователи также могут создавать группы Microsoft 365 и управлять ими, не полагаясь на администраторов для обработки своих запросов, и срок действия неиспользуемых групп истекает автоматически.  Функция [управления правами Azure AD](../../active-directory/governance/entitlement-management-overview.md) обеспечивает делегирование и видимость, автоматизируя рабочие процессы запросов доступа и истечения срока действия.  Вы можете делегировать пользователям, не являющимся администраторами, возможность настраивать собственные пакеты доступа для Teams, групп, приложений и сайтов SharePoint Online, которыми они владеют, применять настраиваемые политики для тех, кто должен утверждать доступ, включая настройку менеджеров сотрудников и спонсоров бизнес-партнеров в качестве утверждающих лиц.

### <a name="implement-azure-ad-access-reviews"></a>Реализация проверок доступа Azure AD

С помощью [проверок доступа Azure AD](../../active-directory/governance/access-reviews-overview.md) можно управлять пакетами доступа и членством в группах, доступом к корпоративным приложениям и назначениями привилегированных ролей, чтобы поддерживать стандарт безопасности.  Согласно регулярному контролю со стороны самих пользователей, владельцев ресурсов и других проверяющих пользователи не получают прав доступа в течение длительных периодов, когда доступ им не нужен.

## <a name="summary"></a>Сводка

Существует множество аспектов, касающихся обеспечения безопасности инфраструктуры удостоверений, но приведенные в этом контрольном списке пять шагов помогут вам быстро сформировать более защищенную и надежную инфраструктуру.

* Укрепление учетных данных
* Уменьшение контактной зоны
* Автоматизация реагирования на угрозы
* Использование Cloud Intelligence
* Обеспечение более предсказуемой и полной безопасности конечных пользователей с возможностью самостоятельного решения вопросов

Мы ценим то, насколько серьезно вы относитесь к вопросам безопасности удостоверений, и надеемся, что этот документ будет полезен при разработке стратегии для защиты вашей организации.

## <a name="next-steps"></a>Дальнейшие действия

Если вам нужна помощь по планированию и развертыванию рекомендаций, см. [планы развертывания проектов Azure AD](https://aka.ms/deploymentplans).

Если вы уверены, что все эти действия выполнены, воспользуйтесь [оценкой безопасности удостоверений Майкрософт](../../active-directory/fundamentals/identity-secure-score.md), которая позволит вам быть в курсе [последних рекомендаций](identity-management-best-practices.md) и угроз безопасности.
