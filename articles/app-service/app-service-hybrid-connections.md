---
title: Гибридные подключения
description: Узнайте, как создавать и использовать гибридные подключения в службе приложений Azure для доступа к ресурсам в разнородных сетях.
author: ccompy
ms.assetid: 66774bde-13f5-45d0-9a70-4e9536a4f619
ms.topic: article
ms.date: 06/08/2020
ms.author: ccompy
ms.custom: seodec18, fasttrack-edit
ms.openlocfilehash: 1cb86f77a6ffcbb0fb45b3a57b57de531822f2b0
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/09/2020
ms.locfileid: "91742610"
---
# <a name="azure-app-service-hybrid-connections"></a>Гибридные подключения к службе приложений Azure

Гибридные подключения — это и служба в Azure, и функция в службе приложений Azure. В качестве службы их применение и возможности шире, чем функциональные возможности в службе приложений Azure. Чтобы узнать больше о гибридных подключениях и их использовании вне службы приложений, изучите раздел [Протокол гибридных подключений ретранслятора Azure][HCService].

В службе приложений гибридные подключения можно использовать для доступа к ресурсам приложения в любой сети, которая может выполнять исходящие вызовы к Azure через порт 443. Гибридные подключения предоставляет доступ из вашего приложения к конечной точке TCP и не обеспечивает новый способ доступа к приложению. При использовании в службе приложений каждое гибридное подключение сопоставляется с одной комбинацией узла TCP и TCP-порта. Это позволяет приложениям получать доступ к ресурсам в любой операционной системе, если это конечная точка TCP. Гибридным подключениям не важен используемый протокол приложения или ресурсы, к которым вы обращаетесь. Он просто предоставляет доступ к сети.  

## <a name="how-it-works"></a>Принцип работы ##
Гибридные подключения требуется развернуть Агент ретранслятора, где он может получить доступ как к нужной конечной точке, так и к Azure. Агент ретрансляции, Диспетчер гибридных подключений (HCM), вызывает для Azure Relay через порт 443. С сайта веб-приложения инфраструктура службы приложений также подключается к Azure Relay от имени вашего приложения. Через подключенные подключения ваше приложение может получить доступ к нужной конечной точке. Это подключение использует протокол TLS 1.2 для обеспечения безопасности и ключи подписанного URL-адреса (SAS) для аутентификации и авторизации.    

![Общая блок-схема гибридного подключения][1]

Когда приложение выполнит запрос DNS, который соответствует настроенной конечной точке гибридного подключения, исходящий трафик TCP будет перенаправлен дальше по этому гибридному подключению.  

> [!NOTE]
> Это означает, что для гибридного подключения следует всегда использовать DNS-имя. Некоторые клиентские программы не выполняют поиск DNS, если конечная точка использует IP-адрес, а не DNS-имя. 
>

### <a name="app-service-hybrid-connection-benefits"></a>Преимущества гибридного подключения службы приложений ###

Гибридные подключения обладают рядом преимуществ, включая следующие:

- Приложения безопасно могут обращаться к локальным службам и системам.
- Для них не требуется конечная точка с доступом к Интернету.
- Их можно быстро и легко настроить. Шлюзы не требуются
- Каждое гибридное подключение соответствует отдельной комбинации "узел:порт", что удобно с точки зрения безопасности.
- Для них обычно не требуются бреши в брандмауэре. Все подключения исходят через стандартные веб-порты.
- Так как гибридные подключения работают на уровне сети, они не зависят ни от языка, используемого приложением, ни от технологии, используемой конечной точкой.
- Их можно использовать для доступа к нескольким сетям из одного приложения. 
- Она поддерживается в общедоступной версии для собственных приложений Windows и доступна для просмотра приложений Linux. Он не поддерживается для приложений-контейнеров Windows.

### <a name="things-you-cannot-do-with-hybrid-connections"></a>Что невозможно сделать с помощью гибридных подключений ###

Существует несколько вещей, которые невозможно выполнить с помощью гибридных подключений, в том числе:

- подключение диска;
- использование протокола UDP;
- доступ к службам на основе протокола TCP, использующим динамические порты (таким как пассивный режим FTP или расширенный пассивный режим FTP);
- поддержка протокола LDAP, так как он иногда требует наличие протокола UDP;
- поддержка Active Directory, потому что вы не можете присоединить к домену рабочую роль службы приложений. 

## <a name="add-and-create-hybrid-connections-in-your-app"></a>Добавление и создание гибридных подключений в приложении ##

Чтобы создать гибридное подключение, перейдите на [портал Azure][portal] и выберите свое приложение. Выберите **сеть**  >  **Настройка конечных точек гибридного подключения**. Здесь можно увидеть гибридные подключения, которые были настроены для приложения.  

![Снимок экрана списка гибридных подключений][2]

Чтобы добавить новое гибридное подключение, нажмите кнопку **[+] Добавить гибридное подключение**.  Отобразится список гибридных подключений, которые вы уже создали. Чтобы добавить одно или несколько из них в приложение, выберите соответствующие подключения, затем щелкните **Добавление выбранного гибридного подключения**.  

![Снимок экрана портала гибридных подключений][3]

Если вы хотите создать гибридное подключение, щелкните **Создать гибридное подключение**. Укажите: 

- Имя гибридного подключения.
- имя узла конечной точки;
- порт конечной точки;
- пространство имен служебной шины, которое вы хотите использовать.

![Снимок экрана диалогового окна создания гибридного подключения][4]

Каждое гибридное подключение привязано к пространству имен служебной шины, а каждое пространство имен служебной шины находится в регионе Azure. Важно стараться использовать пространство имен служебной шины в том же регионе, в котором размещено ваше приложение, чтобы избежать сетевых задержек.

Если вы хотите удалить из приложения гибридное подключение, щелкните его правой кнопкой мыши и выберите **Отключить**.  

После добавления гибридного подключения в приложение вы сможете просмотреть сведения о нем, просто щелкнув это подключение. 

![Снимок экрана сведений о гибридном подключении][5]

### <a name="create-a-hybrid-connection-in-the-azure-relay-portal"></a>Создание гибридного подключения на портале ретранслятора Azure ###

Помимо использования элементов интерфейса портала в приложении, вы можете создавать гибридные подключения на портале ретранслятора Azure. Чтобы гибридное подключение могло использоваться службой приложений, оно должно:

* требовать авторизации клиентов;
* использовать элемент метаданных endpoint, который содержит комбинацию "узел:порт" в качестве значения.

## <a name="hybrid-connections-and-app-service-plans"></a>Гибридные подключения и планы службы приложений ##

Гибридные подключения к службе приложений доступны только для ценовых категорий (SKU) "Базовый", "Стандартный", "Премиум" и "Изолированный". С каждым тарифным планом связаны определенные ограничения.  

| Ценовой план | Количество гибридных подключений в плане |
|----|----|
| Basic | 5 на план |
| Стандартный | 25 на план |
| Категории премиум v2 | 200 на приложение |
| Isolated | 200 на приложение |

В пользовательском интерфейсе плана службы приложений показано, сколько гибридных подключений используется и какими приложениями.  

![Снимок экрана свойств плана службы приложений][6]

Выберите гибридное подключение, чтобы просмотреть сведения о нем. Вы увидите все сведения, можно было увидеть в представлении приложения. Можно также узнать, сколько приложений в том же плане использует это гибридное подключение.

Существует ограничение числа конечных точек гибридного подключения, которые могут использоваться в плане службы приложений. Однако каждое гибридное подключение может использоваться любым числом приложений в этом плане. Например, отдельное гибридное подключение, используемое в пяти отдельных приложениях в плане службы приложений, считается одним гибридным подключением.

### <a name="pricing"></a>Цены ###

В дополнение к требованию к номерам SKU для плана службы приложений существуют дополнительные затраты на использование гибридных подключений. Плата взимается за каждый прослушиватель, используемый гибридным подключением. Прослушиватель — это Диспетчер гибридных подключений (HCM). Если у вас пять гибридных подключений, поддерживаемых двумя Диспетчерами гибридных подключений, в результате у вас 10 прослушивателей. Дополнительные сведения см. на странице [цен на служебную шину][sbpricing].

## <a name="hybrid-connection-manager"></a>Hybrid Connection Manager ##

Для работы гибридных подключений в сети, в которой размещена конечная точка гибридного подключения, необходим агент ретрансляции. Он называется Hybrid Connection Manager (HCM). Чтобы скачать HCM из приложения на [портале Azure][portal], выберите **Сети** > **Настройте конечные точки гибридного подключения**.  

Данный инструмент работает в Windows Server 2012 и более поздних версиях. HCM работает как служба и устанавливает исходящее подключение к Azure Relay на порту 443.  

После установки HCM можно запустить HybridConnectionManagerUi.exe, чтобы использовать пользовательский интерфейс этого инструмента. Этот файл расположен в каталоге установки диспетчера гибридных подключений. В Windows 10 можно также просто выполнить поиск *пользовательского интерфейса диспетчера гибридных подключений* в поле поиска.  

![Снимок экрана диспетчера гибридных подключений][7]

При запуске пользовательского интерфейса HCM первое, что вы видите, — это таблица со списком всех гибридных подключений, настроенных с помощью этого экземпляра HCM. Если вы хотите внести изменения, сначала пройдите аутентификацию в Azure. 

Вот как можно добавить одно или несколько гибридных подключений в HCM.

1. Запустите пользовательский интерфейс HCM.
2. Щелкните **Configure another hybrid connection** (Настроить еще одно гибридное подключение).
![Снимок экрана настройки новых гибридных подключений][8]

1. Войдите с помощью учетной записи Azure, чтобы получить доступ к гибридные подключения в ваших подписках. HCM не будет использовать вашу учетную запись Azure сверх этого. 
1. Выберите подписку.
1. Щелкните гибридные подключения, которые HCM должен транслировать.
![Снимок экрана гибридных подключений][9]

1. Щелкните **Сохранить**.

Теперь вы увидите добавленные гибридные подключения. Можно также выбрать настроенное гибридное подключение, чтобы просмотреть сведения о нем.

![Снимок экрана сведений о гибридном подключении][10]

Чтобы HCM мог обслуживать настроенные для него гибридные подключения, ему требуется:

- доступ к Azure по протоколу TCP через порт 443;
- доступ к конечной точке гибридного подключения по протоколу TCP;
- возможность поиска DNS на узле конечной точки и в пространстве имен служебной шины Azure.

> [!NOTE]
> Ретранслятор Azure использует для подключения веб-сокеты. Эта возможность доступна только в Windows Server 2012 и более поздних версиях. Поэтому HCM не поддерживается в версиях, предшествующих Windows Server 2012.
>

### <a name="redundancy"></a>Избыточность ###

Каждый экземпляр HCM может поддерживать несколько гибридных подключений. Кроме того, любое заданное гибридное подключение может поддерживаться несколькими HCM. Режим по умолчанию — маршрутизация трафика методом циклического перебора между настроенными экземплярами HCM для любой заданной конечной точки. Если требуется обеспечить высокий уровень доступности гибридных подключений из сети, запустите несколько экземпляров HCM на разных компьютерах. Алгоритм распределения нагрузки, используемый службой ретранслятора для распределения трафика между экземплярами HCM, назначается случайно. 

### <a name="manually-add-a-hybrid-connection"></a>Добавление гибридного подключения вручную ###

Если необходимо, чтобы кто-либо вне вашей подписки разместил экземпляр HCM для заданного гибридного подключения, предоставьте ему строку подключения к шлюзу для этого гибридного подключения. Вы можете увидеть строку подключения к шлюзу в свойствах гибридного подключения на [портале Azure][portal]. Чтобы использовать эту строку, нажмите кнопку **Ввести вручную** в HCM и вставьте строку подключения к шлюзу.

![Добавление гибридного подключения вручную][11]

### <a name="upgrade"></a>Обновление ###

Для Диспетчера гибридных подключений выпускаются периодические обновления, предназначенные для устранения проблем или предоставления улучшений. В случае выхода обновлений в пользовательском интерфейсе HCM будет отображаться всплывающее окно. При установке обновления будут применены изменения и HCM перезапустится. 

## <a name="adding-a-hybrid-connection-to-your-app-programmatically"></a>Добавление гибридного подключения к приложению программным способом ##

Гибридные подключения поддерживается Azure CLI. Предоставленные команды работают как в приложении, так и на уровне плана службы приложений.  Ниже приведены команды уровня приложения.

```azurecli
az webapp hybrid-connection

Group
    az webapp hybrid-connection : Methods that list, add and remove hybrid-connections from webapps.
        This command group is in preview. It may be changed/removed in a future release.
Commands:
    add    : Add a hybrid-connection to a webapp.
    list   : List the hybrid-connections on a webapp.
    remove : Remove a hybrid-connection from a webapp.
```

Команды плана службы приложений позволяют задать ключ, который будет использовать данное гибридное подключение. Для каждого гибридного подключения, первичного и вторичного, устанавливаются два ключа. Можно выбрать использование первичного или вторичного ключа с помощью приведенных ниже команд. Это позволяет переключать ключи для периодического повторного создания ключей. 

```azurecli
az appservice hybrid-connection --help

Group
    az appservice hybrid-connection : A method that sets the key a hybrid-connection uses.
        This command group is in preview. It may be changed/removed in a future release.
Commands:
    set-key : Set the key that all apps in an appservice plan use to connect to the hybrid-
                connections in that appservice plan.
```

## <a name="secure-your-hybrid-connections"></a>Защита гибридные подключения ##

Существующее гибридное подключение можно добавить в другие веб-приложения службы приложений любым пользователем, имеющим достаточные разрешения на основе ретранслятора служебной шины Azure. Это означает, что если необходимо предотвратить повторное использование этого же гибридного подключения (например, если целевой ресурс является службой, не имеющей каких-либо дополнительных мер безопасности для предотвращения несанкционированного доступа), необходимо заблокировать доступ к ретранслятору служебной шины Azure.

Любой пользователь, имеющий `Reader` доступ к ретранслятору, сможет _увидеть_ гибридное подключение при попытке добавить его в веб-приложение в портал Azure, но не сможет _добавить_ его, так как у них отсутствуют разрешения на получение строки подключения, которая используется для подключения ретранслятора. Чтобы успешно добавить гибридное подключение, у них должно быть `listKeys` разрешение ( `Microsoft.Relay/namespaces/hybridConnections/authorizationRules/listKeys/action` ). `Contributor`Роль или любая другая роль, которая включает это разрешение для ретранслятора, позволит пользователям использовать гибридное подключение и добавлять его в собственные веб-приложения.

## <a name="troubleshooting"></a>Устранение неполадок ##

Состояние "Подключено" означает, что гибридное подключение настроено по крайней мере в одном HCM и оно позволяет подключиться к Azure. Если состояние вашего гибридного подключения не **Подключено**, то оно не настроено в HCM с доступом к Azure.

Основная причина, по которой клиенты не могут подключиться к своей конечной точке, состоит в том, что для нее был указан IP-адрес, а не DNS-имя. Если приложение не может связаться с требуемой конечной точкой и был указан IP-адрес, перейдите на использование DNS-имени, являющегося действительным на узле, на котором выполняется HCM. Также убедитесь, DNS-имя правильно разрешается для узла, на котором запущен HCM. Убедитесь в наличии подключения между узлом, на котором запущен HCM, и конечной точкой гибридного подключения.  

В службе приложений средство командной строки **tcpping** можно вызвать из консоли Advanced Tools (KUDU). Этот инструмент может сообщить, имеется ли доступ к конечной точке TCP, но не сообщает, имеется ли доступ к конечной точке гибридного подключения. Используя этот инструмент в консоли для конечной точки гибридного подключения, можно только подтвердить, что оно использует сочетание "узел:порт".  

Если у вас есть клиент командной строки для конечной точки, можно проверить возможность подключения из консоли приложения. Например, можно проверить доступ к конечным точкам веб-сервера, используя фигурные скобки.


<!--Image references-->
[1]: ./media/app-service-hybrid-connections/hybridconn-connectiondiagram.png
[2]: ./media/app-service-hybrid-connections/hybridconn-portal.png
[3]: ./media/app-service-hybrid-connections/hybridconn-addhc.png
[4]: ./media/app-service-hybrid-connections/hybridconn-createhc.png
[5]: ./media/app-service-hybrid-connections/hybridconn-properties.png
[6]: ./media/app-service-hybrid-connections/hybridconn-aspproperties.png
[7]: ./media/app-service-hybrid-connections/hybridconn-hcm.png
[8]: ./media/app-service-hybrid-connections/hybridconn-hcmadd.png
[9]: ./media/app-service-hybrid-connections/hybridconn-hcmadded.png
[10]: ./media/app-service-hybrid-connections/hybridconn-hcmdetails.png
[11]: ./media/app-service-hybrid-connections/hybridconn-manual.png
[12]: ./media/app-service-hybrid-connections/hybridconn-bt.png

<!--Links-->
[HCService]: /azure/service-bus-relay/relay-hybrid-connections-protocol/
[portal]: https://portal.azure.com/
[oldhc]: /azure/biztalk-services/integration-hybrid-connection-overview/
[sbpricing]: https://azure.microsoft.com/pricing/details/service-bus/
[armclient]: https://github.com/projectkudu/ARMClient/