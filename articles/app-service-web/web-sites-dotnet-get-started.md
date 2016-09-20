<properties
    pageTitle="Wdrażanie aplikacji ASP.NET w usłudze Azure App Service przy użyciu programu Visual Studio | Microsoft Azure"
    description="Dowiedz się, jak wdrożyć projekt sieci Web ASP.NET w nowej aplikacji sieci Web w usłudze Azure App Service przy użyciu programu Visual Studio."
    services="app-service\web"
    documentationCenter=".net"
    authors="tdykstra"
    manager="wpickett"
    editor=""/>

<tags
    ms.service="app-service-web"
    ms.workload="web"
    ms.tgt_pltfrm="na"
    ms.devlang="dotnet"
    ms.topic="get-started-article"
    ms.date="07/22/2016"
    ms.author="rachelap"/>

# Wdrażanie aplikacji sieci Web programu ASP.NET w usłudze Azure App Service przy użyciu programu Visual Studio

[AZURE.INCLUDE [tabs](../../includes/app-service-web-get-started-nav-tabs.md)]

## Omówienie

Ten samouczek pokazuje, jak wdrożyć aplikację sieci Web programu ASP.NET w nowej [aplikacji sieci Web w usłudze Azure App Service](app-service-web-overview.md) przy użyciu programu Visual Studio 2015.

Samouczek jest przeznaczony dla deweloperów programu ASP.NET, którzy nie mają doświadczenia w korzystaniu z platformy Azure. Po zakończeniu będziesz mieć prostą aplikację sieci Web uruchomioną w chmurze.

Dowiesz się:

* Jak wdrożyć nową aplikację sieci Web w usłudze App Service podczas tworzenia nowego projektu sieci Web w programie Visual Studio.
* Jak wdrożyć projekt sieci Web w aplikacji sieci Web w usłudze App Service przy użyciu programu Visual Studio.

Diagram przedstawia czynności wykonywane w samouczku.

![Diagram tworzenia i wdrażania w programie Visual Studio](./media/web-sites-dotnet-get-started/Create_App.png)

W sekcji [Rozwiązywanie problemów](#troubleshooting) na końcu samouczka przedstawiono sposoby rozwiązywania problemów, a sekcja [Następne kroki](#next-steps) zawiera linki do innych samouczków, w których bardziej szczegółowo omówiono sposoby korzystania z usługi Azure App Service.

Jest to samouczek ułatwiający rozpoczynanie pracy, dlatego przedstawia wdrażanie prostego projektu sieci Web, który nie korzysta z bazy danych, bez użycia uwierzytelniania ani autoryzacji. Linki do bardziej zaawansowanych tematów dotyczących wdrożenia można znaleźć w temacie [How to deploy an Azure web app](web-sites-deploy.md) (Jak wdrożyć aplikację sieci Web platformy Azure).

Oprócz czasu wymaganego do zainstalowania zestawu Azure SDK dla programu .NET wykonanie tego samouczka trwa około 10–15 minut.

## Wymagania wstępne

* Samouczek jest przeznaczony dla użytkowników, którzy pracowali już z aplikacją ASP.NET MVC i programem Visual Studio. Jeśli potrzebujesz wprowadzenia, zobacz [Getting Started with ASP.NET MVC 5](http://www.asp.net/mvc/overview/getting-started/introduction/getting-started) (Wprowadzenie do aplikacji ASP.NET MVC 5).

* Musisz mieć konto platformy Azure. Możesz [utworzyć konto bezpłatnej wersji próbnej Azure](/pricing/free-trial/?WT.mc_id=A261C142F) lub [aktywować korzyści dla subskrybentów programu Visual Studio](/pricing/member-offers/msdn-benefits-details/?WT.mc_id=A261C142F). 

    Jeśli chcesz zacząć korzystać z usługi Azure App Service przed utworzeniem konta platformy Azure, przejdź do artykułu [Wypróbuj usługę App Service](http://go.microsoft.com/fwlink/?LinkId=523751). W tej lokalizacji możesz utworzyć początkową aplikację o krótkim okresie istnienia w usłudze App Service — bez karty kredytowej i bez zobowiązań.

## <a name="setupdevenv"></a>Konfigurowanie środowiska projektowego

Samouczek jest przeznaczony dla programu Visual Studio 2015 z [zestawem Azure SDK dla programu .NET](../dotnet-sdk.md) w wersji 2.9 lub nowszej. 

* [Pobierz najnowszy zestaw Azure SDK dla programu Visual Studio 2015](http://go.microsoft.com/fwlink/?linkid=518003). Zestaw SDK instaluje program Visual Studio 2015, jeśli jest to konieczne.

    >[AZURE.NOTE] Zależnie od liczby składników zależnych zestawu SDK, które znajdują się już na komputerze, instalowanie zestawu SDK może trwać od kilku minut do pół godziny lub dłużej.

Jeśli masz program Visual Studio 2013 i wolisz użyć tego programu, możesz [pobrać najnowszy zestaw Azure SDK dla programu Visual Studio 2013](http://go.microsoft.com/fwlink/?LinkID=324322). Niektóre ekrany mogą różnić się od przedstawionych na ilustracjach.

## Konfigurowanie nowego projektu sieci Web

Następnym krokiem jest utworzenie projektu sieci Web w programie Visual Studio i aplikacji sieci Web w usłudze Azure App Service. W tej sekcji samouczka zostanie skonfigurowany nowy projekt sieci Web. 

1. Otwórz program Visual Studio 2015.

2. Kliknij pozycje **Plik > Nowy > Projekt**.

3. W oknie dialogowym **Nowy projekt** kliknij pozycje **Visual C# > Sieć Web > Aplikacja sieci Web ASP.NET**.

3. Upewnij się, że program **.NET Framework 4.5.2** został wybrany jako platforma docelowa.

4.  Usługa [Azure Application Insights](../application-insights/app-insights-overview.md) monitoruje dostępność, wydajność i użycie aplikacji sieci Web. Pole wyboru **Dodaj usługę Application Insights do projektu** jest domyślnie zaznaczone, gdy projekt sieci Web jest tworzony po raz pierwszy po zainstalowaniu programu Visual Studio. Wyczyść pole wyboru, jeśli zostało zaznaczone, ale nie chcesz wypróbowywać usługi Application Insights.

4. Podaj nazwę aplikacji **Mój_przykład**, a następnie kliknij przycisk **OK**.

    ![Okno dialogowe Nowy projekt](./media/web-sites-dotnet-get-started/GS13newprojdb.png)

5. W oknie dialogowym **Nowy projekt ASP.NET** wybierz szablon **MVC**, a następnie kliknij pozycję **Zmień uwierzytelnianie**.

    W tym samouczku wdrażany jest projekt sieci Web ASP.NET MVC. Jeśli chcesz dowiedzieć się, jak wdrożyć projekt interfejsu API sieci Web programu ASP.NET, zobacz sekcję [Następne kroki](#next-steps). 

    ![Okno dialogowe Nowy projekt ASP.NET](./media/web-sites-dotnet-get-started/GS13changeauth.png)

6. W oknie dialogowym **Zmienianie uwierzytelniania** kliknij pozycję **Bez uwierzytelniania**, a następnie kliknij przycisk **OK**.

    ![Bez uwierzytelniania](./media/web-sites-dotnet-get-started/GS13noauth.png)

    W tym samouczku ułatwiającym rozpoczynanie pracy jest wdrażana prosta aplikacja, do której użytkownik się nie loguje.

5. Upewnij się, że w sekcji **Microsoft Azure** okna dialogowego **Nowy projekt ASP.NET** zostało zaznaczone pole wyboru **Host w chmurze**, a na liście rozwijanej została zaznaczona pozycja **Usługa App Service**.

    ![Okno dialogowe Nowy projekt ASP.NET](./media/web-sites-dotnet-get-started/GS13newaspnetprojdb.png)

    Te ustawienia polecają programowi Visual Studio utworzenie aplikacji sieci Web platformy Azure dla projektu sieci Web.

6. Kliknij przycisk **OK**.

## Konfigurowanie zasobów Azure dla nowej aplikacji sieci Web

Teraz przekaż programowi Visual Studio informacje dotyczące zasobów Azure, które powinien utworzyć.

5. W oknie dialogowym **Tworzenie usługi App Service** kliknij pozycję **Dodaj konto**, a następnie zaloguj się do platformy Azure przy użyciu identyfikatora i hasła konta, których używasz do zarządzania subskrypcją platformy Azure.

    ![Logowanie do platformy Azure](./media/web-sites-dotnet-get-started/configuresitesettings.png)

    Jeśli użytkownik został już zalogowany wcześniej na danym komputerze, przycisk **Dodaj konto** może być niewidoczny. W takim przypadku możesz pominąć ten krok lub może być konieczne ponowne wprowadzenie poświadczeń.
 
3. W polu **Nazwa aplikacji sieci Web** wprowadź nazwę, która jest unikatowa w domenie *azurewebsites.net*. Na przykład można określić nazwę Mój_przykład z numerem, aby utworzyć unikatową nazwę taką jak Mój_przykład810. Jeśli domyślna nazwa sieci Web zostanie utworzona automatycznie, będzie unikatowa i można jej używać.

    Jeśli wprowadzona nazwa jest już używana przez inną osobę, z prawej strony zobaczysz czerwony wykrzyknik zamiast zielonego znacznika wyboru i będzie konieczne wprowadzenie innej nazwy.

    Adres URL aplikacji jest połączeniem tej nazwy z ciągiem *.azurewebsites.net*. Jeśli na przykład nazwa to `MyExample810`, adresem URL jest `myexample810.azurewebsites.net`.

    Możesz również używać domeny niestandardowej z aplikacją sieci Web platformy Azure. Aby uzyskać więcej informacji, zobacz [Konfigurowanie niestandardowej nazwy domeny w usłudze Azure App Service](web-sites-custom-domain-name.md).

6. Kliknij przycisk **Nowa** obok pola **Grupa zasobów**, a następnie wprowadź „Mój_przykład” lub inną wybraną nazwę. 

    ![Okno dialogowe Tworzenie usługi App Service](./media/web-sites-dotnet-get-started/rgcreate.png)

    Grupa zasobów to kolekcja zasobów platformy Azure, takich jak aplikacje sieci Web, bazy danych i maszyny wirtualne. W przypadku korzystania z samouczka najlepiej utworzyć nową grupę zasobów, ponieważ ułatwi to usunięcie w jednym kroku dowolnych zasobów platformy Azure utworzonych na potrzeby samouczka. Aby uzyskać więcej informacji, zobacz [Omówienie usługi Azure Resource Manager](../resource-group-overview.md).

4. Kliknij przycisk **Nowy** obok listy rozwijanej **Plan usługi App Service**.

    ![Okno dialogowe Tworzenie usługi App Service](./media/web-sites-dotnet-get-started/createasplan.png)

    Zostanie wyświetlone okno dialogowe **Konfigurowanie planu usługi App Service**.

    ![Okno dialogowe Konfigurowanie usługi App Service](./media/web-sites-dotnet-get-started/configasp.png)

    Poniższe kroki umożliwiają konfigurowanie planu usługi App Service dla nowej grupy zasobów. Plan usługi App Service określa zasoby obliczeniowe, których używa aplikacja sieci Web. Jeśli na przykład wybierzesz warstwę Bezpłatna, aplikacja interfejsu API będzie działać na udostępnionych maszynach wirtualnych. W przypadku niektórych warstw płatnych będzie ona działać na dedykowanych maszynach wirtualnych. Aby uzyskać więcej informacji, zobacz [Omówienie planów usługi App Service](../app-service/azure-web-sites-web-hosting-plans-in-depth-overview.md).

5. W oknie dialogowym **Konfigurowanie planu usługi App Service** wprowadź „Mój_plan_przykładowy” lub inną wybraną nazwę.

5. Z listy rozwijanej **Lokalizacja** wybierz lokalizację w najbliższej okolicy.

    To ustawienie służy do określania, w którym centrum danych Azure aplikacja zostanie uruchomiona. W przypadku korzystania z tego samouczka można wybrać dowolny region — nie sprawi to znaczącej różnicy. Ale jeśli korzystasz z aplikacji produkcyjnej, serwer sieci Web powinien znajdować się możliwie najbliżej klientów, którzy uzyskują do niego dostęp. Dzięki temu można zminimalizować [opóźnienie](http://www.bing.com/search?q=web%20latency%20introduction&qs=n&form=QBRE&pq=web%20latency%20introduction&sc=1-24&sp=-1&sk=&cvid=eefff99dfc864d25a75a83740f1e0090).

5. Na liście rozwijanej **Rozmiar** kliknij pozycję **Bezpłatna**.

    W przypadku korzystania z tego samouczka warstwa cenowa Bezpłatna zapewni dostateczną wydajność.

6. W oknie dialogowym **Konfigurowanie planu usługi App Service** kliknij przycisk **OK**.

7. W oknie dialogowym **Tworzenie usługi App Service** kliknij przycisk **Utwórz**.

## Tworzenie projektu i aplikacji sieci Web przez program Visual Studio

Program Visual Studio szybko (zazwyczaj trwa to nie dłużej niż minutę) tworzy projekt sieci Web i aplikację sieci Web.  

W oknie **Eksplorator rozwiązań** wyświetlane są pliki i foldery w nowym projekcie.

![Eksplorator rozwiązań](./media/web-sites-dotnet-get-started/solutionexplorer.png)

W oknie **Działanie usługi Azure App Service** wyświetlane jest potwierdzenie utworzenia aplikacji sieci Web.

![Utworzona aplikacja sieci Web w oknie Działanie usługi Azure App Service](./media/web-sites-dotnet-get-started/GS13sitecreated1.png)

Korzystając z okna **Eksplorator chmury**, można wyświetlać zasoby platformy Azure i zarządzać nimi, w tym nową aplikacją sieci Web, która została właśnie utworzona.

![Aplikacja sieci Web utworzona w Eksploratorze chmury](./media/web-sites-dotnet-get-started/siteinse.png)
    
## Wdrażanie projektu sieci Web w aplikacji sieci Web platformy Azure

W tej sekcji projekt sieci Web jest wdrażany w aplikacji sieci Web.

1. W **Eksploratorze rozwiązań** kliknij projekt prawym przyciskiem myszy i wybierz pozycję **Opublikuj**.

    ![Wybieranie polecenia Opublikuj w menu programu Visual Studio](./media/web-sites-dotnet-get-started/choosepublish.png)

    W ciągu kilku sekund zostanie wyświetlony kreator **Publikowanie w sieci Web**. W kreatorze jest otwierany *profil publikowania* zawierający ustawienia wdrażania projektu sieci Web w nowej aplikacji sieci Web.

    Profil publikowania zawiera nazwę użytkownika i hasło dla wdrożenia.  Te poświadczenia zostały wygenerowane automatycznie i nie trzeba ich wprowadzać. Hasło jest szyfrowane w ukrytym pliku specyficznym dla użytkownika w folderze `Properties\PublishProfiles`.
 
8. Na karcie **Połączenie** kreatora **Publikowanie w sieci Web** kliknij przycisk **Dalej**.

    ![Kliknięcie przycisku Dalej na karcie Połączenie kreatora Publikowanie w sieci Web](./media/web-sites-dotnet-get-started/GS13ValidateConnection.png)

    Następna karta to **Ustawienia**. W tym miejscu możesz zmienić konfigurację kompilacji, aby wdrożyć kompilację debugowania dla [zdalnego debugowania](../app-service-web/web-sites-dotnet-troubleshoot-visual-studio.md#remotedebug). Na tej karcie jest również wyświetlanych kilka [opcji publikowania pliku](https://msdn.microsoft.com/library/dd465337.aspx#Anchor_2).

10. Na karcie **Ustawienia** kliknij przycisk **Dalej**.

    ![Karta Ustawienia kreatora Publikowanie w sieci Web](./media/web-sites-dotnet-get-started/GS13SettingsTab.png)

    Następna karta to **Podgląd**. W tej lokalizacji możesz zobaczyć pliki, które zostaną skopiowane z projektu do aplikacji interfejsu API. Podczas wdrażania projektu w aplikacji interfejsu API, która została już wcześniej wdrożona, kopiowane są tylko zmienione pliki. Jeśli chcesz wyświetlić listę plików, które zostaną skopiowane, możesz kliknąć przycisk **Uruchom podgląd**.

11. Na karcie **Podgląd** kliknij przycisk **Opublikuj**.

    ![Karta Podgląd kreatora Publikowanie w sieci Web](./media/web-sites-dotnet-get-started/GS13previewoutput.png)

    Po kliknięciu przycisk **Opublikuj** program Visual Studio rozpoczyna proces kopiowania plików na serwer platformy Azure. Może to potrwać minutę lub dwie.

    W oknach **Dane wyjściowe** i **Działanie usługi Azure App Service** wyświetlane są podejmowane akcje wdrożenia i raport potwierdzający pomyślne ukończenie wdrożenia.

    ![Okno Dane wyjściowe programu Visual Studio z raportem potwierdzającym pomyślne ukończenie wdrożenia](./media/web-sites-dotnet-get-started/PublishOutput.png)

    Po pomyślnym wdrożeniu przeglądarka domyślna automatycznie otwiera adres URL wdrożonej aplikacji sieci Web, a utworzona aplikacja działa w chmurze. Adres URL na pasku adresu przeglądarki potwierdza, że aplikacja sieci Web jest ładowana z Internetu.

    ![Aplikacja sieci Web uruchomiona na platformie Azure](./media/web-sites-dotnet-get-started/GS13deployedsite.png)

    > [AZURE.TIP] Możesz włączyć pasek narzędzi do szybkiego wdrożenia **Publikacja w sieci Web pojedynczym kliknięciem**. Kliknij pozycje **Widok > Paski narzędzi**, a następnie wybierz pozycję **Publikacja w sieci Web pojedynczym kliknięciem**. Korzystając z paska narzędzi, możesz wybrać profil, kliknąć przycisk w celu opublikowania lub kliknąć przycisk służący do otwierania kreatora **Publikowanie w sieci Web**.
    > ![Pasek narzędzi Publikacja w sieci Web pojedynczym kliknięciem](./media/web-sites-dotnet-get-started/weboneclickpublish.png)

## Rozwiązywanie problemów

Jeśli wystąpi problem podczas korzystania z tego samouczka, upewnij się, że używasz najnowszej wersji zestawu Azure SDK dla platformy .NET. Najłatwiejszym sposobem wykonania tego zadania jest [pobranie zestawu Azure SDK dla programu Visual Studio 2015](http://go.microsoft.com/fwlink/?linkid=518003). Jeśli masz zainstalowaną bieżącą wersję, Instalator platformy sieci Web potwierdzi, że instalacja nie jest wymagana.

Jeśli korzystasz z komputera działającego w sieci firmowej i próbujesz przeprowadzić wdrożenie w usłudze Azure App Service przez zaporę, upewnij się, że porty 443 i 8172 są otwarte dla narzędzia Web Deploy. Jeśli nie możesz otworzyć tych portów, skorzystaj z innych opcji wdrażania z sekcji Następne kroki.

Po uruchomieniu aplikacji sieci Web programu ASP.NET w usłudze Azure App Service warto dowiedzieć się więcej na temat funkcji programu Visual Studio ułatwiających rozwiązywanie problemów. Aby uzyskać informacje na temat rejestrowania, zdalnego debugowania i innych funkcji, zobacz [Troubleshooting Azure web apps in Visual Studio](web-sites-dotnet-troubleshoot-visual-studio.md) (Rozwiązywanie problemów z aplikacjami sieci Web platformy Azure w programie Visual Studio).

## Następne kroki

W tym samouczku przedstawiono sposób tworzenia prostej aplikacji sieci Web i wdrożenia jej w aplikacji sieci Web platformy Azure. Aby dowiedzieć się więcej na temat usługi Azure App Service, zobacz poniższe tematy pokrewne i zasoby:

* Monitorowanie aplikacji sieci Web i zarządzanie nią w [Portalu Azure](https://portal.azure.com/). 

    Aby uzyskać więcej informacji, zobacz [omówienie Portalu Azure](/services/management-portal/) i temat [Configure web apps in Azure App Service](web-sites-configure.md) (Konfigurowanie aplikacji sieci Web w usłudze Azure App Service).

* Wdrażanie istniejącego projektu sieci Web w nowej aplikacji sieci Web przy użyciu programu Visual Studio.

    Kliknij prawym przyciskiem myszy projekt w **Eksploratorze rozwiązań**, a następnie kliknij przycisk **Opublikuj**. Wybierz pozycję **Microsoft Azure App Service** jako docelową lokalizację publikacji, a następnie kliknij przycisk **Nowy**. Okna dialogowe są takie same jak opisane w tym samouczku.

* Wdrażanie projektu sieci Web z systemu kontroli źródła

    Aby uzyskać informacje na temat [automatyzacji wdrażania](http://www.asp.net/aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/continuous-integration-and-continuous-delivery) z [systemu kontroli źródła](http://www.asp.net/aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/source-control), zobacz [Wprowadzenie do aplikacji sieci Web w usłudze Azure App Service](app-service-web-get-started.md) i [How to deploy an Azure web app](web-sites-deploy.md) (Jak wdrożyć aplikację sieci Web platformy Azure).

* Wdrażanie aplikacji interfejsu API platformy ASP.NET w aplikacji interfejsu API w usłudze Azure App Service

    Przedstawiono sposób tworzenia wystąpienia usługi Azure App Service przeznaczonego przede wszystkim do hostowania witryny sieci Web. Usługa App Service oferuje również funkcje do hostingu interfejsów API sieci Web, takich jak obsługa mechanizmu CORS i obsługa metadanych interfejsu API w celu generowania kodu klienta. Możesz używać funkcji interfejsu API w aplikacji sieci Web, ale jeśli chcesz przede wszystkim hostować interfejs API w wystąpieniu usługi App Service, **aplikacja interfejsu API** będzie lepszym rozwiązaniem. Aby uzyskać więcej informacji, zobacz [Get started with API Apps and ASP.NET in Azure App Service](../app-service-api/app-service-api-dotnet-get-started.md) (Wprowadzenie do aplikacji interfejsu API i programu ASP.NET w usłudze Azure App Service). 

* Dodawanie niestandardowej nazwy domeny i protokołu SSL

    Aby uzyskać informacje na temat sposobu użycia protokołu SSL i własnej nazwy domeny (na przykład www.contoso.com zamiast contoso.azurewebsites.net), zobacz następujące zasoby:

    * [Konfigurowanie niestandardowej nazwy domeny w usłudze Azure App Service](web-sites-custom-domain-name.md)
    * [Włączanie obsługi protokołu HTTPS dla witryny sieci Web platformy Azure](web-sites-configure-ssl-certificate.md)

* Usuwanie grupy zasobów zawierającej Twoją aplikację sieci Web i wszystkie powiązane zasoby Azure po zakończeniu korzystania z tych zasobów.

    Aby uzyskać informacje na temat sposobu pracy z grupami zasobów w witrynie Azure Portal, zobacz [Deploy resources with Resource Manager templates and Azure portal](../resource-group-template-deploy-portal.md) (Wdrażanie zasobów za pomocą szablonów usługi Resource Manager i Portalu Azure).   

*   Aby zobaczyć więcej przykładów dotyczących tworzenia aplikacji sieci Web platformy ASP.NET w usłudze App Service, zobacz [Create and deploy an ASP.NET web app in Azure App Service](https://github.com/Microsoft/HealthClinic.biz/wiki/Create-and-deploy-an-ASP.NET-web-app-in-Azure-App-Service) (Tworzenie i wdrażanie aplikacji sieci web ASP.NET w usłudze Azure App Service) i [Create and deploy a mobile app in Azure App Service](https://github.com/Microsoft/HealthClinic.biz/wiki/Create-and-deploy-a-mobile-app-in-Azure-App-Service) (Tworzenie i wdrażanie aplikacji mobilnej w usłudze Azure App Service) w [pokazie](https://blogs.msdn.microsoft.com/visualstudio/2015/12/08/connectdemos-2015-healthclinic-biz/) 2015 Connect witryny [HealthClinic.biz](https://github.com/Microsoft/HealthClinic.biz). Aby uzyskać więcej materiałów szybkiego startu z w wersji demonstracyjnej witryny HealthClinic.biz, zobacz [Azure Developer Tools Quickstarts](https://github.com/Microsoft/HealthClinic.biz/wiki/Azure-Developer-Tools-Quickstarts) (Materiały szybkiego startu narzędzi deweloperskich platformy Azure).



<!--HONumber=sep16_HO1-->


