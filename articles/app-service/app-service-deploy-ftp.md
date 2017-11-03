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
ms.date: 01/05/2016
ms.author: cephalin;dariac
ms.openlocfilehash: e3ac2f2156719ad975049b0c2b4cbca81d88e779
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/11/2017
---
# <a name="deploy-your-app-to-azure-app-service-using-ftps"></a>Wdrażanie aplikacji w usłudze Azure App Service przy użyciu FTP/S

W tym artykule przedstawiono sposób użycia FTP i FTPS, aby wdrożyć aplikację sieci web, zaplecza aplikacji mobilnej lub aplikacji interfejsu API [usłudze Azure App Service](http://go.microsoft.com/fwlink/?LinkId=529714).

Punkt końcowy FTP/S dla aplikacji jest już aktywna. Konfiguracja nie jest niezbędne do obsługi wdrożenia FTP/S.

> [!IMPORTANT]
> Firma Microsoft stale trwa kroków w celu poprawy zabezpieczeń platformy Microsoft Azure. Jako część tego ciągłego wysiłku regiony Niemcy centralnej i Niemcy północno-wschodnie planowana jest uaktualnienie aplikacji sieci Web. Podczas tego aplikacje sieci Web spowoduje wyłączenie użycia protokołu zwykły tekst FTP dla wdrożeń. Nasze zalecenie, aby klientów jest aby przełączyć się do FTPS wdrożeń. Firma Microsoft nie oczekuje żadnych przerw w działaniu z usługą podczas tego uaktualnienia, którą jest planowana 9/5. Dziękujemy za obsługuje w tym wysiłku.

<a name="step1"></a>
## <a name="step-1-set-deployment-credentials"></a>Krok 1: Konfigurowanie poświadczeń wdrożenia

Aby uzyskać dostęp do serwera FTP dla aplikacji, najpierw poświadczenia wdrożenia. 

Aby skonfigurować lub zresetować poświadczenia wdrażania, zobacz [poświadczenia wdrożenia usługi aplikacji Azure](app-service-deployment-credentials.md). W tym samouczku przedstawiono na używanie poświadczeń na poziomie użytkownika.

## <a name="step-2-get-ftp-connection-information"></a>Krok 2: Pobieranie informacji o połączeniu FTP

1. W [portalu Azure](https://portal.azure.com), otwórz aplikacji [bloku zasobów](../azure-resource-manager/resource-group-portal.md#manage-resources).
2. Wybierz **omówienie** w menu po lewej stronie, a następnie zanotuj wartości **użytkownika serwera FTP/wdrożenia**, **nazwa hosta FTP**, i **nazwy hosta FTPS**. 

    ![Informacje o połączeniu FTP](./media/app-service-deploy-ftp/FTP-Connection-Info.PNG)

    > [!NOTE]
    > **Użytkownika serwera FTP/wdrożenia** użytkownika wartość wyświetlaną w portalu Azure, łącznie z nazwą aplikacji w celu zapewnienia prawidłowego kontekstu dla serwera FTP.
    > Te same informacje można znaleźć po wybraniu **właściwości** w lewym menu. 
    >
    > Ponadto hasła wdrożenia nigdy nie jest wyświetlana. Jeśli użytkownik zapomni hasła wdrożenia, wróć do [krok 1](#step1) i zresetuj hasło wdrożenia.
    >
    >

## <a name="step-3-deploy-files-to-azure"></a>Krok 3: Wdrażanie plików na platformie Azure

1. Z tego klienta FTP ([programu Visual Studio](https://www.visualstudio.com/vs/community/), [FileZilla](https://filezilla-project.org/download.php?type=client)itp), Użyj zebranych do nawiązania połączenia aplikacji informacje o połączeniu.
3. Kopiowanie plików i ich struktury katalogów odpowiednich do [ **/lokacji/wwwroot** katalogu](https://github.com/projectkudu/kudu/wiki/File-structure-on-azure) na platformie Azure (lub **/lokacji/wwwroot/App_Data/zadania/** katalogu dla zadań Webjob).
4. Przejdź do adresu URL aplikacji, aby sprawdzić, czy aplikacja działa prawidłowo. 

> [!NOTE] 
> W odróżnieniu od [wdrożenia oparte na Git](app-service-deploy-local-git.md), wdrożenie FTP nie obsługuje następujących automatyzacji wdrażania: 
>
> - Przywracanie zależności (na przykład automatyzacji NuGet, NPM, PIP i kompozytor)
> - Kompilacja plików binarnych .NET
> - Generowanie pliku Web.config (w tym miejscu jest [przykład Node.js](https://github.com/projectkudu/kudu/wiki/Using-a-custom-web.config-for-Node-apps))
> 
> Należy przywrócić, kompilacji i generuje te niezbędne pliki ręcznie na komputerze lokalnym i wdrażać je razem z aplikacji.
>
>

## <a name="next-steps"></a>Następne kroki

W przypadku bardziej zaawansowanych scenariuszy wdrażania, spróbuj [wdrażanie na platformie Azure za pomocą narzędzia Git](app-service-deploy-local-git.md). Wdrożenie oparte na Git na platformie Azure umożliwia kontroli wersji, Przywracanie pakietu MSBuild i więcej.

## <a name="more-resources"></a>Więcej zasobów

* [Poświadczenia wdrożenia usługi aplikacji Azure](app-service-deploy-ftp.md)
