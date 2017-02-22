---
title: "Wdrażanie pierwszej aplikacji sieci Web w języku Java na platformie Azure w ciągu pięciu minut (interfejs wiersza polecenia 2.0 w wersji zapoznawczej) | Dokumentacja firmy Microsoft"
description: "Dowiedz się, jak łatwo można uruchamiać aplikacje sieci Web w usłudze App Service, wdrażając przykładową aplikację. Szybko rozpocznij rzeczywiste tworzenie aplikacji i od razu zobacz wyniki."
services: app-service\web
documentationcenter: 
author: cephalin
manager: wpickett
editor: 
ms.assetid: 8bacfe3e-7f0b-4394-959a-a88618cb31e1
ms.service: app-service-web
ms.workload: web
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: hero-article
ms.date: 01/04/2017
ms.author: cephalin
translationtype: Human Translation
ms.sourcegitcommit: b1a633a86bd1b5997d5cbf66b16ec351f1043901
ms.openlocfilehash: af27369b0ae8de0ece6da38a78b434e595fbfc4e


---
# <a name="deploy-your-first-java-web-app-to-azure-in-five-minutes-cli-20-preview"></a>Wdrażanie pierwszej aplikacji sieci Web w języku Java na platformie Azure w ciągu pięciu minut (interfejs wiersza polecenia 2.0 w wersji zapoznawczej)

> [!div class="op_single_selector"]
> * [Pierwsza witryna HTML](app-service-web-get-started-html-cli-nodejs.md)
> * [Pierwsza aplikacja platformy .NET](app-service-web-get-started-dotnet-cli-nodejs.md)
> * [Pierwsza aplikacja PHP](app-service-web-get-started-php-cli-nodejs.md)
> * [Pierwsza aplikacja Node.js](app-service-web-get-started-nodejs-cli-nodejs.md)
> * [Pierwsza aplikacja w języku Python](app-service-web-get-started-python-cli-nodejs.md)
> * [Pierwsza aplikacja w języku Java](app-service-web-get-started-java.md)
> 
> 

Ten samouczek ułatwia wdrażanie prostej aplikacji sieci Web Java w [usłudze Azure App Service](../app-service/app-service-value-prop-what-is.md).
Za pomocą usługi App Service można tworzyć aplikacje sieci Web, [zaplecza aplikacji mobilnych](/documentation/learning-paths/appservice-mobileapps/) oraz [aplikacje interfejsu API](../app-service-api/app-service-api-apps-why-best-platform.md).

Wykonasz następujące zadania: 

* utworzysz aplikację sieci Web w usłudze Azure App Service;
* wdrożysz przykładową aplikację Java,
* sprawdzisz działanie kodu w środowisku produkcyjnym;

## <a name="prerequisites"></a>Wymagania wstępne
* Pobierz klienta FTP/FTPS, np. program [FileZilla](https://filezilla-project.org/).
* Utwórz konto platformy Microsoft Azure. Jeśli nie masz konta, możesz [utworzyć konto bezpłatnej wersji próbnej](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A261C142F) lub [aktywować korzyści dla subskrybentów programu Visual Studio](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/?WT.mc_id=A261C142F).

> [!NOTE]
> Usługę [App Service](https://azure.microsoft.com/try/app-service/) możesz wypróbować, nie mając konta platformy Azure. Utwórz aplikację startową i testuj ją nawet przez godzinę — bez kart kredytowych i bez zobowiązań.
> 
> 

<a name="create"></a>

## <a name="create-a-web-app"></a>Tworzenie aplikacji sieci Web
1. Zaloguj się do [witryny Azure Portal](https://portal.azure.com) przy użyciu danych konta Azure.
2. W lewym menu kliknij kolejno pozycje **Nowy** > **Sieci Web i mobilność** > **Aplikacja sieci Web**.
   
    ![](./media/app-service-web-get-started-languages/create-web-app-portal.png)
3. W bloku tworzenia aplikacji użyj następujących ustawień dotyczących nowej aplikacji:
   
   * **Nazwa aplikacji**: wpisz unikatową nazwę.
   * **Grupa zasobów**: wybierz pozycję **Utwórz nową** i nadaj nazwę grupie zasobów.
   * **Plan/lokalizacja usługi App Service**: kliknij tę pozycję, aby skonfigurować, a następnie kliknij pozycję **Utwórz nowe**, aby ustawić nową nazwę, lokalizację i warstwę cenową planu usługi App Service. Możesz skorzystać z warstwy cenowej **Bezpłatna**.
     
     Po wprowadzeniu ustawień blok tworzenia aplikacji powinien wyglądać następująco:
     
     ![](./media/app-service-web-get-started-languages/create-web-app-settings.png)
4. Kliknij pozycję **Utwórz** na dole. Po kliknięciu ikony **Powiadomienie** u góry można zobaczyć postęp.
   
    ![](./media/app-service-web-get-started-languages/create-web-app-started.png)
5. Po zakończeniu wdrożenia powinien pojawić się taki komunikat powiadomienia. Kliknij ten komunikat, aby otworzyć blok wdrożenia.
   
    ![](./media/app-service-web-get-started-languages/create-web-app-finished.png)
6. W bloku **Wdrożenie powiodło się** kliknij link **Zasób**, aby otworzyć blok nowej aplikacji sieci Web.
   
    ![](./media/app-service-web-get-started-languages/create-web-app-resource.png)

## <a name="deploy-a-java-app-to-your-web-app"></a>Wdrażanie aplikacji Java w aplikacji sieci Web
Wdrożymy teraz aplikację Java na platformie Azure przy użyciu protokołu FTPS.

1. W bloku aplikacji sieci Web przewiń w dół do pozycji **Ustawienia aplikacji** lub ją wyszukaj, a następnie kliknij tę pozycję. 
   
    ![](./media/app-service-web-get-started-languages/set-java-application-settings.png)
2. W obszarze **Wersja języka Java** wybierz pozycję **Java 8** i kliknij przycisk **Zapisz**.
   
    ![](./media/app-service-web-get-started-languages/set-java.png)
   
    Po wyświetleniu powiadomienia **Pomyślnie zaktualizowano ustawienia aplikacji sieci Web** przejdź do strony http://*&lt;nazwa_aplikacji>*.azurewebsites.net, aby zobaczyć domyślny serwlet JSP w działaniu.
3. Wróć do bloku aplikacji sieci Web, przewiń w dół do pozycji **Poświadczenia wdrożenia** lub ją wyszukaj, a następnie kliknij tę pozycję.
4. Ustaw poświadczenia wdrożenia i kliknij przycisk **Zapisz**.
5. Wróć do bloku aplikacji sieci Web i kliknij pozycję **Przegląd**. Obok pozycji **Nazwa użytkownika serwera FTP/wdrożenia** i **Nazwa hosta FTPS** kliknij przycisk **Kopiuj**, aby skopiować te wartości.
   
    ![](./media/app-service-web-get-started-languages/get-ftp-url.png)
   
    Teraz można przystąpić do wdrażania aplikacji Java przy użyciu protokołu FTPS.
6. W kliencie FTP/FTPS zaloguj się do serwera FTP swojej aplikacji sieci Web na platformie Azure, używając wartości skopiowanych w ostatnim kroku. Użyj utworzonego wcześniej hasła wdrożenia.
   
    Poniższy zrzut ekranu przedstawia logowanie się przy użyciu programu FileZilla.
   
    ![](./media/app-service-web-get-started-languages/filezilla-login.png)
   
    W przypadku nierozpoznanego certyfikatu SSL od platformy Azure mogą zostać wyświetlone ostrzeżenia o zabezpieczeniach. Kontynuuj.
7. Kliknij [ten link](https://github.com/Azure-Samples/app-service-web-java-get-started/raw/master/webapps/ROOT.war), aby pobrać plik WAR na komputer lokalny.
8. W kliencie FTP/FTPS przejdź do katalogu **/site/wwwroot/webapps** w witrynie zdalnej i przeciągnij pobrany plik WAR do tego katalogu zdalnego na komputerze lokalnym.
   
    ![](./media/app-service-web-get-started-languages/transfer-war-file.png)
   
    Kliknij przycisk **OK**, aby zastąpić ten plik na platformie Azure.
   
   > [!NOTE]
   > Zgodnie z domyślnym zachowaniem kontenera Tomcat nazwa pliku **ROOT.war** w katalogu /site/wwwroot/webapps daje w rezultacie aplikację sieci Web w katalogu głównym (http://*&lt;nazwa_aplikacji>*.azurewebsites.net), a nazwa pliku ***&lt;dowolna_nazwa>*.war** daje w wyniku nazwaną aplikację sieci Web (http://*&lt;nazwa_aplikacji>*.azurewebsites.net/*&lt;dowolna_nazwa>*).
   > 
   > 

Gotowe. Od teraz aplikacja języka Java działa na platformie Azure. Aby sprawdzić jego działanie, w przeglądarce przejdź do adresu http://*&lt;nazwa_aplikacji>*.azurewebsites.net. 

## <a name="make-updates-to-your-app"></a>Aktualizowanie aplikacji
Zawsze, gdy trzeba dokonać aktualizacji, wystarczy przekazać nowy plik WAR do tego samego katalogu zdalnego za pomocą klienta FTP/FTPS.

## <a name="next-steps"></a>Następne kroki
[Tworzenie aplikacji sieci Web Java na podstawie szablonu w portalu Azure Marketplace](web-sites-java-get-started.md#marketplace). Możesz pobrać kontener Tomcat z pełnymi możliwościami dostosowywania i uzyskać znajomy interfejs użytkownika menedżera. 

Przeprowadź debugowanie swojej aplikacji sieci Web na platformie Azure bezpośrednio w środowisku [IntelliJ](app-service-web-debug-java-web-app-in-intellij.md) lub [Eclipse](app-service-web-debug-java-web-app-in-eclipse.md).

Możesz także wykonać inne zadania związane ze swoją pierwszą aplikacją sieci Web . Na przykład:

* Wypróbuj [inne sposoby wdrożenia kodu na platformie Azure](web-sites-deploy.md). 
* Przenieś swoją aplikację Azure na wyższy poziom. Uwierzytelniaj użytkowników. Skaluj ją zależnie od potrzeb. Skonfiguruj alerty dotyczące wydajności. Wszystkie te czynności możesz wykonać za pomocą kilku kliknięć. Zobacz [Dodawanie funkcji do pierwszej aplikacji sieci Web](app-service-web-get-started-2.md).




<!--HONumber=Feb17_HO3-->


