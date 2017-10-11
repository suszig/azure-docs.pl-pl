---
title: "Przygotowanie do publikowania lub wdrażania aplikacji Azure w programie Visual Studio | Dokumentacja firmy Microsoft"
description: "Dowiedz się procedury konfigurowania chmur i konto usługi magazynu i konfigurowania aplikacji platformy Azure."
services: visual-studio-online
documentationcenter: na
author: kraigb
manager: ghogen
editor: 
ms.assetid: 92ee2f9e-ec49-4c7a-900d-620abe5e9d8a
ms.service: multiple
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: multiple
ms.date: 11/11/2016
ms.author: kraigb
ms.openlocfilehash: cc4fb87e559f554634ae062a59bee31f0831da64
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 08/29/2017
---
# <a name="prepare-to-publish-or-deploy-an-azure-application-from-visual-studio"></a>Przygotowanie do publikowania lub wdrażania aplikacji Azure w programie Visual Studio
## <a name="overview"></a>Omówienie
Zanim będzie można opublikować projekt usługi w chmurze, należy skonfigurować następujące usługi:

* A **usługi w chmurze** do uruchomienia z ról w środowisku platformy Azure
* A **konta magazynu** , który zapewnia dostęp do usług obiektów Blob, kolejki i tabeli.

Użyj następujących procedur, aby skonfigurować tych usług i aplikacji

## <a name="create-a-cloud-service"></a>Tworzenie usługi w chmurze
Aby opublikować usługi w chmurze na platformie Azure, musi najpierw utworzyć usługi w chmurze, uruchamianego ról w środowisku platformy Azure. Można utworzyć usługi w chmurze w [klasycznego portalu Azure](http://go.microsoft.com/fwlink/?LinkID=213885), zgodnie z opisem w sekcji **można utworzyć usługi w chmurze przy użyciu klasycznego portalu Azure**w dalszej części tego tematu. Usługi w chmurze można również utworzyć w programie Visual Studio, używając Kreatora publikacji.

### <a name="to-create-a-cloud-service-by-using-visual-studio"></a>Aby utworzyć usługi w chmurze przy użyciu programu Visual Studio
1. Otwórz menu skrótów dla usługi Azure projektu i wybierz polecenie **publikowania**.

    ![VST_PublishMenu](./media/vs-azure-tools-cloud-service-publish-set-up-required-services-in-visual-studio/vst-publish-menu.png)
2. Jeśli masz konta, zaloguj się przy użyciu nazwy użytkownika i hasła dla konta Microsoft lub konta organizacyjnego, który został skojarzony z subskrypcją platformy Azure.
3. Wybierz **dalej** przycisk, aby przejść do **ustawienia** strony.

    ![Typowe ustawienia publikowania Kreatora](./media/vs-azure-tools-cloud-service-publish-set-up-required-services-in-visual-studio/publish-settings-page.png)
4. W **usługi w chmurze** wybierz **Utwórz nowy**. **Tworzenie usług Azure** zostanie wyświetlone okno dialogowe.
5. Wprowadź nazwę usługi w chmurze. Nazwa stanowi część adresu URL usługi i dlatego musi być globalnie unikatowa. Nazwa nie jest rozróżniana wielkość liter.

### <a name="to-create-a-cloud-service-by-using-the-azure-classic-portal"></a>Aby utworzyć usługi w chmurze przy użyciu klasycznego portalu Azure
1. Zaloguj się do [klasycznego portalu Azure](http://go.microsoft.com/fwlink/?LinkId=253103) w witrynie firmy Microsoft.
2. (opcjonalnie) Aby wyświetlić listę usług w chmurze, które zostały już utworzone, wybierz link usługi w chmurze w lewej części strony.
3. Wybierz  **+**  ikonę w lewym dolnym rogu, a następnie wybierz pozycję **usługi w chmurze** w wyświetlonym menu. Inny ekran z dwóch opcji **szybkie tworzenie** i **Utwórz niestandardowy**, zostanie wyświetlone. Jeśli wybierzesz **szybkie tworzenie**, można utworzyć usługi w chmurze tylko przez określenie adresu URL i regionu, w którym będzie fizycznie obsługiwana. Jeśli wybierzesz **Utwórz niestandardowy**, natychmiast można opublikować usługi w chmurze, określając pakietu (pliku cspkg), plik konfiguracji (cscfg), a certyfikat. Tworzenie niestandardowych nie jest wymagane, jeśli zamierzasz opublikować usługi w chmurze za pomocą **publikowania** polecenia w projekcie platformy Azure. **Publikowania** polecenie jest dostępne w menu skrótów projektu platformy Azure.
4. Wybierz **szybkie tworzenie** później publikowania usługi w chmurze przy użyciu programu Visual Studio.
5. Określ nazwę usługi w chmurze. Pełny adres URL jest wyświetlany obok nazwy.
6. Na liście Wybierz region, w którym znajdują się większość użytkowników.
7. W dolnej części okna wybierz **Tworzenie usługi w chmurze** łącza.

## <a name="create-a-storage-account"></a>Tworzenie konta magazynu
Konto magazynu zapewnia dostęp do usług obiektów Blob, kolejki i tabeli. Konto magazynu można utworzyć za pomocą programu Visual Studio lub [klasycznego portalu Azure](http://go.microsoft.com/fwlink/?LinkId=253103).

### <a name="to-create-a-storage-account-by-using-visual-studio"></a>Aby utworzyć konto magazynu przy użyciu programu Visual Studio
1. W **Eksploratora rozwiązań**, otwórz menu skrótów **magazynu** węzeł, a następnie wybierz pozycję **Utwórz konto magazynu**.

    ![Utwórz nowe konto magazynu Azure](./media/vs-azure-tools-cloud-service-publish-set-up-required-services-in-visual-studio/IC744166.png)
2. Wybierz lub wprowadź następujące informacje dotyczące nowego konta magazynu w **Utwórz konto magazynu** okno dialogowe.

   * Subskrypcja platformy Azure, do której chcesz dodać konto magazynu.
   * Nazwa, którą chcesz użyć dla nowego konta magazynu.
   * Region lub grupę koligacji, (na przykład zachodnie stany USA lub Azja Wschodnia).
   * Typ replikacji, który ma być używany dla konta magazynu, takich jak geograficznie nadmiarowy.
3. Gdy wszystko będzie gotowe, wybierz pozycję **Utwórz**. Nowe konto magazynu jest wyświetlana w **magazynu** na liście **Eksploratora serwera**.

### <a name="to-create-a-storage-account-by-using-the-azure-classic-portal"></a>Aby utworzyć konto magazynu przy użyciu klasycznego portalu Azure
1. Zaloguj się do [klasycznego portalu Azure](http://go.microsoft.com/fwlink/?LinkId=253103) w witrynie firmy Microsoft.
2. (Opcjonalnie) Aby wyświetlić kont magazynu, wybierz **magazynu** łącze w panelu w lewej części strony.
3. W lewym dolnym rogu strony, wybierz  **+**  ikony.
4. W wyświetlonym menu wybierz **magazynu**, a następnie wybierz pozycję **szybkie tworzenie**.
5. Nadaj nazwę, która spowoduje unikatowy adres url konta magazynu.
6. Nadaj nazwę usługi w chmurze. Pełny adres URL jest wyświetlany obok nazwy.
7. Na liście regionów wybierz region, w którym znajdują się większość użytkowników.
8. Określ, czy chcesz włączyć replikację geograficzną. Jeśli możesz włączyć replikację geograficzną, dane zostaną zapisane w wielu lokalizacjach fizycznych, aby zmniejszyć prawdopodobieństwo utraty. Ta funkcja umożliwia bardziej kosztownego magazynowania, ale można zmniejszyć koszty przez włączenie lokalizacji geograficznej, podczas tworzenia konta magazynu, zamiast dodawania funkcji później. Aby uzyskać więcej informacji, zobacz [— replikacja geograficzna](http://go.microsoft.com/fwlink/?LinkId=253108).
9. W dolnej części okna wybierz **Utwórz konto magazynu** łącza.

Po utworzeniu konta magazynu zobaczysz adresów URL, które umożliwiają dostęp do zasobów w każdej z usług Azure storage, a także klucze podstawowe i pomocnicze dostępu dla konta. Te klucze służy do uwierzytelniania żądań wysyłanych z usług magazynu.

> [!NOTE]
> Pomocniczy klucz dostępu zawiera takie same prawa dostępu do konta magazynu jako podstawowy klucz dostępu i jest generowany na nazwę kopii zapasowej klucza podstawowego dostępu zagrożenia bezpieczeństwa. Ponadto zaleca się ponowne generowanie kluczy dostępu na bieżąco. Można zmodyfikować ustawienie parametrów połączenia, aby używać klucza pomocniczego podczas ponownego generowania klucza podstawowego, a następnie zmodyfikować, aby użyć wygenerowano ponownie klucz podstawowy, podczas ponownego generowania klucza pomocniczego.
>
>

## <a name="configure-your-app-to-use-services-provided-by-the-storage-account"></a>Skonfiguruj aplikację, aby używać usług świadczonych przez konta magazynu
Należy skonfigurować dowolnej roli, który uzyskuje dostęp do usługi magazynu do użycia usług Azure storage, które zostały utworzone. Aby to zrobić, można użyć wielu konfiguracji usługi Azure projektu. Domyślnie dwa są tworzone w projekcie platformy Azure. Za pomocą wielu konfiguracji usługi, możesz użyć tych samych parametrach połączenia w kodzie, ale ma inną wartość parametrów połączenia w każdej konfiguracji usługi. Na przykład można użyć jedną konfigurację usługi do uruchamiania i debugowania aplikacji lokalnie za pomocą emulatora magazynu Azure i konfiguracji innej usługi, aby opublikować aplikację na platformie Azure. Aby uzyskać więcej informacji dotyczących konfiguracji usługi, zobacz [Konfigurowanie Your Azure projektu przy użyciu usługi konfiguracji z wieloma](vs-azure-tools-multiple-services-project-configurations.md).

### <a name="to-configure-your-application-to-use-services-that-the-storage-account-provides"></a>Aby skonfigurować aplikację do korzystania z usług, które udostępnia konta magazynu
1. W programie Visual Studio Otwórz rozwiązania Azure. W Eksploratorze rozwiązań Otwórz menu skrótów dla każdej roli w projekcie platformy Azure, który uzyskuje dostęp do usług magazynu i wybierz polecenie **właściwości**. Strona z nazwą roli jest wyświetlany w edytorze programu Visual Studio. Na stronie są wyświetlane pola **konfiguracji** kartę.
2. Strony właściwości dla roli, wybierz **ustawienia**.
3. W **konfiguracji usługi** listy, wybierz nazwę konfiguracji usługi, które chcesz edytować. Jeśli chcesz wprowadzić zmiany dotyczące wszystkich konfiguracji usługi dla tej roli, można wybrać **wszystkie konfiguracje**.  Aby uzyskać więcej informacji o sposobie aktualizowania konfiguracji usługi, zobacz sekcję **Zarządzanie parametry połączenia dla konta magazynu** w temacie [konfigurowania ról dla usługi w chmurze platformy Azure z programem Visual Studio](vs-azure-tools-configure-roles-for-cloud-service.md).
4. Aby zmodyfikować jakiekolwiek ustawienie ciągu połączenia, wybierz **...** przycisk obok ustawienia. **Utworzyć parametry połączenia magazynu** zostanie wyświetlone okno dialogowe.
5. W obszarze **łączyć się przy użyciu**, wybierz **subskrypcji** opcji.
6. W **subskrypcji** Wybierz subskrypcję. Jeśli na liście nie zawiera jedną, która ma, wybierz **pobieranie ustawień publikowania** łącza.
7. W **nazwa konta** listy, wybierz nazwę konta magazynu. Narzędzia Azure automatycznie uzyskuje poświadczeń konta magazynu przy użyciu pliku .publishsettings. Aby ręcznie określić poświadczenia konta magazynu, wybierz **ręcznie wprowadzić poświadczenia** opcji, a następnie kontynuuj tej procedury. Można uzyskać nazwę konta magazynu i klucz podstawowy z [klasycznego portalu Azure](http://go.microsoft.com/fwlink/p/?LinkID=213885). Jeśli nie chcesz określać ustawienia ręcznie, wybierz konto magazynu **OK** przycisk, aby zamknąć okno dialogowe.
8. Wybierz **wprowadź konto magazynu** łącze poświadczeń.
9. W **nazwa konta** wprowadź nazwę konta magazynu.

   > [!NOTE]
   > Zaloguj się do [klasycznego portalu Azure](http://go.microsoft.com/fwlink/?LinkID=213885), a następnie wybierz pozycję **magazynu** przycisku. Portal zawiera listę kont magazynu. Jeśli wybierzesz konto otwiera stronę dla niego. Nazwa konta magazynu można skopiować z tej strony. Jeśli używasz poprzedniej wersji klasycznego portalu nazwę konta magazynu jest wyświetlana w **kont magazynu** widoku. Aby skopiować tej nazwy, zaznacz go w **właściwości** okna tego wyświetlić, a następnie wybierz klawiszy Ctrl-C. Aby wkleić nazwę do programu Visual Studio, wybierz **nazwa konta** tekst pola, a następnie wybierz klawiszy Ctrl + V.
   >
   >
10. W **klucz konta** polu, wprowadź klucz podstawowy lub skopiuj i wklej go z [klasycznego portalu Azure](http://go.microsoft.com/fwlink/?LinkID=213885).
     Aby skopiować ten klucz:

    1. W dolnej części strony dla konta magazynu odpowiednie, wybierz **zarządzanie kluczami** przycisku.
    2. Na **zarządzanie kluczami dostępu** strony, zaznacz tekst podstawowy klucz dostępu, a następnie wybierz klawiszy Ctrl + C.
    3. W menu Narzędzia Azure, Wklej klucz do **klucz konta** pole.
    4. Należy wybrać jedną z poniższych opcji, aby określić, jak usługa będą uzyskiwać dostęp do konta magazynu:

       * **Korzystanie z protokołu HTTP**. Jest to opcja standardowa. Na przykład `http://<account name>.blob.core.windows.net`.
       * **Użycie protokołu HTTPS** bezpiecznego połączenia. Na przykład `https://<accountname>.blob.core.windows.net`.
       * **Określ niestandardowe punkty końcowe** dla każdego z trzech usług. Następnie możesz wpisać tymi punktami końcowymi do pola dla określonej usługi.

         > [!NOTE]
         > Tworząc niestandardowe punkty końcowe, można utworzyć bardziej złożoną parametry połączenia. Korzystając z tego formatu ciągu, można określić punktów końcowych usługi magazynu zawierające nazwę domeny niestandardowej zarejestrowany dla konta magazynu w usłudze obiektów Blob. Można również przyznać dostęp tylko do zasobów obiektów blob w kontenerze pojedynczego za pomocą sygnatury dostępu współdzielonego. Aby uzyskać więcej informacji na temat tworzenia niestandardowych punktów końcowych, zobacz [skonfigurować parametry połączenia magazynu Azure](storage/common/storage-configure-connection-string.md).
         >
         >
11. Aby zapisać te zmiany w ciągu połączenia, wybierz **OK** przycisk, a następnie wybierz pozycję **zapisać** przycisk na pasku narzędzi. Po zapisaniu zmian można pobrać wartości tych parametrów połączenia w kodzie za pomocą [GetConfigurationSettingValue](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleenvironment.getconfigurationsettingvalue.aspx). Podczas publikowania aplikacji na platformie Azure, wybierz konfigurację usługi zawierającej konto magazynu Azure w ciągu połączenia. Po opublikowaniu aplikacji, sprawdź, czy aplikacja działa zgodnie z oczekiwaniami w odniesieniu do usług Azure storage

## <a name="next-steps"></a>Następne kroki
Aby dowiedzieć się więcej na temat publikowania aplikacji na platformie Azure w programie Visual Studio, zobacz [publikowania usługi w chmurze przy użyciu narzędzia Azure](vs-azure-tools-publishing-a-cloud-service.md).
