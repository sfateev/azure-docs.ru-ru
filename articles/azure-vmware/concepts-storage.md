---
title: Основные понятия — хранилище
description: Сведения о возможностях хранилища ключей в частных облаках решений VMware для Azure.
ms.topic: conceptual
ms.date: 05/04/2020
ms.openlocfilehash: 02378040061080d3c9abbfafb26180c9d22e9073
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/09/2020
ms.locfileid: "91316823"
---
#  <a name="azure-vmware-solution-storage-concepts"></a>Основные понятия хранилища решений VMware для Azure

Частные облака решения Azure VMware обеспечивают собственное хранилище на уровне кластера с помощью VMware vSAN. Все локальное хранилище из каждого узла в кластере используется в хранилище данных vSAN, а шифрование с неактивных данными доступно и включено по умолчанию. Ресурсы службы хранилища Azure можно использовать для расширения возможностей хранения частных облаков.

## <a name="vsan-clusters"></a>кластеры vSAN

Локальное хранилище на каждом узле кластера используется как часть хранилища данных vSAN. Все дискграупс используют уровень кэша NVMe 1,6 ТБ с необработанными данными на каждом узле, емкостью на основе SSD — 15,4 ТБ. Размер необработанного уровня емкости кластера равен количеству узлов на уровне каждого узла. Например, четыре кластера узлов обеспечивают объем необработанных ресурсов 61,6 ТБ на уровне емкости vSAN.

Локальное хранилище на узлах кластера используется в хранилище данных vSAN на уровне кластера. Все хранилища данных создаются как часть развертывания частного облака и доступны для немедленного использования. Пользователь cloudadmin и все пользователи в группе CloudAdmin могут управлять хранилищами данных с этими привилегиями vSAN:
- Datastore.AllocateSpace
- Datastore.Browse
- Datastore.Config
- Хранилище данных. DeleteFile
- Хранилище данных. FileManagement
- Хранилище данных. Упдатевиртуалмачинеметадата

## <a name="data-at-rest-encryption"></a>Шифрование неактивных данных

хранилища данных vSAN по умолчанию используют шифрование с неактивных данными. Решение для шифрования основано на службе KMS и поддерживает операции vCenter для управления ключами. Ключи хранятся в зашифрованном виде, заключенном в оболочку Azure Key Vault главного ключа на основе HSM. Когда узел удаляется из кластера по какой-либо причине, данные в SSDs становятся недействительными немедленно.

## <a name="scaling"></a>Масштабирование

Емкость хранилища собственных кластеров масштабируется путем добавления узлов в кластер. Для кластеров, использующих этот узел, емкость необработанного кластера увеличивается на 15,4 ТБ с каждым дополнительным узлом. Объем необработанных кластеров, построенных с помощью узлов GP, увеличился на 7,7 ТБ с каждым дополнительным узлом. В обоих типах кластеров узлы в кластере занимает около 10 минут. Инструкции по масштабированию кластеров см. в [руководстве по масштабированию частного облака][tutorial-scale-private-cloud] .

## <a name="azure-storage-integration"></a>Интеграция службы хранилища Azure

Службы хранилища Azure можно использовать на рабочих нагрузках, работающих в частном облаке. Службы хранилища Azure включают учетные записи хранения, хранилище таблиц и хранилище BLOB-объектов. Подключение рабочих нагрузок к службам хранилища Azure не проходит через Интернет. Такое подключение обеспечивает дополнительную безопасность и позволяет использовать службы хранилища Azure на основе соглашения об уровне обслуживания в рабочих нагрузках частного облака.

## <a name="next-steps"></a>Дальнейшие шаги

Следующим шагом является изучение [концепций удостоверений частного облака][concepts-identity].

<!-- LINKS - external-->

<!-- LINKS - internal -->
[tutorial-scale-private-cloud]: ./tutorial-scale-private-cloud.md
[concepts-identity]: ./concepts-identity.md
