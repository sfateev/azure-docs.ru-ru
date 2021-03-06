---
title: Руководство по настройке G Suite для автоматической подготовки пользователей с помощью Azure Active Directory | Документация Майкрософт
description: Узнайте, как настроить автоматическую отмену и подготовку учетных записей пользователей из Azure AD в G Suite.
services: active-directory
author: zchia
writer: zchia
manager: CelesteDG
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.topic: article
ms.date: 01/06/2020
ms.author: Zhchia
ms.openlocfilehash: 3f2f62fe158b946e00c7f81d0cb7eeb0d8f09437
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/09/2020
ms.locfileid: "91331137"
---
# <a name="tutorial-configure-g-suite-for-automatic-user-provisioning"></a>Руководство по настройке G Suite для автоматической подготовки пользователей

В этом учебнике описываются действия, которые необходимо выполнить в G Suite и Azure Active Directory (Azure AD) для настройки автоматической подготовки пользователей. После настройки Azure AD автоматически подготавливает и отменяет подготовку пользователей и групп к [G Suite](https://gsuite.google.com/) с помощью службы подготовки Azure AD. Подробные сведения о том, что делает эта служба, как она работает, и часто задаваемые вопросы см. в статье [Автоматическая подготовка пользователей и ее отзыв для приложений SaaS в Azure Active Directory](../manage-apps/user-provisioning.md). 

> [!NOTE]
> В этом руководстве рассматривается соединитель, созданный на базе службы подготовки пользователей Azure AD. Подробные сведения о том, что делает эта служба, как она работает, и часто задаваемые вопросы см. в статье [Автоматическая подготовка пользователей и ее отзыв для приложений SaaS в Azure Active Directory](../app-provisioning/user-provisioning.md).

> [!NOTE]
> Соединитель G Suite был недавно обновлен в 2019 октября. В соединитель G Suite внесены следующие изменения:
>
> * Добавлена поддержка дополнительных атрибутов пользователей и групп G Suite.
> * Обновлены имена целевых атрибутов G Suite в соответствии с определенным [здесь](https://developers.google.com/admin-sdk/directory).
> * Обновлены сопоставления атрибутов по умолчанию.

## <a name="capabilities-supported"></a>Поддерживаемые возможности
> [!div class="checklist"]
> * Создание пользователей в G Suite
> * Удалять пользователей из G Suite, когда им больше не нужен доступ
> * Синхронизация атрибутов пользователей между Azure AD и G Suite
> * Подготавливайте группы и членство в группах в G Suite
> * [Единый вход](https://docs.microsoft.com/azure/active-directory/saas-apps/google-apps-tutorial) в G Suite (рекомендуется)

## <a name="prerequisites"></a>Предварительные требования

В сценарии, описанном в этом руководстве, предполагается, что у вас уже имеется:

* [Клиент Azure AD.](https://docs.microsoft.com/azure/active-directory/develop/quickstart-create-new-tenant) 
* Учетная запись пользователя в Azure AD с [разрешением](https://docs.microsoft.com/azure/active-directory/users-groups-roles/directory-assign-admin-roles) на настройку подготовки (например, администратор приложений, администратор облачных приложений, владелец приложения или глобальный администратор). 
* [Клиент G Suite](https://gsuite.google.com/pricing.html)
* Учетная запись пользователя в G Suite с разрешениями администратора.

## <a name="step-1-plan-your-provisioning-deployment"></a>Шаг 1. Планирование развертывания для подготовки
1. Узнайте, [как работает служба подготовки](https://docs.microsoft.com/azure/active-directory/manage-apps/user-provisioning).
2. Определите, кто будет находиться в [области подготовки](https://docs.microsoft.com/azure/active-directory/manage-apps/define-conditional-rules-for-provisioning-user-accounts).
3. Определите, какие данные следует [сопоставлять между Azure AD и G Suite](https://docs.microsoft.com/azure/active-directory/manage-apps/customize-application-attributes). 

## <a name="step-2-configure-g-suite-to-support-provisioning-with-azure-ad"></a>Шаг 2. Настройка G Suite для поддержки подготовки с помощью Azure AD

Перед настройкой G Suite для автоматической подготовки пользователей с помощью Azure AD необходимо включить подготовку SCIM в G Suite.

1. Войдите в [консоль администрирования G Suite](https://admin.google.com/) , используя учетную запись администратора, а затем выберите **Безопасность**. Если эта ссылка не отображается, она может быть скрыта в меню **More Controls** (Другие элементы управления) в нижней части экрана.

    ![Безопасность G Suite](./media/google-apps-provisioning-tutorial/gapps-security.png)

2. На странице **Security** (Безопасность) выберите **API Reference** (Справочник по API).

    ![API G Suite](./media/google-apps-provisioning-tutorial/gapps-api.png)

3. Выберите **Включить доступ через API**.

    ![API G Suite включен](./media/google-apps-provisioning-tutorial/gapps-api-enabled.png)

    > [!IMPORTANT]
   > Для каждого пользователя, который вы собираетесь подготавливать к G Suite, его имя пользователя в Azure AD **должно** быть привязано к личному домену. Например, такие имена пользователей, как bob@contoso.onmicrosoft.com, не будут приняты G Suite. а bob@contoso.com — будут. Вы можете изменить домен существующего пользователя, следуя приведенным [здесь](https://docs.microsoft.com/azure/active-directory/fundamentals/add-custom-domain)инструкциям.

4. После добавления и проверки требуемых пользовательских доменов в Azure AD их необходимо проверить снова с помощью G Suite. Чтобы проверить домены в G Suite, ознакомьтесь со следующими шагами:

    a. В [консоли администрирования G Suite](https://admin.google.com/)выберите **домены**.

    ![Домены G Suite](./media/google-apps-provisioning-tutorial/gapps-domains.png)

    b. Выберите **Add a domain or a domain alias** (Добавить домен или псевдоним домена).

    ![Добавление домена G Suite](./media/google-apps-provisioning-tutorial/gapps-add-domain.png)

    c. Выберите **Add another domain** (Добавить другой домен), а затем введите имя домена, который нужно добавить.

    ![Добавить еще один пакет G Suite](./media/google-apps-provisioning-tutorial/gapps-add-another.png)

    d. Выберите **Continue and verify domain ownership** (Перейти к проверке владельца домена). Затем выполните действия, подтверждающие, что вы владеете доменным именем. Подробные инструкции по проверке домена с помощью Google см. в статье [Проверка принадлежности сайта](https://support.google.com/webmasters/answer/35179).

    д) Повторите предыдущие шаги для всех дополнительных доменов, которые вы собираетесь добавить в G Suite.

5. Затем определите учетную запись администратора, которую вы хотите использовать для управления подготовкой пользователей в G Suite. Перейдите к **роли администратора**.

    ![Администратор G Suite](./media/google-apps-provisioning-tutorial/gapps-admin.png)

6. Для **роли администратора** этой учетной записи измените **права доступа** для этой роли. Убедитесь, что для учетной записи выбраны все параметры в разделе **Admin API Privileges** (Административные права доступа к API). Это позволит использовать ее для подготовки.

    ![Права администратора G Suite](./media/google-apps-provisioning-tutorial/gapps-admin-privileges.png)

## <a name="step-3-add-g-suite-from-the-azure-ad-application-gallery"></a>Шаг 3. Добавление G Suite из коллекции приложений Azure AD

Добавьте G Suite из коллекции приложений Azure AD, чтобы начать управление подготовкой к работе с G Suite. Если вы ранее настроили G Suite для единого входа, вы можете использовать то же приложение. Однако при первоначальном тестировании интеграции рекомендуется создать отдельное приложение. Дополнительные сведения о добавлении приложения из коллекции см. [здесь](https://docs.microsoft.com/azure/active-directory/manage-apps/add-gallery-app). 

## <a name="step-4-define-who-will-be-in-scope-for-provisioning"></a>Шаг 4. Определение пользователей для включения в область подготовки 

Служба подготовки Azure AD позволяет определить пользователей, которые будут подготовлены, на основе назначения приложению и (или) атрибутов пользователя или группы. Если вы решили указать, кто именно будет подготовлен к работе в приложении, на основе назначения, можно выполнить следующие [действия](../manage-apps/assign-user-or-group-access-portal.md), чтобы назначить пользователей и группы приложению. Если вы решили указать, кто именно будет подготовлен, на основе одних только атрибутов пользователя или группы, можете использовать фильтр задания области, как описано [здесь](https://docs.microsoft.com/azure/active-directory/manage-apps/define-conditional-rules-for-provisioning-user-accounts). 

* При назначении пользователей и групп в G Suite необходимо выбрать роль, отличную от **доступа по умолчанию**. Пользователи с ролью "Доступ по умолчанию" исключаются из подготовки и будут помечены в журналах подготовки как не назначенные явно. Кроме того, если эта роль является единственной, доступной в приложении, можно [изменить манифест приложения](https://docs.microsoft.com/azure/active-directory/develop/howto-add-app-roles-in-azure-ad-apps), чтобы добавить дополнительные роли. 

* Начните с малого. Протестируйте небольшой набор пользователей и групп, прежде чем выполнять развертывание для всех. Если в область подготовки включены назначенные пользователи и группы, проверьте этот механизм, назначив приложению одного или двух пользователей либо одну или две группы. Если в область включены все пользователи и группы, можно указать [фильтр области на основе атрибутов](https://docs.microsoft.com/azure/active-directory/manage-apps/define-conditional-rules-for-provisioning-user-accounts). 


## <a name="step-5-configure-automatic-user-provisioning-to-g-suite"></a>Шаг 5. Настройка автоматической подготовки пользователей в G Suite 

В этом разделе описывается, как настроить службу подготовки Azure AD для создания, обновления и отключения пользователей и (или) групп в TestApp на основе их назначений в Azure AD.

> [!NOTE]
> Дополнительные сведения о конечной точке API каталога G Suite см. в разделе [API каталога](https://developers.google.com/admin-sdk/directory).

### <a name="to-configure-automatic-user-provisioning-for-g-suite-in-azure-ad"></a>Чтобы настроить автоматическую подготовку пользователей для G Suite в Azure AD, сделайте следующее:

1. Войдите на [портал Azure](https://portal.azure.com). Выберите **Корпоративные приложения**, а затем **Все приложения**. Пользователям потребуется войти в portal.azure.com и не сможет использовать aad.portal.azure.com

    ![Колонка "Корпоративные приложения"](./media/google-apps-provisioning-tutorial/enterprise-applications.png)

    ![Колонка "Все приложения"](./media/google-apps-provisioning-tutorial/all-applications.png)

2. В списке приложений выберите **G Suite**.

    ![Ссылка на G Suite в списке "Приложения"](common/all-applications.png)

3. Выберите вкладку **Подготовка**. Нажмите кнопку **Начало работы**.

    ![Снимок экрана параметров управления с вызываемым параметром подготовки.](common/provisioning.png)

      ![Колонка "Начало работы"](./media/google-apps-provisioning-tutorial/get-started.png)

4. Для параметра **Режим подготовки к работе** выберите значение **Automatic** (Автоматически).

    ![Снимок экрана: раскрывающийся список режима подготовки с вызываемым автоматическим параметром.](common/provisioning-automatic.png)

5. В разделе **учетные данные администратора** щелкните **авторизовать**. Вы будете перенаправлены в диалоговое окно "Авторизация Google" в новом окне браузера.

      ![Разрешающий G Suite](./media/google-apps-provisioning-tutorial/authorize-1.png)

6. Подтвердите, что вы хотите предоставить разрешения Azure AD на внесение изменений в клиент G Suite. Нажмите кнопку **Принять**.

     ![Проверка подлинности клиента G Suite](./media/google-apps-provisioning-tutorial/gapps-auth.png)

7. В портал Azure щелкните **проверить подключение** , чтобы убедиться, что Azure AD может подключиться к G Suite. Если подключение не выполняется, убедитесь, что у учетной записи G Suite есть разрешения администратора, и повторите попытку. Затем повторите шаг **авторизации**.

6. В поле **Почтовое уведомление** введите адрес электронной почты пользователя или группы, которые должны получать уведомления об ошибках подготовки, а также установите флажок **Отправить уведомление по электронной почте при сбое**.

    ![Почтовое уведомление](common/provisioning-notification-email.png)

7. Щелкните **Сохранить**.

8. В разделе **Сопоставления** выберите **Подготовка пользователей Azure Active Directory**.

9. Изучите пользовательские атрибуты, которые синхронизированы из Azure AD, с G Suite в разделе **сопоставление атрибутов** . Атрибуты, выбранные как свойства **Matching** , используются для сопоставления учетных записей пользователей в G Suite для операций обновления. Если вы решили изменить [соответствующий целевой атрибут](https://docs.microsoft.com/azure/active-directory/manage-apps/customize-application-attributes), необходимо убедиться, что API G Suite поддерживает фильтрацию пользователей на основе этого атрибута. Нажмите кнопку **Сохранить**, чтобы зафиксировать все изменения.

   |attribute|Тип|
   |---|---|
   |примаремаил|Строковый тип|
   |соответствие. [Type EQ "Manager"]. Value|Строка|
   |name.familyName|Строка|
   |name.givenName|Строка|
   |suspended (приостановлено)|Строковый тип|
   |Екстерналидс. [Type EQ "Custom"]. значение|Строковый тип|
   |Екстерналидс. [Type EQ "Организация"]. значение|Строковый тип|
   |Addresses. [Введите EQ "Рабочая"]. Country|Строковый тип|
   |Addresses. [Введите EQ "Рабочий"]. streetAddress|Строковый тип|
   |Addresses. [Type EQ "Рабочая"]. регион|Строковый тип|
   |Addresses. [Введите EQ "Рабочий"]. локальное значение|Строковый тип|
   |Addresses. [Type EQ "Рабочий"]. postalCode|Строковый тип|
   |сообщений электронной почты. [Введите EQ "Рабочий"]. адрес|Строковый тип|
   |включающ. [Введите EQ "Рабочая"]. Department|Строковый тип|
   |включающ. [Введите EQ "Рабочий"]. Title|Строковый тип|
   |Фоненумберс. [Type EQ "Рабочий"]. значение|Строковый тип|
   |Фоненумберс. [Введите EQ "мобильное"]. значение|Строковый тип|
   |Фоненумберс. [Type EQ "work_fax"]. Value|Строковый тип|
   |сообщений электронной почты. [Введите EQ "Рабочий"]. адрес|Строковый тип|
   |включающ. [Введите EQ "Рабочая"]. Department|Строковый тип|
   |включающ. [Введите EQ "Рабочий"]. Title|Строковый тип|
   |Фоненумберс. [Type EQ "Рабочий"]. значение|Строковый тип|
   |Фоненумберс. [Введите EQ "мобильное"]. значение|Строковый тип|
   |Фоненумберс. [Type EQ "work_fax"]. Value|Строковый тип|
   |Addresses. [Введите EQ "Домашняя страница"]. страна|Строковый тип|
   |Addresses. [Введите EQ "Home"]. форматированный|Строковый тип|
   |Addresses. [Введите EQ "Home"]. локальность|Строковый тип|
   |Addresses. [Type EQ "Home"]. postalCode|Строковый тип|
   |Addresses. [Type EQ "Home"]. регион|Строковый тип|
   |Addresses. [Введите EQ "Home"]. streetAddress|Строковый тип|
   |Addresses. [Введите EQ "другое"]. Country|Строковый тип|
   |Addresses. [Введите EQ "Other"]. форматированный|Строковый тип|
   |Addresses. [Введите EQ "Other"]. локальное значение|Строковый тип|
   |Addresses. [Введите EQ "Other"]. postalCode|Строковый тип|
   |Addresses. [Введите EQ "Other"]. регион|Строковый тип|
   |Addresses. [Введите EQ "Other"]. streetAddress|Строковый тип|
   |Addresses. [Введите EQ "Рабочий"]. форматированный|Строковый тип|
   |чанжепассвордатнекстлогин|Строковый тип|
   |сообщений электронной почты. [Введите EQ "Home"]. адрес|Строковый тип|
   |сообщений электронной почты. [Введите EQ "Other"]. адрес|Строковый тип|
   |Екстерналидс. [тип EQ "учетная запись"]. значение|Строковый тип|
   |Екстерналидс. [Type EQ "Custom"]. Кустомтипе|Строковый тип|
   |Екстерналидс. [Введите EQ "Customer"]. значение|Строковый тип|
   |Екстерналидс. [Type EQ "login_id"]. Value|Строковый тип|
   |Екстерналидс. [Type EQ "сеть"]. значение|Строковый тип|
   |Тип пола.|Строковый тип|
   |женератедиммутаблеид|Строковый тип|
   |Идентификатор|Строковый тип|
   |мгновен. [Введите EQ "Home"]. Протокол|Строковый тип|
   |мгновен. [Введите EQ "Other"]. Протокол|Строковый тип|
   |мгновен. [Введите EQ "Рабочий"]. Протокол|Строковый тип|
   |инклудеинглобаладдресслист|Строковый тип|
   |ипвхителистед|Строковый тип|
   |включающ. [Type EQ "школа"]. costCenter|Строковый тип|
   |включающ. [Type EQ "School"]. Department|Строковый тип|
   |включающ. [Type EQ "School"]. domain|Строковый тип|
   |включающ. [Type EQ "школа"]. Фуллтимикуивалент|Строковый тип|
   |включающ. [Type EQ "School"]. Location|Строковый тип|
   |включающ. [Type EQ "школа"]. имя|Строковый тип|
   |включающ. [Type EQ "School"]. Symbol|Строковый тип|
   |включающ. [Введите EQ "школа"]. Title|Строковый тип|
   |включающ. [Введите EQ "Рабочий"]. costCenter|Строковый тип|
   |включающ. [Введите EQ "Рабочий"]. домен|Строковый тип|
   |включающ. [Введите EQ "Рабочий"]. Фуллтимикуивалент|Строковый тип|
   |включающ. [Введите EQ "Рабочий"]. расположение|Строковый тип|
   |включающ. [Введите EQ "Рабочий"]. имя|Строковый тип|
   |включающ. [Введите EQ "Рабочий"]. Symbol|Строковый тип|
   |оргунитпас|Строковый тип|
   |Фоненумберс. [Type EQ "Home"]. значение|Строковый тип|
   |Фоненумберс. [Введите EQ "другое"]. значение|Строковый тип|
   |веб-сайтов. [Type EQ "Home"]. значение|Строковый тип|
   |веб-сайтов. [Введите EQ "другое"]. значение|Строковый тип|
   |веб-сайтов. [Type EQ "Рабочий"]. значение|Строковый тип|
   

10. В разделе **сопоставления** выберите **подготавливать Azure Active Directory группы**.

11. Изучите атрибуты группы, синхронизированные из Azure AD, с G Suite в разделе **сопоставление атрибутов** . Атрибуты, выбранные как свойства **Matching** , используются для сопоставления групп в G Suite для операций обновления. Нажмите кнопку **Сохранить**, чтобы зафиксировать все изменения.

      |attribute|Тип|
      |---|---|
      |email|Строковый тип|
      |Члены|Строковый тип|
      |name|Строка|
      |description|Строка|

12. Чтобы настроить фильтры области, ознакомьтесь со следующими инструкциями, предоставленными в [руководстве по фильтрам области](../manage-apps/define-conditional-rules-for-provisioning-user-accounts.md).

13. Чтобы включить службу подготовки Azure AD для G Suite, измените значение параметра **состояние подготовки** на **включено** в разделе **Параметры** .

    ![Состояние подготовки "Включено"](common/provisioning-toggle-on.png)

14. Определите пользователей и (или) группы, которые вы хотите подготавливать к G Suite, выбрав нужные значения в **области** в разделе **Параметры** .

    ![Область действия подготовки](common/provisioning-scope.png)

15. Когда будете готовы выполнить подготовку, нажмите кнопку **Сохранить**.

    ![Сохранение конфигурации подготовки](common/provisioning-configuration-save.png)

После этого начнется цикл начальной синхронизации всех пользователей и групп, определенных в поле **Область** в разделе **Параметры**. Начальный цикл занимает больше времени, чем последующие циклы. Пока служба подготовки Azure AD запущена, они выполняются примерно каждые 40 минут.

> [!NOTE]
> Если у пользователей уже есть Персональная учетная запись или пользователь, использующий адрес электронной почты пользователя Azure AD, это может привести к возникновению проблемы, которую можно разрешить с помощью средства Google, прежде чем выполнять синхронизацию каталога.

## <a name="step-6-monitor-your-deployment"></a>Шаг 6. Мониторинг развертывания
После настройки подготовки используйте следующие ресурсы для мониторинга развертывания.

1. Используйте [журналы подготовки](https://docs.microsoft.com/azure/active-directory/reports-monitoring/concept-provisioning-logs), чтобы определить, какие пользователи были подготовлены успешно или неудачно.
2. Используйте [индикатор выполнения](https://docs.microsoft.com/azure/active-directory/app-provisioning/application-provisioning-when-will-provisioning-finish-specific-user), чтобы узнать состояние цикла подготовки и приблизительное время до его завершения.
3. Если конфигурация подготовки, вероятно, находится в неработоспособном состоянии, приложение перейдет в карантин. Дополнительные сведения о режимах карантина см. [здесь](https://docs.microsoft.com/azure/active-directory/manage-apps/application-provisioning-quarantine-status).

## <a name="additional-resources"></a>Дополнительные ресурсы

* [Управление подготовкой учетных записей пользователей для корпоративных приложений](../manage-apps/configure-automatic-user-provisioning-portal.md)
* [Что такое доступ к приложениям и единый вход с помощью Azure Active Directory?](../manage-apps/what-is-single-sign-on.md)

## <a name="next-steps"></a>Дальнейшие действия

* [Сведения о просмотре журналов и получении отчетов о действиях по подготовке](../manage-apps/check-status-user-account-provisioning.md)
