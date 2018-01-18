---
title: "Wdrażanie aplikacji w usłudze Azure App Service przy użyciu FTP/S | Dokumentacja firmy Microsoft"
description: "Dowiedz się, jak wdrożyć aplikację w usłudze Azure App Service przy użyciu FTP i FTPS."
services: app-service
documentationcenter: 
author: cephalin
manager: erikre
editor: 
ms.assetid: ae78b410-1bc0-4d72-8fc4-ac69801247ae
ms.service: app-service
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/06/2016
ms.author: cephalin;dariac
ms.openlocfilehash: fcd079306a8968505349bb3f4a805f203a5c9999
ms.sourcegitcommit: f1c1789f2f2502d683afaf5a2f46cc548c0dea50
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 01/18/2018
---
# <a name="deploy-your-app-to-azure-app-service-using-ftps"></a>Wdrażanie aplikacji w usłudze Azure App Service przy użyciu FTP/S

W tym artykule przedstawiono sposób użycia FTP i FTPS, aby wdrożyć aplikację sieci web, zaplecza aplikacji mobilnej lub aplikacji interfejsu API [usłudze Azure App Service](http://go.microsoft.com/fwlink/?LinkId=529714).

Punkt końcowy FTP/S dla aplikacji jest już aktywna. Konfiguracja nie jest niezbędne do obsługi wdrożenia FTP/S.

<a name="step1"></a>
## <a name="step-1-set-deployment-credentials"></a>Krok 1: Konfigurowanie poświadczeń wdrożenia

Aby uzyskać dostęp do serwera FTP dla aplikacji, najpierw poświadczenia wdrożenia. 

Aby skonfigurować lub zresetować poświadczenia wdrażania, zobacz [poświadczenia wdrożenia usługi aplikacji Azure](app-service-deployment-credentials.md). W tym samouczku przedstawiono na używanie poświadczeń na poziomie użytkownika.

## <a name="step-2-get-ftp-connection-information"></a>Krok 2: Pobieranie informacji o połączeniu FTP

1. W [portalu Azure](https://portal.azure.com), otwórz aplikacji [zasobu strony](../azure-resource-manager/resource-group-portal.md#manage-resources).
2. Wybierz **omówienie** w menu po lewej stronie, a następnie zanotuj wartości **użytkownika serwera FTP/wdrożenia**, **nazwa hosta FTP**, i **nazwy hosta FTPS**. 

    ![Informacje o połączeniu FTP](./media/app-service-deploy-ftp/FTP-Connection-Info.PNG)

    > [!NOTE]
    > Do zapewnienia prawidłowego kontekstu serwera FTP **użytkownika serwera FTP/wdrożenia** wartość wyświetlana w portalu Azure zawiera nazwę aplikacji.
    > Te same informacje można znaleźć po wybraniu **właściwości** w lewym menu. 
    >
    > Ponadto hasła wdrożenia nigdy nie jest wyświetlana. Jeśli użytkownik zapomni hasła wdrożenia, wróć do [krok 1](#step1) i zresetuj hasło wdrożenia.
    >
    >

## <a name="step-3-deploy-files-to-azure"></a>Krok 3: Wdrażanie plików na platformie Azure

1. Z tego klienta FTP (na przykład [programu Visual Studio](https://www.visualstudio.com/vs/community/) lub [FileZilla](https://filezilla-project.org/download.php?type=client)), Użyj zebranych do nawiązania połączenia aplikacji informacje o połączeniu.
3. Kopiowanie plików i ich struktury katalogów odpowiednich do [ **/lokacji/wwwroot** katalogu](https://github.com/projectkudu/kudu/wiki/File-structure-on-azure) na platformie Azure (lub **/lokacji/wwwroot/App_Data/zadania/** katalogu dla zadań Webjob).
4. Przejdź do adresu URL aplikacji, aby sprawdzić, czy aplikacja działa prawidłowo. 

> [!NOTE] 
> W odróżnieniu od [wdrożenia oparte na Git](app-service-deploy-local-git.md), wdrożenie FTP nie obsługuje następujących automatyzacji wdrażania: 
>
> - Przywraca zależności (na przykład automatyzacji NuGet, NPM, PIP i kompozytor)
> - Kompilacja plików binarnych .NET
> - Generowanie pliku Web.config (w tym miejscu jest [przykład Node.js](https://github.com/projectkudu/kudu/wiki/Using-a-custom-web.config-for-Node-apps))
> 
> Generowanie te niezbędne pliki ręcznie na komputerze lokalnym, a następnie wdrożyć je razem z aplikacji.
>
>

## <a name="next-steps"></a>Następne kroki

W przypadku bardziej zaawansowanych scenariuszy wdrażania, spróbuj [wdrażanie na platformie Azure za pomocą narzędzia Git](app-service-deploy-local-git.md). Wdrożenie oparte na Git na platformie Azure umożliwia kontroli wersji, Przywracanie pakietu MSBuild i więcej.

## <a name="more-resources"></a>Więcej zasobów

* [Poświadczenia wdrożenia usługi aplikacji Azure](app-service-deploy-ftp.md)
