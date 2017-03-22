---
title: "Wprowadzenie do wtyczki środowiska Eclipse dla usługi Azure Service Fabric | Microsoft Docs"
description: "Wprowadzenie do wtyczki środowiska Eclipse dla usługi Azure Service Fabric."
services: service-fabric
documentationcenter: java
author: sayantancs
manager: timlt
editor: 
ms.assetid: bf84458f-4b87-4de1-9844-19909e368deb
ms.service: service-fabric
ms.devlang: java
ms.topic: get-started-article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 12/27/2016
ms.author: saysa
translationtype: Human Translation
ms.sourcegitcommit: 24d86e17a063164c31c312685c0742ec4a5c2f1b
ms.openlocfilehash: 891df38aa685cac7bbfecb71b15c88d557ac493b
ms.lasthandoff: 03/11/2017


---

# <a name="getting-started-with-eclipse-plugin-for-service-fabric-java-application-development"></a>Wprowadzenie do programowania aplikacji Java za pomocą wtyczki środowiska Eclipse dla usługi Service Fabric
Środowisko Eclipse to jedno ze zintegrowanych środowisk projektowych najczęściej używanych przez deweloperów języka Java. W tym artykule omówiono konfigurowanie środowiska deweloperskiego Eclipse do pracy z usługą Service Fabric. Ten artykuł zawiera informacje na temat instalowania wtyczki usługi Service Fabric, tworzenia aplikacji usługi Service Fabric i wdrażania jej w lokalnym lub zdalnym klastrze usługi Service Fabric.

## <a name="install-or-update-service-fabric-plugin-on-eclipse-neon"></a>Instalowanie lub aktualizowanie wtyczki usługi Service Fabric w środowisku Eclipse Neon
Usługa Service Fabric udostępnia wtyczkę dla środowiska **Eclipse IDE for Java Developers**, która upraszcza proces kompilowania i wdrażania usług Java.

1. Upewnij się, że masz zainstalowaną najnowszą wersję środowiska Eclipse **Neon** i zestawu Buildship (1.0.17 lub nowszą). Wersje zainstalowanych składników możesz sprawdzić, wybierając pozycję ``Help => Installation Details`` (Pomoc > Szczegóły instalacji). Zestaw Buildship można zaktualizować, postępując zgodnie z instrukcjami znajdującymi się [tutaj][buildship-update]. Aby sprawdzić i zaktualizować środowisko Eclipse Neon do najnowszej wersji, możesz użyć pozycji ``Help => Check for Updates`` (Pomoc > Sprawdź dostępność aktualizacji).

2. Aby zainstalować wtyczkę usługi Service Fabric, wybierz pozycję ``Help => Install New Software...`` (Pomoc > Instaluj nowe oprogramowanie...).
  1. W polu tekstowym „Work with” (Pracuj z) wpisz: ``http://dl.windowsazure.com/eclipse/servicefabric``.
  2. Kliknij pozycję Add (Dodaj).

  ![Wtyczka środowiska Eclipse Neon dla usługi Service Fabric][sf-eclipse-plugin-install]

  3. Wybierz wtyczkę usługi Service Fabric i kliknij przycisk Next (Dalej).
  4. Postępuj zgodnie z instrukcjami instalacji i zaakceptuj umowę licencyjną użytkownika końcowego.

Jeśli wtyczka środowiska Eclipse dla usługi Service Fabric jest już zainstalowana, upewnij się, że używasz najnowszej wersji. Dostępność aktualizacji możesz sprawdzić za pomocą pozycji ``Help => Installation Details`` (Pomoc > Szczegóły instalacji). Następnie wyszukaj usługę Service Fabric na liście zainstalowanych wtyczek i kliknij polecenie aktualizacji. Jeśli istnieje oczekująca aktualizacja, to zostanie pobrana i zainstalowana.

> [!NOTE]
> Jeśli instalacja lub aktualizacja wtyczki środowiska Eclipse dla usługi Service Fabric w środowisku Eclipse zajmuje dużo czasu, jest to spowodowane każdorazowym pobieraniem przez środowisko Eclipse metadanych wszystkich nowych zmian, które zostały wprowadzone we wszystkich witrynach aktualizacji zarejestrowanych w używanym wystąpieniu środowiska Eclipse. Aby przyspieszyć te procesy za pomocą prostego obejścia, możesz przejść do pozycji `Available Software Sites` (Dostępne witryny aktualizacji) i usunąć zaznaczenie wszystkich pozycji poza tymi, które wskazują lokalizację wtyczki usługi Service Fabric: `http://dl.windowsazure.com/eclipse/servicefabric`.
>

## <a name="create-service-fabric-application-using-eclipse"></a>Tworzenie aplikacji usługi Service Fabric przy użyciu środowiska Eclipse

1. Przejdź do pozycji ``File => New => Other`` (Plik > Nowy > Inny). Wybierz pozycję ``Service Fabric Project`` (Projekt usługi Service Fabric). Kliknij pozycję ``Next`` (Dalej).

    ![Nowy projekt usługi Service Fabric — strona 1][create-application/p1]

2. Nadaj projektowi nazwę. Kliknij pozycję ``Next`` (Dalej).

    ![Nowy projekt usługi Service Fabric — strona 2][create-application/p2]

3. Wybierz pozycję Service Template (Szablon usługi) z zestawu dostępnych szablonów: Actor (Aktor), Stateless (Bezstanowa), Container (Kontener) lub Guest-executable (Wykonywalna-gość). Kliknij pozycję ``Next`` (Dalej).

    ![Nowy projekt usługi Service Fabric — strona 3][create-application/p3]

4. Podaj nazwę usługi i/lub odpowiednie szczegóły usługi na tej stronie, a następnie kliknij pozycję ``Finish`` (Zakończ).

    ![Nowy projekt usługi Service Fabric — strona 4][create-application/p4]

5. Po utworzeniu pierwszego projektu usługi Service Fabric zostanie wyświetlony monit z pytaniem o ustawienie perspektywy usługi Service Fabric — wybierz pozycję ``yes`` (Tak), aby kontynuować.

    ![Nowy projekt usługi Service Fabric — strona 5][create-application/p5]

6. Po pomyślnym utworzeniu projekt wygląda następująco —

    ![Nowy projekt usługi Service Fabric — strona 6][create-application/p6]

## <a name="build-and-deploy-the-service-fabric-application-using-eclipse"></a>Kompilowanie i wdrażanie aplikacji usługi Service Fabric przy użyciu środowiska Eclipse

* Kliknij prawym przyciskiem myszy właśnie utworzoną aplikację usługi Service Fabric. Wybierz pozycję ``Service Fabric`` (Usługa Service Fabric) w menu kontekstowym. Spowoduje to wyświetlenie podmenu z wieloma opcjami. Wygląda ono następująco —

    ![Menu prawego przycisku myszy usługi Service Fabric][publish/RightClick]

  Po kliknięciu opcji kompilacji, ponownej kompilacji i czyszczenia zostaną wykonane odpowiednie akcje.
  - Pozycja ``Build Application`` (Kompiluj aplikację) umożliwia skompilowanie aplikacji bez czyszczenia
  - Pozycja ``Rebuild Application`` (Kompiluj ponownie aplikację) umożliwia skompilowanie aplikacji z wcześniejszym czyszczeniem
  - Pozycja ``Clean Application`` (Wyczyść aplikację) umożliwia wyczyszczenie artefaktów kompilacji z aplikacji


* To menu umożliwia także wdrożenie, cofnięcie wdrożenia i opublikowanie aplikacji.
  - Pozycja ``Deploy Application`` (Wdróż aplikację) umożliwia wdrożenie aplikacji do lokalnego klastra
  - Pozycja ``Publish Application...`` (Publikuj aplikację...) umożliwia wybranie profilu publikowania: ``Local.json`` lub ``Cloud.json``. Te pliki JSON służą do przechowywania informacji (takich jak punkty końcowe połączenia i informacje o zabezpieczeniach) wymaganych do nawiązania połączenia z klastrem lokalnym lub w chmurze (Azure).

  ![Menu prawego przycisku myszy usługi Service Fabric][publish/Publish]

* Istnieje alternatywny sposób wdrożenia aplikacji usługi Service Fabric za pomocą konfiguracji uruchamiania środowiska Eclipse.

  1. Wybierz pozycję ``Run => Run Configurations`` (Uruchom > Konfiguracje uruchamiania). Wybierz konfigurację uruchamiania ``ServiceFabricDeployer`` (Wdrażanie usługi Service Fabric) w ramach pozycji ``Grade Project`` (Projekt Gradle).
  2. Na karcie ``Arguments`` (Argumenty) w prawym okienku określ opcję **local** (lokalna) lub **cloud** (chmura) dla pozycji ``publishProfile`` (Profil publikowania). Domyślna konfiguracja to **local** (lokalna). W przypadku wdrażania w klastrze zdalnym lub w chmurze wybierz opcję **cloud** (chmura).
  3. Upewnij się, że w profilach publikowania zostały umieszczone odpowiednie informacje, w tym szczegóły punktu końcowego i poświadczenia zabezpieczeń, jeśli takie są dostępne, sprawdzając odpowiednio plik `Local.json` lub `Cloud.json`.
  4. Upewnij się, że pozycja ``Working Directory`` (Katalog roboczy) w prawym okienku w obszarze ``Grade Project`` (Projekt Gradle) wskazuje aplikację do wdrożenia. Jeśli nie, kliknij przycisk ``Workspace...`` (Obszar roboczy...) i wybierz aplikację.
  5. Kliknij polecenia **Apply** (Zastosuj) i **Run** (Uruchom).

Aplikacja zostanie skompilowana i wdrożona w ciągu kilku minut. Możesz monitorować jej stan z poziomu narzędzia Service Fabric Explorer.  

## <a name="add-new-service-fabric-service-to-your-service-fabric-application"></a>Dodawanie nowej usługi Service Fabric do aplikacji usługi Service Fabric

Usługę Service Fabric można dodać do istniejącej aplikacji usługi Service Fabric, wykonując następujące kroki:

1. Kliknij prawym przyciskiem myszy projekt do dodania do usługi, aby otworzyć menu kontekstowe, i wybierz opcję „Service Fabric” (Usługa Service Fabric). Spowoduje to wyświetlenie podmenu z wieloma opcjami.

    ![Dodawanie usługi Service Fabric — strona 1][add-service/p1]

2. Wybierz opcję `Add ServiceFabric Service` (Dodaj usługę Service Fabric). Wykonaj kolejne wyświetlane kroki, aby dodać usługę do projektu.
3. Wybierz szablon usługi do dodania do projektu i kliknij pozycję „Next” (Dalej).

    ![Dodawanie usługi Service Fabric — strona 2][add-service/p2]

4. Podaj nazwę usługi (i inne potrzebne szczegóły tam, gdzie będzie to wymagane) i kliknij przycisk „Add Service” (Dodaj usługę) poniżej.  

    ![Dodawanie usługi Service Fabric — strona 3][add-service/p3]

5. Po pomyślnym dodaniu usługi struktura całego projektu będzie wyglądać podobnie do poniższej —

    ![Dodawanie usługi Service Fabric — strona 4][add-service/p4]

## <a name="upgrade-your-service-fabric-java-application"></a>Uaktualnianie aplikacji Java usługi Service Fabric

Dla przykładu przyjmijmy, że utworzono projekt ``App1`` za pomocą wtyczki środowiska Eclipse dla usługi Service Fabric i wdrożono go za pomocą wtyczki w celu utworzenia aplikacji o nazwie ``fabric:/App1Application`` z typem aplikacji ``App1AppicationType`` i parametrem application-version równym 1.0. Teraz można uaktualnić aplikację bez konieczności wyłączania jej.

Wprowadź zmiany w aplikacji i skompiluj ponownie zmodyfikowaną usługę.  Zaktualizuj manifest zmodyfikowanej usługi (``ServiceManifest.xml``) za pomocą zaktualizowanych wersji usługi (i kodu, konfiguracji lub danych — odpowiednio do potrzeb). Zmodyfikuj także manifest aplikacji (``ApplicationManifest.xml``), określając zaktualizowany numer wersji dla aplikacji i zmodyfikowaną usługę.  

W celu uaktualnienia aplikacji za pomocą środowiska Eclipse możesz utworzyć duplikat konfiguracji uruchamiania i użyć go do uaktualnienia aplikacji odpowiednio do potrzeb, wykonując następujące kroki —
1. Wybierz pozycję ``Run => Run Configurations`` (Uruchom > Konfiguracje uruchamiania). Kliknij małą strzałkę po lewej stronie pozycji ``Grade Project`` (Projekt Gradle) w lewym okienku.
2. Kliknij prawym przyciskiem myszy pozycję ``ServiceFabricDeployer`` (Wdrażanie usługi Service Fabric) i wybierz polecenie ``Duplicate`` (Duplikuj). Nadaj nową nazwę tej konfiguracji, np. ``ServiceFabricUpgrader`` (Uaktualnienie usługi Service Fabric).
3. W prawym panelu na karcie ``Arguments`` (Argumenty) zmień wpis ``-Pconfig='deploy'`` na ``-Pconfig=upgrade`` i wybierz polecenie ``Apply`` (Zastosuj).
4. Konfiguracja uruchamiania na potrzeby uaktualniania aplikacji została utworzona i zapisana — teraz można użyć dla niej polecenia ``Run`` (Uruchom). Ten proces obsłuży pobranie najnowszej wartości parametru wersji application-type z pliku application-manifest.

Teraz możesz monitorować uaktualnianie aplikacji przy użyciu eksploratora usługi Service Fabric. W ciągu kilku minut aplikacja zostanie zaktualizowana.

<!-- Images -->

[sf-eclipse-plugin-install]: ./media/service-fabric-get-started-mac/sf-eclipse-plugin-install.png

[create-application/p1]:./media/service-fabric-get-started-eclipse/create-application/p1.png
[create-application/p2]:./media/service-fabric-get-started-eclipse/create-application/p2.png
[create-application/p3]:./media/service-fabric-get-started-eclipse/create-application/p3.png
[create-application/p4]:./media/service-fabric-get-started-eclipse/create-application/p4.png
[create-application/p5]:./media/service-fabric-get-started-eclipse/create-application/p5.png
[create-application/p6]:./media/service-fabric-get-started-eclipse/create-application/p6.png

[publish/Publish]: ./media/service-fabric-get-started-eclipse/publish/Publish.png
[publish/RightClick]: ./media/service-fabric-get-started-eclipse/publish/RightClick.png

[add-service/p1]: ./media/service-fabric-get-started-eclipse/add-service/p1.png
[add-service/p2]: ./media/service-fabric-get-started-eclipse/add-service/p2.png
[add-service/p3]: ./media/service-fabric-get-started-eclipse/add-service/p3.png
[add-service/p4]: ./media/service-fabric-get-started-eclipse/add-service/p4.png

<!-- Links -->
[buildship-update]: https://projects.eclipse.org/projects/tools.buildship

