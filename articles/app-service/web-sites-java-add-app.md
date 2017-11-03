---
title: "Dodawanie aplikacji Java do aplikacji sieci Web usługi aplikacji Azure"
description: "W tym samouczku przedstawiono sposób dodawania strony lub aplikacji do Twojego wystąpienia usługi aplikacje sieci Web usługi aplikacji Azure, która jest już skonfigurowana do używania języka Java."
services: app-service\web
documentationcenter: java
author: rmcmurray
manager: erikre
editor: 
ms.assetid: 9b46528b-e2d0-4f26-b8d7-af94bd8c31ef
ms.service: app-service-web
ms.workload: web
ms.tgt_pltfrm: na
ms.devlang: Java
ms.topic: article
ms.date: 04/25/2017
ms.author: robmcm
ms.openlocfilehash: 1309985d7f1b93230b38ada2ee2687b1db10a791
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/11/2017
---
# <a name="add-a-java-application-to-azure-app-service-web-apps"></a>Dodawanie aplikacji Java do aplikacji sieci Web usługi aplikacji Azure
Po już zainicjować aplikacji sieci web Java w [usłudze Azure App Service] [ Azure App Service] zgodnie z opisem w [tworzenie aplikacji sieci web Java w usłudze Azure App Service](app-service-web-get-started-java.md), możesz przekazać aplikacji przez umieszczenie Twojej WAR w **webapps** folderu.

Ścieżka nawigacji do **webapps** folderu różni się w zależności od konfiguracji wystąpienia aplikacji sieci Web.

* Po skonfigurowaniu aplikacji sieci web przy użyciu portalu Azure Marketplace, ścieżka do **webapps** folder znajduje się w formularzu **d:\home\site\wwwroot\bin\application\_server\webapps**, gdzie **aplikacji\_serwera** jest nazwą serwera aplikacji dotyczące wystąpienia aplikacji sieci Web. 
* Po skonfigurowaniu aplikacji sieci web przy użyciu interfejsu użytkownika, ścieżka do konfiguracji platformy Azure **webapps** folder znajduje się w formularzu **d:\home\site\wwwroot\webapps**. 

Należy pamiętać, że można użyć do kontroli źródła do przekazania aplikacji lub strony sieci web, w tym [scenariuszy integracji ciągłej](app-service-continuous-deployment.md). FTP jest również opcja przekazywania aplikacji lub strony sieci web; Aby uzyskać więcej informacji na temat wdrażania aplikacji za pośrednictwem FTP, zobacz [wdrażanie aplikacji przy użyciu protokołu FTP](app-service-deploy-ftp.md).

Uwaga dla aplikacji sieci web Tomcat: po przesłaniu pliku WAR do **webapps** folderu, serwera aplikacji Tomcat wykryje, że został dodany i załaduje go automatycznie. Należy zauważyć, że po skopiowaniu plików (innych niż pliki WAR) do katalogu głównego serwera aplikacji będzie należy ponownie uruchomić te pliki są używane. Funkcjonalność automatyczne ładowanie aplikacji sieci web Tomcat Java działających na platformie Azure jest oparta na plik WAR dodawany lub nowe pliki lub katalogi są dodawane do **webapps** folderu. 

<a name="see-also"></a>

## <a name="see-also"></a>Zobacz też
Aby uzyskać więcej informacji o używaniu platformy Azure z językiem Java, zobacz [Azure Java Developer Center].

[Application-insights-App-insights-Java-Get-Started](../application-insights/app-insights-java-get-started.md)

<!-- URL List -->

[Azure Java Developer Center]: https://azure.microsoft.com/develop/java/
[Azure App Service]: http://go.microsoft.com/fwlink/?LinkId=529714
