---
title: "Ciągłego dostarczania dla chmury usługi z programem TFS na platformie Azure | Dokumentacja firmy Microsoft"
description: "Dowiedz się, jak skonfigurować ciągłego dostarczania dla aplikacji w chmurze Azure. Przykłady kodu dla instrukcji wiersza polecenia programu MSBuild i skryptów programu PowerShell."
services: cloud-services
documentationcenter: 
author: kraigb
manager: ghogen
editor: 
ms.assetid: 4f3c93c6-5c82-4779-9d19-7404a01e82ca
ms.service: cloud-services
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: article
ms.date: 06/12/2017
ms.author: kraigb
ms.openlocfilehash: 0979722b9ec715e91825c7aba74657451df6e83f
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/11/2017
---
# <a name="continuous-delivery-for-cloud-services-in-azure"></a>Ciągłego dostarczania dla usług w chmurze na platformie Azure
Proces opisany w tym artykule pokazano, jak skonfigurować ciągłego dostarczania dla aplikacji w chmurze Azure. Ten proces umożliwia automatyczne tworzenie pakietów i wdrożenie pakietu w systemie Azure po każdym zaewidencjonowaniu kodu. Proces kompilacji pakietu, które są opisane w tym artykule jest odpowiednikiem **pakietu** polecenie w programie Visual Studio i kroki publikowania są równoważne **publikowania** polecenia w programie Visual Studio.
Artykuł obejmuje metody ma zostać użyty do utworzenia serwera kompilacji MSBuild instrukcje wiersza polecenia i skryptów programu Windows PowerShell, a on również pokazano, jak opcjonalnie skonfigurować Visual Studio Team Foundation Server - Team Build definicje do użycia MSBuild poleceń i skryptów programu PowerShell. Ten proces jest można dostosować do własnego środowiska kompilacji i środowisk docelowej platformy Azure.

Można również użyć programu Visual Studio Team Services, wersji programu TFS, która jest hostowana na platformie Azure, aby łatwiej. 

Przed rozpoczęciem należy opublikować aplikacji z programu Visual Studio.
Zapewni to, że wszystkie zasoby są dostępne i została zainicjowana przy próbie zautomatyzować proces publikacji.

## <a name="1-configure-the-build-server"></a>1: Konfigurowanie serwera kompilacji
Przed utworzeniem pakietu Azure przy użyciu programu MSBuild, należy zainstalować wymaganego oprogramowania i narzędzi na serwerze kompilacji.

Program Visual Studio nie jest wymagane do zainstalowania na serwerze kompilacji. Jeśli chcesz użyć do zarządzania serwerem kompilacji usługi kompilacji programu Team Foundation, postępuj zgodnie z instrukcjami [usługi kompilacji programu Team Foundation] [ Team Foundation Build Service] dokumentacji.

1. Zainstalować na serwerze kompilacji [.NET Framework 4.5.2][.NET Framework 4.5.2], która obejmuje MSBuild.
2. Zainstaluj najnowszą [Azure Authoring Tools dla programu .NET](https://azure.microsoft.com/develop/net/).
3. Zainstaluj [bibliotek platformy Azure dla platformy .NET](http://go.microsoft.com/fwlink/?LinkId=623519).
4. Skopiuj plik Microsoft.WebApplication.targets z instalacją programu Visual Studio do serwera kompilacji.

   Na komputerze z programem Visual Studio zainstalowany, ten plik znajduje się w katalogu C:\\Program Files(x86)\\MSBuild\\Microsoft\\VisualStudio\\14.0\\WebApplications. Skopiuj go do tego samego katalogu na serwerze kompilacji.
5. Zainstaluj [Azure Tools dla Visual Studio](https://www.visualstudio.com/features/azure-tools-vs.aspx).

## <a name="2-build-a-package-using-msbuild-commands"></a>2: Tworzenie pakietu przy użyciu polecenia programu MSBuild
Ta sekcja opisuje sposób tworzenia polecenie MSBuild, która tworzy pakiet Azure. Wykonaj ten krok, na serwerze kompilacji, aby sprawdzić, czy wszystko jest poprawnie skonfigurowany i czy wykonywanie polecenia programu MSBuild, co ma wykonania. Można dodać ten wiersz polecenia do istniejących skryptów kompilacji na serwerze kompilacji lub użyć wiersza polecenia w definicji kompilacji TFS, zgodnie z opisem w następnej sekcji. Aby uzyskać więcej informacji na temat parametrów wiersza polecenia i MSBuild, zobacz [informacje w wierszu polecenia programu MSBuild](https://msdn.microsoft.com/library/ms164311%28v=vs.140%29.aspx).

1. Jeśli program Visual Studio jest zainstalowany na serwerze kompilacji, Znajdź i wybierz **wiersz polecenia programu Visual Studio** w **programu Visual Studio Tools** folderu systemu Windows.

   Jeśli program Visual Studio nie jest zainstalowany na serwerze kompilacji, otwórz wiersz polecenia i upewnij się, że MSBuild.exe jest dostępny w ścieżce. Platformy .NET Framework w ścieżce % WINDIR % jest zainstalowany program MSBuild\\Microsoft.NET\\Framework\\*wersji*. Na przykład aby dodać MSBuild.exe do zmiennej środowiskowej PATH, jeśli masz zainstalowany program .NET Framework 4, wpisz następujące polecenie w wierszu polecenia:

       set PATH=%PATH%;"C:\Windows\Microsoft.NET\Framework\v4.0.30319"
2. W wierszu polecenia przejdź do folderu zawierającego plik projektu platformy Azure, który chcesz utworzyć.
3. Uruchom program MSBuild z opcji/target: publikowanie opcja jak w poniższym przykładzie:

       MSBuild /target:Publish

   Tej opcji można stosować skrót /t: publikowania. Opcja /t:Publish w programie MSBuild nie należy mylić z polecenia Opublikuj dostępne w programie Visual Studio, jeśli masz zainstalowany zestaw SDK usługi Azure. /T: publikowanie tylko kompilacje opcji Azure pakietów. Nie wdrażać pakiety jak polecenia Opublikuj w programie Visual Studio.

   Opcjonalnie można określić nazwę projektu jako parametr MSBuild. Jeśli nie zostanie określony, używany jest bieżący katalog. Aby uzyskać więcej informacji na temat opcji wiersza polecenia programu MSBuild, zobacz [informacje w wierszu polecenia programu MSBuild](https://msdn.microsoft.com/library/ms164311%28v=vs.140%29.aspx).
4. Znajdź dane wyjściowe. Domyślnie to polecenie tworzy katalog w odniesieniu do folderu głównego dla projektu, takie jak *ProjectDir*\\bin\\*konfiguracji*\\app.publish \\. Podczas tworzenia projektu platformy Azure, generowanie dwóch plików, sam plik pakietu i towarzyszący plik konfiguracji:

   * Project.cspkg
   * Element ServiceConfiguration. *TargetProfile*.cscfg

   Domyślnie każdy projekt Azure zawiera jeden usługi konfiguracji pliku (cscfg) lokalne kompilacje (debugowanie) i drugi dla kompilacji chmury (tymczasowym czy produkcyjnym), ale można dodać lub usunąć pliki konfiguracji usługi, zgodnie z potrzebami. Podczas tworzenia pakietu w programie Visual Studio, użytkownik zostanie poproszony które pliku konfiguracji usługi, aby uwzględnić obok pakietu.
5. Określ plik konfiguracji usługi. Podczas tworzenia pakietu przy użyciu programu MSBuild, domyślnie znajduje się plik konfiguracji Usługa lokalna. Aby dołączyć plik konfiguracji innej usługi, należy ustawić właściwość TargetProfile polecenia programu MSBuild, jak w poniższym przykładzie:

       MSBuild /t:Publish /p:TargetProfile=Cloud
6. Określ lokalizację dla danych wyjściowych. Ustawianie ścieżki przy użyciu /p:PublishDir =*katalogu* \\ opcji, w tym końcowe separatora ukośnik odwrotny, jak w poniższym przykładzie:

       MSBuild /target:Publish /p:PublishDir=\\myserver\drops\

   Po utworzone i przetestowane odpowiedni wiersz polecenia programu MSBuild do tworzenia projektów i połączyć je w pakiecie Azure, można dodać ten wiersz polecenia do skrypty kompilacji. Jeśli serwer kompilacji obsługuje skrypty niestandardowe, ten proces będzie zależeć od specyfiki procesu kompilacji niestandardowej. Jeśli używasz TFS jako środowiska kompilacji, można postępuj zgodnie z instrukcjami w następnym kroku, aby dodać pakiet Azure kompilacji do procesu kompilacji.

## <a name="3-build-a-package-using-tfs-team-build"></a>3: Tworzenie pakietu za pomocą kompilacji TFS zespołu
Jeśli masz skonfigurowany jako kontroler kompilacji i serwer kompilacji Team Foundation Server (TFS), skonfigurować jako maszyna kompilacji TFS, a następnie możesz opcjonalnie skonfigurować automatyczne kompilacji pakietu Azure. Aby uzyskać informacje na temat sposobu konfigurowania i używania jako system kompilacji Team Foundation server, zobacz [skalowania w poziomie system kompilacji][Scale out your build system]. W szczególności w poniższej procedurze przyjęto, że na serwerze kompilacji zostało skonfigurowane zgodnie z opisem w [Wdróż i skonfiguruj serwer kompilacji][Deploy and configure a build server], i utworzyć projektu zespołowego, należy utworzyć chmury projekt usługi w projekcie zespołowym.

Aby skonfigurować TFS do tworzenia pakietów platformy Azure, wykonaj następujące czynności:

1. W programie Visual Studio na komputerze deweloperskim, w menu Widok wybierz polecenie **Team Explorer**, lub wybierz polecenie Ctrl +\\, Ctrl + M. W oknie programu Team Explorer rozwiń **kompilacje** węzła lub wybierz **kompilacje** stronie, a następnie wybierz **nową definicję kompilacji**.

   ![Nowa opcja definicji kompilacji][0]
2. Wybierz **wyzwalacza** , a następnie określić żądaną warunki, gdy chcesz pakietu, który ma zostać utworzony. Na przykład określić **ciągłej integracji** do utworzenia pakietu przy każdym kontroli źródła zaewidencjonowania występuje.
3. Wybierz **ustawienia źródła** karcie i upewnij się, że folder projektu jest wymieniony w **folderu kontroli źródła** kolumny, a stan ma **Active**.
4. Wybierz **kompilacji domyślne** , a następnie w obszarze kontroler kompilacji, sprawdź nazwę serwera kompilacji.  Ponadto wybierz opcję **Kopiuj dane wyjściowe kompilacji do następującego folderu docelowego** i określ lokalizację docelową żądany.
5. Wybierz **procesu** kartę. Na karcie procesu wybierz szablon domyślny, w obszarze **kompilacji**, wybierz projekt, jeśli nie została jeszcze wybrana, a rozwiń **zaawansowane** sekcji **kompilacji** sekcji siatki.
6. Wybierz **argumenty programu MSBuild**i ustaw odpowiednie argumenty wiersza polecenia programu MSBuild, zgodnie z opisem w kroku 2. powyżej. Na przykład wprowadź **/t: publikowanie /p:PublishDir =\\\\MójSerwer\\porzuca\\**  do tworzenia pakietu i skopiuj pliki pakietu do lokalizacji \\ \\ MójSerwer\\porzuca\\:

   ![Argumenty programu MSBuild][2]

   > [!NOTE]
   > Kopiowanie plików do udziału publicznego ułatwia ręcznie wdrażania pakietów na komputerze projektowym.
7. Testowanie Powodzenie z kroku kompilacji sprawdzając w przypadku zmiany do projektu lub kolejki nowej kompilacji. Do kolejki z nowej kompilacji w programie Team Explorer, kliknij prawym przyciskiem myszy **wszystkie definicje kompilacji,** , a następnie wybierz **Tworzenie nowej kolejki**.

## <a name="4-publish-a-package-using-a-powershell-script"></a>4: publikowanie pakietu przy użyciu skryptu programu PowerShell
Ta sekcja opisuje sposób tworzenia skryptu programu Windows PowerShell, która będzie publikować dane wyjściowe pakietu aplikacji chmurze na platformie Azure przy użyciu parametrów opcjonalnych. Ten skrypt można wywołać po wykonaniu kroku kompilacji w Twojej automatyzacji niestandardowej kompilacji. Można również nadać mu z szablonu procesu działania przepływu pracy w Visual Studio TFS Team Build.

1. Zainstaluj [poleceń cmdlet programu Azure PowerShell] [ Azure PowerShell cmdlets] (v0.6.1 lub nowszej).
   Podczas fazy instalacji polecenia cmdlet należy wybrać opcję instalacji jako przystawki. Należy zwrócić uwagę, czy tego oficjalnie obsługiwana wersja zastępuje starszej wersji oferowane za pośrednictwem portalu CodePlex, chociaż poprzednie wersje zostały numerowane 2.x.x.
2. Uruchom program Azure PowerShell przy użyciu Start menu lub strony początkowej. Jeśli uruchamiasz w ten sposób, poleceń cmdlet programu Azure PowerShell zostanie załadowany.
3. W wierszu polecenia programu PowerShell, sprawdź, czy polecenia cmdlet programu PowerShell są załadowane przez wprowadzenie polecenia częściowe `Get-Azure` , a następnie naciśnięcie klawisza Tab do uzupełniania instrukcji.

   Po naciśnięciu klawisza Tab wielokrotnie, powinny pojawić różne polecenia programu PowerShell systemu Azure.
4. Sprawdź, czy możesz nawiązać subskrypcji platformy Azure, przez zaimportowanie informacji o subskrypcji z pliku .publishsettings.

   `Import-AzurePublishSettingsFile c:\scripts\WindowsAzure\default.publishsettings`

   Następnie wprowadź polecenie

   `Get-AzureSubscription`

   Oznacza to, informacje o Twojej subskrypcji. Sprawdź, czy wszystkie ustawienia są poprawne.
5. Zapisz szablon skryptu podana na końcu tego artykułu w folderze skryptów jako c:\\skryptów\\WindowsAzure\\**PublishCloudService.ps1**.
6. Zapoznaj się z sekcją Parametry skryptu. Dodaj lub zmodyfikuj wartości domyślne. Te wartości zawsze może być zastąpiona przez przekazywanie parametrów jawnych.
7. Upewnij się, są prawidłową chmurą konta usługi i Magazyn tworzone w ramach subskrypcji, która może być celem skryptu publikowania. Konto magazynu (magazynu obiektów blob) będzie używany do przekazywania i tymczasowe przechowywanie wdrożenia pakietu i pliku konfiguracji podczas tworzenia wdrożenia.

   * Aby utworzyć nową usługę w chmurze, można wywołać tego skryptu lub użyć [portalu Azure](https://portal.azure.com). Nazwa usługi w chmurze będzie służyć jako prefiksu w pełni kwalifikowaną nazwę domeny i dlatego musi być unikatowa.

         New-AzureService -ServiceName "mytestcloudservice" -Location "North Central US" -Label "mytestcloudservice"
   * Aby utworzyć nowe konto magazynu, można wywołać tego skryptu lub użyć [portalu Azure](https://portal.azure.com). Nazwa konta magazynu będzie służyć jako prefiksu w pełni kwalifikowaną nazwę domeny i dlatego musi być unikatowa. Można spróbować użyć takiej samej nazwie jako usługa w chmurze.

         New-AzureStorageAccount -ServiceName "mytestcloudservice" -Location "North Central US" -Label "mytestcloudservice"
8. Wywołanie skryptu bezpośrednio z programu Azure PowerShell lub połączenie się tego skryptu do kompilacji hosta automatyzacji wystąpić po kompilacji pakietu.

   > [!IMPORTANT]
   > Skrypt będzie zawsze usunąć lub zamienić istniejący wdrożeń domyślnie, jeśli ich wykrycia. Jest to niezbędne do włączenia ciągłego dostarczania automatyzacji, jeśli to możliwe nie monitowania użytkownika.
   >
   >

   **Przykładowy scenariusz 1:** ciągłe wdrażanie w środowisku przemieszczania usługi:

       PowerShell c:\scripts\windowsazure\PublishCloudService.ps1 -environment Staging -serviceName mycloudservice -storageAccountName mystoragesaccount -packageLocation c:\drops\app.publish\ContactManager.Azure.cspkg -cloudConfigLocation c:\drops\app.publish\ServiceConfiguration.Cloud.cscfg -subscriptionDataFile c:\scripts\default.publishsettings

   To jest zazwyczaj następuje testu weryfikacji i wymiany wirtualnych adresów IP. Wymiany wirtualnych adresów IP może odbywać się za pośrednictwem [portalu Azure](https://portal.azure.com) lub za pomocą polecenia cmdlet Move-wdrożenia.

   **Przykładowy scenariusz 2:** ciągłego wdrażania do środowiska produkcyjnego usługi dedykowanych testu

       PowerShell c:\scripts\windowsazure\PublishCloudService.ps1 -environment Production -enableDeploymentUpgrade 1 -serviceName mycloudservice -storageAccountName mystorageaccount -packageLocation c:\drops\app.publish\ContactManager.Azure.cspkg -cloudConfigLocation c:\drops\app.publish\ServiceConfiguration.Cloud.cscfg -subscriptionDataFile c:\scripts\default.publishsettings

   **Pulpit zdalny:**

   Włączenie pulpitu zdalnego w projekcie platformy Azure, konieczne będzie wykonanie dodatkowych kroków jednorazowe, aby upewnić się, że przekazano poprawny certyfikat usługi chmury do wszystkich usług w chmurze celem tego skryptu.

   Znajdź wartości odcisku palca certyfikatu oczekiwany przez poszczególnych ról. Wartości odcisku palca są widoczne w sekcji certyfikaty w pliku konfiguracji chmury (tj. ServiceConfiguration.Cloud.cscfg). Istnieje również wyświetlane w oknie dialogowym Konfiguracja usług pulpitu zdalnego w programie Visual Studio po wyświetleniu opcji i wyświetlanie wybranego certyfikatu.

       <Certificates>
             <Certificate name="Microsoft.WindowsAzure.Plugins.RemoteAccess.PasswordEncryption" thumbprint="C33B6C432C25581601B84C80F86EC2809DC224E8" thumbprintAlgorithm="sha1" />
       </Certificates>

   Przekaż certyfikaty pulpitu zdalnego krokiem jednorazowej konfiguracji za pomocą następującego skryptu apletu polecenia:

       Add-AzureCertificate -serviceName <CLOUDSERVICENAME> -certToDeploy (get-item cert:\CurrentUser\MY\<THUMBPRINT>)

   Na przykład:

       Add-AzureCertificate -serviceName 'mytestcloudservice' -certToDeploy (get-item cert:\CurrentUser\MY\C33B6C432C25581601B84C80F86EC2809DC224E8

   Możesz też wyeksportować plik PFX certyfikatu z kluczem prywatnym i przekaż certyfikaty do każdej docelowej chmury usługi za pomocą [portalu Azure](https://portal.azure.com).

   <!---
   Fixing broken links for Azure content migration from ACOM to DOCS. I'm unable to find a replacement links, so I'm commenting out this reference for now. The author can investigate in the future. "Read the following article to learn more: http://msdn.microsoft.com/library/windowsazure/gg443832.aspx.
   -->
   **Uaktualnienie a wdrożenia. Usunięcie wdrożenia —\> nowego wdrożenia**

   Skrypt będzie domyślnie wykonywać uaktualniania wdrożenia ($enableDeploymentUpgrade = 1) gdy brak parametru jest przekazano lub nie została jawnie przekazana wartość 1. Dla pojedynczego wystąpienia to zaletą jest mniej czasu niż pełne wdrożenie. Dla wystąpień, które wymagają wysokiej dostępności to również zaletą jest pozostawienie niektórych wystąpień uruchomiona, podczas gdy inne uaktualnienia (przejście domenę aktualizacji), a także Twojego adresu VIP, nie zostaną usunięte.

   Wdrożenia uaktualnienia można wyłączyć w skrypcie ($enableDeploymentUpgrade = 0) lub przez przekazanie *- enableDeploymentUpgrade 0* jako parametru, która zmienia zachowanie skryptu, najpierw usuń wszystkie istniejące wdrożenia, a następnie utworzyć nowy wdrożenia.

   > [!IMPORTANT]
   > Skrypt będzie zawsze usunąć lub zamienić istniejący wdrożeń domyślnie, jeśli ich wykrycia. Jest to niezbędne do włączenia ciągłego dostarczania automatyzacji, jeśli to możliwe nie monitowania użytkownika / — operator.
   >
   >

## <a name="5-publish-a-package-using-tfs-team-build"></a>5: publikowanie pakietu za pomocą kompilacji TFS zespołu
Ten opcjonalny krok łączy TFS Team Build do skryptu utworzonego w kroku 4, która obsługuje publikowanie kompilacji pakietu Azure. Dzięki temu modyfikowania szablonu procesu, używany przez definicję kompilacji, wykonywania działań publikowania na końcu przepływu pracy. Działanie publikowania zostanie wykonane polecenie programu PowerShell przekazywanie parametrów z kompilacji. Dane wyjściowe programu MSBuild elementów docelowych i opublikować skrypt będzie przekazywany w potoku do wyjścia standardowego kompilacji.

1. Edytowanie definicji kompilacji odpowiedzialny za ciągłego wdrażania.
2. Wybierz **procesu** kartę.
3. Postępuj zgodnie z [tych instrukcji](http://msdn.microsoft.com/library/dd647551.aspx) można dodać projektu działania dla szablonu procesu kompilacji, Pobierz domyślny szablon, dodaj go do projektu i jego zaewidencjonowania. Nadaj nazwę nowej, takich jak AzureBuildProcessTemplate szablonu procesu kompilacji.
4. Wróć do **procesu** , a następnie użyć **Pokaż szczegóły** wyświetlić listę szablonów procesu kompilacji dostępne. Wybierz **nowych...**  przycisk, a następnie przejdź do projektu po prostu dodane i zaewidencjonowane. Znajdź szablon, który został utworzony i wybierz polecenie **OK**.
5. Otwórz wybrany szablon procesu do edycji. Możesz otworzyć bezpośrednio w Projektancie przepływów pracy lub w edytorze XML, aby pracować z XAML.
6. Dodaj poniższą listę nowych argumentów jako oddzielne pozycje w karcie argumenty projektanta przepływów pracy. Wszystkie argumenty powinny posiadać kierunek = w i wpisz = ciąg. Te posłuży do parametry przepływu z definicji kompilacji do przepływu pracy, które następnie Uzyskaj używane do wywoływania skryptu publikowania.

       SubscriptionName
       StorageAccountName
       CloudConfigLocation
       PackageLocation
       Environment
       SubscriptionDataFileLocation
       PublishScriptLocation
       ServiceName

   ![Lista argumentów][3]

   Odpowiednie XAML wygląda następująco:

       <Activity  _ />
         <x:Members>
           <x:Property Name="BuildSettings" Type="InArgument(mtbwa:BuildSettings)" />
           <x:Property Name="TestSpecs" Type="InArgument(mtbwa:TestSpecList)" />
           <x:Property Name="BuildNumberFormat" Type="InArgument(x:String)" />
           <x:Property Name="CleanWorkspace" Type="InArgument(mtbwa:CleanWorkspaceOption)" />
           <x:Property Name="RunCodeAnalysis" Type="InArgument(mtbwa:CodeAnalysisOption)" />
           <x:Property Name="SourceAndSymbolServerSettings" Type="InArgument(mtbwa:SourceAndSymbolServerSettings)" />
           <x:Property Name="AgentSettings" Type="InArgument(mtbwa:AgentSettings)" />
           <x:Property Name="AssociateChangesetsAndWorkItems" Type="InArgument(x:Boolean)" />
           <x:Property Name="CreateWorkItem" Type="InArgument(x:Boolean)" />
           <x:Property Name="DropBuild" Type="InArgument(x:Boolean)" />
           <x:Property Name="MSBuildArguments" Type="InArgument(x:String)" />
           <x:Property Name="MSBuildPlatform" Type="InArgument(mtbwa:ToolPlatform)" />
           <x:Property Name="PerformTestImpactAnalysis" Type="InArgument(x:Boolean)" />
           <x:Property Name="CreateLabel" Type="InArgument(x:Boolean)" />
           <x:Property Name="DisableTests" Type="InArgument(x:Boolean)" />
           <x:Property Name="GetVersion" Type="InArgument(x:String)" />
           <x:Property Name="PrivateDropLocation" Type="InArgument(x:String)" />
           <x:Property Name="Verbosity" Type="InArgument(mtbw:BuildVerbosity)" />
           <x:Property Name="Metadata" Type="mtbw:ProcessParameterMetadataCollection" />
           <x:Property Name="SupportedReasons" Type="mtbc:BuildReason" />
           <x:Property Name="SubscriptionName" Type="InArgument(x:String)" />
           <x:Property Name="StorageAccountName" Type="InArgument(x:String)" />
           <x:Property Name="CloudConfigLocation" Type="InArgument(x:String)" />
           <x:Property Name="PackageLocation" Type="InArgument(x:String)" />
           <x:Property Name="Environment" Type="InArgument(x:String)" />
           <x:Property Name="SubscriptionDataFileLocation" Type="InArgument(x:String)" />
           <x:Property Name="PublishScriptLocation" Type="InArgument(x:String)" />
           <x:Property Name="ServiceName" Type="InArgument(x:String)" />
         </x:Members>

         <this:Process.MSBuildArguments>
7. Dodaj nową sekwencję na koniec uruchom na agenta:

   1. Rozpocznij od dodania działanie Jeśli instrukcji, aby sprawdzić prawidłowym plikiem skryptu. Warunek tej wartości:

          Not String.IsNullOrEmpty(PublishScriptLocation)
   2. Następnie przypadek Jeśli instrukcji, Dodaj nowe działanie w sekwencji. Ustaw nazwę wyświetlaną na "publish Start"
   3. Z początkiem publikowania sekwencji nadal wybrane, należy dodać na następującej liście nowe zmienne jako oddzielne pozycje w karcie Zmienne projektanta przepływów pracy. Wszystkie zmienne powinny mieć typ zmiennej = ciągu i zakres = rozpoczęcie publikowania. Te posłuży do parametry przepływu z definicji kompilacji do przepływu pracy, które następnie Uzyskaj używane do wywoływania skryptu publikowania.

      * SubscriptionDataFilePath typu String
      * PublishScriptFilePath typu String

        ![Nowe zmienne][4]
   4. Jeśli używasz programu TFS 2012 lub starszy, Dodaj działanie ConvertWorkspaceItem na początku nowego sekwencji. Jeśli używasz programu TFS 2013 lub nowszy, Dodaj działanie GetLocalPath na początku nowego sekwencji. Dla ConvertWorkspaceItem, ustaw właściwości w następujący sposób: kierunek = ServerToLocal, nazwa wyświetlana = 'Convert opublikować nazwę pliku skryptu', dane wejściowe = "PublishScriptLocation", wynik = "PublishScriptFilePath", obszar roboczy = "Obszar roboczy". Dla działania GetLocalPath ustaw właściwość IncomingPath do "PublishScriptLocation" i "PublishScriptFilePath" wynik. To działanie konwertuje ścieżka skryptu publikowania z TFS lokalizacji serwera (jeśli dotyczy) do ścieżki standardowe dysku lokalnym.
   5. Jeśli używasz programu TFS 2012 lub starszy, należy dodać innego działania ConvertWorkspaceItem na końcu nowej sekwencji. Kierunek = ServerToLocal, nazwa wyświetlana = "Dokonać konwersji subskrypcji nazwa_pliku", dane wejściowe = "SubscriptionDataFileLocation", wynik = "SubscriptionDataFilePath", obszar roboczy = "Obszar roboczy". Jeśli używasz programu TFS 2013 lub nowszy, Dodaj inny GetLocalPath. IncomingPath = "SubscriptionDataFileLocation", a wynik = "SubscriptionDataFilePath."
   6. Dodaj działanie InvokeProcess na końcu nowej sekwencji.
      To działanie wywołuje PowerShell.exe z argumentami przekazany przez definicję kompilacji.

      + Argumenty = String.Format ("-""{0}" "- serviceName {1} - storageAccountName {2} tabelę packageLocation —""{3}" "- cloudConfigLocation""{4}" "- subscriptionDataFile""{5}" "- selectedSubscription {6} pliku-środowiska""{7}" "",  PublishScriptFilePath, ServiceName, StorageAccountName, tabelę PackageLocation, CloudConfigLocation, SubscriptionDataFilePath, Nazwa subskrypcji, środowisko)
      + Nazwa wyświetlana = Execute publikowania skryptu
      + Nazwa pliku = "PowerShell" (m.in. cudzysłowy)
      + OutputEncoding = System.Text.Encoding.GetEncoding(System.Globalization.CultureInfo.InstalledUICulture.TextInfo.OEMCodePage)
   7. W **Obsługa standardowego wyjścia** sekcji textbox InvokeProcess, ustaw wartość pola tekstowego "dane". To jest zmienną do przechowywania dane wyjścia standardowego.
   8. Dodaj działanie WriteBuildMessage tylko poniżej **Obsługa standardowego wyjścia** sekcji. Ustaw ważność = "Microsoft.TeamFoundation.Build.Client.BuildMessageImportance.High" i komunikatem = "dane". Dzięki temu standardowe dane wyjściowe skryptu zostanie zapisana w danych wyjściowych kompilacji.
   9. W **obsługi błędów wyjścia** sekcji textbox InvokeProcess, ustaw wartość pola tekstowego "dane". To jest zmienną do przechowywania danych błędów.
   10. Dodaj działanie WriteBuildError tylko poniżej **obsługi błędów wyjścia** sekcji. Ustaw komunikat = "dane". Dzięki temu, że błędy standardowe skryptu zostanie zapisana w danych wyjściowych błędu kompilacji.
   11. Popraw błędy wskazywanym przez niebieski znaczniki wykrzyknika. Umieść kursor nad znaczników wykrzyknik, aby uzyskać wskazówki dotyczące błędu. Zapisywanie przepływu pracy, aby usunąć błędy.

   Wynik końcowy publikowania działań przepływu pracy będzie wyglądać następująco w Projektancie:

   ![Działania przepływu pracy][5]

   Wynik końcowy publikowania działań przepływu pracy będzie wyglądać następująco w języku XAML:

       <If Condition="[Not String.IsNullOrEmpty(PublishScriptLocation)]" sap2010:WorkflowViewState.IdRef="If_1">
           <If.Then>
             <Sequence DisplayName="Start Publish" sap2010:WorkflowViewState.IdRef="Sequence_4">
               <Sequence.Variables>
                 <Variable x:TypeArguments="x:String" Name="SubscriptionDataFilePath" />
                 <Variable x:TypeArguments="x:String" Name="PublishScriptFilePath" />
               </Sequence.Variables>
               <mtbwa:ConvertWorkspaceItem DisplayName="Convert publish script filename" sap2010:WorkflowViewState.IdRef="ConvertWorkspaceItem_1" Input="[PublishScriptLocation]" Result="[PublishScriptFilePath]" Workspace="[Workspace]" />
               <mtbwa:ConvertWorkspaceItem DisplayName="Convert subscription filename" sap2010:WorkflowViewState.IdRef="ConvertWorkspaceItem_2" Input="[SubscriptionDataFileLocation]" Result="[SubscriptionDataFilePath]" Workspace="[Workspace]" />
               <mtbwa:InvokeProcess Arguments="[String.Format(&quot; -File &quot;&quot;{0}&quot;&quot; -serviceName {1}&#xD;&#xA;            -storageAccountName {2} -packageLocation &quot;&quot;{3}&quot;&quot;&#xD;&#xA;            -cloudConfigLocation &quot;&quot;{4}&quot;&quot; -subscriptionDataFile &quot;&quot;{5}&quot;&quot;&#xD;&#xA;            -selectedSubscription {6} -environment &quot;&quot;{7}&quot;&quot;&quot;,&#xD;&#xA;            PublishScriptFilePath, ServiceName, StorageAccountName,&#xD;&#xA;            PackageLocation, CloudConfigLocation,&#xD;&#xA;            SubscriptionDataFilePath, SubscriptionName, Environment)]" DisplayName="'Execute Publish Script'" FileName="[PowerShell]" sap2010:WorkflowViewState.IdRef="InvokeProcess_1">
                 <mtbwa:InvokeProcess.ErrorDataReceived>
                   <ActivityAction x:TypeArguments="x:String">
                     <ActivityAction.Argument>
                       <DelegateInArgument x:TypeArguments="x:String" Name="data" />
                     </ActivityAction.Argument>
                     <mtbwa:WriteBuildError Message="{x:Null}" sap2010:WorkflowViewState.IdRef="WriteBuildError_1" />
                   </ActivityAction>
                 </mtbwa:InvokeProcess.ErrorDataReceived>
                 <mtbwa:InvokeProcess.OutputDataReceived>
                   <ActivityAction x:TypeArguments="x:String">
                     <ActivityAction.Argument>
                       <DelegateInArgument x:TypeArguments="x:String" Name="data" />
                     </ActivityAction.Argument>
                     <mtbwa:WriteBuildMessage sap2010:WorkflowViewState.IdRef="WriteBuildMessage_2" Importance="[Microsoft.TeamFoundation.Build.Client.BuildMessageImportance.High]" Message="[data]" mva:VisualBasic.Settings="Assembly references and imported namespaces serialized as XML namespaces" />
                   </ActivityAction>
                 </mtbwa:InvokeProcess.OutputDataReceived>
               </mtbwa:InvokeProcess>
             </Sequence>
           </If.Then>
         </If>
       </Sequence>
8. Zapisywanie przepływu pracy szablonu procesu kompilacji i zaewidencjonuj tego pliku.
9. Edytowanie definicji kompilacji (je zamknąć, jeżeli jest już otwarty) i wybierz **nowy** przycisku, jeśli nie ma jeszcze nowego szablonu na liście szablonów procesów.
10. Ustaw dla parametru wartości właściwości w sekcji różne w następujący sposób:

    1. CloudConfigLocation = "c:\\porzuca\\app.publish\\ServiceConfiguration.Cloud.cscfg" *ta wartość jest określana na podstawie: ($PublishDir)ServiceConfiguration.Cloud.cscfg*
    2. Tabelę PackageLocation = "c:\\porzuca\\app.publish\\ContactManager.Azure.cspkg" *ta wartość jest określana na podstawie: (cspkg)($ProjectName) $PublishDir*
    3. PublishScriptLocation = "c:\\skryptów\\WindowsAzure\\PublishCloudService.ps1"
    4. ServiceName = "mycloudservicename" *nazwa usługi w chmurze odpowiednie w tym miejscu użyć*
    5. Środowisko = "Tymczasowe"
    6. StorageAccountName = "mystorageaccountname" *Użyj nazwy konta magazynu odpowiednie tutaj*
    7. SubscriptionDataFileLocation = "c:\\skryptów\\WindowsAzure\\Subscription.xml"
    8. Nazwa subskrypcji = "default"

    ![Wartości właściwości parametru][6]
11. Zapisz zmiany w definicji kompilacji.
12. Kolejka kompilacji do wykonywania kompilacji pakietu i publikowania. Jeśli masz ustawioną ciągłej integracji wyzwalacz wykona to zachowanie na każdym zaewidencjonowania.

### <a name="publishcloudserviceps1-script-template"></a>PublishCloudService.ps1 skrypt szablonu
```
Param(  $serviceName = "",
        $storageAccountName = "",
        $packageLocation = "",
        $cloudConfigLocation = "",
        $environment = "Staging",
        $deploymentLabel = "ContinuousDeploy to $servicename",
        $timeStampFormat = "g",
        $alwaysDeleteExistingDeployments = 1,
        $enableDeploymentUpgrade = 1,
        $selectedsubscription = "default",
        $subscriptionDataFile = ""
     )


function Publish()
{
    $deployment = Get-AzureDeployment -ServiceName $serviceName -Slot $slot -ErrorVariable a -ErrorAction silentlycontinue
    if ($a[0] -ne $null)
    {
        Write-Output "$(Get-Date -f $timeStampFormat) - No deployment is detected. Creating a new deployment. "
    }
    #check for existing deployment and then either upgrade, delete + deploy, or cancel according to $alwaysDeleteExistingDeployments and $enableDeploymentUpgrade boolean variables
    if ($deployment.Name -ne $null)
    {
        switch ($alwaysDeleteExistingDeployments)
        {
            1
            {
                switch ($enableDeploymentUpgrade)
                {
                    1  #Update deployment inplace (usually faster, cheaper, won't destroy VIP)
                    {
                        Write-Output "$(Get-Date -f $timeStampFormat) - Deployment exists in $servicename.  Upgrading deployment."
                        UpgradeDeployment
                    }
                    0  #Delete then create new deployment
                    {
                        Write-Output "$(Get-Date -f $timeStampFormat) - Deployment exists in $servicename.  Deleting deployment."
                        DeleteDeployment
                        CreateNewDeployment

                    }
                } # switch ($enableDeploymentUpgrade)
            }
            0
            {
                Write-Output "$(Get-Date -f $timeStampFormat) - ERROR: Deployment exists in $servicename.  Script execution cancelled."
                exit
            }
        } #switch ($alwaysDeleteExistingDeployments)
    } else {
            CreateNewDeployment
    }
}

function CreateNewDeployment()
{
    write-progress -id 3 -activity "Creating New Deployment" -Status "In progress"
    Write-Output "$(Get-Date -f $timeStampFormat) - Creating New Deployment: In progress"

    $opstat = New-AzureDeployment -Slot $slot -Package $packageLocation -Configuration $cloudConfigLocation -label $deploymentLabel -ServiceName $serviceName

    $completeDeployment = Get-AzureDeployment -ServiceName $serviceName -Slot $slot
    $completeDeploymentID = $completeDeployment.deploymentid

    write-progress -id 3 -activity "Creating New Deployment" -completed -Status "Complete"
    Write-Output "$(Get-Date -f $timeStampFormat) - Creating New Deployment: Complete, Deployment ID: $completeDeploymentID"

    StartInstances
}

function UpgradeDeployment()
{
    write-progress -id 3 -activity "Upgrading Deployment" -Status "In progress"
    Write-Output "$(Get-Date -f $timeStampFormat) - Upgrading Deployment: In progress"

    # perform Update-Deployment
    $setdeployment = Set-AzureDeployment -Upgrade -Slot $slot -Package $packageLocation -Configuration $cloudConfigLocation -label $deploymentLabel -ServiceName $serviceName -Force

    $completeDeployment = Get-AzureDeployment -ServiceName $serviceName -Slot $slot
    $completeDeploymentID = $completeDeployment.deploymentid

    write-progress -id 3 -activity "Upgrading Deployment" -completed -Status "Complete"
    Write-Output "$(Get-Date -f $timeStampFormat) - Upgrading Deployment: Complete, Deployment ID: $completeDeploymentID"
}

function DeleteDeployment()
{

    write-progress -id 2 -activity "Deleting Deployment" -Status "In progress"
    Write-Output "$(Get-Date -f $timeStampFormat) - Deleting Deployment: In progress"

    #WARNING - always deletes with force
    $removeDeployment = Remove-AzureDeployment -Slot $slot -ServiceName $serviceName -Force

    write-progress -id 2 -activity "Deleting Deployment: Complete" -completed -Status $removeDeployment
    Write-Output "$(Get-Date -f $timeStampFormat) - Deleting Deployment: Complete"

}

function StartInstances()
{
    write-progress -id 4 -activity "Starting Instances" -status "In progress"
    Write-Output "$(Get-Date -f $timeStampFormat) - Starting Instances: In progress"

    $deployment = Get-AzureDeployment -ServiceName $serviceName -Slot $slot
    $runstatus = $deployment.Status

    if ($runstatus -ne 'Running')
    {
        $run = Set-AzureDeployment -Slot $slot -ServiceName $serviceName -Status Running
    }
    $deployment = Get-AzureDeployment -ServiceName $serviceName -Slot $slot
    $oldStatusStr = @("") * $deployment.RoleInstanceList.Count

    while (-not(AllInstancesRunning($deployment.RoleInstanceList)))
    {
        $i = 1
        foreach ($roleInstance in $deployment.RoleInstanceList)
        {
            $instanceName = $roleInstance.InstanceName
            $instanceStatus = $roleInstance.InstanceStatus

            if ($oldStatusStr[$i - 1] -ne $roleInstance.InstanceStatus)
            {
                $oldStatusStr[$i - 1] = $roleInstance.InstanceStatus
                Write-Output "$(Get-Date -f $timeStampFormat) - Starting Instance '$instanceName': $instanceStatus"
            }

            write-progress -id (4 + $i) -activity "Starting Instance '$instanceName'" -status "$instanceStatus"
            $i = $i + 1
        }

        sleep -Seconds 1

        $deployment = Get-AzureDeployment -ServiceName $serviceName -Slot $slot
    }

    $i = 1
    foreach ($roleInstance in $deployment.RoleInstanceList)
    {
        $instanceName = $roleInstance.InstanceName
        $instanceStatus = $roleInstance.InstanceStatus

        if ($oldStatusStr[$i - 1] -ne $roleInstance.InstanceStatus)
        {
            $oldStatusStr[$i - 1] = $roleInstance.InstanceStatus
            Write-Output "$(Get-Date -f $timeStampFormat) - Starting Instance '$instanceName': $instanceStatus"
        }

        $i = $i + 1
    }

    $deployment = Get-AzureDeployment -ServiceName $serviceName -Slot $slot
    $opstat = $deployment.Status

    write-progress -id 4 -activity "Starting Instances" -completed -status $opstat
    Write-Output "$(Get-Date -f $timeStampFormat) - Starting Instances: $opstat"
}

function AllInstancesRunning($roleInstanceList)
{
    foreach ($roleInstance in $roleInstanceList)
    {
        if ($roleInstance.InstanceStatus -ne "ReadyRole")
        {
            return $false
        }
    }

    return $true
}

#configure powershell with Azure 1.7 modules
Import-Module Azure

#configure powershell with publishsettings for your subscription
$pubsettings = $subscriptionDataFile
Import-AzurePublishSettingsFile $pubsettings
Set-AzureSubscription -CurrentStorageAccountName $storageAccountName -SubscriptionName $selectedsubscription
Select-AzureSubscription $selectedsubscription

#set remaining environment variables for Azure cmdlets
$subscription = Get-AzureSubscription $selectedsubscription
$subscriptionname = $subscription.subscriptionname
$subscriptionid = $subscription.subscriptionid
$slot = $environment

#main driver - publish & write progress to activity log
Write-Output "$(Get-Date -f $timeStampFormat) - Azure Cloud Service deploy script started."
Write-Output "$(Get-Date -f $timeStampFormat) - Preparing deployment of $deploymentLabel for $subscriptionname with Subscription ID $subscriptionid."

Publish

$deployment = Get-AzureDeployment -slot $slot -serviceName $servicename
$deploymentUrl = $deployment.Url

Write-Output "$(Get-Date -f $timeStampFormat) - Created Cloud Service with URL $deploymentUrl."
Write-Output "$(Get-Date -f $timeStampFormat) - Azure Cloud Service deploy script finished."
```

## <a name="next-steps"></a>Następne kroki
Aby włączyć debugowanie zdalne, korzystając z ciągłego dostarczania, zobacz [Włączanie debugowania zdalnego, gdy publikowanie na platformie Azure przy użyciu ciągłego dostarczania](cloud-services-virtual-machines-dotnet-continuous-delivery-remote-debugging.md).

[Team Foundation Build Service]: https://msdn.microsoft.com/library/ee259687.aspx
[.NET Framework 4]: https://www.microsoft.com/download/details.aspx?id=17851
[.NET Framework 4.5]: https://www.microsoft.com/download/details.aspx?id=30653
[.NET Framework 4.5.2]: https://www.microsoft.com/download/details.aspx?id=42643
[Scale out your build system]: https://msdn.microsoft.com/library/dd793166.aspx
[Deploy and configure a build server]: https://msdn.microsoft.com/library/ms181712.aspx
[Azure PowerShell cmdlets]: /powershell/azureps-cmdlets-docs
[the .publishsettings file]: https://manage.windowsazure.com/download/publishprofile.aspx?wa=wsignin1.0
[0]: ./media/cloud-services-dotnet-continuous-delivery/tfs-01bc.png
[2]: ./media/cloud-services-dotnet-continuous-delivery/tfs-02.png
[3]: ./media/cloud-services-dotnet-continuous-delivery/common-task-tfs-03.png
[4]: ./media/cloud-services-dotnet-continuous-delivery/common-task-tfs-04.png
[5]: ./media/cloud-services-dotnet-continuous-delivery/common-task-tfs-05.png
[6]: ./media/cloud-services-dotnet-continuous-delivery/common-task-tfs-06.png
