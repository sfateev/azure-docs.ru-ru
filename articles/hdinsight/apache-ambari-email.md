---
title: Руководство по настройке уведомлений по электронной почте Apache Ambari в Azure HDInsight
description: В этой статье описывается, как использовать SendGrid с Apache Ambari для уведомлений по электронной почте.
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: hrasheed
ms.service: hdinsight
ms.topic: tutorial
ms.date: 03/10/2020
ms.openlocfilehash: 40f8c36b197b0c68b9f04a02dc7731877b27ddd2
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/09/2020
ms.locfileid: "91541668"
---
# <a name="tutorial-configure-apache-ambari-email-notifications-in-azure-hdinsight"></a>Руководство по настройке уведомлений по электронной почте Apache Ambari в Azure HDInsight

В этом руководстве вы узнаете как настраивать уведомления по электронной почте Apache Ambari с помощью SendGrid. [Apache Ambari](./hdinsight-hadoop-manage-ambari.md) упрощает управление кластером HDInsight и его мониторинг за счет удобного пользовательского веб-интерфейса и интерфейса REST API. Ambari предоставляется с кластерами HDInsight и используется для мониторинга кластера и внесения изменений в его конфигурацию. [SendGrid](https://sendgrid.com/solutions/) — это бесплатная облачная служба электронной почты, которая обеспечивает надежную доставку транзакционных писем, предоставляет возможности масштабирования и аналитики в режиме реального времени, а также предоставляет гибкие интерфейсы API, которые упрощают пользовательскую интеграцию. Клиенты Azure могут разблокировать 25 000 бесплатных сообщений электронной почты каждый месяц.

В этом руководстве описано следующее:

> [!div class="checklist"]
> * Получение имени пользователя Sendgrid
> * Настройка уведомлений по электронной почте Apache Ambari

## <a name="prerequisites"></a>Предварительные требования

* Учетная запись электронной почты SendGrid. Инструкции см. в статье [Отправка электронной почты с помощью SendGrid в Azure](https://docs.microsoft.com/azure/sendgrid-dotnet-how-to-send-email).

* Кластер HDInsight. См. [Создание кластеров под управлением Linux в HDInsight с помощью портала Azure](./hdinsight-hadoop-create-linux-clusters-portal.md).

## <a name="obtain-sendgrid-username"></a>Получение имени пользователя SendGrid

1. На [портале Azure](https://portal.azure.com) перейдите к ресурсу SendGrid.

1. На странице "Обзор" выберите **Управление**, чтобы открыть веб-страницу SendGrid для своей учетной записи.

    ![Общие сведения о SendGrid на портале Azure](./media/apache-ambari-email/azure-portal-sendgrid-manage.png)

1. В меню слева перейдите к имени учетной записи, а затем выберите **Сведения об учетной записи**.

    ![Навигация по панели мониторинга SendGrid](./media/apache-ambari-email/sendgrid-dashboard-navigation.png)

1. На странице **Сведения об учетной записи** запишите **Имя пользователя**.

    ![Сведения об учетной записи SendGrid](./media/apache-ambari-email/sendgrid-account-details.png)

## <a name="configure-ambari-e-mail-notification"></a>Настройка уведомлений Ambari по электронной почте

1. В веб-браузере перейдите на страницу `https://CLUSTERNAME.azurehdinsight.net/#/main/alerts`, где `CLUSTERNAME` — это имя вашего кластера.

1. В раскрывающемся списке **Действия** выберите **Управление уведомлениями**.

1. В окне **Manage Alert Notifications** (Управление уведомлениями об оповещениях) выберите значок **+** .

    ![Снимок экрана: диалоговое окно Manage Alert Notifications (Управление уведомлениями об оповещениях)](./media/apache-ambari-email/azure-portal-create-notification.png)

1. В диалоговом окне **Create Alert Notification** (Создание уведомления об оповещениях) укажите следующие сведения:

    |Свойство |Описание |
    |---|---|
    |Имя|Указание имени для уведомления.|
    |Группы|Настройте расписание желаемым образом.|
    |Severity|Настройте расписание желаемым образом.|
    |Описание|Необязательный параметр.|
    |Метод|Оставьте **EMAIL**.|
    |Электронное письмо (кому)|Укажите адреса электронной почты для получения уведомлений, разделите их запятыми.|
    |SMTP-сервер|`smtp.sendgrid.net`|
    |Порт SMTP|25 или 587 (для незашифрованных или TLS-соединений).|
    |Электронное письмо от|Укажите адрес электронной почты. Адрес не обязательно должен быть подлинным.|
    |Используйте проверку подлинности|Установите этот флажок.|
    |Имя пользователя|Укажите имя пользователя SendGrid.|
    |Пароль|Укажите пароль, который вы использовали при создании ресурса SendGrid в Azure.|
    |Подтверждение паролем|Введите пароль еще раз.|
    |Запустите TLS|Установите этот флажок|

    ![Снимок экрана: диалоговое окно Create Alert Notification (Создание уведомления об оповещениях)](./media/apache-ambari-email/ambari-create-alert-notification.png)

    Щелкните **Сохранить**. Вы вернетесь в окно **Manage Alert Notifications** (Управление уведомлениями об оповещениях).

1. В окне **Manage Alert Notifications** (Управление уведомлениями об оповещениях) выберите **Закрыть**.

## <a name="next-steps"></a>Дальнейшие действия

В этом руководстве вы узнали как настраивать уведомления по электронной почте Apache Ambari с помощью SendGrid. Дополнительные сведения о работе Apache Ambari.

* [Управление кластерами HDInsight с помощью веб-интерфейса Apache Ambari](./hdinsight-hadoop-manage-ambari.md)

* [Создание уведомления об оповещении](https://docs.cloudera.com/HDPDocuments/Ambari-latest/managing-and-monitoring-ambari/content/amb_create_an_alert_notification.html)
