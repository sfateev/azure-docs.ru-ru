---
title: Обозреватель службы хранилища Azure рекомендации по безопасности | Документация Майкрософт
description: Руководство по безопасности для Обозреватель службы хранилища Azure
services: storage
author: cralvord
ms.service: storage
ms.topic: best-practice
ms.date: 07/30/2020
ms.author: cralvord
ms.openlocfilehash: e3bbe39077cf6d7781f7e11fde044cf272aa83e8
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/09/2020
ms.locfileid: "91714382"
---
# <a name="azure-storage-explorer-security-guide"></a>Обозреватель службы хранилища Azure рекомендации по безопасности

Обозреватель службы хранилища Microsoft Azure позволяет легко и безопасно работать с данными службы хранилища Azure в Windows, macOS и Linux. Следуя этим рекомендациям, можно обеспечить защиту данных.

## <a name="general"></a>Общие сведения

- **Всегда используйте последнюю версию Обозреватель службы хранилища.** Обозреватель службы хранилища выпуски могут содержать обновления для системы безопасности. Актуальность помогает обеспечить общую безопасность.
- **Подключайтесь только к доверенным ресурсам.** Данные, загружаемые из ненадежных источников, могут быть вредоносными, а передача данных в ненадежный источник может привести к потере или краже данных.
- **По возможности используйте HTTPS.** По умолчанию Обозреватель службы хранилища использует протокол HTTPS. Некоторые сценарии позволяют использовать протокол HTTP, но протокол HTTP следует использовать только в качестве последнего средства.
- **Убедитесь, что пользователям, которым они необходимы, предоставляются только необходимые разрешения.** При предоставлении доступа к ресурсам не рекомендуется использовать более строгие разрешения.
- **Будьте внимательны при выполнении критических операций.** Некоторые операции, такие как удаление и перезапись, являются необратимыми и могут привести к потере данных. Прежде чем выполнять эти операции, убедитесь, что вы работаете с правильными ресурсами.

## <a name="choosing-the-right-authentication-method"></a>Выбор правильного метода аутентификации

Обозреватель службы хранилища предоставляет различные способы доступа к ресурсам службы хранилища Azure. Какой бы метод вы ни выбрали, вот наши рекомендации.

### <a name="azure-ad-authentication"></a>Аутентификация Azure AD

Самый простой и безопасный способ доступа к ресурсам службы хранилища Azure — это вход с помощью учетной записи Azure. Для входа используется аутентификация Azure AD, которая позволяет:

- Предоставление доступа отдельным пользователям и группам.
- Отменяйте доступ к конкретным пользователям и группам в любое время.
- Принудительно применять условия доступа, например требовать многофакторную проверку подлинности.

Мы рекомендуем использовать аутентификацию Azure AD везде, где это возможно.

В этом разделе описаны две технологии на основе Azure AD, которые можно использовать для защиты ресурсов хранилища.

#### <a name="azure-role-based-access-control-azure-rbac"></a>Управление доступом Azure на основе ролей (Azure RBAC)

[Управление доступом на основе ролей Azure (Azure RBAC)](https://docs.microsoft.com/azure/role-based-access-control/overview) обеспечивает детальный контроль доступа к ресурсам Azure. Для управления ролями и разрешениями Azure можно использовать портал Azure.

Обозреватель службы хранилища поддерживает доступ Azure RBAC к учетным записям хранения, BLOB-объектам и очередям. Если вам нужен доступ к общим папкам или таблицам, вам потребуется назначить роли Azure, предоставляющие разрешение на получение списка ключей учетной записи хранения.

#### <a name="access-control-lists-acls"></a>Списки управления доступом (ACL)

[Списки управления доступом (ACL)](https://docs.microsoft.com/azure/storage/blobs/data-lake-storage-access-control) позволяют управлять доступом на уровне файлов и папок в ADLS 2-го поколения контейнерах больших двоичных объектов. Управлять списками ACL можно с помощью Обозреватель службы хранилища.

### <a name="shared-access-signatures-sas"></a>Подписанные URL-адреса (SAS)

Если вы не можете использовать проверку подлинности Azure AD, рекомендуется использовать подписанные URL. С помощью подписей общего доступа можно:

- Предоставьте анонимный ограниченный доступ к защищенным ресурсам.
- Отозвать SAS немедленно, если она создана из политики общего доступа (SAP).

Однако с подписанными URL, вы не можете:

- Ограничьте круг пользователей, которым разрешено использовать SAS. Действительные SAS могут использоваться любым пользователем.
- Отозвать SAS, если он не создан из политики общего доступа (SAP).

При использовании SAS в Обозреватель службы хранилища рекомендуется следовать приведенным ниже рекомендациям.

- **Ограничьте распределение токенов SAS и URI.** Распространять маркеры SAS и URI только среди доверенных лиц. Ограничение распределения SAS снижает вероятность незаконного использования SAS.
- **Используйте токены SAS и URI только из доверенных сущностей.**
- **Если это возможно, используйте политики общего доступа (SAP) при создании маркеров SAS и URI.** SAS, основанный на политике общего доступа, более безопасен, чем простой, так как SAS можно отозвать, удалив SAP.
- **Создание маркеров с минимальным доступом к ресурсам и разрешениями.** Минимальный набор разрешений ограничивает потенциальные повреждения, которые могут быть выполнены при неиспользовании SAS.
- **Создайте токены, которые действительны только до тех пор, пока это необходимо.** Короткий срок существования особенно важен для исходного SAS, так как вы не можете отозвать их после создания.

> [!IMPORTANT]
> При совместном использовании маркеров SAS и URI для устранения неполадок, например в запросах на обслуживание или отчетах об ошибках, всегда следует исправить по крайней мере часть сигнатуры SAS.

### <a name="storage-account-keys"></a>Ключи учетной записи хранения

Ключи учетной записи хранения предоставляют неограниченный доступ к службам и ресурсам в учетной записи хранения. По этой причине рекомендуется ограничить использование ключей для доступа к ресурсам в Обозреватель службы хранилища. Используйте возможности Azure RBAC или SAS для предоставления доступа.

Некоторые роли Azure предоставляют разрешение на получение ключей учетной записи хранения. Пользователи с этими ролями могут эффективно обходить разрешения, предоставленные или запрещенные в Azure RBAC. Рекомендуется не предоставлять это разрешение, если это не требуется.

Обозреватель службы хранилища попытается использовать ключи учетной записи хранения, если они доступны, для проверки подлинности запросов. Эту функцию можно отключить в параметрах (**службы > учетные записи хранения > отключить использование ключей**). Некоторые функции не поддерживают Azure RBAC, например работа с классическими учетными записями хранения. Эти функции по-прежнему нуждаются в ключах и не затрагиваются этим параметром.

Если для доступа к ресурсам хранилища необходимо использовать ключи, рекомендуется следовать приведенным ниже рекомендациям.

- **Не используйте ключи вашей учетной записи совместно с другими пользователями.**
- **Рассматривайте ключи учетной записи хранения, например пароли.** Если необходимо сделать ключи доступными, используйте безопасные решения для хранения данных, например [Azure Key Vault](https://azure.microsoft.com/services/key-vault/).

> [!NOTE]
> Если вы считаете, что ключ учетной записи хранения был общим или распространен по ошибке, можно создать новые ключи для учетной записи хранения из портал Azure.

### <a name="public-access-to-blob-containers"></a>Общий доступ к контейнерам больших двоичных объектов

Обозреватель службы хранилища позволяет изменить уровень доступа контейнеров хранилища BLOB-объектов Azure. Незакрытые контейнеры больших двоичных объектов позволяют любому анонимному доступу для чтения к данным в этих контейнерах.

При включении общего доступа для контейнера больших двоичных объектов рекомендуется следовать приведенным ниже рекомендациям.

- **Не включайте открытый доступ к контейнеру больших двоичных объектов, который может содержать какие-либо потенциально конфиденциальные данные.** Убедитесь, что контейнер больших двоичных объектов свободен от всех частных данных.
- **Не отправляйте потенциально конфиденциальные данные в контейнер больших двоичных объектов с доступом к BLOB-объектам или контейнерам.** 

## <a name="next-steps"></a>Дальнейшие шаги

- [Рекомендации по обеспечению безопасности](https://docs.microsoft.com/azure/storage/blobs/security-recommendations)
