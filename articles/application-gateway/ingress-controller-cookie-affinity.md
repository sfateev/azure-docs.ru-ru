---
title: Включение сходства на основе файлов cookie с помощью шлюза приложений
description: В этой статье содержатся сведения о том, как включить сходство на основе файлов cookie с шлюзом приложений.
services: application-gateway
author: caya
ms.service: application-gateway
ms.topic: how-to
ms.date: 11/4/2019
ms.author: caya
ms.openlocfilehash: 3af2705fedbb9c19d4f128e8e997d3fa73f8b5a7
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/09/2020
ms.locfileid: "84807974"
---
# <a name="enable-cookie-based-affinity-with-an-application-gateway"></a>Включение сходства на основе файлов cookie с шлюзом приложений
Как описано в [документации по шлюзу приложений Azure](https://docs.microsoft.com/azure/application-gateway/application-gateway-components#http-settings), шлюз приложений поддерживает сопоставление на основе файлов cookie. Это означает, что он может направить последующий трафик из сеанса пользователя на тот же сервер для обработки.

## <a name="example"></a>Пример
```yaml
apiVersion: extensions/v1beta1
kind: Ingress
metadata:
  name: guestbook
  annotations:
    kubernetes.io/ingress.class: azure/application-gateway
    appgw.ingress.kubernetes.io/cookie-based-affinity: "true"
spec:
  rules:
  - http:
      paths:
      - backend:
          serviceName: frontend
          servicePort: 80
```
