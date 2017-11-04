---
title: "Co się stało mój projekt usługi w chmurze? | Microsoft Docs"
description: "W tym artykule opisano, co się stanie w projekcie usługi w chmurze po łączenie się z kontem magazynu platformy Azure przy użyciu programu Visual Studio połączenia usługi"
services: storage
documentationcenter: 
author: kraigb
manager: ghogen
editor: 
ms.assetid: ca0ea68d-f417-4ce8-9413-40d76f69cdea
ms.service: storage
ms.workload: web
ms.tgt_pltfrm: vs-what-happened
ms.devlang: na
ms.topic: article
ms.date: 12/02/2016
ms.author: kraigb
ms.openlocfilehash: 4c9de56f6daf07097c0f593db37d14dce3bce05f
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/11/2017
---
# <a name="what-happened-to-my-cloud-services-project-visual-studio-azure-storage-connected-service"></a>Co się stało z projektu usługi w chmurze (usługi magazynu Azure w usłudze Visual Studio połączony)?
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

## <a name="connection-string-for-azure-storage-added"></a>Parametry połączenia dla usługi Azure Storage dodane
Elementy zostały utworzone za pomocą wybranego konta magazynu przez ciąg połączenia i klucza. Następujące pliki zostały wprowadzone zmiany:

* **ServiceDefinition.csdef**
* **ServiceConfiguration.Cloud.cscfg**
* **ServiceConfiguration.Local.cscfg**

