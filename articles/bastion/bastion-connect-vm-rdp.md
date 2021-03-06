---
title: Подключение к виртуальной машине Windows с помощью Azure бастиона
description: Из этой статьи вы узнаете, как подключиться к виртуальной машине Azure под управлением Windows с помощью Azure бастиона.
services: bastion
author: cherylmc
ms.service: bastion
ms.topic: how-to
ms.date: 10/12/2020
ms.author: cherylmc
ms.openlocfilehash: 8ffb2d2f52e1bdfece7fe1bdcd04dcf9b1b600f3
ms.sourcegitcommit: a92fbc09b859941ed64128db6ff72b7a7bcec6ab
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/15/2020
ms.locfileid: "92077649"
---
# <a name="connect-to-a-windows-virtual-machine-using-azure-bastion"></a>Подключение к виртуальной машине Windows с помощью Azure бастиона

С помощью Azure бастиона можно безопасно и легко подключаться к виртуальным машинам по протоколу SSL непосредственно в портал Azure. При использовании Azure бастиона виртуальные машины не нуждаются в клиенте, агенте или дополнительном программном обеспечении. В этой статье показано, как подключиться к виртуальным машинам Windows. Сведения о подключении к виртуальной машине Linux см. [в статье подключение к виртуальной машине Linux](bastion-connect-vm-ssh.md).

Azure бастиона обеспечивает безопасное подключение ко всем виртуальным машинам в виртуальной сети, в которых она подготовлена. Бастион Azure позволяет защитить порты RDP и SSH ваших виртуальных машин от внешних проникновений, обеспечивая при этом безопасный доступ с использованием этих протоколов. Дополнительные сведения см. в статье [что такое Azure бастиона?](bastion-overview.md).

## <a name="prerequisites"></a>Предварительные требования

Прежде чем начать, убедитесь, что выполнены следующие условия.

* Виртуальная сеть с узлом бастиона уже установлена.

   Убедитесь, что вы настроили узел бастиона Azure для виртуальной сети, в которой находится виртуальная машина. После подготовки и развертывания службы бастиона в виртуальной сети ее можно использовать для подключения к любой виртуальной машине в виртуальной сети. Чтобы настроить узел бастиона для Azure, см. раздел [Создание узла бастиона](tutorial-create-host-portal.md#createhost).
* Виртуальная машина Windows в виртуальной сети.
* Следующие необходимые роли:
  * Роль читателя на виртуальной машине.
  * Роль читателя на сетевом адаптере с частным IP-адресом виртуальной машины.
  * Роль читателя в ресурсе Azure бастиона.
* Порты. для подключения к виртуальной машине Windows на виртуальной машине Windows должны быть открыты следующие порты:
  * Входящие порты: RDP (3389)

## <a name="connect"></a><a name="rdp"></a>Подключение

[!INCLUDE [Connect to a Windows VM](../../includes/bastion-vm-rdp.md)]
 
## <a name="next-steps"></a>Дальнейшие действия

Ознакомьтесь с [часто задаваемыми вопросами о Бастионе Azure](bastion-faq.md).