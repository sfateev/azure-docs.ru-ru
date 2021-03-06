---
title: Двойное шифрование инфраструктуры — база данных Azure для MySQL
description: Узнайте, как использовать двойное шифрование инфраструктуры, чтобы добавить второй уровень шифрования с управляемыми службами ключами.
author: kummanish
ms.author: manishku
ms.service: mysql
ms.topic: conceptual
ms.date: 6/30/2020
ms.openlocfilehash: 24ec674c35a4e218c105febf6471ae8427f3c1c3
ms.sourcegitcommit: 7dacbf3b9ae0652931762bd5c8192a1a3989e701
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/16/2020
ms.locfileid: "92125736"
---
# <a name="azure-database-for-mysql-infrastructure-double-encryption"></a>Двойное шифрование инфраструктуры базы данных Azure для MySQL

База данных Azure для MySQL использует [Шифрование неактивных данных в](concepts-security.md#at-rest) хранилище для данных с помощью управляемых ключей Майкрософт. Данные, включая резервные копии, шифруются на диске, и это шифрование всегда включено и не может быть отключено. Шифрование использует проверенный криптографический модуль FIPS 140-2 и шифр AES 256-bit для шифрования службы хранилища Azure.

При двойном шифровании инфраструктуры добавляется второй уровень шифрования с помощью управляемых службой ключей. Он использует проверенный криптографический модуль FIPS 140-2, но с другим алгоритмом шифрования. Это обеспечивает дополнительный уровень защиты для неактивных данных. Ключ, используемый в двойном шифровании инфраструктуры, также управляется службой "база данных Azure для MySQL". Двойное шифрование инфраструктуры не включено по умолчанию, так как дополнительный уровень шифрования может повлиять на производительность.

> [!NOTE]
> Эта функция поддерживается только для ценовых категорий "общего назначения" и "оптимизировано для памяти" в базе данных Azure для PostgreSQL.

При шифровании уровня инфраструктуры преимущество реализуется на уровне, близком к устройству хранения или сетевому беспроводному подключению. База данных Azure для MySQL реализует два уровня шифрования с помощью управляемых службой ключей. Несмотря на то, что технически все еще находится в слое служб, очень близко к оборудованию, в котором хранятся неактивных данные. При необходимости шифрование неактивных данных можно включить с помощью [управляемого клиентом ключа](concepts-data-encryption-mysql.md) для подготовленного сервера MySQL. 

Реализация на уровнях инфраструктуры также поддерживает разнообразие ключей. Инфраструктура должна быть осведомлена о различных кластерах компьютеров и сетей. Таким образом, для снижения радиуса атак с инфраструктурой и различных сбоев оборудования и сети используются разные ключи. 

> [!NOTE]
> Использование двойного шифрования инфраструктуры приведет к снижению производительности сервера базы данных Azure для MySQL из-за дополнительного процесса шифрования.

## <a name="benefits"></a>Преимущества

Двойное шифрование инфраструктуры для базы данных Azure для MySQL предоставляет следующие преимущества.

1. **Дополнительное разнообразие реализаций криптографии** . Запланированное перемещение на аппаратное шифрование будет дополнительно изменить реализации, предоставляя аппаратную реализацию в дополнение к реализации на основе программного обеспечения.
2. **Ошибки реализации** . два уровня шифрования на уровне инфраструктуры защищают от ошибок кэширования или управления памятью на более высоких уровнях, предоставляя данные в виде открытого текста. Кроме того, эти два уровня также гарантируют ошибки в реализации шифрования в целом.

Их сочетание обеспечивает надежную защиту от распространенных угроз и слабых мест, используемых для атаки на криптографию.

## <a name="supported-scenarios-with-infrastructure-double-encryption"></a>Поддерживаемые сценарии с двойным шифрованием инфраструктуры

Возможности шифрования, предоставляемые базой данных Azure для MySQL, можно использовать вместе. Ниже приведена сводка различных сценариев, которые можно использовать.

|  ##   | Шифрование по умолчанию | Двойное шифрование инфраструктуры | Шифрование данных с помощью управляемых клиентом ключей  |
|:------|:------------------:|:--------------------------------:|:--------------------------------------------:|
| 1     | *Да*              | *Нет*                             | *Нет*                                         |
| 2     | *Да*              | *Да*                            | *Нет*                                         |
| 3     | *Да*              | *Нет*                             | *Да*                                        |
| 4     | *Да*              | *Да*                            | *Да*                                        |
|       |                    |                                  |                                              |

> [!Important]
> - Сценарий 2 и 4 могут привести к снижению пропускной способности в 5-10% на основе типа рабочей нагрузки для сервера базы данных Azure для MySQL из-за дополнительного уровня шифрования инфраструктуры.
> - Настройка двойного шифрования инфраструктуры для базы данных Azure для MySQL разрешена только во время создания сервера. После подготовки сервера вы не сможете изменить шифрование хранилища. Тем не менее вы по-прежнему можете включить шифрование данных с помощью управляемых клиентом ключей для сервера, созданного с помощью двойного шифрования/без использования инфраструктуры.

## <a name="limitations"></a>Ограничения

Для базы данных Azure для MySQL Поддержка двойного шифрования инфраструктуры с помощью ключа, управляемого службой, имеет следующие ограничения.

* Поддержка этой функции ограничена **общего назначения** и **оптимизированными для памяти** ценовыми категориями.
* Вы можете создать базу данных Azure для MySQL с включенным шифрованием инфраструктуры в следующих регионах:

   * Восточная часть США
   * Центрально-южная часть США
   * Западная часть США 2
   
* * Эта функция поддерживается только в регионах и серверах, поддерживающих хранилище объемом до 16 ТБ. Список регионов Azure, поддерживающих хранилище объемом до 16 ТБ, см. в [документации по хранилищу](concepts-pricing-tiers.md#storage).

    > [!NOTE]
    > - Все **новые** серверы MySQL, созданные в перечисленных выше регионах, также поддерживают шифрование данных с помощью ключей менеджера клиента. В этом случае серверы, созданные с помощью восстановления на момент времени (PITR) или реплики чтения, не соответствуют "новым".
    > - Чтобы проверить, поддерживает ли подготовленный сервер до 16 ТБ, перейдите в колонку ценовая категория на портале и убедитесь, что ползунок хранилища можно переместить до 16 ТБ. Если вы можете переместить ползунок только до 4 ТБ, сервер может не поддерживать шифрование с помощью управляемых клиентом ключей. Однако данные шифруются с помощью ключей, управляемых службой, в любое время. AskAzureDBforMySQL@service.microsoft.comЕсли у вас возникнут вопросы, свяжитесь с вами.

## <a name="next-steps"></a>Дальнейшие шаги

Узнайте, как [настроить двойное шифрование инфраструктуры для базы данных Azure для MySQL](howto-double-encryption.md).
