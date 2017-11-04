---
title: "Publikowanie za pomocą narzędzia Azure usługa w chmurze | Dokumentacja firmy Microsoft"
description: "Więcej informacji na temat sposobu publikowania projektów usług w chmurze platformy Azure przy użyciu programu Visual Studio."
services: visual-studio-online
documentationcenter: na
author: kraigb
manager: ghogen
editor: 
ms.assetid: 1a07b6e4-3678-4cbf-b37e-4520b402a3d9
ms.service: multiple
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: multiple
ms.date: 8/14/2017
ms.author: kraigb
ms.openlocfilehash: e617d600dbc8287eea737fc4969833e873365288
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/11/2017
---
# <a name="publishing-a-cloud-service-using-the-azure-tools"></a>Publikowanie za pomocą narzędzia Azure usługa w chmurze
Narzędzia systemu Azure dla programu Microsoft Visual Studio umożliwiają opublikowanie aplikacji systemu Azure bezpośrednio z poziomu programu Visual Studio. Program Visual Studio obsługuje zintegrowane publikowanie w tymczasowym lub produkcyjnym środowisku usługi w chmurze.

Przed opublikowaniem aplikacji Azure, musi mieć subskrypcję platformy Azure. Należy również skonfigurować konto magazyn i usługa w chmurze ma być używany przez aplikację. Można je skonfigurować w [klasycznego portalu Azure](http://go.microsoft.com/fwlink/?LinkID=213885).

> [!IMPORTANT]
> Podczas publikowania, można wybrać środowiska wdrażania dla usługi w chmurze. Musisz także wybrać konto magazynu, który jest używany do przechowywania pakietu aplikacji do wdrożenia. Po wdrożeniu pakietu aplikacji zostaną usunięte z konta magazynu.
> 
> 

Opublikuj zmiany przyrostowo dla poszczególnych ról sieci web w podczas tworzenia i testowania aplikacji Azure, można użyć narzędzia Web Deploy. Po opublikowaniu aplikacji w środowisku wdrażania narzędzia Web Deploy umożliwia wdrażanie zmian bezpośrednio do maszyny wirtualnej, na którym jest uruchomiona rola sieci web. Nie masz pakietu i opublikować całą aplikację Azure zawsze chcesz zaktualizować roli sieci web do testowania zmiany. Z tej metody może mieć zmiany roli sieci web dostępnych w chmurze na potrzeby testowania bez oczekiwania na zostały opublikowane w środowisku wdrażania aplikacji.

Użyj poniższych procedur, aby opublikować aplikację usługi Azure i zaktualizuj rolę sieci web za pomocą narzędzia Web Deploy:

* Publikowanie lub pakietu aplikacji Azure w programie Visual Studio
* Zaktualizuj rolę sieci web w ramach projektowania i testowania cyklu

## <a name="publish-or-package-an-azure-application-from-visual-studio"></a>Publikowanie lub pakietu aplikacji Azure w programie Visual Studio
Podczas publikowania aplikacji platformy Azure, możesz wybrać jedną z następujących czynności:

* Tworzenie pakietu usług: ten pakiet i pliku konfiguracji usługi można użyć do publikowania aplikacji w środowisku wdrażania z [klasycznego portalu Azure](http://go.microsoft.com/fwlink/?LinkID=213885).
* Opublikować projekt platformy Azure w programie Visual Studio: Aby opublikować aplikację bezpośrednio na platformie Azure, należy użyć Kreatora publikowania. Aby uzyskać informacje, zobacz [Kreator publikowania aplikacji Azure](vs-azure-tools-publish-azure-application-wizard.md).

### <a name="to-create-a-service-package-from-visual-studio"></a>Aby utworzyć pakiet usługi w programie Visual Studio
1. Gdy wszystko będzie gotowe opublikować aplikację, otwórz Eksploratora rozwiązań, otwórz menu skrótów projektu platformy Azure, który zawiera role i wybierz polecenie publikowania.
2. Aby utworzyć pakietu usług, wykonaj następujące kroki:  
   
   1. Menu skrótów projektu platformy Azure, wybierz **pakietu**.
   2. W **pakietu aplikacji Azure** okno dialogowe Wybieranie konfiguracji usługi, dla której chcesz utworzyć pakiet, a następnie wybierz konfigurację kompilacji.
   3. (opcjonalnie) Aby włączyć funkcję pulpitu zdalnego dla usługi w chmurze po opublikowaniu, zaznacz **Włącz pulpit zdalny dla wszystkich ról** pole wyboru, a następnie wybierz **ustawienia** Konfigurowanie pulpitu zdalnego. Jeśli chcesz debugować usługi w chmurze po opublikowaniu, Włącz debugowanie zdalne wybierając **Włącz zdalny debuger dla wszystkich ról**.
      
      Aby uzyskać więcej informacji, zobacz [za pomocą pulpitu zdalnego z rolami Azure](vs-azure-tools-remote-desktop-roles.md).
   4. Aby utworzyć pakiet, należy wybrać **pakietu** łącza.
      
      Eksplorator plików zawiera lokalizację pliku nowo utworzony pakiet. Możesz skopiować tej lokalizacji, aby można używać go z [klasycznego portalu Azure](http://go.microsoft.com/fwlink/?LinkID=213885).
   5. Aby opublikować środowiska wdrażania tego pakietu, należy użyć tej lokalizacji jako lokalizacja pakietu podczas tworzenia usługi w chmurze i wdrażania tego pakietu do środowiska z [klasycznego portalu Azure](http://go.microsoft.com/fwlink/?LinkID=213885).
3. (Opcjonalnie) Aby anulować proces wdrażania dla pozycji w dzienniku aktywności, w menu skrótów wybierz **Anuluj i Usuń**. Powoduje to zatrzymanie procesu wdrażania i usuwa środowiska wdrażania z platformy Azure.
   
   > [!NOTE]
   > Aby usunąć to środowisko wdrażania po jej wdrożeniu, należy użyć [klasycznego portalu Azure](http://go.microsoft.com/fwlink/?LinkID=213885).
   > 
   > 
4. (Opcjonalnie) Po uruchomiona wystąpienia roli, Visual Studio automatycznie wyświetlany środowiska wdrażania w **usługi w chmurze** węzła w Eksploratorze serwera. W tym miejscu można wyświetlić stan wystąpień poszczególnych ról. Zobacz [Azure zarządzanie zasobami za pomocą Eksploratora chmury](vs-azure-tools-resources-managing-with-cloud-explorer.md). Na poniższej ilustracji przedstawiono wystąpienia roli, gdy są one nadal w stanie inicjowanie:
   
    ![VST_DeployComputeNode](./media/vs-azure-tools-publishing-a-cloud-service/IC744134.png)

## <a name="update-a-web-role-as-part-of-the-development-and-testing-cycle"></a>Zaktualizuj rolę sieci Web w ramach projektowania i testowania cyklu
Zaktualizuj rolę sieci web w projekcie w Jeśli infrastruktury wewnętrznej bazy danych aplikacji jest stabilna, ale role sieci web trzeba więcej często uaktualniać, można użyć narzędzia Web Deploy. Jest to przydatne, gdy nie chcesz ponowne skompilowanie i wdrożenie roli proces roboczy wewnętrznej bazy danych lub jeśli masz wiele ról sieci web i zaktualizować tylko jedną z ról sieci web.

### <a name="requirements"></a>Wymagania
Poniżej przedstawiono wymagania dotyczące korzystania z narzędzia Web Deploy można zaktualizować roli sieci web:

* **Do projektowania i testowania celów tylko:** bezpośrednio do maszyny wirtualnej zostały wprowadzone zmiany, którym jest uruchomiona rola sieci web. Jeśli tej maszyny wirtualnej musi zostać przetworzony ponownie, zmiany zostaną utracone, ponieważ oryginalny pakiet opublikowaną jest używana do odtworzenia dla roli maszyny wirtualnej. Należy ponownie opublikować aplikację, aby pobrać najnowsze zmiany roli sieci web.
* **Można aktualizować tylko role sieci web:** nie można zaktualizować roli proces roboczy. Ponadto nie można zaktualizować RoleEntryPoint w role.cs sieci web.
* **Może obsługiwać tylko jedno wystąpienie roli sieci web:** nie może mieć wielu wystąpień dowolnej roli sieci web w środowisku wdrażania. Jednak wielu role sieci web każdego z tylko jednym wystąpieniem są obsługiwane.
* **Należy włączyć połączenia pulpitu zdalnego:** jest to wymagane, aby narzędzia Web Deploy można użytkownika i hasła się połączyć z maszyną wirtualną, aby wdrożyć zmiany na serwerze z systemem Internet Information Services (IIS). Ponadto konieczne może się połączyć z maszyną wirtualną, aby dodać zaufanego certyfikatu usług IIS na tej maszynie wirtualnej. (To zapewnia, że połączenia zdalnego dla usług IIS, który jest używany przez narzędzie Web Deploy jest bezpieczny).

W poniższej procedurze przyjęto, że używasz **publikowanie aplikacji platformy Azure** kreatora.

### <a name="to-enable-web-deploy-when-you-publish-your-application"></a>Aby włączyć wdrażanie opublikować aplikację sieci Web
1. Aby włączyć **Włącz narzędzia Web Deploy** dla wszystkich sieci web pole wyboru ról, należy najpierw skonfigurować połączenia pulpitu zdalnego. Wybierz **Włącz pulpit zdalny** dla wszystkich ról, a następnie podaj poświadczenia, które będą używane do połączenia zdalnego w **konfigurację usług pulpitu zdalnego** pojawi się okno. Zobacz [za pomocą pulpitu zdalnego z rolami Azure](vs-azure-tools-remote-desktop-roles.md) Aby uzyskać więcej informacji.
2. Aby włączyć narzędzia Web Deploy w dla wszystkich ról sieci web w aplikacji, wybierz **Włącz narzędzia Web Deploy dla wszystkich ról sieci web**.
   
    Żółty trójkąt ostrzeżenie zostanie wyświetlone. Narzędzie Web Deploy używa certyfikatów niezaufanych, podpisem domyślnie, co nie jest zalecane do przekazywania poufnych danych. Jeśli trzeba zabezpieczyć ten proces dla danych poufnych, można dodać certyfikat SSL służący do połączenia narzędzia Web Deploy. Ten certyfikat musi być zaufanym certyfikatem. Aby dowiedzieć się, jak to zrobić, zobacz sekcję **do należy wdrożyć zabezpieczenia sieci Web** dalszej części tego tematu.
3. Wybierz **dalej** pokazanie **Podsumowanie** ekranu, a następnie wybierz pozycję **publikowania** do wdrożenia usługi w chmurze.
   
    Usługi w chmurze jest opublikowana. Maszyny wirtualnej, która jest tworzona ma włączone dla usług IIS tak, aby narzędzia Web Deploy może posłużyć do zaktualizowania role sieci web bez ponownego publikowania ich połączeń zdalnych.
   
   > [!NOTE]
   > Jeśli masz więcej niż jedno wystąpienie skonfigurowany dla roli sieci web, zostanie wyświetlony komunikat ostrzegawczy informujący, że każda rola sieci web będzie ograniczony do jednego wystąpienia tylko w pakiecie, który służy do publikowania aplikacji. Wybierz **OK** aby kontynuować. Jak już wspomniano w sekcji wymagań, może mieć więcej niż jednej roli sieci web, ale tylko jedno wystąpienie każdej roli.
   > 
   > 

### <a name="to-update-your-web-role-by-using-web-deploy"></a>Aby zaktualizować roli sieci Web za pomocą narzędzia Web Deploy
1. Aby korzystać z narzędzia Web Deploy, wprowadzać zmiany kodu do projektu dla żadnej z ról użytkownika sieci web w programie Visual Studio, który chcesz opublikować, a następnie kliknij prawym przyciskiem myszy węzeł tego projektu w rozwiązaniu i wskaż polecenie **publikowania**. **Publikowanie w sieci Web** zostanie wyświetlone okno dialogowe.
2. (Opcjonalnie) Jeśli dodano zaufany certyfikat SSL do użycia dla połączeń zdalnych dla usług IIS, można wyczyścić **niezaufany Zezwalaj** pole wyboru. Aby dowiedzieć się, jak dodać certyfikat, aby zabezpieczyć narzędzia Web Deploy, zobacz sekcję **do upewnij sieci Web wdrażanie bezpiecznych** dalszej części tego tematu.
3. Aby korzystać z narzędzia Web Deploy, mechanizmu publikowania wymaga nazwy użytkownika i hasło, które można ustawić dla Podłączanie pulpitu zdalnego za pierwszej publikacji pakietu.
   
   1. W **nazwy użytkownika**, wprowadź nazwę użytkownika.
   2. W **hasło**, wprowadź hasło.
   3. (Opcjonalnie) Jeśli chcesz zapisać to hasło do tego profilu, wybierz **Zapisz hasło**.
4. Aby opublikować zmiany roli sieci web, wybierz **publikowania**.
   
    Wyświetla wiersz stanu **publikowania uruchomiona**. Po zakończeniu publikowania **publikowanie zakończyło się pomyślnie** pojawi się. Zmiany zostały wdrożone obecnie do roli sieci web na maszynie wirtualnej. Teraz można uruchomić aplikacji platformy Azure w środowisku platformy Azure, aby sprawdzić zmiany.

### <a name="to-make-web-deploy-secure"></a>Aby zabezpieczyć wdrażania w sieci Web
1. Narzędzie Web Deploy używa certyfikatów niezaufanych, podpisem domyślnie, co nie jest zalecane do przekazywania poufnych danych. Jeśli trzeba zabezpieczyć ten proces dla danych poufnych, można dodać certyfikat SSL służący do połączenia narzędzia Web Deploy. Ten certyfikat musi być zaufany certyfikat, który można uzyskać od urzędu certyfikacji (CA).
   
    Aby zabezpieczyć narzędzia Web Deploy dla każdej maszyny wirtualnej dla każdej z ról użytkownika sieci web, musisz przekazać zaufanego certyfikatu, który ma być używany dla sieci web wdrażanie na [klasycznego portalu Azure](http://go.microsoft.com/fwlink/?LinkID=213885). Dzięki temu się upewnić, że certyfikat został dodany do maszyny wirtualnej, która jest tworzona dla roli sieci web podczas publikowania aplikacji.
2. Aby dodać zaufany certyfikat SSL do usługi IIS mają używać dla połączeń zdalnych, wykonaj następujące kroki:
   
   1. Aby połączyć maszynę wirtualną, która jest uruchomiona rola sieci web, wybierz wystąpienie roli sieci web w **Eksplorator chmury** lub **Eksploratora serwera**, a następnie wybierz pozycję **łączyć się przy użyciu pulpitu zdalnego**  polecenia. Aby uzyskać szczegółowe instrukcje na temat nawiązywania połączenia z maszyną wirtualną, zobacz [za pomocą pulpitu zdalnego z rolami Azure](vs-azure-tools-remote-desktop-roles.md).
      
      Przeglądarka wyświetli monit o do pobrania. Plik RDP.
   2. Aby dodać certyfikat SSL, otwórz usługę zarządzania w Menedżerze usług IIS. W Menedżerze usług IIS, Włącz protokół SSL, otwierając **powiązania** łącze w **akcji** okienka. **Dodawanie powiązania witryny** zostanie wyświetlone okno dialogowe. Wybierz **Dodaj**, a następnie wybierz pozycję HTTPS w **typu** listy rozwijanej. W **certyfikat SSL** wybierz certyfikat SSL, który ma być podpisany przez urząd certyfikacji i który został przekazany do [klasycznego portalu Azure](http://go.microsoft.com/fwlink/?LinkID=213885). Aby uzyskać więcej informacji, zobacz [skonfigurować ustawienia połączenia dla usługi zarządzania](http://go.microsoft.com/fwlink/?LinkId=215824).
      
      > [!NOTE]
      > Jeśli dodajesz zaufanego certyfikatu SSL, żółty trójkąt ostrzeżenia nie jest już widoczna w **Kreator publikowania**.
      > 
      > 

## <a name="include-files-in-the-service-package"></a>Dołącz pliki w pakiecie usługi
Konieczne może być obejmują określonych plików w pakiecie usługi, aby były one dostępne na maszynie wirtualnej, który jest tworzony dla roli. Na przykład można dodać .exe lub pliku msi, który jest używany przez skrypt uruchamiania pakietu usługi. Lub może być konieczne dodanie zestawu, który wymaga projektu roli sieć web, jak rola lub procesu roboczego. Aby uwzględnić pliki muszą zostać dodane do rozwiązania dla aplikacji platformy Azure.

### <a name="to-include-files-in-the-service-package"></a>Aby uwzględnić pliki w pakiecie usługi
1. Aby dodać zestawu do pakietu usługi, użyj następujących kroków:
   
   1. W **Eksploratora rozwiązań** Otwórz węzeł projektu dla projektu, w którym brakuje przywoływanego zestawu.
   2. Aby dodać zestaw do projektu, otwórz menu skrótów **odwołania** folder, a następnie wybierz **Dodaj odwołanie**. Zostanie wyświetlone okno dialogowe Dodaj odwołanie.
   3. Wybierz odwołanie, które chcesz dodać, a następnie wybierz pozycję **OK** przycisku.
      
      Odwołanie zostanie dodany do listy w obszarze **odwołania** folderu.
   4. Otwórz menu skrótów dla zestawu, który został dodany i wybierz polecenie **właściwości**. **Właściwości** zostanie wyświetlone okno.
      
      Do uwzględnienia w tym zestawie w pakiecie usługi **kopii lokalnej listy** wybierz **True**.
2. W **Eksploratora rozwiązań** Otwórz węzeł projektu dla projektu, w którym brakuje przywoływanego zestawu.
3. Aby dodać zestaw do projektu, otwórz menu skrótów **odwołania** folder, a następnie wybierz **Dodaj odwołanie**. **Dodaj odwołanie** zostanie wyświetlone okno dialogowe.
4. Wybierz odwołanie, które chcesz dodać, a następnie wybierz pozycję **OK** przycisku.
   
    Odwołanie zostanie dodany do listy w obszarze **odwołania** folderu.
5. Otwórz menu skrótów dla zestawu, który został dodany i wybierz polecenie **właściwości**. Zostanie wyświetlone okno właściwości.
6. Do uwzględnienia w tym zestawie w pakiecie usługi **Kopiuj lokalnie** wybierz **True**.
7. Aby uwzględnić pliki w pakiecie usługi, które zostały dodane do projektu roli sieci web, otwórz menu skrótów dla pliku, a następnie wybierz **właściwości**. Z **właściwości** okna, wybierz **zawartości** z **Akcja kompilacji** pola listy.
8. Aby uwzględnić pliki w pakiecie usługi, które zostały dodane do projektu roli proces roboczy, otwórz menu skrótów dla pliku, a następnie wybierz **właściwości**. Z **właściwości** okna, wybierz **Kopiuj, jeśli nowszy** z **Kopiuj do katalogu wyjściowego** pola listy.

## <a name="next-steps"></a>Następne kroki
Aby uzyskać więcej informacji na temat publikowania na platformie Azure w programie Visual Studio, zobacz [Kreator publikowania aplikacji Azure](vs-azure-tools-publish-azure-application-wizard.md).

