---
author: DCtheGeek
ms.service: azure-policy
ms.topic: include
ms.date: 10/07/2020
ms.author: dacoulte
ms.custom: generated
ms.openlocfilehash: 2848e009edff87f387a7871d1a1d8884e3371b98
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/09/2020
ms.locfileid: "91822691"
---
## <a name="azure-security-benchmark"></a>Azure Security Benchmark

[Azure Security Benchmark](../../../../articles/security/benchmarks/overview.md) содержит рекомендации по обеспечению безопасности облачных решений в Azure. Сведения о том, как эта служба полностью сопоставляется с Azure Security Benchmark, см. [на этой странице](https://github.com/MicrosoftDocs/SecurityBenchmarks/tree/master/Azure%20Offer%20Security%20Baselines).

Дополнительную информацию о том, как доступные встроенные компоненты Политики Azure для всех служб Azure сопоставляются с этим стандартом соответствия, см. в статье [Подробные сведения о встроенной инициативе о соответствии нормативным требованиям теста производительности системы безопасности Azure](../../../../articles/governance/policy/samples/azure-security-benchmark.md).

|Домен |Идентификатор элемента управления |Заголовок элемента управления |Политика<br /><sub>(портал Azure)</sub> |Версия политики<br /><sub>(GitHub)</sub>  |
|---|---|---|---|---|
|Защита данных |4.4. |Шифрование любой конфиденциальной информации при передаче |[Использование только безопасных подключений к Кэшу Azure для Redis](https://portal.azure.com/#blade/Microsoft_Azure_Policy/PolicyDetailBlade/definitionId/%2Fproviders%2FMicrosoft.Authorization%2FpolicyDefinitions%2F22bee202-a82f-4305-9a2a-6d7f44d4dedb) |[1.0.0](https://github.com/Azure/azure-policy/blob/master/built-in-policies/policyDefinitions/Cache/RedisCache_AuditSSLPort_Audit.json) |

## <a name="hipaa-hitrust-92"></a>HIPAA HITRUST 9.2

Дополнительную информацию о том, как доступные встроенные компоненты Политики Azure для всех служб Azure сопоставляются с этим стандартом соответствия, см. в статье со [сведениями о встроенной инициативе по обеспечению соответствия стандарту HIPAA HITRUST 9.2](../../../../articles/governance/policy/samples/hipaa-hitrust-9-2.md).
Дополнительные сведения об этом стандарте соответствия см. на странице описания [HIPAA HITRUST 9.2](https://www.hhs.gov/hipaa/for-professionals/security/laws-regulations/index.html).

|Домен |Идентификатор элемента управления |Заголовок элемента управления |Политика<br /><sub>(портал Azure)</sub> |Версия политики<br /><sub>(GitHub)</sub>  |
|---|---|---|---|---|
|Управление сетевым подключением |0809.01n2Organizational.1234 — 01.n |Сетевой трафик контролируется в соответствии с корпоративной политикой управления доступом с применением брандмауэра или других сетевых средств ограничения для каждой точки доступа к сети или управляемого интерфейса внешней службы телекоммуникации. |[Использование только безопасных подключений к Кэшу Azure для Redis](https://portal.azure.com/#blade/Microsoft_Azure_Policy/PolicyDetailBlade/definitionId/%2Fproviders%2FMicrosoft.Authorization%2FpolicyDefinitions%2F22bee202-a82f-4305-9a2a-6d7f44d4dedb) |[1.0.0](https://github.com/Azure/azure-policy/blob/master/built-in-policies/policyDefinitions/Cache/RedisCache_AuditSSLPort_Audit.json) |
|Управление сетевым подключением |0810.01n2Organizational.5 — 01.n |Передаваемые данные защищены и, как минимум, зашифрованы через открытые общедоступные сети. |[Использование только безопасных подключений к Кэшу Azure для Redis](https://portal.azure.com/#blade/Microsoft_Azure_Policy/PolicyDetailBlade/definitionId/%2Fproviders%2FMicrosoft.Authorization%2FpolicyDefinitions%2F22bee202-a82f-4305-9a2a-6d7f44d4dedb) |[1.0.0](https://github.com/Azure/azure-policy/blob/master/built-in-policies/policyDefinitions/Cache/RedisCache_AuditSSLPort_Audit.json) |
|Управление сетевым подключением |0811.01n2Organizational.6 — 01.n |Исключения из политики потока трафика задокументированы с указанием обусловившей его критической потребности или бизнес-потребности и длительности исключения. Они пересматриваются по меньшей мере ежегодно. Исключения политики потока трафика удаляются, если они больше не обусловлены явной критической потребностью или бизнес-потребностью. |[Использование только безопасных подключений к Кэшу Azure для Redis](https://portal.azure.com/#blade/Microsoft_Azure_Policy/PolicyDetailBlade/definitionId/%2Fproviders%2FMicrosoft.Authorization%2FpolicyDefinitions%2F22bee202-a82f-4305-9a2a-6d7f44d4dedb) |[1.0.0](https://github.com/Azure/azure-policy/blob/master/built-in-policies/policyDefinitions/Cache/RedisCache_AuditSSLPort_Audit.json) |
|Управление сетевым подключением |0812.01n2Organizational.8 — 01.n |Удаленным устройствам, устанавливающим неудаленное подключение, не разрешается обмениваться данными с внешними (удаленными) ресурсами. |[Использование только безопасных подключений к Кэшу Azure для Redis](https://portal.azure.com/#blade/Microsoft_Azure_Policy/PolicyDetailBlade/definitionId/%2Fproviders%2FMicrosoft.Authorization%2FpolicyDefinitions%2F22bee202-a82f-4305-9a2a-6d7f44d4dedb) |[1.0.0](https://github.com/Azure/azure-policy/blob/master/built-in-policies/policyDefinitions/Cache/RedisCache_AuditSSLPort_Audit.json) |
|Управление сетевым подключением |0814.01n1Organizational.12 — 01.n |Возможность пользователей подключаться к внутренней сети ограничена с помощью политики "По умолчанию запрещено все, кроме разрешенного исключениями" на управляемых интерфейсах в соответствии с политикой управления доступом и требованиями бизнес-приложений и приложений для медицинской отрасли. |[Использование только безопасных подключений к Кэшу Azure для Redis](https://portal.azure.com/#blade/Microsoft_Azure_Policy/PolicyDetailBlade/definitionId/%2Fproviders%2FMicrosoft.Authorization%2FpolicyDefinitions%2F22bee202-a82f-4305-9a2a-6d7f44d4dedb) |[1.0.0](https://github.com/Azure/azure-policy/blob/master/built-in-policies/policyDefinitions/Cache/RedisCache_AuditSSLPort_Audit.json) |
|Определение рисков, связанных с внешними сторонами |1451.05iCSPOrganizational.2 — 05.i |Поставщики облачных служб разрабатывают и реализуют элементы управления для устранения и подавления угроз безопасности данных благодаря правильному разделению обязанностей, доступу на основе ролей и доступу с минимальными привилегиями для всех сотрудников в цепочке поставок. |[Использование только безопасных подключений к Кэшу Azure для Redis](https://portal.azure.com/#blade/Microsoft_Azure_Policy/PolicyDetailBlade/definitionId/%2Fproviders%2FMicrosoft.Authorization%2FpolicyDefinitions%2F22bee202-a82f-4305-9a2a-6d7f44d4dedb) |[1.0.0](https://github.com/Azure/azure-policy/blob/master/built-in-policies/policyDefinitions/Cache/RedisCache_AuditSSLPort_Audit.json) |
|Транзакции в Интернете |0946.09y2Organizational.14 — 09.y |Организация требует использования шифрования при обмене данными между сторонами, вовлеченными в транзакцию, и использования электронных подписей каждой из них. |[Использование только безопасных подключений к Кэшу Azure для Redis](https://portal.azure.com/#blade/Microsoft_Azure_Policy/PolicyDetailBlade/definitionId/%2Fproviders%2FMicrosoft.Authorization%2FpolicyDefinitions%2F22bee202-a82f-4305-9a2a-6d7f44d4dedb) |[1.0.0](https://github.com/Azure/azure-policy/blob/master/built-in-policies/policyDefinitions/Cache/RedisCache_AuditSSLPort_Audit.json) |

## <a name="nist-sp-800-171-r2"></a>NIST SP 800-171 R2

Дополнительную информацию о том, как доступные встроенные компоненты Политики Azure для всех служб Azure сопоставляются с этим стандартом соответствия, см. в статье [Сведения о встроенной инициативе по соответствию требованиям стандарта NIST SP 800-171 R2](../../../../articles/governance/policy/samples/nist-sp-800-171-r2.md).
Дополнительные сведения об этом стандарте соответствия см. на странице описания [NIST SP 800-171 R2](https://csrc.nist.gov/publications/detail/sp/800-171/rev-2/final) (специальной публикации Национального института стандартов и технологий).

|Домен |Идентификатор элемента управления |Заголовок элемента управления |Политика<br /><sub>(портал Azure)</sub> |Версия политики<br /><sub>(GitHub)</sub>  |
|---|---|---|---|---|
|Защита системы и средств передачи данных |3.13.1 |Мониторинг, контроль и защита обмена данными (т. е. информации, передаваемой или полученной системами организации) на внешних и основных внутренних границах систем организации |[Использование только безопасных подключений к Кэшу Azure для Redis](https://portal.azure.com/#blade/Microsoft_Azure_Policy/PolicyDetailBlade/definitionId/%2Fproviders%2FMicrosoft.Authorization%2FpolicyDefinitions%2F22bee202-a82f-4305-9a2a-6d7f44d4dedb) |[1.0.0](https://github.com/Azure/azure-policy/blob/master/built-in-policies/policyDefinitions/Cache/RedisCache_AuditSSLPort_Audit.json) |
|Защита системы и средств передачи данных |3.13.8 |Реализация механизма шифрования для предотвращения несанкционированного раскрытия контролируемой несекретной информации в процессе передачи при отсутствии альтернативных физических средств безопасности. |[Использование только безопасных подключений к Кэшу Azure для Redis](https://portal.azure.com/#blade/Microsoft_Azure_Policy/PolicyDetailBlade/definitionId/%2Fproviders%2FMicrosoft.Authorization%2FpolicyDefinitions%2F22bee202-a82f-4305-9a2a-6d7f44d4dedb) |[1.0.0](https://github.com/Azure/azure-policy/blob/master/built-in-policies/policyDefinitions/Cache/RedisCache_AuditSSLPort_Audit.json) |

## <a name="nist-sp-800-53-r4"></a>NIST SP 800-53 R4

Дополнительную информацию о том, как доступные встроенные компоненты Политики Azure для всех служб Azure сопоставляются с этим стандартом соответствия, см. в статье [Сведения о встроенной инициативе по соответствию требованиям стандарта NIST SP 800-53 R4](../../../../articles/governance/policy/samples/nist-sp-800-53-r4.md).
Дополнительные сведения об этом стандарте соответствия см. в разделе о [NIST SP 800-53 R4 — специальной публикации 800-53 Национального института стандартов и технологий (версия 4)](https://nvd.nist.gov/800-53).

|Домен |Идентификатор элемента управления |Заголовок элемента управления |Политика<br /><sub>(портал Azure)</sub> |Версия политики<br /><sub>(GitHub)</sub>  |
|---|---|---|---|---|
|Защита системы и средств передачи данных |SC-8 (1) |Конфиденциальность и целостность передаваемых данных \| Шифрование или дополнительная физическая защита |[Использование только безопасных подключений к Кэшу Azure для Redis](https://portal.azure.com/#blade/Microsoft_Azure_Policy/PolicyDetailBlade/definitionId/%2Fproviders%2FMicrosoft.Authorization%2FpolicyDefinitions%2F22bee202-a82f-4305-9a2a-6d7f44d4dedb) |[1.0.0](https://github.com/Azure/azure-policy/blob/master/built-in-policies/policyDefinitions/Cache/RedisCache_AuditSSLPort_Audit.json) |

