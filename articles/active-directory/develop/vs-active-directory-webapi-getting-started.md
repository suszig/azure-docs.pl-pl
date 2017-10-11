---
title: "Rozpoczynanie pracy z usługą Azure AD w projektach Visual Studio WebApi | Dokumentacja firmy Microsoft"
description: "Jak rozpocząć pracę przy użyciu usługi Azure Active Directory w projektach WebApi po nawiązywania połączenia lub utworzenie usługi Azure AD przy użyciu programu Visual Studio połączenia usługi"
services: active-directory
documentationcenter: 
author: kraigb
manager: ghogen
editor: 
ms.assetid: bf1eb32d-25cd-4abf-8679-2ead299fedaa
ms.service: active-directory
ms.workload: web
ms.tgt_pltfrm: vs-getting-started
ms.devlang: na
ms.topic: article
ms.date: 03/19/2017
ms.author: kraigb
ms.custom: aaddev
ms.openlocfilehash: a756316054dd3bb63f3b0a9f59621bb1345bc693
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 08/29/2017
---
# <a name="get-started-with-azure-active-directory-and-visual-studio-connected-services-webapi-projects"></a>Get Started with Azure Active Directory i programu Visual Studio połączone usługi (WebApi projekty)
> [!div class="op_single_selector"]
> * [Wprowadzenie](vs-active-directory-webapi-getting-started.md)
> * [Co się stało](vs-active-directory-webapi-what-happened.md)
> 
> 

## <a name="requiring-authentication-to-access-controllers"></a>Wymaganie uwierzytelniania do kontrolerów dostępu
Wszystkie kontrolery w projekcie zostały adorned z **autoryzacji** atrybutu. Ten atrybut wymaga od użytkownika mają być uwierzytelniani przed uzyskaniem dostępu do interfejsów API zdefiniowany przez te kontrolery. Aby umożliwić kontrolerowi można uzyskać dostępu do anonimowo, Usuń ten atrybut z kontrolera. Aby ustawić uprawnienia na poziomie bardziej szczegółowego, należy zastosować atrybut do każdej metody, która wymaga uwierzytelnienia, zamiast stosować go do klasy kontrolera.

## <a name="next-steps"></a>Następne kroki
[Dowiedz się więcej o usłudze Azure Active Directory](https://azure.microsoft.com/services/active-directory/)

