---
title: Что такое Брандмауэр Azure?
description: Брандмауэр Azure — это управляемая облачная служба сетевой безопасности, которая защищает ресурсы виртуальной сети Azure.
author: vhorne
ms.service: firewall
services: firewall
ms.topic: overview
ms.custom: mvc, contperfq1
ms.date: 09/24/2020
ms.author: victorh
Customer intent: As an administrator, I want to evaluate Azure Firewall so I can determine if I want to use it.
ms.openlocfilehash: 24b30842bea51394a375cf48e09b7547e057405c
ms.sourcegitcommit: 32c521a2ef396d121e71ba682e098092ac673b30
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 09/25/2020
ms.locfileid: "91261742"
---
# <a name="what-is-azure-firewall"></a>Что такое Брандмауэр Azure?

![Сертификация ICSA](media/overview/icsa-cert-firewall-small.png)

Брандмауэр Azure — это управляемая облачная служба сетевой безопасности, которая защищает ресурсы виртуальной сети Azure. Это высокодоступная служба с полным отслеживанием состояния и неограниченными возможностями облачного масштабирования.

![Общие сведения о брандмауэре](media/overview/firewall-threat.png)

Вы можете централизованно создавать, применять и регистрировать политики приложений и сетевых подключений в подписках и виртуальных сетях. Брандмауэр Azure использует статический общедоступный IP-адрес для виртуальных сетевых ресурсов, позволяя внешним брандмауэрам идентифицировать трафик, исходящий из виртуальной сети.  Служба полностью интегрирована с Azure Monitor для ведения журналов и аналитики.

## <a name="features"></a>Компоненты

Дополнительные сведения о функциях брандмауэра Azure см. в статье [Функции службы "Брандмауэр Azure"](features.md).

## <a name="known-issues"></a>Известные проблемы

В брандмауэре Azure существуют следующие известные проблемы.

|Проблема  |Описание  |Меры по снижению риска  |
|---------|---------|---------|
Правила сетевой фильтрации для протоколов, которые отличаются от TCP или UDP (например, ICMP), не работают для трафика, связанного с Интернетом|Правила сетевой фильтрации для протоколов, которые отличаются от TCP или UDP, не работают со SNAT для общедоступных IP-адресов. Протоколы, которые отличаются от TCP или UDP, поддерживаются между периферийными зонами подсетей и виртуальной сетью.|Брандмауэр Azure использует Load Balancer (цен. категория "Стандартный"), [который сейчас не поддерживает SNAT для IP-протоколов](https://docs.microsoft.com/azure/load-balancer/load-balancer-overview). Изучаются варианты поддержки этого сценария в будущем выпуске.|
|В PowerShell и CLI отсутствует поддержка протокола ICMP|Azure PowerShell и CLI не поддерживают ICMP как допустимый протокол в правилах сети.|Протокол ICMP по-прежнему можно использовать с помощью портала и REST API. Мы добавим поддержку ICMP в PowerShell и CLI в ближайшее время.|
|Для тегов FQDN требуется указать протокол порта|Для правила приложения с тегами FQDN требуется указать определение протокола порта.|В качестве значения протокола порта можно использовать **HTTPS**. Мы планируем сделать это поле необязательным при использовании тегов FQDN.|
|Не поддерживается перемещение брандмауэра в другую группу ресурсов или подписку|Не поддерживается перемещение брандмауэра в другую группу ресурсов или подписку.|Мы планируем реализовать эту функцию. Чтобы переместить брандмауэр в другую группу ресурсов или подписку, нужно удалить текущий экземпляр и повторно создать его в новой группе ресурсов или подписке.|
|Оповещения Threat Intelligence могут быть замаскированы.|Правила сети, настроенные на режим только предупреждения и назначенные на порт 80/443 для исходящей фильтрации, маскируют предупреждения аналитики угроз.|С помощью правил приложения создайте фильтрацию исходящего трафика для порта 80/443. Или измените режим аналитики угроз на **Alert and Deny** (Оповещение и отказ).|
|Функция DNAT Брандмауэра Azure не поддерживает целевые частные IP-адреса|Поддержка DNAT в Брандмауэре Azure предоставляется только для входящего и исходящего трафика Интернета. Сейчас функция DNAT не поддерживает целевые частные IP-адреса. Например, при передаче из луча в луч в сети типа "звезда".|Это текущее ограничение.|
|Не удается удалить первую конфигурацию общедоступных IP-адресов|Каждый общедоступный IP-адрес Брандмауэра Azure присваивается *конфигурации IP-адресов*.  Первая конфигурация IP-адресов присваивается во время развертывания брандмауэра и обычно также содержит ссылку на подсеть брандмауэра (за исключением случаев, когда она настраивается явно иначе через развертывание шаблона). Эту конфигурацию IP-адресов удалить нельзя, так как это приведет к отмене распределения брандмауэра. Но вы можете изменить или удалить общедоступный IP-адрес, связанный с этой конфигурацией IP-адресов, если в брандмауэре есть еще хотя бы один общедоступный IP-адрес, который можно использовать.|Это сделано намеренно.|
|Зоны доступности можно настроить только во время развертывания.|Зоны доступности можно настроить только во время развертывания. После развертывания брандмауэра невозможно настроить зоны доступности.|Это сделано намеренно.|
|SNAT на входящих соединениях|В дополнении к DNAT подключения через общедоступный IP-адрес брандмауэра (входящий трафик) используют механизм SNAT для одного из частных IP-адресов брандмауэра. На сегодняшний день это требование (также для активно-активных NVA) обеспечивает симметричность маршрутизации.|Чтобы сохранить исходный источник для HTTP/S, следует задуматься об использовании заголовков [XFF](https://en.wikipedia.org/wiki/X-Forwarded-For). Например, перед брандмауэром следует использовать такую службу, как [Azure Front Door](../frontdoor/front-door-http-headers-protocol.md#front-door-to-backend) или [Шлюз приложений Azure](../application-gateway/rewrite-http-headers.md). Также WAF можно добавить в качестве части службы Azure Front Door и привязать ее к брандмауэру.
|Поддержка фильтрации SQL FQDN только в режиме прокси-сервера (порт 1433)|Для Базы данных SQL Azure, Azure Synapse Analytics и Управляемого экземпляра SQL Azure:<br><br>в предварительной версии фильтрация SQL FQDN поддерживается только в режиме прокси-сервера (порт 1433).<br><br>Для Azure SQL IaaS:<br><br>если вы используете нестандартные порты, их можно указать в правилах приложения.|Для SQL в режиме перенаправления (по умолчанию используется для подключения из Azure) можно фильтровать доступ с помощью тега службы SQL в правилах сети Брандмауэра Azure.
|Исходящий трафик через TCP-порт 25 запрещен.| Заблокированы исходящие SMTP-подключения, в которых используется TCP-порт 25 (порт 25 в основном используется для доставки электронной почты, не прошедшей аутентификацию). Это поведение платформы по умолчанию для виртуальных машин. Подробные сведения см. в статье [Troubleshoot outbound SMTP connectivity issues in Azure](../virtual-network/troubleshoot-outbound-smtp-connectivity.md) (Устранение проблем с исходящими SMTP-подключениями в Azure). Но, в отличие от виртуальных машин, в настоящее время эту функцию невозможно включить на Брандмауэре Azure. Примечание. Чтобы разрешить SMTP с аутентификацией (порт 587) или SMTP с использованием порта, отличного от 25, убедитесь, что вы настроили правило сети, а не правило приложения, так как проверка SMTP в настоящее время не поддерживается.|Следуйте рекомендациям по отправке электронной почты, которые приведены в статье об устранении проблем с SMTP. Вы также можете исключить виртуальную машину, для которой требуется исходящий доступ по протоколу SMTP, из маршрута по умолчанию к брандмауэру. Вместо этого настройте исходящий доступ напрямую в Интернет.
|Активное FTP-подключение не поддерживается|Активный FTP отключен в Брандмауэре Azure для защиты от атак через FTP с помощью команды FTP PORT.|Вместо этого можно использовать пассивный FTP. Вам по прежнему необходимо открыть TCP-порты 20 и 21 на брандмауэре.
|Метрика использования порта SNAT показывает 0 %|Метрика использования порта SNAT службы "Брандмауэр Azure" может показывать 0 %, даже если порты SNAT используются. В этом случае использование метрики в качестве компонента метрики работоспособности брандмауэра приводит к неправильному результату.|Эта проблема исправлена, а развертывание в рабочую среду предназначено на май 2020 г. В некоторых случаях повторное развертывание брандмауэра решает проблему, но она не последовательна. В качестве промежуточного обходного пути используйте только состояние работоспособности брандмауэра, чтобы найти случаи, когда *состояние=снижение работоспособности*, а не *состояние=неработоспособное*. Нехватка портов будет отображаться статусом *снижение работоспособности*. Состояние *неисправные* зарезервировано для использования в будущем, когда количество метрик, влияющих на работоспособность брандмауэра, увеличится.
|DNAT не поддерживается при включенном принудительном туннелировании|Брандмауэры, развернутые с включенным принудительным туннелированием, не поддерживают входящий трафик из Интернета из-за асимметричной маршрутизации.|Это обусловлено схемой работы асимметричной маршрутизации. Обратный путь входящих подключений проходит через локальный брандмауэр, в котором нет данных об установленном соединении.
|Исходящий пассивный FTP не работает для брандмауэров с несколькими общедоступными IP-адресами|Пассивный FTP устанавливает разные подключения для каналов управления и данных. Когда брандмауэр с несколькими общедоступными IP-адресами отправляет данные для исходящего трафика, он случайным образом выбирает один из общедоступных IP-адресов в качестве исходного. На FTP происходит сбой, когда каналы данных и управления используют разные исходные IP-адреса.|Планирование явной конфигурации SNAT. Тем временем рассмотрите использование одного IP-адреса в такой ситуации.|
|В метрике NetworkRuleHit отсутствует измерение протокола|Метрика ApplicationRuleHit разрешает протокол на основе фильтрации, но этот компонент отсутствует в соответствующей метрике NetworkRuleHit.|Соответствующее исправление рассматривается.|
|Правила NAT с портами в диапазоне от 64000 до 65535 не поддерживаются|Брандмауэр Azure разрешает порты в диапазоне 1–65535 в правилах сети и приложений, но правила NAT поддерживают порты только в диапазоне 1–63999.|Это текущее ограничение.
|Среднее время обновления конфигурации может занять пять минут|Для обновления конфигурации брандмауэра Azure в среднем может потребоваться 3–5 минут. Параллельные обновления не поддерживаются.|Соответствующее исправление рассматривается.|
|Брандмауэр Azure использует заголовки TLS на основе SNI для фильтрации трафика HTTPS и MSSQL|Если программное обеспечение браузера или сервера не поддерживает расширение индикатора имени сервера, вы не сможете подключиться через Брандмауэр Azure.|Если программное обеспечение браузера или сервера не поддерживает SNI, вы можете управлять подключением с помощью правила сети, а не правила приложения. Сведения о программном обеспечении, поддерживающем SNI, см. в [этой статье](https://wikipedia.org/wiki/Server_Name_Indication).|
|Пользовательская служба DNS (предварительная версия) не работает c принудительным туннелированием.|Если включено принудительное туннелирование, пользовательская служба DNS (предварительная версия) не работает.|Соответствующее исправление рассматривается.|
|Поддержка новых общедоступных IP-адресов для нескольких Зон доступности|Вы не можете добавить новый общедоступный IP-адрес при развертывании брандмауэра с двумя зонами доступности (доступны варианты 1 и 2, 2 и 3 или 1 и 3).|Это ограничение ресурса общедоступного IP-адреса.

## <a name="next-steps"></a>Дальнейшие действия

- [Руководство. развертыванию и настройке службы "Брандмауэр Azure" с помощью портала Azure](tutorial-firewall-deploy-portal.md)
- [Deploy Azure Firewall using a template](deploy-template.md) (Развертывание службы "Брандмауэр Azure" с помощью шаблона)
- [Create an Azure Firewall test environment](scripts/sample-create-firewall-test.md) (Создание тестовой среды службы "Брандмауэр Azure")
