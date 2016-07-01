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
   ms.date="05/17/2016"
   ms.author="tomfitz" />

# Tworzenie i wdrażanie grup zasobów platformy Azure za pomocą programu Visual Studio

Program Visual Studio i zestaw [Azure SDK](https://azure.microsoft.com/downloads/) umożliwiają utworzenie projektu służącego do wdrażania infrastruktury i kodu na platformie Azure. Można na przykład zdefiniować host sieci Web, witrynę sieci Web i bazę danych dla aplikacji, a następnie wdrożyć tę infrastrukturę wraz z kodem. Można również zdefiniować maszynę wirtualną, usługę Virtual Network i konto usługi Storage, a następnie wdrożyć tę infrastrukturę wraz ze skryptem wykonywanym na maszynie wirtualnej. Projekt wdrożenia **grupy zasobów platformy Azure** umożliwia wdrożenie wszystkich niezbędnych zasobów w ramach pojedynczej i powtarzalnej operacji. Aby uzyskać więcej informacji dotyczących wdrażania zasobów i zarządzania nimi, zobacz [Omówienie usługi Azure Resource Manager](resource-group-overview.md).

Projekty grupy zasobów platformy Azure zawierają szablony JSON usługi Azure Resource Manager, które określają zasoby wdrażane na platformie Azure. Aby uzyskać informacje na temat elementów szablonu usługi Resource Manager, zobacz [Authoring Azure Resource Manager templates](resource-group-authoring-templates.md) (Tworzenie szablonów usługi Azure Resource Manager). Program Visual Studio umożliwia edytowanie tych szablonów i oferuje narzędzia ułatwiające pracę z szablonami.

W tym temacie omówiono wdrażanie aplikacji sieci Web i bazy danych SQL Database, jednak kroki są prawie takie same dla każdego typu zasobu. Równie łatwo możesz wdrożyć maszynę wirtualną i powiązane zasoby. Program Visual Studio zapewnia wiele różnych szablonów początkowych do wdrażania typowych scenariuszy.

Ten artykuł dotyczy programu Visual Studio 2015 Update 2 i zestawu Microsoft Azure SDK dla programu .NET 2.9. Jeśli korzystasz z programu Visual Studio 2013 z zestawem Azure SDK 2.9, czynności będą w większości takie same. Możesz korzystać z zestawu Azure SDK w wersji 2.6 lub nowszej, jednak czynności mogą różnić się od przedstawionych w tym artykule. Zdecydowanie zaleca się zainstalowanie najnowszej wersji zestawu [Azure SDK](https://azure.microsoft.com/downloads/) przed rozpoczęciem wykonywania tych kroków. 

## Tworzenie projektu grupy zasobów platformy Azure

W tej procedurze omówiono tworzenie projektu grupy zasobów platformy Azure przy użyciu szablonu **Aplikacja sieci Web i baza danych SQL**.

1. W programie Visual Studio wybierz pozycję **Plik**, **Nowy projekt** i wybierz opcję **C#** lub **Visual Basic**. Wybierz pozycję **Chmura**, a następnie wybierz projekt **Grupa zasobów platformy Azure**.

    ![Projekt wdrażania w chmurze](./media/vs-azure-tools-resource-groups-deployment-projects-create-deploy/create-project.png)

1. Wybierz szablon, który ma zostać wdrożony w usłudze Azure Resource Manager. Należy zauważyć, że dostępnych jest wiele różnych opcji zależnie od typu projektu, który chcesz wdrożyć. W tym temacie wybrano szablon **Aplikacja sieci Web i baza danych SQL**.

    ![Wybieranie szablonu](./media/vs-azure-tools-resource-groups-deployment-projects-create-deploy/select-project.png)

    Wybrany szablon jest tylko punktem wyjściowym. Możesz dodać lub usunąć zasoby, aby wykonać dany scenariusz.

    >[AZURE.NOTE] Lista dostępnych szablonów jest pobierana w trybie online i może ulec zmianie.

    Program Visual Studio tworzy projekt wdrożenia grupy zasobów dla aplikacji sieci Web i Bazy danych SQL.

1. Rozwiń węzły w projekcie wdrożenia, aby wyświetlić utworzone elementy.

    ![Wyświetlanie węzłów](./media/vs-azure-tools-resource-groups-deployment-projects-create-deploy/show-items.png)

    Ponieważ w tym przykładzie wybrano szablon Aplikacja sieci Web i baza danych SQL, zostaną wyświetlone następujące pliki. 

  	|Nazwa pliku|Opis|
  	|---|---|
  	|Deploy-AzureResourceGroup.ps1|Skrypt programu PowerShell, który wywołuje polecenia w celu wdrożenia do usługi Azure Resource Manager.<br />**Uwaga:** ten skrypt programu PowerShell jest używany przez program Visual Studio do wdrażania szablonu. Należy zachować ostrożność, ponieważ wszystkie zmiany wprowadzone w tym skrypcie będą miały wpływ na wdrożenie w programie Visual Studio.|
  	|WebSiteSQLDatabase.json|Szablon usługi Resource Manager określający infrastrukturę, którą chcesz wdrożyć na platformie Azure, oraz parametry, które można podać podczas wdrażania. Umożliwia on również definiowanie zależności między zasobami w celu wdrażania ich w odpowiedniej kolejności.|
  	|WebSiteSQLDatabase.parameters.json|Plik parametrów zawierający wartości wymagane przez szablon. Są to wartości przekazywane w celu dostosowania poszczególnych wdrożeń.|

    Wszystkie projekty wdrażania grup zasobów zawierają te podstawowe pliki. Inne projekty mogą zawierać dodatkowe pliki do obsługi innych funkcji.

## Dostosowywanie szablonu usługi Resource Manager

Można dostosować projekt wdrożenia, modyfikując szablony JSON określające zasoby, które chcesz wdrożyć. JSON (JavaScript Object Notation) jest łatwym w użyciu formatem serializowanych danych. Pliki JSON używają schematu, który przedstawiono u góry każdego pliku. Możesz pobrać i przeanalizować schemat, aby lepiej go zrozumieć. Schemat określa dozwolone elementy, typy i formaty pól, dopuszczalne wartości wyliczane itd. Aby uzyskać informacje na temat elementów szablonu usługi Resource Manager, zobacz [Authoring Azure Resource Manager templates](resource-group-authoring-templates.md) (Tworzenie szablonów usługi Azure Resource Manager).

Aby rozpocząć pracę nad szablonem, otwórz plik **WebSiteSQLDatabase.json**.

Edytor programu Visual Studio udostępnia narzędzia pomocne podczas edytowania szablonu usługi Resource Manager. Okno **Konspekt pliku JSON** ułatwia wyświetlanie elementów zdefiniowanych w szablonie.

![wyświetlanie konspektu pliku JSON](./media/vs-azure-tools-resource-groups-deployment-projects-create-deploy/show-json-outline.png)

Wybranie dowolnego elementu w konspekcie powoduje przejście do odpowiedniej części szablonu i wyróżnienie powiązanego kodu JSON.

![przechodzenie do elementów JSON](./media/vs-azure-tools-resource-groups-deployment-projects-create-deploy/navigate-json.png)

Nowy zasób można dodać do szablonu, wybierając przycisk **Dodaj zasób** u góry okna Konspekt pliku JSON lub klikając prawym przyciskiem myszy węzeł **zasoby** i wybierając polecenie **Dodaj nowy zasób**.

![dodawanie zasobu](./media/vs-azure-tools-resource-groups-deployment-projects-create-deploy/add-resource.png)

W ramach tego samouczka wybierz pozycję **Konto usługi Storage** i nadaj mu nazwę. Nazwa konta magazynu może zawierać tylko cyfry i małe litery, a jej długość nie może przekraczać 24 znaków. Projekt doda 13-znakowy unikatowy ciąg do podanej nazwy, dlatego należy się upewnić, że nazwa nie zawiera więcej niż 11 znaków.

![dodawanie magazynu](./media/vs-azure-tools-resource-groups-deployment-projects-create-deploy/add-storage.png)

Należy zauważyć, że dodany został nie tylko zasób, ale również parametr typu konta magazynu oraz zmienna nazwy konta magazynu.

![wyświetlanie konspektu](./media/vs-azure-tools-resource-groups-deployment-projects-create-deploy/show-new-items.png)

Parametr **storageType** jest wstępnie zdefiniowany wraz z dozwolonymi typami i typem domyślnym. Możesz pozostawić te wartości bez zmian lub edytować je dla danego scenariusza. Jeśli nie chcesz zezwalać innym osobom na wdrażanie konta magazynu **Premium_LRS** przy użyciu tego szablonu, po prostu usuń go z dozwolonych typów, jak pokazano poniżej. 

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

Program Visual Studio oferuje również funkcję IntelliSense ułatwiającą zrozumienie dostępnych właściwości podczas edytowania szablonu. Aby na przykład edytować właściwości planu usługi App Service, przejdź do zasobu **HostingPlan** i dodaj nową wartość dla obiektu **properties**. Należy zauważyć, że funkcja IntelliSense wyświetla dostępne wartości oraz ich opisy.

![wyświetlanie funkcji intellisense](./media/vs-azure-tools-resource-groups-deployment-projects-create-deploy/show-intellisense.png)

Dla wartości **numberOfWorkers** można ustawić wartość 1.

    "properties": {
      "name": "[parameters('hostingPlanName')]",
      "numberOfWorkers": 1
    }

## Wdrażanie projektu grupy zasobów na platformie Azure

Teraz można przystąpić do wdrażania projektu. Projekt grupy zasobów platformy Azure jest wdrażany do grupy zasobów platformy Azure, która jest logiczną grupą zasobów na platformie Azure, takich jak aplikacje sieci Web, bazy danych itd.

1. W menu skrótów węzła projektu wdrażania wybierz pozycję **Wdróż** > **Nowe wdrożenie**.

    ![Element menu Wdróż, Nowe wdrażanie](./media/vs-azure-tools-resource-groups-deployment-projects-create-deploy/deploy.png)

    Zostanie wyświetlone okno dialogowe **Wdrażanie w grupie zasobów**.

    ![Okno dialogowe Wdrażanie w grupie zasobów](./media/vs-azure-tools-resource-groups-deployment-projects-create-deploy/show-deployment.png)

1. W polu listy rozwijanej **Grupa zasobów** wybierz istniejącą grupę zasobów lub utwórz nową. Aby utworzyć grupę, otwórz pole listy rozwijanej **Grupa zasobów** i wybierz pozycję **Utwórz nowy**.

    ![Okno dialogowe Wdrażanie w grupie zasobów](./media/vs-azure-tools-resource-groups-deployment-projects-create-deploy/create-new-group.png)

    Zostanie wyświetlone okno dialogowe **Tworzenie grupy zasobów**. Podaj nazwę i lokalizację grupy i wybierz przycisk **Utwórz**.

    ![Okno dialogowe Tworzenie grupy zasobów](./media/vs-azure-tools-resource-groups-deployment-projects-create-deploy/create-resource-group.png)
   
1. Parametry wdrożenia można edytować, wybierając przycisk **Edytuj parametry**. Podaj wartości parametrów i wybierz przycisk **Zapisz**.

    ![Okno dialogowe Edytowanie parametrów](./media/vs-azure-tools-resource-groups-deployment-projects-create-deploy/provide-parameters.png)
    
    Opcja **Zapisz hasła jako czysty tekst w pliku parametrów** nie jest bezpieczna.

1. Wybierz przycisk **Wdróż**, aby wdrożyć projekt na platformie Azure. Postęp wdrażania jest wyświetlany w oknie **Dane wyjściowe**. Wdrażanie może potrwać kilka minut w zależności od konfiguracji. Po wyświetleniu monitu wprowadź hasło administratora bazy danych w konsoli programu PowerShell. Jeśli postęp wdrażania został zatrzymany, możliwe, że proces oczekuje na wprowadzenie hasła w konsoli programu PowerShell.

    >[AZURE.NOTE] Może pojawić się monit o zainstalowanie poleceń cmdlet programu Azure PowerShell. Instalacja poleceń cmdlet jest konieczna, ponieważ są one wymagane do wdrożenia grup zasobów platformy Azure.
    
1. Po zakończeniu wdrażania w oknie **Dane wyjściowe** powinien zostać wyświetlony następujący komunikat:

        ...
        15:19:19 - DeploymentName     : websitesqldatabase-0212-2318
        15:19:19 - CorrelationId      : 6cb43be5-86b4-478f-9e2c-7e7ce86b26a2
        15:19:19 - ResourceGroupName  : DemoSiteGroup
        15:19:19 - ProvisioningState  : Succeeded
        ...

1. W przeglądarce otwórz [Portal Azure](https://portal.azure.com/) i zaloguj się do swojego konta. Aby wyświetlić grupę zasobów wybierz pozycję **Grupy zasobów**, a następnie wybierz grupę, do której wykonano wdrożenie.

    ![wybieranie grupy](./media/vs-azure-tools-resource-groups-deployment-projects-create-deploy/select-group.png)

1. Zostaną wyświetlone wszystkie wdrożone zasoby.

    ![wyświetlanie zasobów](./media/vs-azure-tools-resource-groups-deployment-projects-create-deploy/show-deployed-resources.png)

1. Jeśli wprowadzono zmiany i chcesz ponownie wdrożyć projekt, możesz wybrać istniejącą grupę zasobów bezpośrednio przy użyciu menu skrótów projektu zasobów platformy Azure. W menu skrótów wybierz pozycję **Wdróż**, a następnie wybierz grupę zasobów, do której wykonano wdrożenie.

    ![wdrożona grupa zasobów platformy Azure](./media/vs-azure-tools-resource-groups-deployment-projects-create-deploy/redeploy.png)

## Wdrażanie kodu przy użyciu infrastruktury

Do tej pory wdrożono infrastrukturę aplikacji, ale z projektem nie został wdrożony rzeczywisty kod. W tym temacie przedstawiono sposób wdrażania aplikacji sieci Web i tabel bazy danych SQL Database podczas wdrażania. Jeśli wdrażasz maszynę wirtualną zamiast aplikacji sieci Web, w ramach wdrożenia konieczne będzie uruchomienie kodu na maszynie. Proces wdrażania kodu dla aplikacji sieci Web lub konfigurowania maszyny wirtualnej jest niemal taki sam.

1. W rozwiązaniu programu Visual Studio dodaj aplikację, wybierając pozycję **Aplikacja sieci Web ASP.NET**. 

    ![dodawanie aplikacji sieci Web](./media/vs-azure-tools-resource-groups-deployment-projects-create-deploy/add-app.png)
    
1. Wybierz pozycję **MVC** i usuń zaznaczenie pola **Hostuj w chmurze**, ponieważ to zadanie zostanie wykonane przez projekt grupy zasobów.

    ![wybieranie pozycji MVC](./media/vs-azure-tools-resource-groups-deployment-projects-create-deploy/select-mvc.png)
    
1. Po utworzeniu aplikacji sieci Web dodaj odwołanie do projektu aplikacji sieci Web w projekcie grupy zasobów.

    ![dodawanie odwołania](./media/vs-azure-tools-resource-groups-deployment-projects-create-deploy/add-reference.png)
    
    Dodanie odwołania powoduje połączenie projektu aplikacji sieci Web z projektem grupy zasobów i automatyczne ustawienie trzech głównych właściwości.  
    
    - Właściwość **Additional Properties** zawiera tymczasową lokalizację pakietu wdrażania, który zostanie przekazany do usługi Azure Storage. 
    - Właściwość **Include File Path** zawiera ścieżkę, w której zostanie utworzony pakiet.  Właściwość **Include Targets** zawiera polecenie, które zostanie wykonane podczas wdrażania. 
    - Wartość domyślna właściwości **Build;Package** umożliwia skompilowanie i utworzenie pakietu wdrażania w sieci Web (pakiet.zip) podczas wdrażania.  
    
    Profil publikowania nie jest wymagany, ponieważ wdrożenie pobiera niezbędne informacje z właściwości w celu utworzenia pakietu.
    
      ![wyświetlanie odwołania](./media/vs-azure-tools-resource-groups-deployment-projects-create-deploy/see-reference.png)
      
1. Dodaj nowy zasób do szablonu, tym razem wybierając pozycję **Web Deploy dla usługi Web Apps**. 

    ![dodawanie narzędzia web deploy](./media/vs-azure-tools-resource-groups-deployment-projects-create-deploy/add-web-deploy.png)
    
1. Ponownie wdróż projekt grupy zasobów w grupie zasobów. Tym razem występuje kilka nowych parametrów. Nie musisz podawać wartości parametrów **_artifactsLocation** lub **_artifactsLocationSasToken**, ponieważ są one generowane automatycznie. Ustaw folder i nazwę pliku na ścieżkę zawierającą pakiet wdrożeniowy.

    ![dodawanie narzędzia web deploy](./media/vs-azure-tools-resource-groups-deployment-projects-create-deploy/set-new-parameters.png)
    
    Jako wartość pozycji **Konto magazynu artefaktów** możesz użyć konta wdrożonego z tą grupą zasobów.
    
Po zakończeniu wdrażania możesz przejść do witryny, aby zobaczyć, że została wdrożona domyślna aplikacja ASP.NET.

![wyświetlanie wdrożonej aplikacji](./media/vs-azure-tools-resource-groups-deployment-projects-create-deploy/show-deployed-app.png)

## Następne kroki

- Aby uzyskać informacje dotyczące zarządzania zasobami przy użyciu portalu, zobacz [Korzystanie z Portalu Azure do zarządzania zasobami Azure](./azure-portal/resource-group-portal.md).
- Aby uzyskać więcej informacji o szablonach, zobacz [Authoring Azure Resource Manager templates](resource-group-authoring-templates.md) (Tworzenie szablonów usługi Azure Resource Manager).



<!--HONumber=Jun16_HO2-->


