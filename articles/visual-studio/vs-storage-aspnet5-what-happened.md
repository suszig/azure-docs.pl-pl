---
title: "Co się stało z projektu programu ASP.NET 5 (Visual Studio połączenia usług) | Dokumentacja firmy Microsoft"
description: "W tym artykule opisano, co się stanie po połączeniu z kontem magazynu platformy Azure w projekcie programu Visual Studio ASP.NET 5 za pomocą programu Visual Studio połączenia usługi"
services: storage
documentationcenter: 
author: kraigb
manager: ghogen
editor: 
ms.assetid: e7caa9fa-c780-45eb-a546-299fc1c68455
ms.service: storage
ms.workload: web
ms.tgt_pltfrm: vs-what-happened
ms.devlang: na
ms.topic: article
ms.date: 12/02/2016
ms.author: kraigb
ms.openlocfilehash: 2a25c24fd7625374d269622a805f386fcd52bb5f
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 08/29/2017
---
# <a name="what-happened-to-my-aspnet-5-project-visual-studio-azure-storage-connected-services"></a>Co się stało z projektu programu ASP.NET 5 (Visual Studio usługi Azure Storage połączenia usług)?
## <a name="references-added"></a>Odwołania dodane
Pakiet NuGet magazynu Azure został dodany do projektu programu Visual Studio.  
Ten pakiet zawiera następujące odwołania .NET:

* **Microsoft.Data.Edm**
* **Microsoft.Data.OData**
* **Microsoft.Data.Services.Client**
* **Microsoft.WindowsAzure.Configuration**
* **Microsoft.WindowsAzure.Storage**
* **Newtonsoft.Json**
* **Dane systemowe**
* **System.Spatial**

Ponadto pakiet NuGet **Microsoft.Framework.Configuration.Json** został dodany.

## <a name="connection-string-for-azure-storage-added"></a>Parametry połączenia dla usługi Azure Storage dodane
W pliku config.json projektu element został utworzony z parametrów połączenia i kluczem konta magazynu wybranego.

Aby uzyskać więcej informacji, zobacz [ASP.NET 5](http://www.asp.net/vnext).

