---
title: "Jak migrować i publikowanie aplikacji sieci Web do usługi w chmurze platformy Azure w programie Visual Studio | Dokumentacja firmy Microsoft"
description: "Informacje o migracji i publikowanie aplikacji sieci web do usługi w chmurze platformy Azure przy użyciu programu Visual Studio."
services: visual-studio-online
documentationcenter: na
author: kraigb
manager: ghogen
editor: 
ms.assetid: 9394adfd-a645-4664-9354-dd5df08e8c91
ms.service: multiple
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: multiple
ms.date: 11/11/2016
ms.author: kraigb
ms.openlocfilehash: d5de4f5a7357cf5adde7773867356d47ad447bab
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/11/2017
---
# <a name="how-to-migrate-and-publish-a-web-application-to-an-azure-cloud-service-from-visual-studio"></a>Porady: migracja i publikowanie aplikacji sieci Web do usługi w chmurze platformy Azure w programie Visual Studio
Uwzględnienie hostingu usług i skalowalność Azure, można migrować i publikowanie aplikacji sieci web do usługi w chmurze Azure. Można uruchomić aplikacji sieci web na platformie Azure przy minimalnych zmianach w istniejącej aplikacji.

> [!NOTE]
> Ten temat dotyczy wdrażania usługi w chmurze, nie do witryn sieci web. Aby uzyskać informacje o wdrażaniu do witryn sieci web, zobacz [wdrażanie aplikacji sieci web w usłudze Azure App Service](app-service/app-service-deploy-local-git.md).
>
>

Lista określonych szablonów, które są obsługiwane zarówno dla programu Visual C# i Visual Basic, zobacz sekcję **obsługiwane szablony projektów** dalszej części tego tematu.

Należy najpierw włączyć aplikacji sieci web dla platformy Azure w programie Visual Studio. Na poniższej ilustracji przedstawiono podstawowe etapy Opublikuj istniejącej aplikacji sieci web, dodając projektu platformy Azure do zastosowania podczas wdrażania. Ten proces powoduje dodanie projektu z rolą sieci web platformy Azure do rozwiązania. Na podstawie typu projektu sieci web, czy masz, właściwości projektu dla zestawów są zaktualizowano także jeśli pakiet usługi wymaga następującej liczby dodatkowych zestawów do wdrożenia.

![Publikowanie aplikacji sieci Web do systemu Microsoft Azure](./media/vs-azure-tools-migrate-publish-web-app-to-cloud-service/IC748917.png)

> [!NOTE]
> **Przekonwertować**, **Konwertuj na projekt usługi w chmurze Azure** polecenia są wyświetlane tylko dla projektu sieci web w rozwiązaniu. Na przykład polecenie nie jest dostępna dla projektu Silverlight w rozwiązaniu.
> Podczas tworzenia pakietu usług lub opublikować aplikację na platformie Azure, ostrzeżeń lub błędów mogą wystąpić. Te ostrzeżenia i błędy może pomóc rozwiązać problemy przed wdrożeniem na platformie Azure. Na przykład może pojawić się ostrzeżenie o brakować zestawu. Aby uzyskać więcej informacji o sposobie traktuje wszystkie ostrzeżenia jako błędy, zobacz [skonfigurować projekt usługi w chmurze Azure z programem Visual Studio](vs-azure-tools-configuring-an-azure-project.md). Jeśli skompilować aplikację, uruchom lokalnie przy użyciu emulatora obliczeń lub opublikowania go w usłudze Azure mogą pojawić następujący błąd w **listy błędów** okna: **określona ścieżka i nazwa pliku jest zbyt długa**. Ten błąd występuje, ponieważ długość nazwy FQDN projektu platformy Azure jest zbyt długa. Długość nazwy projektu, łącznie z pełną ścieżką, nie może być więcej niż 146 znaków. Na przykład jest to nazwa projektu pełną ścieżkę do pliku dla projektu platformy Azure, który jest tworzony dla aplikacji Silverlight w tym: `c:\users\<user name>\documents\visual studio 2015\Projects\SilverlightApplication4\SilverlightApplication4.Web.Azure.ccproj`. Trzeba przenieść do innego katalogu, który o krótszej ścieżce, aby zmniejszyć długość nazwy FQDN projektu rozwiązania.
>
>

Aby dokonać migracji i publikowanie aplikacji sieci web na platformie Azure w programie Visual Studio, wykonaj następujące kroki.

## <a name="enable-a-web-application-for-deployment-to-azure"></a>Włączanie aplikacji sieci Web do wdrożenia na platformie Azure
### <a name="to-enable-a-web-application-for-deployment-to-azure"></a>Aby umożliwić aplikacji sieci web do wdrożenia na platformie Azure
1. Aby włączyć aplikacji sieci web do wdrożenia na platformie Azure, otwórz menu skrótów dla projektu sieci web w rozwiązaniu i wybierz polecenie Dodaj projekt wdrożenia usługi Azure.

    Są wykonywane następujące akcje:

   * Projekt platformy Azure o nazwie `<name of the web project>.Azure` zostanie dodany do rozwiązania dla aplikacji.
   * Rolę sieci web dla projektu sieci web jest dodawany do tego projektu platformy Azure.
   * **Kopiuj lokalnie** właściwość ma wartość true dla dowolnych zestawów, które są wymagane dla platformy MVC 2 MVC, MVC 3, 4 i aplikacje biznesowe Silverlight. Spowoduje to dodanie tych zestawów pakiet usługi, który jest używany do wdrożenia.

   > [!IMPORTANT]
   > Jeśli masz inne zestawy lub pliki, które są wymagane dla tej aplikacji sieci web, należy ręcznie ustawić właściwości dla tych plików. Aby uzyskać informacje na temat ustawiania tych właściwości, zobacz sekcję **obejmują pliki w pakiecie usługi** dalszej części tego artykułu.
   >
   > [!NOTE]
   > Jeśli rolę sieci web dla określonego projektu sieci web już istnieje w projekcie platformy Azure w rozwiązaniu, **przekonwertować**, **Konwertuj na projekt usługi w chmurze Azure** nie jest wyświetlany w menu skrótów dla tego projektu sieci web.
   >
   >

   Jeśli masz wiele projektów sieci web w aplikacji sieci web, aby utworzyć role sieci web dla każdego projektu sieci web należy wykonać kroki opisane w tej procedurze dla każdego projektu sieci web. Spowoduje to utworzenie oddzielnych projektów platformy Azure dla każdej roli sieci web. Każdy projekt sieci web mogą być publikowane oddzielnie. Alternatywnie można dodawać ręcznie innej roli sieci web do istniejącego projektu platformy Azure w aplikacji sieci web. Aby to zrobić, otwórz menu skrótów **ról** folderu w projekcie platformy Azure, wybierz **Dodaj**, następnie **projektu roli sieć Web w rozwiązaniu**, wybierz projekt, aby dodać rolę sieci web, i następnie wybierz pozycję **OK** przycisku.

## <a name="use-an-azure-sql-database-for-your-application"></a>Użyj bazy danych Azure SQL dla aplikacji
Jeśli parametry połączenia aplikacji sieci web, która używa bazy danych programu SQL Server, która jest używana lokalnie, należy zmienić ten ciąg połączenia, aby użyć wystąpienia bazy danych SQL Azure obsługującego zamiast tego.

> [!IMPORTANT]
> Subskrypcję należy włączyć można używać bazy danych SQL. Jeśli dostęp do subskrypcji z [klasycznego portalu Azure](http://go.microsoft.com/fwlink/?LinkID=213885), można określić, jakie usługi oferuje subskrypcji. Poniższe instrukcje dotyczą wydanych [klasycznego portalu Azure](http://go.microsoft.com/fwlink/?LinkID=213885). Jeśli używasz [portalu Azure](http://portal.microsoft.com), przejdź do następnej procedury.
>
>

### <a name="to-use-a-sql-database-instance-in-your-web-role-for-your-connection-string"></a>Aby użyć wystąpienia bazy danych SQL w roli sieci web dla parametrów połączenia
1. Aby utworzyć wystąpienie bazy danych SQL w [klasycznego portalu Azure](http://go.microsoft.com/fwlink/?LinkID=213885), wykonaj kroki opisane w artykule: [utworzyć serwer bazy danych SQL](http://go.microsoft.com/fwlink/?LinkId=225109).

   > [!NOTE]
   > Podczas konfigurowania reguł zapory dla swojego wystąpienia bazy danych SQL, należy wybrać **Zezwalaj innymi usługami platformy Azure na dostęp do tego serwera** pole wyboru.
   >
   >
2. Aby utworzyć wystąpienie bazy danych SQL do użycia na potrzeby parametrów połączenia, wykonaj czynności opisane w następnej sekcji w następującym artykule: [Utwórz bazę danych SQL](http://go.microsoft.com/fwlink/?LinkId=225110).
3. Aby skopiować parametry połączenia ADO.NET dla parametrów połączenia, wykonaj następujące kroki w [klasycznego portalu Azure](http://go.microsoft.com/fwlink/?LinkID=213885).  

   1. Wybierz **bazy danych** przycisk, a następnie otwórz węzeł subskrypcji, który został użyty do utworzenia wystąpienia bazy danych SQL.
   2. Aby wyświetlić dostępne wystąpienia bazy danych SQL, wybierz pozycję **baz danych SQL** węzła.
   3. Aby wyświetlić właściwości dla bazy danych, wybierz bazę danych. **Właściwości** zostanie wyświetlony widok.

      > [!NOTE]
      > Jeśli **właściwości** widoku nie jest wyświetlane, może być konieczne go otworzyć przy użyciu dzielnik.
      >
      >
   4. Aby wyświetlić parametry połączenia, wybierz przycisk wielokropka (...) obok pozycji widok.

      **Parametry połączenia** zostanie wyświetlone okno dialogowe.
   5. Aby skopiować parametry połączenia ADO.NET, zaznacz tekst, a następnie wybierz pozycję klawiszy Ctrl + C.
   6. Aby zamknąć okno dialogowe, wybierz pozycję **zamknąć** przycisku.
4. Aby zastąpić ciąg połączenia w pliku web.config, aby użyć tego wystąpienia bazy danych SQL, otwórz plik web.config, zaznacz istniejący wpis ciąg połączenia i następnie wybierz pozycję klawiszy Ctrl + V. Parametry połączenia ADO.NET dla wystąpienia bazy danych SQL zastępuje istniejące parametry połączenia.
5. Musisz również dodać parametr `MultipleActiveResultSets=True` w parametrach połączenia. Ciąg połączenia powinien mieć następujący format:

    ```
    connectionString=”Server=tcp:<database_server>.database.windows.net,1433;Database=<database_name>;User ID=<user_name>@<database_server>;Password=<myPassword>;Trusted_Connection=False;Encrypt=True;MultipleActiveResultSets=True"
    ```
6. (Opcjonalnie) Alternatywną metodą na zmieniające się bezpośrednio w pliku web.config ciągu połączenia jest dodawanie sekcji do jednego z plików transformacji pliku web.config, w zależności od konfiguracji kompilacji, który jest używany do utworzenia pakietu usługi. Otwórz plik Web.Debug.Config lub plik Web.Release.Config. Dodaj poniższą sekcję do tego pliku:

    ```
    XMLCopy<connectionStrings><addname="DefaultConnection"connectionString="Server=tcp:<database_server>.database.windows.net,1433;Database=<database_name>;User ID=<user_name>@<database_server>;Password=<myPassword>;Trusted_Connection=False;Encrypt=True;MultipleActiveResultSets=True"xdt:Transform="SetAttributes"xdt:Locator="Match(name)"/></connectionStrings>
    ```
7. Zapisz plik, który był modyfikowany i ponownie opublikować aplikację.

### <a name="to-use-an-instance-of-sql-database-by-using-the-azure-classic-portal"></a>Aby użyć wystąpienia bazy danych SQL przy użyciu klasycznego portalu Azure
1. W [klasycznego portalu Azure](http://go.microsoft.com/fwlink/?LinkID=213885), wybierz węzeł bazy danych SQL.

   * Jeśli zostanie wyświetlone wystąpienie bazy danych SQL, którego chcesz używać, wybierz go otworzyć.
   * Jeśli nie utworzono żadnych wystąpień, wybierz odpowiednie łącze, a następnie utwórz wystąpienie.
2. Po otwarciu lub Utwórz wystąpienie bazy danych, wybierz **parametry połączenia** łącza.
3. W dolnej części strony wybierz link, aby skonfigurować ustawienia zapory i zaakceptuj wartości domyślne lub skonfigurować wartości, które są potrzebne.
4. Skopiuj parametry połączenia ADO.NET, wklej go do pliku web.config przez stary ciąg połączenia dla bazy danych lokalnych i należy je dodać `MultipleActiveResultSets=True`.

## <a name="publish-a-web-application-to-azure"></a>Publikowanie aplikacji sieci Web na platformie Azure
### <a name="to-publish-a-web-application-to-azure"></a>Aby opublikować aplikację sieci Web na platformie Azure
1. Aby przetestować aplikację do tworzenia lokalnego środowiska przy użyciu platformy Azure emulator obliczeń, otwórz menu skrótów dla usługi Azure projektu roli sieć web i wybierz polecenie **Ustaw jako projekt startowy**. Następnie wybierz pozycję **debugowania**, **Rozpocznij debugowanie** (klawiatury: **F5**).

    **Uruchomić środowisko debugowania Azure** zostanie otwarte okno dialogowe i uruchamiania aplikacji w przeglądarce. Aby uzyskać szczegółowe informacje na temat sposobu uruchamiania każdego typu aplikacji sieci web w emulatorze obliczeń zobacz tabelę w tej sekcji.
2. Aby skonfigurować usługi dla aplikacji do publikowania na platformie Azure, musi mieć konto Microsoft i subskrypcji platformy Azure. Aby skonfigurować usługi, wykonaj kroki w następującym temacie: [przygotowanie do publikowania lub wdrażania aplikacji Azure z programu Visual Studio](vs-azure-tools-cloud-service-publish-set-up-required-services-in-visual-studio.md).
3. Aby opublikować aplikację sieci web na platformie Azure, otwórz menu skrótów dla projektu sieci web i wybierz polecenie **publikowania na platformie Azure**.

    **Publikowanie aplikacji platformy Azure** zostanie otwarte okno dialogowe i Visual Studio rozpoczyna proces wdrażania. Aby uzyskać więcej informacji o sposobie publikowania aplikacji, zobacz sekcję **opublikować aplikację Azure w programie Visual Studio** w [publikowania usługi w chmurze przy użyciu narzędzia Azure](vs-azure-tools-publishing-a-cloud-service.md).

   > [!NOTE]
   > Można również opublikować aplikacji sieci web z projektu platformy Azure. Aby to zrobić, otwórz menu skrótów dla usługi Azure projektu i wybierz polecenie **publikowania**.
   >
   >
4. Aby wyświetlić postęp wdrażania, można wyświetlić **dziennika aktywności platformy Azure** okna. Ten dziennik jest automatycznie wyświetlana podczas uruchamiania procesu wdrażania. Można rozwinąć ten element wiersza w dzienniku aktywności, aby wyświetlić szczegółowe informacje, jak pokazano na poniższej ilustracji:

    ![VST_AzureActivityLog](./media/vs-azure-tools-migrate-publish-web-app-to-cloud-service/IC744149.png)
5. (Opcjonalnie) Aby anulować proces wdrażania, otwórz menu skrótów dla pozycji wiersza w dzienniku aktywności i wybierz polecenie **Anuluj i Usuń**. Powoduje to zatrzymanie procesu wdrażania i usuwa środowiska wdrażania z platformy Azure.

   > [!NOTE]
   > Aby usunąć to środowisko wdrażania po jej wdrożeniu, należy użyć [klasycznego portalu Azure](http://go.microsoft.com/fwlink/?LinkID=213885).
   >
   >
6. (Opcjonalnie) Po uruchomiona wystąpienia roli, Visual Studio automatycznie wyświetlany środowiska wdrażania w **rozwiązań usługi obliczenia Azure** w węźle **Eksplorator chmury** lub **Eksploratora serwera**. W tym miejscu można wyświetlić stan wystąpień poszczególnych ról.

    Na poniższej ilustracji przedstawiono wystąpień roli w **Eksploratora serwera** gdy są one nadal w stanie inicjowanie:

    ![VST_DeployComputeNode](./media/vs-azure-tools-migrate-publish-web-app-to-cloud-service/IC744134.png)
7. Aby uzyskać dostęp do aplikacji po wdrożeniu, wybierz strzałkę obok danego wdrożenia, gdy stan **Ukończono** pojawia się w **dziennik aktywności platformy Azure**. Spowoduje to wyświetlenie adresu URL aplikacji sieci web na platformie Azure. Zobacz poniższą tabelę, aby uzyskać więcej informacji dotyczących sposobu uruchamiania aplikacji sieci web dla określonego typu z platformy Azure.

    W poniższej tabeli przedstawiono szczegółowe informacje dotyczące uruchamiania aplikacji sieci web określonego z platformy Azure lub uruchom lub debugowanie aplikacji sieci web lokalnie przy użyciu emulatora obliczeniowe Azure:

   | Typ aplikacji sieci Web | Uruchom/Debug lokalnie przy użyciu emulatora obliczeń | działające na platformie Azure |
   | --- | --- | --- |
   | Aplikacja sieci Web ASP.NET |Na pasku menu wybierz **debugowania**, **Rozpocznij debugowanie** (klawiatury: Wybierz **F5** klucza.). |Wybierz hiperłącze adres URL wyświetlany w **wdrożenia** karcie **dziennik aktywności platformy Azure** można załadować strony początkowej w przeglądarce. |
   | Aplikacja sieci Web platformy ASP.NET MVC 2 |Na pasku menu wybierz **debugowania**, **Rozpocznij debugowanie** (klawiatury: Wybierz **F5** klucza.). |Wybierz hiperłącze adres URL wyświetlany w **wdrożenia** karcie **dziennik aktywności platformy Azure** można załadować strony początkowej w przeglądarce. |
   | Aplikacja sieci Web platformy ASP.NET MVC 3 |Na pasku menu wybierz **debugowania**, **Rozpocznij debugowanie** (klawiatury: Wybierz **F5** klucza.). |Wybierz hiperłącze adres URL wyświetlany w **wdrożenia** karcie **dziennik aktywności platformy Azure** można załadować strony początkowej w przeglądarce. |
   | Aplikacja sieci Web platformy ASP.NET MVC 4 |Na pasku menu wybierz **debugowania**, **Rozpocznij debugowanie** (klawiatury: Wybierz **F5** klucza.). |Wybierz hiperłącze adres URL wyświetlany w **wdrożenia** karcie **dziennik aktywności platformy Azure** można załadować strony początkowej w przeglądarce. |
   | ASP.NET pusta aplikacja sieci Web |Należy dodać stronę .aspx w aplikacji, która zostanie ustawiony jako strony początkowej dla projektu sieci web. Następnie na pasku menu wybierz **debugowania**, **Rozpocznij debugowanie** (klawiatury: Wybierz **F5** klucza.). |Jeśli masz domyślną stronę .aspx w aplikacji, wybierz hiperłącze adres URL wyświetlany w **wdrożenia** karcie **dziennik aktywności platformy Azure** i załadowaniu tę stronę w przeglądarce. Jeśli masz strony .aspx różnych musisz przejdź do tej strony przy użyciu następującego formatu dla danego adresu url:`<url for deployment>/<name of page>.aspx` |
   | Aplikacji Silverlight |Na pasku menu wybierz **debugowania**, **Rozpocznij debugowanie** (klawiatury: Wybierz **F5** klucza.). |Musisz przejść do określonej strony aplikacji przy użyciu następującego formatu dla danego adresu url:`<url for deployment>/<name of page>.aspx` |
   | Aplikacja biznesowa Silverlight |Na pasku menu wybierz **debugowania**, **Rozpocznij debugowanie** (klawiatury: Wybierz **F5** klucza.). |Musisz przejść do określonej strony aplikacji przy użyciu następującego formatu dla danego adresu url:`<url for deployment>/<name of page>.aspx` |
   | Aplikacja nawigacji Silverlight |Na pasku menu wybierz **debugowania**, **Rozpocznij debugowanie** (klawiatury: Wybierz **F5** klucza.). |Musisz przejść do określonej strony aplikacji przy użyciu następującego formatu dla danego adresu url:`<url for deployment>/<name of page>.aspx` |
   | Aplikacja usługi WCF |Należy ustawić w pliku svc jako strony początkowej w projekcie usługi WCF. Następnie na pasku menu wybierz **debugowania**, **Rozpocznij debugowanie** (klawiatury: Wybierz **F5** klucza.). |Musisz przejść do pliku svc aplikacji przy użyciu następującego formatu dla danego adresu url:`<url for deployment>/<name of service file>.svc` |
   | Aplikacja usługi przepływu pracy WCF |Należy ustawić w pliku svc jako strony początkowej w projekcie usługi WCF. Następnie na pasku menu wybierz **debugowania**, **Rozpocznij debugowanie** (klawiatury: Wybierz **F5** klucza.). |Musisz przejść do pliku svc aplikacji przy użyciu następującego formatu dla danego adresu url:`<url for deployment>/<name of service file>.svc` |
   | ASP.NET Dynamic jednostek |Na pasku menu wybierz **debugowania**, **Rozpocznij debugowanie** (klawiatury: Wybierz **F5** klucza.). |Należy zaktualizować parametry połączenia (zobacz następną sekcję). Należy również przejdź do odpowiedniej strony aplikacji przy użyciu następującego formatu dla danego adresu url:`<url for deployment>/<name of page>.aspx` |
   | ASP.NET Dynamic Data Linq do SQL |Na pasku menu wybierz **debugowania**, **Rozpocznij debugowanie** (klawiatury: Wybierz **F5** klucza.). |Wykonaj kroki opisane w tej procedurze: Użyj bazy danych SQL Azure dla aplikacji (zobacz wcześniejszą sekcję w tym temacie). Należy również przejdź do odpowiedniej strony aplikacji przy użyciu następującego formatu dla danego adresu url:`<url for deployment>/<name of page>.aspx` |

## <a name="update-a-connection-string-for-aspnet-dynamic-entities"></a>Zaktualizuj parametry połączenia dla jednostek ASP.NET Dynamic
### <a name="to-update-a-connection-string-for-aspnet-dynamic-entities"></a>Aby zaktualizować parametry połączenia dla jednostek ASP.NET Dynamic
1. Aby utworzyć bazę danych SQL Azure, która może służyć do aplikacji sieci web ASP.NET dynamiczne jednostek, wykonaj kroki opisane w procedurze **użyć bazy danych SQL Azure dla aplikacji** wcześniej w tym temacie.
2. Dodawanie tabel i pól, które są potrzebne dla tej bazy danych z [klasycznego portalu Azure](http://go.microsoft.com/fwlink/?LinkID=213885).
3. Parametry połączenia dla tego typu aplikacji ma następujący format w pliku web.config:  

    ```
    <addname="tempdbEntities"connectionString="metadata=res://*/Model1.csdl|res://*/Model1.ssdl|res://*/Model1.msl;provider=System.Data.SqlClient;provider connection string=&quot;data source=<server name>\SQLEXPRESS;initial catalog=<database name>;integrated security=True;multipleactiveresultsets=True;App=EntityFramework&quot;"providerName="System.Data.EntityClient"/>
    ```

    Aktualizacja *connectionString* wartości z parametrów połączenia ADO.NET dla bazy danych SQL Azure w następujący sposób:

    ```
    XMLCopy<addname="tempdbEntities"connectionString="metadata=res://*/Model1.csdl|res://*/Model1.ssdl|res://*/Model1.msl;provider=System.Data.SqlClient;provider connection string=&quot;Server=tcp:<SQL Azure server name>.database.windows.net,1433;Database=<database name>;User ID=<user name>;Password=<password>;Trusted_Connection=False;Encrypt=True;multipleactiveresultsets=True;App=EntityFramework&quot;"providerName="System.Data.EntityClient"/>
    ```
4. Aby zapisać zmiany wprowadzone w parametrach połączenia w pliku web.config, na pasku menu wybierz **pliku**, **zapisać pliku web.config**.

## <a name="supported-project-templates"></a>Szablony projektów obsługiwanych
Aby opublikować aplikację sieci web na platformie Azure, aplikacja musi używać jednego z szablonów projektu dla języka C# lub Visual Basic, który znajduje się w poniższej tabeli.

| Grupa szablonu projektu | Szablon projektu |
| --- | --- |
| Sieć Web |Aplikacja sieci Web ASP.NET |
| Sieć Web |Aplikacja sieci Web platformy ASP.NET MVC 2 |
| Sieć Web |Aplikacja sieci Web platformy ASP.NET MVC 3 |
| Sieć Web |Aplikacja sieci Web platformy ASP.NET MVC 4 |
| Sieć Web |ASP.NET pusta aplikacja sieci Web |
| Sieć Web |Aplikacja pusty sieci Web platformy ASP.NET MVC 2 |
| Sieć Web |Aplikacja sieci Web ASP.NET Dynamic Data Entities |
| Sieć Web |ASP.NET Dynamic Data Linq do aplikacji sieci Web SQL |
| Silverlight |Aplikacji Silverlight |
| Silverlight |Aplikacja biznesowa Silverlight |
| Silverlight |Aplikacja nawigacji Silverlight |
| WCF |Aplikacja usługi WCF |
| WCF |Aplikacja usługi przepływu pracy WCF |
| Przepływ pracy |Aplikacja usługi przepływu pracy WCF |

## <a name="next-steps"></a>Następne kroki
Aby uzyskać więcej informacji dotyczących publikowania, zobacz [przygotowanie do publikowania lub wdrażania aplikacji Azure z programu Visual Studio](vs-azure-tools-cloud-service-publish-set-up-required-services-in-visual-studio.md). Sprawdź również [ustawienie zapasową o nazwie poświadczenia uwierzytelniania](vs-azure-tools-setting-up-named-authentication-credentials.md).
