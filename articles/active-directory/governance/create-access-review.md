---
title: Создание проверки доступа для групп & приложений — Azure AD
description: Узнайте, как создать проверку доступа к членам группы или доступ к приложениям в Azure Active Directory проверках доступа.
services: active-directory
author: barclayn
manager: daveba
editor: markwahl-msft
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: how-to
ms.subservice: compliance
ms.date: 09/15/2020
ms.author: barclayn
ms.reviewer: mwahl
ms.collection: M365-identity-device-management
ms.openlocfilehash: 02d1c40c26dd6b6992d8df85a986b4157a22226a
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/09/2020
ms.locfileid: "90602937"
---
# <a name="create-an-access-review-of-groups-and-applications-in-azure-ad-access-reviews"></a>Создание проверки доступа для групп и приложений в проверках доступа Azure AD

Доступ сотрудников и гостей к группам и приложениям изменяется со временем. Чтобы уменьшить риск, связанный с устаревшими назначениями доступа, администраторы могут создать в Azure Active Directory (AAD) проверки доступа для участников групп или доступа к приложениям. Если вы хотите проверять доступ регулярно, создайте повторяющиеся проверки доступа. Дополнительные сведения об этих сценариях см. в разделах [Управление пользовательским доступом с помощью проверок доступа Azure AD](manage-user-access-with-access-reviews.md) и [Управление гостевым доступом с помощью проверок доступа Azure AD](manage-guest-access-with-access-reviews.md).

Вы можете просмотреть краткую видеобеседу о включении проверок доступа:

>[!VIDEO https://www.youtube.com/embed/X1SL2uubx9M]

В этой статье описывается, как создать одну или несколько проверок доступа для членов группы или доступа к приложениям.

## <a name="prerequisites"></a>Предварительные требования

- Azure AD Premium P2
- глобальный администратор или администратор пользователей.

Дополнительные сведения см. в статье [Лицензионные требования](access-reviews-overview.md#license-requirements).

## <a name="create-one-or-more-access-reviews"></a>Создание одной или нескольких проверок доступа

1. Войдите в портал Azure и откройте [страницу Управление удостоверениями](https://portal.azure.com/#blade/Microsoft_AAD_ERM/DashboardBlade/).

1. В меню слева щелкните **Проверка доступа**.

1. Щелкните **Создать проверку доступа**, чтобы создать проверку доступа.

    ![Панель "проверки доступа" в управлении удостоверениями](./media/create-access-review/access-reviews.png)

1. Назовите проверку доступа. При необходимости укажите описание проверки. Имя и описание отображаются для рецензентов.

    ![Создание проверки доступа — имя и описание проверки](./media/create-access-review/name-description.png)

1. Установите **дату начала**. По умолчанию проверка доступа выполняется однократно, начинается в то же самое время, когда она создается, и завершается через один месяц. Можно изменить даты начала и окончания таким образом, чтобы проверка доступа началась в будущем и длилась необходимое количество дней.

    ![Создание проверки доступа — даты начала и окончания](./media/create-access-review/start-end-dates.png)

1. Чтобы сделать проверку доступа повторяющейся, измените значение параметра **Частота** с **одного раза** на **еженедельное**, **ежемесячное**, **ежеквартальное**, в **два раза**в год **.** Используйте ползунок **длительности** или текстовое поле, чтобы определить, сколько дней каждая проверка из повторяющейся серии будет открыта для ответов рецензентов. Например, для ежемесячной проверки вы можете настроить продолжительность не более 27 дней, что позволяет избежать перекрытия интервалов проверки.

1. Параметр **Окончание** позволяет указать, как закончится эта серия повторяющихся проверок доступа. Последовательность может завершаться тремя способами: 
    1. Он выполняется непрерывно для неограниченного запуска проверок
    1. До определенной даты,
    1. До завершения определенного числа вхождений. 
  
    Вы, другой администратор пользователей или другой глобальный администратор могут остановить серию после создания, указав в **параметрах** нужную дату окончания.

1. В разделе **Пользователи** укажите пользователей, к которым применяется проверка доступа. Проверки доступа могут выполняться для участников группы или пользователей, которые были назначены для приложения. Можно также сузить область проверки доступа только до гостевых пользователей, которые являются участниками (или назначены для приложения), а не проверять всех пользователей-участников или пользователей с доступом к приложению.

    ![Создание проверки доступа — пользователи](./media/create-access-review/users.png)

1. В разделе **Группа** выберите одну или несколько групп, членство в которых вы хотите проверить.

    > [!NOTE]
    > При выборе нескольких групп будет создано несколько проверок доступа. Например, если выбрать пять групп, будут созданы пять отдельных проверок доступа.
    
    ![Создание проверки доступа — выбор группы](./media/create-access-review/select-group.png)

1. В разделе **приложения** (если вы выбрали пункт **назначено для приложения** на шаге 8) выберите приложения, к которым вы хотите получить доступ.

    > [!NOTE]
    > При выборе нескольких приложений будет создано несколько проверок доступа. Например, если выбрать пять приложений, будут созданы пять отдельных проверок доступа.
    
    ![Создание проверки доступа — Выбор приложения](./media/create-access-review/select-application.png)

1. В разделе **Рецензенты** выберите одного или нескольких пользователей для проверки всех пользователей в области. Или можно указать, чтобы участники сами проверяли собственный доступ. Если ресурс является группой, вы можете попросить владельцев группы выполнить проверку. Можно также потребовать, чтобы рецензенты указывали причину при подтверждении доступа.

    ![Создание проверки доступа — рецензенты](./media/create-access-review/reviewers.png)

1. В разделе **Программы** выберите программы, которые вы хотите использовать. На ней всегда присутствует **программа по умолчанию**.

    ![Создание проверки доступа — программы](./media/create-access-review/programs.png)

    Вы можете упростить сбор и отслеживание проверок доступа, организуя их в программы. Каждую проверку доступа можно связать с программой. Затем при подготовке отчетов для аудитора вы можете сосредоточиться на проверках доступа, входящих в область определенной инициативы. Программы и результаты проверки доступа видны пользователям в роли глобального администратора, администратора пользователя, администратора безопасности или читателя безопасности.

    Чтобы просмотреть список программ, перейдите на страницу проверок доступа и выберите вкладку**Программы**. Если вы являетесь глобальным администратором или ролью администратора пользователей, можно создать дополнительные программы. Например, вы можете использовать одну программу для каждой инициативы по обеспечению соответствия или бизнес-цели. Если программа больше не нужна и с ней не связаны какие-либо элементы управления, ее можно удалить.

### <a name="upon-completion-settings"></a>Настройки после завершения

1. Чтобы указать, что происходит после завершения проверки, разверните раздел **Настройки после завершения**.

    ![Создание параметров завершения проверки доступа](./media/create-access-review/upon-completion-settings-new.png)

2. Если вы хотите автоматически удалить доступ для запрещенных пользователей, установите для параметра **Автоматическое применение результатов значение ресурс** , чтобы **включить**. Если вы хотите вручную применять результаты по завершении проверки, выберите здесь значение **Отключить**.

3. Используйте параметр **если рецензенты не отвечают** на список, чтобы указать, что происходит для пользователей, не просмотренных рецензентом в течение периода проверки. Этот параметр не влияет на пользователей, для которых рецензенты выполняют проверку вручную. Если в качестве решения рецензент выберет запрет, доступ пользователя будет заблокирован.

    - **Без изменений** — доступ пользователя сохраняется без изменений.
    - **Удалить доступ** — доступ пользователя блокируется.
    - **Утвердить доступ** — доступ пользователя утверждается.
    - **Получить рекомендации** — применить рекомендации системы в отношении запрета или утверждения доступа пользователя.

    ![Создание проверки доступа — дополнительные параметры](./media/create-access-review/advanced-settings-preview-new.png)

4. Образца Используйте действие для применения к запрещенным пользователям, чтобы указать, что произойдет с гостевыми пользователями, если они запрещены.
    - **Вариант 1** приведет к удалению доступа запрещенного пользователя к проверяемой группе или приложению, но они смогут войти в клиент. 
    - **Вариант 2** блокирует вход запрещенных пользователей в клиент, независимо от того, есть ли у них доступ к другим ресурсам. Если произошла ошибка или администратор решает повторно включить доступ, он может сделать это в течение 30 дней после отключения пользователя. Если для отключенных пользователей не выполняются никакие действия, они будут удалены из клиента.

Дополнительные сведения о рекомендациях по удалению гостевых пользователей, которые больше не имеют доступа к ресурсам в Организации, см. в статье [использование Azure AD Identity Governance для просмотра и удаления внешних пользователей, у которых больше нет доступа к ресурсам.](access-reviews-external-users.md)

>[!NOTE]
> Действие, применяемое к запрещенным пользователям, работает только в том случае, если предварительная проверка выполнялась только для гостевых пользователей (см. раздел **Создание одного или нескольких проверок доступа** , шаг 8).

### <a name="advanced-settings"></a>Дополнительные параметры

1. Чтобы настроить дополнительные параметры, разверните раздел **Дополнительные параметры**.

1. Задайте для параметра **Показывать рекомендации** значение **Включить**, чтобы рецензенты видели рекомендации системы на основе данных о доступе пользователя.

1. Задайте для параметра **Требовать причину утверждения** значение **Включить**, чтобы требовать от рецензентов указывать причину утверждения.

1. Задайте для параметра **Оповещения по электронной почте** значение **Включить**, чтобы служба AAD отправляла рецензентам по электронной почте уведомления о начале проверки доступа и сообщала администраторам о ее завершении.

1. Задайте для параметра **Напоминания** значение **Включить**, чтобы AAD отправляла напоминания о текущей проверке доступа рецензентам, которые еще не завершили проверку. 

    >[!NOTE]
    > По умолчанию Azure AD автоматически отправляет напоминания по дате окончания в список рецензентов, которые еще не ответили.

1. Образца Содержимое сообщения электронной почты, отправленного рецензентам, формируется автоматически на основе сведений о проверке, таких как имя рецензии, имя ресурса, Дата выполнения и т. д. Если вам нужен способ передачи дополнительных сведений, таких как дополнительные инструкции или контактные данные, можно указать эти сведения в **дополнительном содержимом сообщения для рецензента** , которое будет включаться в приглашение и напоминание электронной почты, отправленные назначенным рецензентам. В выделенном ниже разделе показано, где будут отображаться эти сведения.

    ![Проверка доступа пользователей к группе](./media/create-access-review/review-users-access-group.png)

## <a name="start-the-access-review"></a>Запуск проверки доступа

Завершив настройку параметров для проверки доступа, щелкните **Запустить**. Проверка доступа будет отображаться в списке с индикатором состояния.

![Список проверок доступа и их состояние](./media/create-access-review/access-reviews-list.png)

По умолчанию Azure AD отправляет электронные сообщения рецензентам вскоре после запуска проверки. Если вы выбрали не отправлять сообщения через службу Azure AD, обязательно сообщите рецензентам, что им необходимо выполнить проверку доступа. Вы можете просмотреть инструкции по [просмотру доступа к группам или приложениям](perform-access-review.md). Если ваш отзыв предназначен для того, чтобы проверить собственный доступ для гостей, покажите им инструкции по [просмотру доступа к группам или приложениям](review-your-access.md).

Если вы назначили гостей в качестве рецензентов и не приняли приглашение, они не получат сообщение электронной почты от проверки доступа, так как они должны принять приглашение перед просмотром.

## <a name="access-review-status-table"></a>Таблица состояния проверки доступа

| Состояние | Определение |
|--------|------------|
|NotStarted | Была создана проверка, обнаружение пользователя ожидает запуска. |
|Инициализация   | Выполняется обнаружение пользователей для обнаружения всех пользователей, которые являются частью проверки. |
|Запуск | Проверка начинается. Если уведомления по электронной почте включены, сообщения электронной почты отправляются рецензентам. |
|InProgress | Проверка начата. Если уведомления по электронной почте включены, они были отправлены рецензентам. Рецензенты могут отправлять решения до наступления срока. |
|Окончания | Проверка завершена, и сообщения электронной почты отправляются владельцу проверки. |
|Автоматическая проверка | Проверка находится на этапе проверки системы. Система записывает решения для пользователей, которые не были проверены на основе рекомендаций или предварительно настроенных решений. |
|Автоматическое рецензирование | Решения были зарегистрированы системой для всех пользователей, которые не были проверены. Проверка готова к **применению** , если включено автоматическое применение. |
|Развертывание | Пользователи, которые были утверждены, не будут изменять доступ. |
|Применено | Запрещенные пользователи, если они есть, были удалены из ресурса или каталога. |

## <a name="create-reviews-via-apis"></a>Создание проверок через API

Проверки доступа можно также создать с помощью API. Все действия по управлению проверками доступа для групп и пользователей приложений, которые выполняются на портале Azure, доступны и через API Microsoft Graph. Дополнительные сведения см. в [справочнике по API проверок доступа Azure AD](/graph/api/resources/accessreviews-root?view=graph-rest-beta). Пример кода см. в разделе [Пример получения проверок доступа Azure AD с помощью Microsoft Graph](https://techcommunity.microsoft.com/t5/Azure-Active-Directory/Example-of-retrieving-Azure-AD-access-reviews-via-Microsoft/m-p/236096).

## <a name="next-steps"></a>Дальнейшие шаги

- [Проверка доступа к группам или приложениям](perform-access-review.md)
- [Обзор доступа к группам или приложениям](review-your-access.md)
- [Выполнение проверки доступа для групп или приложений](complete-access-review.md)