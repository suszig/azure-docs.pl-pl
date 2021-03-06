---
title: "Synchronizowanie zawartości z folderu chmury do usługi Azure App Service"
description: "Dowiedz się, jak wdrożyć aplikację w usłudze Azure App Service za pośrednictwem synchronizacji zawartości z folderu chmury."
services: app-service
documentationcenter: 
author: dariagrigoriu
manager: erikre
editor: mollybos
ms.assetid: 88d3a670-303a-4fa2-9de9-715cc904acec
ms.service: app-service
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/14/2016
ms.author: dariagrigoriu
ms.openlocfilehash: 8e132e4d4a65588d57e3cfb969e785f5a164206c
ms.sourcegitcommit: f1c1789f2f2502d683afaf5a2f46cc548c0dea50
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 01/18/2018
---
# <a name="sync-content-from-a-cloud-folder-to-azure-app-service"></a>Synchronizowanie zawartości z folderu chmury do usługi Azure App Service
Ten samouczek pokazuje, jak można zsynchronizować zawartość do [usłudze Azure App Service](http://go.microsoft.com/fwlink/?LinkId=529714) z usługi magazynu w chmurze popularnych jak Dropbox i OneDrive. 

## <a name="overview"></a>Omówienie wdrażania zawartości synchronizacji
Wdrażanie synchronizacji zawartości na żądanie jest obsługiwany przez [aparat wdrażania Kudu](https://github.com/projectkudu/kudu/wiki) zintegrowany z usługi aplikacji. W [portalu Azure](https://portal.azure.com), można wyznaczyć folder w magazynie w chmurze, pracy z kodu aplikacji, a zawartość w tym folderze oraz synchronizacji w usłudze App Service z kliknięcie przycisku. Zawartości synchronizacji wykorzystuje Kudu proces kompilacji i wdrożenia. 

## <a name="contentsync"></a>Jak włączyć wdrożenie zawartości synchronizacji
Aby włączyć zawartości synchronizacji [portalu Azure](https://portal.azure.com), wykonaj następujące kroki:

1. Na stronie aplikacji w portalu Azure kliknij **ustawienia** > **źródło wdrożenia**. Kliknij przycisk **wybierz źródło**, a następnie wybierz pozycję **OneDrive** lub **Dropbox** jako źródło dla wdrożenia. 
   
    ![Synchronizacja zawartości](./media/app-service-deploy-content-sync/deployment_source.png)
   
   > [!NOTE]
   > Z powodu podstawowych różnic w interfejsach API **OneDrive dla firm** nie jest obsługiwana w tej chwili. 
   > 
   > 
2. Zakończenie przepływu pracy autoryzacji, aby włączyć usługę aplikacji, aby uzyskiwać dostęp do określonej ścieżki wyznaczonych wstępnie zdefiniowane dla usługi OneDrive lub Dropbox, w którym wszystkie usługi aplikacji zawartości są przechowywane. Po autoryzacji platformy aplikacji usługi z opcją Utwórz folder zawartości w określonej ścieżce zawartości lub wybierz istniejący folder zawartości w obszarze tej określonej ścieżce zawartości. Wyznaczone ścieżek zawartości w ramach kont magazynu chmurze używane do synchronizacji usługi App Service są następujące:  
   
   * **OneDrive**:`Apps\Azure Web Apps` 
   * **Dropbox**:`Dropbox\Apps\Azure`
3. Po początkowej synchronizacji zawartości można zainicjować synchronizacji zawartości na żądanie z portalu Azure. Historia wdrożenia jest dostępna na **wdrożeń** strony.
   
    ![Historia wdrażania](./media/app-service-deploy-content-sync/onedrive_sync.png)

Więcej informacji do wdrożenia usługi Dropbox znajduje się w obszarze [Wdróż z Dropbox] (https://azure.microsoft.com/en-in/blog/new-deploy-to-windows-azure-web-sites-from-dropbox/).

