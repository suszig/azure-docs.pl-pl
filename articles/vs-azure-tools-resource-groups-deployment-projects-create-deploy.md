<properties
   pageTitle="Projekty grup zasobów platformy Azure programu Visual Studio | Microsoft Azure"
   description="Tworzenie projektu grupy zasobów platformy Azure przy użyciu programu Visual Studio i wdrażanie zasobów na platformie Azure."
   services="azure-resource-manager"
   documentationCenter="na"
   authors="tfitzmac"
   manager="timlt"
   editor="tysonn" />
<tags
   ms.service="azure-resource-manager"
   ms.devlang="multiple"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="09/20/2016"
   ms.author="tomfitz" />


# Tworzenie i wdrażanie grup zasobów platformy Azure za pomocą programu Visual Studio

Program Visual Studio i zestaw [Azure SDK](https://azure.microsoft.com/downloads/) umożliwiają utworzenie projektu służącego do wdrażania infrastruktury i kodu na platformie Azure. Można na przykład zdefiniować host sieci Web, witrynę sieci Web i bazę danych dla aplikacji, a następnie wdrożyć tę infrastrukturę wraz z kodem. Można również zdefiniować maszynę wirtualną, usługę Virtual Network i konto usługi Storage, a następnie wdrożyć tę infrastrukturę wraz ze skryptem wykonywanym na maszynie wirtualnej. Projekt wdrożenia **grupy zasobów platformy Azure** umożliwia wdrożenie wszystkich niezbędnych zasobów w ramach pojedynczej i powtarzalnej operacji. Aby uzyskać więcej informacji dotyczących wdrażania zasobów i zarządzania nimi, zobacz [Omówienie usługi Azure Resource Manager](resource-group-overview.md).

Projekty grupy zasobów platformy Azure zawierają szablony JSON usługi Azure Resource Manager, które określają zasoby wdrażane na platformie Azure. Aby uzyskać informacje na temat elementów szablonu usługi Resource Manager, zobacz [Authoring Azure Resource Manager templates](resource-group-authoring-templates.md) (Tworzenie szablonów usługi Azure Resource Manager). Program Visual Studio umożliwia edytowanie tych szablonów i oferuje narzędzia ułatwiające pracę z szablonami.

Ten temat dotyczy wdrażania aplikacji sieci Web i bazy danych SQL Database. Jednak kroki są prawie identyczne dla dowolnego typu zasobu. Równie łatwo możesz wdrożyć maszynę wirtualną i powiązane zasoby. Program Visual Studio zapewnia wiele różnych szablonów początkowych do wdrażania typowych scenariuszy.

Ten artykuł dotyczy programu Visual Studio 2015 Update 2 i zestawu Microsoft Azure SDK dla platformy .NET 2.9. Jeśli korzystasz z programu Visual Studio 2013 z zestawem Azure SDK 2.9, czynności są w większości takie same. Możesz korzystać z zestawu Azure SDK w wersji 2.6 lub nowszej, jednak interfejs użytkownika może różnić się od przedstawionego w tym artykule. Zdecydowanie zalecamy zainstalowanie najnowszej wersji zestawu [Azure SDK](https://azure.microsoft.com/downloads/) przed rozpoczęciem wykonywania tych kroków. 

## Tworzenie projektu grupy zasobów platformy Azure

W tej procedurze omówiono tworzenie projektu grupy zasobów platformy Azure przy użyciu szablonu **Aplikacja sieci Web i baza danych SQL**.

1. W programie Visual Studio wybierz pozycję **Plik**, **Nowy projekt** i wybierz opcję **C#** lub **Visual Basic**. Wybierz pozycję **Chmura**, a następnie wybierz projekt **Grupa zasobów platformy Azure**.

    ![Projekt wdrażania w chmurze](./media/vs-azure-tools-resource-groups-deployment-projects-create-deploy/create-project.png)

1. Wybierz szablon, który ma zostać wdrożony w usłudze Azure Resource Manager. Należy zauważyć, że dostępnych jest wiele różnych opcji zależnie od typu projektu, który chcesz wdrożyć. Na potrzeby tego tematu wybierz szablon **Aplikacja sieci Web i baza danych SQL**.

    ![Wybieranie szablonu](./media/vs-azure-tools-resource-groups-deployment-projects-create-deploy/select-project.png)

    Wybrany szablon jest tylko punktem wyjściowym. Możesz dodać lub usunąć zasoby, aby wykonać dany scenariusz.

    >[AZURE.NOTE] Program Visual Studio pobiera listę dostępnych szablonów w trybie online. Może ona ulec zmianie.

    Program Visual Studio tworzy projekt wdrożenia grupy zasobów dla aplikacji sieci Web i Bazy danych SQL.

1. Aby wyświetlić utworzone elementy, rozwiń węzły w projekcie wdrożenia.

    ![Wyświetlanie węzłów](./media/vs-azure-tools-resource-groups-deployment-projects-create-deploy/show-items.png)

    Ponieważ w tym przykładzie wybrano szablon Aplikacja sieci Web i baza danych SQL, zostaną wyświetlone następujące pliki: 

  	|Nazwa pliku|Opis|
  	|---|---|
  	|Deploy-AzureResourceGroup.ps1|Skrypt programu PowerShell, który wywołuje polecenia w celu wdrożenia do usługi Azure Resource Manager.<br />**Uwaga:** program Visual Studio używa tego skryptu programu PowerShell do wdrażania szablonu. Należy zachować ostrożność, ponieważ wszystkie zmiany wprowadzone w tym skrypcie mają wpływ na wdrożenie w programie Visual Studio.|
  	|WebSiteSQLDatabase.json|Szablon usługi Resource Manager określający infrastrukturę, którą chcesz wdrożyć na platformie Azure, oraz parametry, które można podać podczas wdrażania. Umożliwia on również definiowanie zależności między zasobami, tak aby usługa Resource Manager wdrożyła je w odpowiedniej kolejności.|
  	|WebSiteSQLDatabase.parameters.json|Plik parametrów zawierający wartości wymagane przez szablon. Poszczególne wdrożenia są dostosowywane za pomocą przekazywanych wartości parametrów.|

    Wszystkie projekty wdrażania grup zasobów zawierają te podstawowe pliki. Inne projekty mogą zawierać dodatkowe pliki do obsługi innych funkcji.

## Dostosowywanie szablonu usługi Resource Manager

Można dostosować projekt wdrożenia, modyfikując szablony JSON określające zasoby, które chcesz wdrożyć. JSON (JavaScript Object Notation) jest łatwym w użyciu formatem serializowanych danych. Pliki JSON używają schematu, do którego odwołujesz się w górnej części każdego pliku. Aby zrozumieć schemat, możesz go pobrać i przeanalizować. Schemat określa prawidłowe elementy, typy i formaty pól, dopuszczalne wartości wyliczane itd. Aby uzyskać informacje na temat elementów szablonu usługi Resource Manager, zobacz [Authoring Azure Resource Manager templates](resource-group-authoring-templates.md) (Tworzenie szablonów usługi Azure Resource Manager).

Aby rozpocząć pracę nad szablonem, otwórz plik **WebSiteSQLDatabase.json**.

Edytor programu Visual Studio udostępnia narzędzia pomocne podczas edytowania szablonu usługi Resource Manager. Okno **Konspekt pliku JSON** ułatwia wyświetlanie elementów zdefiniowanych w szablonie.

![wyświetlanie konspektu pliku JSON](./media/vs-azure-tools-resource-groups-deployment-projects-create-deploy/show-json-outline.png)

Wybranie dowolnego elementu w konspekcie powoduje przejście do odpowiedniej części szablonu i wyróżnienie powiązanego kodu JSON.

![przechodzenie do elementów JSON](./media/vs-azure-tools-resource-groups-deployment-projects-create-deploy/navigate-json.png)

Nowy zasób można dodać, wybierając przycisk **Dodaj zasób** u góry okna Konspekt pliku JSON lub klikając prawym przyciskiem myszy węzeł **zasoby** i wybierając polecenie **Dodaj nowy zasób**.

![dodawanie zasobu](./media/vs-azure-tools-resource-groups-deployment-projects-create-deploy/add-resource.png)

W ramach tego samouczka wybierz pozycję **Konto usługi Storage** i nadaj mu nazwę. Podaj nazwę nie dłuższą niż 11 znaków, zawierającą tylko cyfry i małe litery.

![dodawanie magazynu](./media/vs-azure-tools-resource-groups-deployment-projects-create-deploy/add-storage.png)

Należy zauważyć, że dodany został nie tylko zasób, ale również parametr typu konta magazynu oraz zmienna nazwy konta magazynu.

![wyświetlanie konspektu](./media/vs-azure-tools-resource-groups-deployment-projects-create-deploy/show-new-items.png)

Parametr **storageType** jest wstępnie zdefiniowany wraz z dozwolonymi typami i typem domyślnym. Możesz pozostawić te wartości bez zmian lub edytować je dla danego scenariusza. Jeśli nie chcesz, aby inne osoby wdrażały konta magazynu **Premium_LRS** przy użyciu tego szablonu, usuń go z dozwolonych typów. 

    "storageType": {
      "type": "string",
      "defaultValue": "Standard_LRS",
      "allowedValues": [
        "Standard_LRS",
        "Standard_ZRS",
        "Standard_GRS",
        "Standard_RAGRS"
      ]
    }

Program Visual Studio oferuje również funkcję IntelliSense ułatwiającą zrozumienie dostępnych właściwości podczas edytowania szablonu. Aby na przykład edytować właściwości planu usługi App Service, przejdź do zasobu **HostingPlan** i dodaj wartość dla obiektu **properties**. Należy zauważyć, że funkcja IntelliSense wyświetla dostępne wartości oraz ich opisy.

![wyświetlanie funkcji intellisense](./media/vs-azure-tools-resource-groups-deployment-projects-create-deploy/show-intellisense.png)

Dla wartości **numberOfWorkers** można ustawić wartość 1.

    "properties": {
      "name": "[parameters('hostingPlanName')]",
      "numberOfWorkers": 1
    }

## Wdrażanie projektu grupy zasobów na platformie Azure

Teraz można przystąpić do wdrażania projektu. Projekt grupy zasobów platformy Azure jest wdrażany do grupy zasobów platformy Azure. To logiczna grupa zasobów mających wspólny cykl życia.

1. W menu skrótów węzła projektu wdrażania wybierz pozycję **Wdróż** > **Nowe wdrożenie**.

    ![Element menu Wdróż, Nowe wdrażanie](./media/vs-azure-tools-resource-groups-deployment-projects-create-deploy/deploy.png)

    Zostanie wyświetlone okno dialogowe **Wdrażanie w grupie zasobów**.

    ![Okno dialogowe Wdrażanie w grupie zasobów](./media/vs-azure-tools-resource-groups-deployment-projects-create-deploy/show-deployment.png)

1. W polu listy rozwijanej **Grupa zasobów** wybierz istniejącą grupę zasobów lub utwórz nową. Aby utworzyć grupę, otwórz pole listy rozwijanej **Grupa zasobów** i wybierz pozycję **Utwórz nowy**.

    ![Okno dialogowe Wdrażanie w grupie zasobów](./media/vs-azure-tools-resource-groups-deployment-projects-create-deploy/create-new-group.png)

    Zostanie wyświetlone okno dialogowe **Tworzenie grupy zasobów**. Podaj nazwę i lokalizację grupy i wybierz przycisk **Utwórz**.

    ![Okno dialogowe Tworzenie grupy zasobów](./media/vs-azure-tools-resource-groups-deployment-projects-create-deploy/create-resource-group.png)
   
1. Edytuj parametry wdrożenia, wybierając przycisk **Edytuj parametry**.

    ![Przycisk Edytuj parametry](./media/vs-azure-tools-resource-groups-deployment-projects-create-deploy/edit-parameters.png)

1. Podaj wartości dla pustych parametrów i wybierz przycisk **Zapisz**. Puste parametry to **hostingPlanName**, **administratorLogin**, **administratorLoginPassword** i **databaseName**.

    Parametr **hostingPlanName** określa nazwę [planu usługi App Service](./app-service/azure-web-sites-web-hosting-plans-in-depth-overview.md) do utworzenia. 
    
    Parametr **administratorLogin** określa nazwę użytkownika dla administratora programu SQL Server. Nie należy używać wspólnych nazw administratorów, takich jak **sa** lub **admin**. 
    
    Parametr **administratorLoginPassword** określa hasło administratora programu SQL Server. Opcja **Zapisz hasła jako czysty tekst w pliku parametrów** nie jest bezpieczna, dlatego nie należy jej wybierać. Ponieważ hasło nie jest zapisywane w postaci zwykłego tekstu, należy podać to hasło ponownie podczas wdrażania. 
    
    Parametr **databaseName** określa nazwę bazy danych do utworzenia. 

    ![Okno dialogowe Edytowanie parametrów](./media/vs-azure-tools-resource-groups-deployment-projects-create-deploy/provide-parameters.png)
    
1. Wybierz przycisk **Wdróż**, aby wdrożyć projekt na platformie Azure. Konsola programu PowerShell zostanie otwarta poza wystąpieniem programu Visual Studio. Po wyświetleniu monitu wprowadź hasło administratora programu SQL Server w konsoli programu PowerShell. **Konsola programu PowerShell może być ukryta za innymi elementami lub zminimalizowana na pasku zadań.** Odszukaj tę konsolę i wybierz ją, aby podać hasło.

    >[AZURE.NOTE] W programie Visual Studio może pojawić się monit o zainstalowanie poleceń cmdlet programu Azure PowerShell. Polecenia te są niezbędne do pomyślnego wdrożenia grup zasobów. Jeśli zostanie wyświetlony monit, zainstaluj je.
    
1. Wdrożenie może potrwać kilka minut. W oknach **Dane wyjściowe** jest wyświetlany stan wdrożenia. Po zakończeniu wdrażania ostatni komunikat wskazuje pomyślne wdrożenie i jest podobny do następującego:

        ... 
        18:00:58 - Successfully deployed template 'c:\users\user\documents\visual studio 2015\projects\azureresourcegroup1\azureresourcegroup1\templates\websitesqldatabase.json' to resource group 'DemoSiteGroup'.


1. W przeglądarce otwórz witrynę [Azure Portal](https://portal.azure.com/) i zaloguj się do swojego konta. Aby wyświetlić grupę zasobów wybierz pozycję **Grupy zasobów**, a następnie wybierz grupę, do której wykonano wdrożenie.

    ![wybieranie grupy](./media/vs-azure-tools-resource-groups-deployment-projects-create-deploy/select-group.png)

1. Zostaną wyświetlone wszystkie wdrożone zasoby. Należy zauważyć, że nazwa konta magazynu nie jest identyczna z określoną podczas dodawania tego zasobu. Konto magazynu musi być unikatowe. Szablon automatycznie dodaje ciąg znaków do podanej nazwy w celu zapewnienia unikatowej nazwy. 

    ![wyświetlanie zasobów](./media/vs-azure-tools-resource-groups-deployment-projects-create-deploy/show-deployed-resources.png)

1. Jeśli wprowadzono zmiany i chcesz ponownie wdrożyć projekt, wybierz istniejącą grupę zasobów przy użyciu menu skrótów projektu grupy zasobów platformy Azure. W menu skrótów wybierz pozycję **Wdróż**, a następnie wybierz wdrożoną grupę zasobów.

    ![wdrożona grupa zasobów platformy Azure](./media/vs-azure-tools-resource-groups-deployment-projects-create-deploy/redeploy.png)

## Wdrażanie kodu przy użyciu infrastruktury

Do tej pory wdrożono infrastrukturę aplikacji, ale z projektem nie został wdrożony rzeczywisty kod. W tym temacie przedstawiono sposób wdrażania aplikacji sieci Web i tabel bazy danych SQL Database podczas wdrażania. Jeśli wdrażasz maszynę wirtualną zamiast aplikacji sieci Web, w ramach wdrożenia konieczne będzie uruchomienie kodu na maszynie. Proces wdrażania kodu dla aplikacji sieci Web lub konfigurowania maszyny wirtualnej jest niemal taki sam.

1. Dodaj projekt do rozwiązania programu Visual Studio. Kliknij prawym przyciskiem myszy rozwiązanie, a następnie wybierz pozycje **Dodaj** > **Nowy projekt**.

    ![dodawanie projektu](./media/vs-azure-tools-resource-groups-deployment-projects-create-deploy/add-project.png)

1. Dodaj **aplikację sieci Web ASP.NET**. 

    ![dodawanie aplikacji sieci Web](./media/vs-azure-tools-resource-groups-deployment-projects-create-deploy/add-app.png)
    
1. Wybierz pozycję **MVC** i usuń zaznaczenie pola **Hostuj w chmurze**, ponieważ to zadanie zostanie wykonane przez projekt grupy zasobów.

    ![wybieranie pozycji MVC](./media/vs-azure-tools-resource-groups-deployment-projects-create-deploy/select-mvc.png)
    
1. Po utworzeniu aplikacji sieci Web przez program Visual Studio oba projekty są wyświetlane w rozwiązaniu.

    ![wyświetlanie projektów](./media/vs-azure-tools-resource-groups-deployment-projects-create-deploy/show-projects.png)

1. Musisz teraz upewnić się, że projekt grupy zasobów wie o nowym projekcie. Wróć do projektu grupy zasobów (AzureResourceGroup1). Kliknij prawym przyciskiem myszy pozycję **Odwołania** i wybierz polecenie **Dodaj odwołanie**.

    ![dodawanie odwołania](./media/vs-azure-tools-resource-groups-deployment-projects-create-deploy/add-new-reference.png)

1. Wybierz projekt aplikacji sieci Web, który został utworzony.

    ![dodawanie odwołania](./media/vs-azure-tools-resource-groups-deployment-projects-create-deploy/add-reference.png)
    
    Dodanie odwołania powoduje połączenie projektu aplikacji sieci Web z projektem grupy zasobów i automatyczne ustawienie trzech głównych właściwości. Te właściwości będą wyświetlane w oknie **Właściwości** odwołania.

      ![wyświetlanie odwołania](./media/vs-azure-tools-resource-groups-deployment-projects-create-deploy/see-reference.png)
    
    Właściwości są następujące:

    - Właściwość **Additional Properties** zawiera tymczasową lokalizację pakietu wdrażania w sieci Web, który zostanie przekazany do usługi Azure Storage. Zanotuj folder (ExampleApp) i plik (package.zip). Te właściwości należy podać jako parametry podczas wdrażania aplikacji. 
    - Właściwość **Include File Path** zawiera ścieżkę, w której zostanie utworzony pakiet. Właściwość **Include Targets** zawiera polecenie, które zostanie wykonane podczas wdrażania. 
    - Wartość domyślna właściwości **Build;Package** umożliwia skompilowanie i utworzenie pakietu wdrażania w sieci Web (pakiet.zip) podczas wdrażania.  
    
    Profil publikowania nie jest wymagany, ponieważ wdrożenie pobiera niezbędne informacje z właściwości w celu utworzenia pakietu.
      
1. Dodaj zasób do szablonu.

    ![dodawanie zasobu](./media/vs-azure-tools-resource-groups-deployment-projects-create-deploy/add-resource-2.png)

1. Tym razem wybierz pozycję **Web Deploy dla usługi Web Apps**. 

    ![dodawanie narzędzia web deploy](./media/vs-azure-tools-resource-groups-deployment-projects-create-deploy/add-web-deploy.png)
    
1. Ponownie wdróż projekt grupy zasobów w grupie zasobów. Tym razem występuje kilka nowych parametrów. Nie musisz podawać wartości parametrów **_artifactsLocation** ani **_artifactsLocationSasToken**, ponieważ program Visual Studio generuje je automatycznie. Musisz jednak ustawić folder i nazwę pliku na ścieżkę zawierającą pakiet wdrożeniowy (oznaczoną jako **ExampleAppPackageFolder** i **ExampleAppPackageFileName** na poniższej ilustracji). Podaj wartości zanotowane wcześniej z właściwości odwołania (**ExampleApp** i **package.zip**).

    ![dodawanie narzędzia web deploy](./media/vs-azure-tools-resource-groups-deployment-projects-create-deploy/set-new-parameters.png)
    
    Jako **Konto magazynu artefaktów** wybierz konto wdrożone z tą grupą zasobów.
    
1. Po zakończeniu wdrażania wybierz aplikację sieci Web w witrynie portalu. Wybierz adres URL, aby przejść do lokacji.

    ![przeglądanie lokacji](./media/vs-azure-tools-resource-groups-deployment-projects-create-deploy/browse-site.png)

1. Zobacz, że domyślna aplikacja ASP.NET została pomyślnie wdrożona.

    ![wyświetlanie wdrożonej aplikacji](./media/vs-azure-tools-resource-groups-deployment-projects-create-deploy/show-deployed-app.png)

## Następne kroki

- Aby uzyskać informacje dotyczące zarządzania zasobami przy użyciu portalu, zobacz [Korzystanie z witryny Azure Portal do zarządzania zasobami Azure](./azure-portal/resource-group-portal.md).
- Aby uzyskać więcej informacji o szablonach, zobacz [Authoring Azure Resource Manager templates](resource-group-authoring-templates.md) (Tworzenie szablonów usługi Azure Resource Manager).



<!--HONumber=Sep16_HO3-->


