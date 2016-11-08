---
title: Używanie programu VS Code z szablonami usługi Resource Manager | Microsoft Docs
description: Opis sposobu konfiguracji programu Visual Studio Code w celu tworzenia szablonów usługi Azure Resource Manager.
services: azure-resource-manager
documentationcenter: na
author: cmatskas
manager: timlt
editor: tysonn

ms.service: azure-resource-manager
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 09/26/2016
ms.author: chmatsk;tomfitz

---
# Praca z szablonami usługi Azure Resource Manager w programie Visual Studio Code
Szablony usługi Azure Resource Manager to pliki w formacie JSON opisujące zasób i powiązane zależności. Czasami pliki te są duże i skomplikowane, dlatego ważne jest, aby były dostępne odpowiednie narzędzia. Visual Studio Code to nowy, lekki, międzyplatformowy edytor kodu typu open-source. Obsługuje tworzenie i edytowanie szablonów usługi Resource Manager za pomocą [nowego rozszerzenia](https://marketplace.visualstudio.com/items?itemName=msazurermtools.azurerm-vscode-tools). Program VS Code można uruchomić wszędzie i wymaga on dostępu do Internetu tylko na potrzeby wdrażania szablonów usługi Resource Manager.

Jeśli nie masz jeszcze programu VS Code, możesz go zainstalować z witryny [https://code.visualstudio.com/](https://code.visualstudio.com/).

## Instalowanie rozszerzenia usługi Resource Manager
Aby pracować z szablonami JSON w programie VS Code, należy zainstalować rozszerzenie. Wykonaj poniższe kroki, aby pobrać i zainstalować obsługę języka dla szablonów JSON usługi Resource Manager:

1. Uruchom program VS Code 
2. Otwórz opcję Quick Open (Szybkie otwieranie, Ctrl+P) 
3. Uruchom następujące polecenie: 
   
        ext install azurerm-vscode-tools
4. Uruchom ponownie program VS Code po wyświetleniu monitu, aby włączyć rozszerzenie. 
   
   Gotowe!

## Konfigurowanie fragmentów kodu usługi Resource Manager
W poprzednich krokach zainstalowaliśmy potrzebne narzędzia. Teraz musimy skonfigurować program VS Code tak, aby można było używać fragmentów kodu szablonów JSON.

1. Skopiuj zawartość pliku z repozytorium [azure-xplat-arm-tooling](https://raw.githubusercontent.com/Azure/azure-xplat-arm-tooling/master/VSCode/armsnippets.json) do schowka.
2. Uruchom program VS Code 
3. W programie VS Code możesz otworzyć plik JSON z fragmentami kodu, przechodząc do pozycji **File** (Plik) -> **Preferences** (Preferencje) -> **User Snippets** (Fragmenty kodu użytkownika) -> **JSON** bądź naciskając klawisz **F1** i wpisując ciąg **preferences** (preferencje), aż pojawi się do wyboru pozycja **Preferences: Snippets** (Preferencje: fragmenty kodu).
   
    ![preferencje — fragmenty kodu](./media/resource-manager-vs-code/preferences-snippets.png)
   
    Z opcji wybierz pozycję **JSON**.
   
    ![wybieranie pozycji JSON](./media/resource-manager-vs-code/select-json.png)
4. Wklej zawartość pliku z kroku 1 do pliku z fragmentami kodu użytkownika przed końcowym znakiem „}” 
5. Sprawdź, czy kod JSON wygląda poprawnie i czy nigdzie nie widać podkreśleń. 
6. Zapisz i zamknij plik z fragmentami kodu użytkownika.

To wszystko, co jest potrzebne do rozpoczęcia korzystania z fragmentów kodu usługi Resource Manager. Teraz przetestujemy tę konfigurację.

## Praca z szablonem w programie VS Code
Najprostszym sposobem na rozpoczęcie pracy z szablonem jest pobranie jednego z szablonów Szybki start dostępnych w usłudze [Github](https://github.com/Azure/azure-quickstart-templates) bądź użycie własnego szablonu. Za pośrednictwem portalu można łatwo [wyeksportować szablon](resource-manager-export-template.md) dla dowolnej grupy zasobów. 

1. Jeśli szablon został wyeksportowany na podstawie grupy zasobów, otwórz wyodrębnione pliki w programie VS Code.
   
    ![wyświetlone pliki](./media/resource-manager-vs-code/show-files.png)
2. Otwórz plik template.json, aby można go było edytować i dodawać zasoby. Po ciągu **"resources": [** naciśnij klawisz Enter, aby rozpocząć nowy wiersz. Jeśli wpiszesz ciąg **arm**, zostanie wyświetlona lista opcji. Te opcje to zainstalowane fragmenty kodu szablonu. Powinny wyglądać następująco: 
   
    ![wyświetlone fragmenty kodu](./media/resource-manager-vs-code/type-snippets.png)
3. Wybierz odpowiedni fragment kodu. Na potrzeby tego artykułu wybiorę fragment **arm-ip**, aby utworzyć nowy publiczny adres IP. Po zamykającym nawiasie klamrowym „}” nowo utworzonego zasobu umieść przecinek, aby składnia szablonu była prawidłowa.
   
     ![dodawanie przecinka](./media/resource-manager-vs-code/add-comma.png)
4. W programie VS Code jest wbudowana funkcja IntelliSense. Podczas edytowania szablonów program VS Code podpowiada dostępne wartości. Na przykład aby dodać do szablonu sekcję zmiennych, wprowadź znaki **""** (dwa podwójne cudzysłowy) i naciśnij między nimi kombinację klawiszy **Ctrl+spacja**. Zostanie wyświetlona lista obejmująca opcje **variables** (zmienne).
   
    ![dodawanie zmiennych](./media/resource-manager-vs-code/add-variables.png)
5. Funkcja IntelliSense może także podpowiedzieć dostępne wartości lub funkcje. Aby przypisać właściwość jako wartość parametru, utwórz wyrażenie, wpisując znaki **"[]"**, i naciśnij klawisze **Ctrl+spacja**. Możesz zacząć wpisywać nazwę funkcji. Po znalezieniu odpowiedniej funkcji naciśnij klawisz **Tab**.
   
    ![dodawanie parametru](./media/resource-manager-vs-code/select-parameters.png)
6. Ponownie naciśnij klawisze **Ctrl+spacja** w obrębie funkcji, aby wyświetlić listę parametrów dostępnych w ramach szablonu.
   
    ![dodawanie parametru](./media/resource-manager-vs-code/select-avail-parameters.png)
7. Jeśli w szablonie występują problemy ze sprawdzaniem poprawności schematu, w edytorze zostaną wyświetlone znajome podkreślenia. Aby wyświetlić listę błędów i ostrzeżeń, naciśnij klawisze **Ctrl+Shift+M** lub wybierz symbole znajdujące się na lewym dolnym pasku stanu.
   
    ![błędy](./media/resource-manager-vs-code/errors.png)
   
    Sprawdzanie poprawności szablonu może pomóc w wykryciu błędów składniowych, jednak niektóre z wyświetlanych błędów można zignorować. Czasami edytor porównuje szablon z nieaktualnym schematem i dlatego zgłasza błąd, nawet jeśli wiesz, że szablon jest poprawny. Załóżmy na przykład, że do usługi Resource Manager dodano ostatnio funkcję, ale nie zaktualizowano schematu. Edytor zgłosi błąd pomimo faktu, że podczas wdrażania funkcja działa prawidłowo.
   
    ![komunikat o błędzie](./media/resource-manager-vs-code/unrecognized-function.png)

## Wdrażanie nowych zasobów
Gdy szablon będzie gotowy, możesz wdrożyć nowe zasoby, postępując zgodnie z poniższymi instrukcjami: 

### Windows
1. Otwórz wiersz polecenia programu PowerShell 
2. Aby się zalogować, wpisz polecenie: 
   
        Login-AzureRmAccount 
3. Jeśli masz wiele subskrypcji, uzyskaj ich listę przy użyciu polecenia:
   
        Get-AzureRmSubscription
   
    Wybierz subskrypcję, której chcesz użyć.
   
        Select-AzureRmSubscription -SubscriptionId <Subscription Id>
4. Zaktualizuj parametry w pliku parameters.json
5. Uruchom plik Deploy.ps1, aby wdrożyć szablon na platformie Azure

### OSX/Linux
1. Otwórz okno terminalu 
2. Aby się zalogować, wpisz polecenie:
   
        azure login 
3. Jeśli masz wiele subskrypcji, wybierz odpowiednią z nich za pomocą polecenia:
   
        azure account set <subscriptionNameOrId> 
4. Zaktualizuj parametry w pliku parameters.json.
5. Aby wdrożyć szablon, uruchom polecenie:
   
        azure group deployment create -f <PathToTemplate> 

## Następne kroki
* Aby uzyskać więcej informacji o szablonach, zobacz [Authoring Azure Resource Manager templates](resource-group-authoring-templates.md) (Tworzenie szablonów usługi Azure Resource Manager).
* Aby poznać funkcje szablonu, zobacz [Azure Resource Manager template functions](resource-group-template-functions.md) (Funkcje szablonu usługi Azure Resource Manager).
* Aby uzyskać więcej przykładów korzystania z programu Visual Studio Code, zobacz [Build cloud apps with Visual Studio Code](https://github.com/Microsoft/HealthClinic.biz/wiki/Build-cloud-apps-with-Visual-Studio-Code) (Tworzenie aplikacji chmurowych w programie Visual Studio Code) w [pokazie](https://blogs.msdn.microsoft.com/visualstudio/2015/12/08/connectdemos-2015-healthclinic-biz/) 2015 Connect witryny [HealthClinic.biz](https://github.com/Microsoft/HealthClinic.biz). Aby uzyskać więcej materiałów szybkiego startu z w wersji demonstracyjnej witryny HealthClinic.biz, zobacz [Azure Developer Tools Quickstarts](https://github.com/Microsoft/HealthClinic.biz/wiki/Azure-Developer-Tools-Quickstarts) (Materiały szybkiego startu narzędzi deweloperskich platformy Azure).

<!--HONumber=Sep16_HO4-->


