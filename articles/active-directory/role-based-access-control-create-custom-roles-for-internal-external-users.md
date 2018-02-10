---
title: "Tworzenie niestandardowych dostępu opartej na rolach ról kontroli i przypisać do użytkowników wewnętrznych i zewnętrznych na platformie Azure | Dokumentacja firmy Microsoft"
description: "Przypisz role RBAC niestandardowe utworzone przy użyciu programu PowerShell i interfejsu wiersza polecenia dla użytkowników wewnętrznych i zewnętrznych"
services: active-directory
documentationcenter: 
author: rolyon
manager: mtillman
editor: kgremban
ms.assetid: 
ms.service: active-directory
ms.devlang: 
ms.topic: article
ms.tgt_pltfrm: 
ms.workload: identity
ms.date: 12/06/2017
ms.author: rolyon
ms.reviewer: skwan
ms.custom: it-pro
ms.openlocfilehash: 75a45b492c230b19d2f7237f8ea7fe2c49de29bf
ms.sourcegitcommit: 059dae3d8a0e716adc95ad2296843a45745a415d
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 02/09/2018
---
# <a name="intro-on-role-based-access-control"></a>Wprowadzenie dotyczących kontroli dostępu opartej na rolach

Kontrola dostępu oparta na rolach to Azure portalu funkcja tylko stosowanie właściciele subskrypcji do przypisywania ról szczegółowego do innych użytkowników zarządzających zasobów dla określonych zakresów w swoim środowisku.

RBAC umożliwia lepsze zarządzanie zabezpieczeniami dla dużych organizacji oraz dla małych i średnich firmach praca z zewnętrznym współpracownikom, dostawców lub freelancers wymagających dostępu do określonych zasobów w danym środowisku, ale niekoniecznie całej infrastruktury lub dowolny zakresy związanych z rozliczeniami. RBAC umożliwia elastyczność będący właścicielem jedną subskrypcją platformy Azure zarządzanych przez administratora konta (usługi roli administrator na poziomie subskrypcji) i mieć wielu użytkowników zaproszenie do pracy w ramach tej samej subskrypcji, ale bez jakichkolwiek praw administracyjnych dla niego. Zarządzanie i rozliczeń funkcję RBAC okaże się, że czas i zarządzanie wydajne opcji korzystania z funkcji Azure w różnych scenariuszach.

## <a name="prerequisites"></a>Wymagania wstępne
W środowisku platformy Azure przy użyciu funkcji RBAC wymaga:

* Posiadanie autonomiczny subskrypcji platformy Azure powierzonych użytkownika jako właściciela (rola subskrypcji)
* Rola właściciela subskrypcji platformy Azure
* Ma dostęp do [portalu Azure](https://portal.azure.com)
* Upewnij się, że ma następujących dostawców zasobów w zarejestrowany dla subskrypcji użytkownika: **Microsoft.Authorization**. Aby uzyskać więcej informacji na temat rejestrowania dostawców zasobów, zobacz [dostawców usługi Resource Manager, regiony, wersje interfejsu API i schematów](../azure-resource-manager/resource-manager-supported-services.md).

> [!NOTE]
> Licencje usługi Azure Active Directory lub subskrypcji usługi Office 365 (na przykład: dostęp do usługi Azure Active Directory) pobranego z Centrum nie kwalifikuje się do przy użyciu funkcji RBAC Office 365 Admin.

## <a name="how-can-rbac-be-used"></a>Jak można użyć RBAC
RBAC można zastosować na trzy różne zakresy na platformie Azure. Z najwyższą zakresu na najniższym jeden są następujące:

* Subskrypcja (najwyższy)
* Grupa zasobów
* Zakres zasobów (najniższy poziom dostępu do oferty docelowe uprawnienia do zakresu poszczególnych zasobów platformy Azure)

## <a name="assign-rbac-roles-at-the-subscription-scope"></a>Przypisz role RBAC w zakresie subskrypcji
Istnieją dwie typowe przykłady dotyczące RBAC jest używana (między innymi):

* Użytkowników zewnętrznych z organizacji (nie jest częścią dzierżawy usługi Azure Active Directory dla użytkownika administracyjnego) zaproszenie do zarządzania niektórych zasobów lub całej subskrypcji
* Praca z użytkownikami w organizacji (są one częścią dzierżawy usługi Azure Active Directory użytkownika), ale należy do różnych zespołów lub grup, wymagających szczegółowego dostępu do całej subskrypcji lub do określonych grup zasobów lub zakresy zasobów w środowisku

## <a name="grant-access-at-a-subscription-level-for-a-user-outside-of-azure-active-directory"></a>Udziel dostępu na poziomie subskrypcji dla użytkownika poza usługą Azure Active Directory
Role RBAC może zostać przydzielony tylko przez **właścicieli** subskrypcji w związku z tym administrator musi być zalogowany z nazwą użytkownika, które tej roli wstępnie przypisany lub została utworzona subskrypcja platformy Azure.

W portalu Azure po można zalogować jako administrator, wybierz "Subskrypcji" i wybierz jedno.
![Subskrypcja bloku w portalu Azure](./media/role-based-access-control-create-custom-roles-for-internal-external-users/0.png) domyślnie, jeśli dla użytkownika administracyjnego kupiła subskrypcji platformy Azure, użytkownik będzie wyświetlany jako **administrator konta**, to jest rola subskrypcji. Więcej szczegółów na rolach subskrypcji platformy Azure, zobacz [Dodawanie lub zmienianie ról administrator usługi Azure, które zarządzają subskrypcji lub usługi](/billing/billing-add-change-azure-subscription-administrator.md).

W tym przykładzie użytkownik "alflanigan@outlook.com" jest **właściciela** z "Bezpłatnej wersji próbnej" subskrypcji w usłudze AAD dzierżawy "Dzierżawy Azure Default". Ponieważ ten użytkownik jest twórca subskrypcji platformy Azure z początkowej Account Microsoft "Outlook" (Account Microsoft = programu Outlook, Live itp.) będzie domyślna nazwa domeny dla wszystkich innych użytkowników dodane w tej dzierżawie **"@alflaniganuoutlook.onmicrosoft.com"**. Zgodnie z projektem składni nowej domeny jest tworzony przez zestawienie nazwę użytkownika i domenę nazwę użytkownika, który utworzył dzierżawcy i dodawanie rozszerzenia **". onmicrosoft.com"**.
Ponadto użytkownicy mogą zalogować się przy użyciu niestandardowej nazwy domeny w dzierżawie po dodaniu i weryfikowanie jego dla nowej dzierżawy. Aby uzyskać więcej informacji na temat sposobu zweryfikować niestandardowej nazwy domeny w dzierżawie usługi Azure Active Directory, zobacz [Dodawanie niestandardowej nazwy domeny do katalogu](/active-directory/active-directory-add-domain).

W tym przykładzie katalog "Domyślna dzierżawa usługi Azure" zawiera tylko użytkownicy z nazwą domeny "@alflanigan.onmicrosoft.com".

Po wybraniu subskrypcji, administrator musi kliknij **kontroli dostępu (IAM)** , a następnie **dodania roli**.





![Funkcja IAM kontroli dostępu w portalu Azure](./media/role-based-access-control-create-custom-roles-for-internal-external-users/1.png)





![Dodaj nowego użytkownika w funkcja IAM kontroli dostępu w portalu Azure](./media/role-based-access-control-create-custom-roles-for-internal-external-users/2.png)

Następnym krokiem jest wybranie roli do przypisania i użytkownika, którego rola RBAC zostanie przypisana do. W **roli** menu rozwijanym administratora użytkownik widzi tylko wbudowane role RBAC, które są dostępne w systemie Azure. Aby uzyskać bardziej szczegółowe wyjaśnienia dotyczące poszczególnych ról i ich zakresy możliwe do przypisania, zobacz [wbudowanych ról dla kontroli dostępu](role-based-access-built-in-roles.md).

Następnie administrator musi dodać adres e-mail użytkownika zewnętrznego. Oczekiwane zachowanie jest dla użytkownika zewnętrznego, które nie są wyświetlani w istniejącej dzierżawy. Po Zaproszono użytkownika zewnętrznego, on będą widoczne w obszarze **subskrypcji > kontroli dostępu (IAM)** z wszystkich bieżących użytkowników, które są obecnie przypisane roli RBAC w zakresie subskrypcji.





![Dodaj uprawnienia do nowej roli RBAC](./media/role-based-access-control-create-custom-roles-for-internal-external-users/3.png)





![Lista ról RBAC na poziomie subskrypcji](./media/role-based-access-control-create-custom-roles-for-internal-external-users/4.png)

Użytkownik "chessercarlton@gmail.com" zaproszono jako **właściciela** dla subskrypcji "Bezpłatnej wersji próbnej". Po wysłaniu zaproszenia, zewnętrznych użytkownik otrzyma wiadomość e-mail z potwierdzeniem z link aktywacji.
![wiadomość e-mail z zaproszeniem dla roli RBAC](./media/role-based-access-control-create-custom-roles-for-internal-external-users/5.png)

Trwa spoza organizacji, nowy użytkownik nie ma żadnych istniejących atrybutów w katalogu "Domyślna dzierżawa usługi Azure". Będzie można utworzyć po uzyskaniu zgody użytkownika zewnętrznego mają być rejestrowane w katalogu, który jest skojarzony z subskrypcją został przydzielony do roli.





![wiadomość e-mail zaproszenia dla roli RBAC](./media/role-based-access-control-create-custom-roles-for-internal-external-users/6.png)

Pokazuje użytkownika zewnętrznego w dzierżawcy usługi Azure Active Directory od teraz jako użytkownik zewnętrzny i to można wyświetlić w portalu Azure.





![Użytkownicy portalu Azure usługi active directory bloku azure](./media/role-based-access-control-create-custom-roles-for-internal-external-users/7.png)



W **użytkowników** widoku, użytkownicy zewnętrzni mogą być rozpoznawane przez typ inną ikonę w portalu Azure.

Jednak udzielanie **właściciela** lub **współautora** dostępu do użytkownika zewnętrznego w **subskrypcji** zakresu, nie zezwala na dostęp do katalogu dla użytkownika administracyjnego, chyba że **administratora globalnego** pozwala. W ich właściwości użytkownika **typ użytkownika**, który ma dwie typowe parametry, **elementu członkowskiego** i **gościa** mogą zostać zidentyfikowane. Element członkowski jest użytkownik, który jest zarejestrowany w katalogu, gdy Gość jest użytkownikiem zaproszenie do katalogu z zewnętrznego źródła. Aby uzyskać więcej informacji, zobacz [jak Administratorzy usługi Azure Active Directory dodać użytkowników współpracy B2B](active-directory-b2b-admin-add-users.md).

> [!NOTE]
> Upewnij się, że po wprowadzeniu poświadczeń w portalu, użytkownik zewnętrzny wybiera do logowania się w poprawnym katalogu. Tego samego użytkownika może mieć dostęp do wielu katalogów i można wybrać jedną z nich, klikając nazwę użytkownika w góry po prawej stronie w portalu Azure a następnie wybierz odpowiedniego katalogu z listy rozwijanej.

Będąc gościa w katalogu użytkownika zewnętrznego mogą zarządzać zasobami wszystkich subskrypcji platformy Azure, ale nie można uzyskać dostępu do katalogu.





![dostęp ograniczony do portalu Azure usługi azure active directory](./media/role-based-access-control-create-custom-roles-for-internal-external-users/9.png)

Azure Active Directory i subskrypcji platformy Azure nie ma relacji relacji nadrzędny podrzędny, takich jak innych zasobów platformy Azure (na przykład: maszyn wirtualnych, sieci wirtualnych, aplikacje sieci web, magazynu itp.) z subskrypcją platformy Azure. Wszystkie ostatnie jest utworzony, zarządzane i rozliczany w ramach subskrypcji platformy Azure, podczas gdy subskrypcji platformy Azure jest używany do zarządzania dostępem do usługi Azure directory. Aby uzyskać więcej informacji, zobacz [subskrypcji jak Azure jest powiązana z usługą Azure AD](/active-directory/active-directory-how-subscriptions-associated-directory).

Z wszystkich wbudowane role RBAC **właściciela** i **współautora** oferują pełnego zarządzania dostęp do wszystkich zasobów w środowisku różnica, że współautora nie może tworzyć i usuwać nowe role RBAC. Inne role wbudowane, takich jak **współautora maszyny wirtualnej** oferować pełnego zarządzania dostęp tylko do zasobów, jest określany przez nazwę, niezależnie od tego **grupy zasobów** jest tworzona w.

Przypisywanie roli RBAC wbudowanych **Współautor·maszyny·wirtualnej** na poziomie subskrypcji, oznacza, że użytkownikowi przypisano rolę:

* Można wyświetlić wszystkich maszyn wirtualnych niezależnie od daty wdrożenia i grupy zasobów, które są częścią
* Ma dostęp do pełnego zarządzania do maszyn wirtualnych w subskrypcji
* Nie można wyświetlić inne typy zasobów w subskrypcji
* Nie można wykonać operacji zmiany z punktu widzenia rozliczeń

## <a name="assign-a-built-in-rbac-role-to-an-external-user"></a>Przypisywanie roli RBAC wbudowanych do użytkownika zewnętrznego
Dla innego scenariusza, w tym teście użytkownika zewnętrznego "alflanigan@gmail.com" zostanie dodany jako **Współautor·maszyny·wirtualnej**.




![wbudowana Rola współautora maszyny wirtualnej](./media/role-based-access-control-create-custom-roles-for-internal-external-users/11.png)

Normalne zachowanie dla tego użytkownika zewnętrznego z tą rolą wbudowanych jest wyświetlanie i zarządzanie nimi tylko maszyny wirtualne i ich sąsiadujących ze sobą Menedżer zasobów tylko zasoby niezbędne podczas wdrażania. Zgodnie z projektem te role ograniczone zapewniają dostęp tylko do ich zasobów odpowiedniego utworzone w portalu Azure.



![Omówienie roli współautora maszyny wirtualnej w portalu Azure](./media/role-based-access-control-create-custom-roles-for-internal-external-users/12.png)

## <a name="grant-access-at-a-subscription-level-for-a-user-in-the-same-directory"></a>Udziel dostępu na poziomie subskrypcji dla użytkownika w tym samym katalogu
Przepływ procesu jest taki sam jak dodawanie użytkownika zewnętrznego, zarówno z perspektywy administracyjnej przyznania roli RBAC, a także użytkownika zostanie im przyznany dostęp do roli. Różnica polega na tym że zaproszonych użytkownik nie będzie otrzymywać żadnych zaproszeń do skorzystania z poczty e-mail, jak wszystkie zakresy zasobów w subskrypcji będą dostępne na pulpicie nawigacyjnym po zalogowaniu się.

## <a name="assign-rbac-roles-at-the-resource-group-scope"></a>Przypisz role RBAC w zakresie grupy zasobów
Przypisywanie roli RBAC **grupy zasobów** zakres ma taki sam proces przypisywania roli na poziomie subskrypcji dla obu typów użytkownicy — zewnętrznym lub wewnętrznym (część z tym samym katalogu). Użytkownicy, którym przypisano rolę RBAC ma zobacz w swoim środowisku tylko grupy zasobów z przypisanym dostępem z **grup zasobów** ikonę w portalu Azure.

## <a name="assign-rbac-roles-at-the-resource-scope"></a>Przypisz role RBAC w zakresie zasobów
Przypisywanie roli RBAC w zakresie zasobów na platformie Azure mają identyczne proces przypisywania roli na poziomie subskrypcji lub na poziomie grupy zasobów, po tym samym przepływie pracy w obydwu scenariuszach. Ponownie, użytkowników, którym przypisano rolę RBAC można wyświetlanie tylko tych elementów, które przypisano dostępu do w **wszystkie zasoby** kartę lub bezpośrednio w ich pulpitu nawigacyjnego.

Istotnym elementem do RBAC zarówno w zakresie grupy zasobów lub zasobów zakresie jest przeznaczony dla użytkowników upewnić się zarejestrować się w poprawnym katalogu.





![katalog logowania w portalu Azure](./media/role-based-access-control-create-custom-roles-for-internal-external-users/13.png)

## <a name="assign-rbac-roles-for-an-azure-active-directory-group"></a>Przypisz role RBAC dla grupy usługi Azure Active Directory
Wszystkie scenariusze na trzy różne zakresy na platformie Azure przy użyciu funkcji RBAC oferują uprawnienie Zarządzanie, wdrażanie i administrowanie różnych zasobów jako przypisany użytkownik bez konieczności zarządzania osobiste subskrypcji. Niezależnie od przypisano rolę RBAC dla subskrypcji, grupy zasobów lub zasobów zakresu, wszystkie zasoby utworzone dalej przez przypisanych użytkowników są rozliczane zgodnie z jedną subskrypcją platformy Azure, której użytkownicy mają dostęp do. Dzięki temu użytkowników, którzy mają rozliczeń uprawnień administratora dla całej subskrypcji platformy Azure ma pełny przegląd zużycia, niezależnie od tego, kto jest zarządzania zasobami.

W przypadku większych organizacji role RBAC można zastosować w taki sam sposób dla uwzględnieniu perspektywy administrator chce szczegółowego dostęp dla zespołów lub całego działów, indywidualnie dla każdego użytkownika, w związku z tym uwzględnieniu bardzo czas i zarządzanie wydajne opcja grup usługi Azure Active Directory. Przykład ilustrujący **współautora** rola została dodana do jednej z grup w dzierżawie na poziomie subskrypcji.





![Dodaj rolę RBAC dla grup usługi AAD](./media/role-based-access-control-create-custom-roles-for-internal-external-users/14.png)

Grupy te są grup zabezpieczeń, które są udostępniane i zarządzane tylko w ramach usługi Azure Active Directory.

## <a name="create-a-custom-rbac-role-to-open-support-requests-using-powershell"></a>Utwórz niestandardową rolę RBAC można otworzyć żądania obsługi przy użyciu programu PowerShell
Wbudowane role RBAC, które są dostępne w systemie Azure zapewnia określone poziomy uprawnień na podstawie dostępnych zasobów w środowisku. Jeśli żadna z tych ról potrzeb dla użytkownika administracyjnego, istnieje jednak możliwość ograniczenia dostępu nawet więcej, tworząc niestandardowe role RBAC.

Tworzenie niestandardowych ról RBAC wymaga, aby zająć jedną rolę wbudowanych, edycji, a następnie zaimportuj go ponownie w środowisku. Pobieranie i przekazywania roli są zarządzane przy użyciu programu PowerShell lub interfejsu wiersza polecenia.

Należy zrozumieć wymagania wstępne tworzenie niestandardowej roli zabezpieczeń, które można udostępniać szczegółowe na poziomie subskrypcji, a także umożliwić zaproszonych użytkowników elastyczność otwarcia żądania pomocy technicznej.

Na przykład wbudowana rola **czytnika**, który umożliwia użytkownikom dostęp do wyświetlania wszystkich zakresów zasobów, ale nie można ich edytować lub utworzyć nowe, został dostosowany do Zezwalaj użytkownikom z możliwością otwarcia żądania pomocy technicznej.

Pierwszą akcją eksportowania **czytnika** roli musi zostać wykonane w programie PowerShell został uruchomiony z podwyższonym poziomem uprawnień administratora.

```
Login-AzureRMAccount

Get-AzureRMRoleDefinition -Name "Reader"

Get-AzureRMRoleDefinition -Name "Reader" | ConvertTo-Json | Out-File C:\rbacrole2.json

```





![Zrzut ekranu programu PowerShell dla roli czytnika RBAC](./media/role-based-access-control-create-custom-roles-for-internal-external-users/15.png)

Następnie należy wyodrębnić szablonu JSON w roli.





![Szablon JSON dla niestandardowej roli zabezpieczeń czytnika RBAC](./media/role-based-access-control-create-custom-roles-for-internal-external-users/16.png)

Typowa rola RBAC składa się z trzech głównych sekcji, **akcje**, **NotActions** i **AssignableScopes**.

W **akcji** sekcji są wymienione wszystkie działania dozwolone dla tej roli. Należy zrozumieć, że każda akcja przypisano od dostawcy zasobów. W takim przypadku służący do tworzenia biletów pomocy technicznej **Microsoft.Support** musi być wymieniona dostawcy zasobów.

Aby można było wyświetlić wszystkich dostawców zasobów dostępnych i zarejestrowanych w ramach subskrypcji, można użyć programu PowerShell.
```
Get-AzureRMResourceProvider

```
Ponadto można sprawdzić wszystkie dostępne polecenia cmdlet programu PowerShell do zarządzania dostawcy zasobów.
    ![Zrzut ekranu programu PowerShell do zarządzania dostawcy zasobów](./media/role-based-access-control-create-custom-roles-for-internal-external-users/17.png)

Aby ograniczyć wszystkie akcje dla określonej roli RBAC, dostawców zasobów są wymienione w sekcji **NotActions**.
Ostatnio jest to konieczne, że rola RBAC zawiera jawne subskrypcji identyfikatorów, w którym została użyta. Identyfikatory subskrypcji są wyświetlane w obszarze **AssignableScopes**, w przeciwnym razie będzie nie można zaimportować roli w ramach subskrypcji.

Po utworzeniu i dostosowywanie roli RBAC, musi zostać zaimportowany kopii środowiska.

```
New-AzureRMRoleDefinition -InputFile "C:\rbacrole2.json"

```

W tym przykładzie niestandardową nazwę dla tej roli RBAC jest "Czytnika obsługi biletów poziom dostępu" dzięki czemu użytkownik, aby wyświetlić wszystkie elementy w ramach subskrypcji, a także do otwarcia żądania pomocy technicznej.

> [!NOTE]
> Są dwa tylko wbudowane role RBAC umożliwiając akcji otwarcia żądania pomocy technicznej **właściciela** i **współautora**. Użytkownik może mieć możliwość otwarcia żądania pomocy technicznej on musi posiadać rolę RBAC tylko w zakresie subskrypcji, ponieważ wszystkie żądania pomocy technicznej są tworzone na podstawie subskrypcji platformy Azure.

Ta nowa rola niestandardowych zostanie przypisana do użytkownika z tym samym katalogu.





![Zrzut ekranu przedstawiający niestandardową rolę RBAC zaimportowany w portalu Azure](./media/role-based-access-control-create-custom-roles-for-internal-external-users/18.png)





![Zrzut ekranu przedstawiający przypisywanie niestandardowej roli zabezpieczeń RBAC zaimportowane do użytkownika w tym samym katalogu](./media/role-based-access-control-create-custom-roles-for-internal-external-users/19.png)





![Zrzut ekranu przedstawiający uprawnienia niestandardowe importowanych roli RBAC](./media/role-based-access-control-create-custom-roles-for-internal-external-users/20.png)

Przykład został opisany bardziej szczegółowo aby podkreślić limitów tę rolę niestandardową RBAC w następujący sposób:
* Można tworzyć nowe żądania pomocy technicznej
* Nie można utworzyć nowe zakresy zasobów (na przykład: maszyny wirtualnej)
* Nie można utworzyć nowej grupy zasobów





![Zrzut ekranu przedstawiający niestandardową rolę RBAC tworzenia żądań obsługi](./media/role-based-access-control-create-custom-roles-for-internal-external-users/21.png)





![Zrzut ekranu przedstawiający niestandardową rolę RBAC nie można utworzyć maszyny wirtualne](./media/role-based-access-control-create-custom-roles-for-internal-external-users/22.png)





![Zrzut ekranu przedstawiający niestandardowych nie można utworzyć nowego RGs roli RBAC](./media/role-based-access-control-create-custom-roles-for-internal-external-users/23.png)

## <a name="create-a-custom-rbac-role-to-open-support-requests-using-azure-cli"></a>Utwórz niestandardową rolę RBAC można otworzyć żądania obsługi przy użyciu wiersza polecenia platformy Azure
Uruchomiony na komputerze Mac i bez uzyskiwania dostępu do programu PowerShell, interfejsu wiersza polecenia Azure to sposób go.

Kroki umożliwiające utworzenie niestandardowej roli zabezpieczeń są takie same, z wyjątkiem wyłącznie przy użyciu interfejsu wiersza polecenia roli nie można pobrać szablonu JSON, ale można je wyświetlić w interfejsu wiersza polecenia.

W tym przykładzie wybrano I wbudowana rola **czytnika kopii zapasowej**.

```

azure role show "backup reader" --json

```





![Pokaż zrzut ekranu interfejsu wiersza polecenia rolę czytelnika kopii zapasowej](./media/role-based-access-control-create-custom-roles-for-internal-external-users/24.png)

Edytowanie roli w programie Visual Studio po skopiowaniu ich właściwości w szablonie JSON **Microsoft.Support** dostawca zasobów został dodany w **akcje** sekcjach, aby ten użytkownik może otworzyć żądania pomocy technicznej pozostawiając można czytnik dla magazynów kopii zapasowych. Ponownie należy dodać identyfikator subskrypcji, w którym ta rola będzie używany w **AssignableScopes** sekcji.

```

azure role create --inputfile <path>

```





![Zrzut ekranu interfejsu wiersza polecenia importowania niestandardowej roli zabezpieczeń RBAC](./media/role-based-access-control-create-custom-roles-for-internal-external-users/25.png)

Nowa rola jest teraz dostępna w portalu Azure i proces assignation jest taki sam, jak w poprzednich przykładach.





![Azure portalu zrzut ekranu przedstawiający niestandardową rolę RBAC utworzone za pomocą interfejsu wiersza polecenia 1.0](./media/role-based-access-control-create-custom-roles-for-internal-external-users/26.png)

Od najnowszej 2017 kompilacji powłoka chmury Azure jest ogólnie dostępna. Powłoka chmury Azure jest uzupełnienie IDE i portalu Azure. Z tą usługą Pobierz powłoką bazujące na przeglądarce, która jest uwierzytelniane i hostowanej na platformie Azure i można go użyć zamiast interfejsu wiersza polecenia na komputerze jest zainstalowany.





![Azure Cloud Shell](./media/role-based-access-control-create-custom-roles-for-internal-external-users/27.png)
