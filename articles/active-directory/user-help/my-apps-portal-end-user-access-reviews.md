---
title: Управление доступом организации к приложениям и группам — Azure AD
description: Узнайте, как выполнять проверку доступа для управления безопасным доступом организации к приложениям и группам с портала "Мои приложения".
services: active-directory
author: curtand
manager: daveba
ms.service: active-directory
ms.subservice: user-help
ms.workload: identity
ms.topic: end-user-help
ms.date: 02/03/2020
ms.author: curtand
ms.reviewer: kasimpso
ms.custom: user-help, seo-update-azuread-jan
ms.openlocfilehash: dbe05f264b0fca6c1a5e8e7d944d94a6bed55392
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/09/2020
ms.locfileid: "88798029"
---
# <a name="perform-an-access-review-from-the-my-apps-portal"></a>Выполнение проверки доступа на портале "Мои приложения"

Вы можете использовать рабочую или учебную учетную запись с веб-портала **Мои приложения** для выполнения проверок доступа к приложениям и группам. Проверки доступа позволяют управлять устаревающими или меняющимися требованиями доступа, а также проверять и обновлять их.

Если у вас нет доступа к порталу **Мои приложения**, обратитесь за разрешением в службу технической поддержки.

[!INCLUDE [preview-notice](../../../includes/active-directory-end-user-my-apps-portal.md)]

>[!Important]
>Эти материалы предназначены для пользователей портала **Мои приложения**. Администраторы могут найти дополнительные сведения о настройке облачных приложений и управлении ими в [документации по управлению приложениями](../manage-apps/index.yml).

## <a name="manage-access-reviews"></a>Управление проверками доступа

Если администратор предоставил вам разрешение на выполнение собственных проверок доступа, вы можете управлять доступом к группам или приложениям с элемента **Проверки доступа** на странице **Мои приложения** портала.

>[!Note]
>Если вы не видите элемент **Проверки доступа**, это значит, что у вас нет разрешения на выполнение проверок доступа или проверок, ожидающих вашего утверждения. Если вы считаете, что у вас должен быть доступ к элементу, обратитесь за помощью в службу технической поддержки.

## <a name="to-perform-your-access-reviews"></a>Для выполнения собственных проверок доступа сделайте следующее.

1. Войдите в рабочую или учебную учетную запись.

2. Откройте веб-браузер и перейдите по адресу https://myapps.microsoft.com или воспользуйтесь ссылкой, предоставленной вашей организацией. Вам может быть предоставлена персонализированная корпоративная страница, например https://myapps.microsoft.com/contoso.com.

    Откроется страница **Приложения**, где отображаются все облачные приложения, принадлежащие вашей организации и доступные вам для использования.

    ![Страница "Приложения" на портале "Мои приложения"](media/my-apps-portal/my-apps-portal-apps-page-access-review-tile.png)

3. Выберите элемент **Проверки доступа**, чтобы просмотреть список проверок доступа, ожидающих утверждения.

    ![Страница "Проверки доступа" с ожидающими проверками доступа для организации](media/my-apps-portal/my-apps-portal-access-reviews-page.png)

4. Выберите **Начать проверку**, чтобы начать проверку доступа.

5. Проверьте доступ и убедитесь, что он все еще необходим.

    ![Страница "Проверка доступа" с подробными сведениями](media/my-apps-portal/my-apps-portal-perform-access-reviews-page.png)

    >[!Note]
    >Если вы являетесь администратором и можете просмотреть доступ своей организации к группам и приложениям, вы увидите другую страницу. Дополнительные сведения о проверке групп или приложений для организации см. в статье [Проверка доступа к группам или приложениям в проверке доступа Azure AD](../governance/perform-access-review.md).

6. Выберите **Да** для сохранения доступа или **Нет** для удаления доступа.

    Если выбрать **Да**, может быть необходимо указать обоснование в поле **Причина**.

    ![Страница "Проверка доступа", отображающая поле "Причина" с образцом текста](media/my-apps-portal/my-apps-portal-perform-access-reviews-reason-box.png)

7. Нажмите кнопку **Submit** (Отправить).

    Проверка доступа завершена, и вы вернетесь на портал **Мои приложения**.

    >[!Note]
    >Доступ можно менять в любое время, пока не завершится период проверки доступа. Если удалить доступ к приложению или группе, он не будет удален немедленно. Удаление происходит, когда заканчивается период проверки доступа или когда администратор закрывает проверку.

## <a name="next-steps"></a>Дальнейшие действия

- [Доступ к порталу "Мои приложения" и использование приложений](my-apps-portal-end-user-access.md)
- [Изменение данных в профиле](my-apps-portal-end-user-update-profile.md)
- [Просмотр и обновление сведений о группах](my-apps-portal-end-user-groups.md)