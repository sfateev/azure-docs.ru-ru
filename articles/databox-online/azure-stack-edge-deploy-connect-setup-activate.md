---
title: Учебник. Подключение, настройка и активация устройства Azure Stack Edge Pro на портале Azure | Документация Майкрософт
description: В учебнике по развертыванию Azure Stack Edge Pro описывается, как подключить, настроить и активировать физическое устройство.
services: databox
author: alkohli
ms.service: databox
ms.subservice: edge
ms.topic: tutorial
ms.date: 03/28/2019
ms.author: alkohli
Customer intent: As an IT admin, I need to understand how to connect and activate Azure Stack Edge Pro so I can use it to transfer data to Azure.
ms.openlocfilehash: 8a143dadffb3f89ef67dc20a2038bb3c9bf5a0e4
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/09/2020
ms.locfileid: "91743341"
---
# <a name="tutorial-connect-set-up-and-activate-azure-stack-edge-pro"></a>Руководство по Подключение, настройка и активация Azure Stack Edge Pro 

В этом учебнике описывается, как подключить, настроить и активировать устройство Azure Stack Edge Pro с помощью локального пользовательского веб-интерфейса.

Процесс установки и активации может занять около 20 минут.

В этом руководстве описано следующее:

> [!div class="checklist"]
>
> * подключение к физическому устройству;
> * Настройка и активация физического устройства

## <a name="prerequisites"></a>Предварительные требования

Перед настройкой устройства Azure Stack Edge Pro проверьте следующее:

* Вы установили физическое устройство, как описано в статье [Установка Azure Stack Edge Pro](azure-stack-edge-deploy-install.md).
* У вас есть ключ активации из службы Azure Stack Edge, который вы создали для управления устройством Azure Stack Edge Pro. Дополнительные сведения см. в статье [Подготовка к развертыванию Azure Stack Edge Pro](azure-stack-edge-deploy-prep.md).

## <a name="connect-to-the-local-web-ui-setup"></a>Подключение к настройке локального пользовательского веб-интерфейса

1. Настройте адаптер Ethernet на компьютере, который используется для подключения к устройству Azure Stack Edge Pro, указав статический IP-адрес 192.168.100.5 и подсеть 255.255.255.0.

2. Подключите компьютер к порту 1 на вашем устройстве. С помощью иллюстрации ниже определите порт 1 устройства.

    ![Задняя панель подключенного устройства](./media/azure-stack-edge-deploy-install/backplane-cabled.png)

3. Откройте окно браузера и перейдите к локальному пользовательскому веб-интерфейсу на устройстве по адресу `https://192.168.100.10`.  
    Это действие может занять несколько минут после включения устройства.

    Появится сообщение об ошибке или предупреждение, уведомляющее о проблеме с сертификатом безопасности веб-сайта.
   
    ![Сообщение об ошибке сертификата безопасности веб-сайта](./media/azure-stack-edge-deploy-connect-setup-activate/image2.png)

4. Щелкните **Continue to this webpage** (Перейти на эту веб-страницу).  
    Эти шаги могут отличаться в зависимости от браузера, который вы используете.

5. Войдите в пользовательский веб-интерфейс устройства. Пароль по умолчанию — *Password1*. 
   
    ![Страница входа в устройство Azure Stack Edge Pro](./media/azure-stack-edge-deploy-connect-setup-activate/image3.png)

6. При появлении подсказки измените пароль администратора устройства.  
    Новый пароль должен состоять из 8–16 знаков. Пароль должен содержать три знака из следующих категорий: прописные буквы, строчные буквы, цифры и специальные знаки.

Теперь вы находитесь на панели мониторинга устройства.

## <a name="set-up-and-activate-the-physical-device"></a>Настройка и активация физического устройства
 
На панели мониторинга отображаются различные параметры, необходимые для настройки и регистрации физического устройства с помощью службы Azure Stack Edge. Настройки в разделах **Имя устройства**, **Параметры сети**, **Параметры веб-прокси** и **Параметры времени** являются необязательными. Обязательны только настройки в разделе **Параметры облака**.
   
![Страница "Панель мониторинга" в локальном пользовательском веб-интерфейсе](./media/azure-stack-edge-deploy-connect-setup-activate/set-up-activate-1.png)

1. В левой области выберите **Имя устройства**, а затем введите понятное имя устройства.  
    Понятное имя должно включать от 1 до 15 символов и содержать буквы, цифры и дефисы.

    ![Страница "Имя устройства" в локальном пользовательском веб-интерфейсе](./media/azure-stack-edge-deploy-connect-setup-activate/set-up-activate-2.png)

2. (Необязательно.) В левой области выберите **Параметры сети**, а затем настройте параметры.  
    На физическом устройстве доступно шесть сетевых интерфейсов. Порты 1 и 2 — это сетевые интерфейсы со скоростью 1 Гбит/с. Порты 3, 4, 5 и 6 — это сетевые интерфейсы со скоростью 25 Гбит/с, которые также могут работать со скоростью 10 Гбит/с. Порт 1 автоматически настроен только для управления, тогда как порты 2–6 являются портами данных. Страница **Параметры сети** показана ниже.
    
    ![Страница "Параметры сети" в локальном веб-интерфейсе](./media/azure-stack-edge-deploy-connect-setup-activate/set-up-activate-3.png)
   
    При настройке параметров сети учитывайте следующее.

   - Если в среде включен DHCP, сетевые интерфейсы настраиваются автоматически. IP-адрес, подсеть, шлюз и DNS назначаются автоматически.
   - Если протокол DHCP не включен, при необходимости вы можете назначить статические IP-адреса.
   - Сетевой интерфейс можно настроить как IPv4.

     >[!NOTE] 
     > Мы советуем не переключать локальный IP-адрес сетевого интерфейса со статического на DCHP при отсутствии другого IP-адреса для подключения к устройству. Если, используя один сетевой интерфейс, вы переключитесь на DHCP, вы не сможете определить адрес DHCP. Если вы хотите переключиться на адрес DHCP, дождитесь, пока устройство зарегистрируется в службе, а затем измените адрес. Затем вы сможете просмотреть IP-адреса всех адаптеров в **свойствах устройства** на портале Azure для службы.

3. (Необязательно.) В левой области выберите **Параметры веб-прокси**, а затем настройте сервер веб-прокси. Хотя конфигурация веб-прокси не является обязательной, если вы используете веб-прокси, вы можете настроить его только на этой странице.
   
   ![Страница "Параметры веб-прокси" в локальном пользовательском веб-интерфейсе](./media/azure-stack-edge-deploy-connect-setup-activate/set-up-activate-4.png)
   
   На странице **Параметры веб-прокси** выполните следующие действия.
   
   а. В поле **URL-адрес веб-прокси** введите URL-адрес в следующем формате: `http://host-IP address or FQDN:Port number`. URL-адреса HTTPS не поддерживаются.

   b. В поле **Проверка подлинности** выберите **Нет** или **NTLM**. Если вы включили вычисления и используете модуль IoT Edge на устройстве Azure Stack Edge Pro, мы рекомендуем задать для аутентификации веб-прокси значение **Нет**. **NTLM** не поддерживается.

   c. При прохождении аутентификации введите имя пользователя и пароль.

   d. Чтобы проверить и применить настроенные параметры веб-прокси, выберите **Применить параметры**.

   > [!NOTE]
   > Файлы автоматической настройки прокси-сервера (PAC-файлы) не поддерживаются. PAC-файл определяет, как веб-браузеры и другие агенты пользователей могут автоматически выбрать соответствующий прокси-сервер (метод доступа) для получения заданного URL-адреса.
   > Прокси-серверы, пытающиеся перехватить и прочесть весь трафик (а затем повторно подписать все собственным сертификатом) несовместимы, так как сертификат прокси-сервера не является доверенным.
   > Обычно прозрачные прокси-серверы без проблем работают с Azure Stack Edge Pro.

4. (Необязательно.) В левой области выберите **Параметры времени**, а затем настройте часовой пояс, а также основной и дополнительный NTP-серверы для своего устройства.  
    NTP-серверы необходимы, так как устройство должно синхронизировать время, чтобы проходить проверку подлинности в облачных службах.
       
    На странице **Параметры времени** выполните следующие действия.
    
    1. В раскрывающемся списке выберите **часовой пояс**, соответствующий географическому расположению, в котором будет развернуто устройство.
        Часовой пояс по умолчанию для устройства — тихоокеанское стандартное время (PST). Устройство будет использовать заданный часовой пояс для всех запланированных операций.

    2. В поле **Основной NTP-сервер** введите основной сервер устройства или примите значение по умолчанию time.windows.com.  
        Убедитесь, что ваша сеть позволяет передавать NTP-трафик из вашего центра обработки данных в Интернет.

    3. При необходимости в поле **Вторичный NTP-сервер** введите сервер-получатель устройства.

    4. Чтобы проверить и применить настроенные параметры времени, выберите **Применить параметры**.

        ![Страница "Параметры времени" в локальном пользовательском веб-интерфейсе](./media/azure-stack-edge-deploy-connect-setup-activate/set-up-activate-5.png)

5. (Необязательно) На панели слева выберите **Параметры хранилища**, чтобы настроить устойчивость хранилища на вашем устройстве. Эта функция в настоящее время находится на стадии предварительной версии. По умолчанию хранилище на устройстве не обеспечивает устойчивость, и при сбое диска на устройстве данные будут потеряны. Если вы включите параметр "Устойчивость", хранилище на устройстве будет перенастроено, а устройство сможет выдержать сбой одного диска без потери данных. Если хранилище будет настроено как устойчивое, объем полезной емкости устройства уменьшится.

    > [!IMPORTANT] 
    > Устойчивость можно настроить только до активации устройства. 

    ![Страница "Параметры хранилища" в локальном пользовательском веб-интерфейсе](./media/azure-stack-edge-deploy-connect-setup-activate/storage-settings.png)

6. В левой области выберите **Настройки облака**, а затем активируйте свое устройство с помощью службы Azure Stack Edge на портале Azure.
    
    1. В поле **Ключ активации** введите ключ, который вы получили при работе с разделом [Получение ключа активации](azure-stack-edge-deploy-prep.md#get-the-activation-key) для Azure Stack Edge Pro.
    2. Нажмите кнопку **Применить**.
       
        ![Страница "Настройки облака" в локальном пользовательском веб-интерфейсе](./media/azure-stack-edge-deploy-connect-setup-activate/set-up-activate-6.png)

    3. Сначала устройство активируется. Затем выполняется проверка на наличие критических обновлений, которые устанавливаются автоматически при их доступности. При этом вы получите соответствующее уведомление.

        В диалоговом окне также будет отображаться ключ восстановления, который вам нужно скопировать и сохранить в безопасном расположении. Этот ключ используется для восстановления данных в случае невозможности загрузить устройство.

        ![Обновленная страница "Настройки облака" в локальном пользовательском веб-интерфейсе](./media/azure-stack-edge-deploy-connect-setup-activate/set-up-activate-7.png)

    4. После завершения обновления вам, возможно, придется подождать несколько минут. Страница будет обновлена с указанием того, что устройство успешно активировано.

        ![Обновленная страница "Настройки облака" в локальном пользовательском веб-интерфейсе (2)](./media/azure-stack-edge-deploy-connect-setup-activate/set-up-activate-8.png)

Настройка устройства завершена. Теперь в него можно добавить общие папки.

## <a name="next-steps"></a>Дальнейшие действия

В этом руководстве вы узнали, как выполнять следующие задачи:

> [!div class="checklist"]
> * подключение к физическому устройству;
> * Настройка и активация физического устройства

Чтобы узнать, как передавать данные с помощью устройства Azure Stack Edge Pro, см. следующее руководство:

> [!div class="nextstepaction"]
> [Передача данных с помощью Azure Stack Edge Pro](./azure-stack-edge-deploy-add-shares.md).
