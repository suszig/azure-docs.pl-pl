---
title: "Azure zarządzanych aplikacji w witrynie Marketplace | Dokumentacja firmy Microsoft"
description: "W tym artykule opisano Azure zarządzane aplikacje, które są dostępne na rynku."
services: azure-resource-manager
author: ravbhatnagar
manager: rjmax
ms.service: azure-resource-manager
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.date: 07/09/2017
ms.author: gauravbh; tomfitz
ms.openlocfilehash: 58ac7665abf7e75a43bb0b92bdf6f41005c3efe8
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 08/18/2017
---
# <a name="azure-managed-applications-in-the-marketplace"></a>Azure zarządzanych aplikacji w witrynie Marketplace

 MSPs, niezależnym dostawcom oprogramowania i integratorów systemów. platforma (SIs) można używać platformy Azure zarządzanych aplikacji w celu zaoferowania ich rozwiązania dla wszystkich klientów w portalu Azure Marketplace. W przypadku takich rozwiązań zmniejszyć konserwacji i obsługi obciążenia dla klientów. Wydawcy sprzedać infrastruktury i oprogramowaniem za pośrednictwem portalu Marketplace. One dołączyć usług oraz obsługi do zarządzanych aplikacji. Aby uzyskać więcej informacji, zobacz [Omówienie aplikacji zarządzanych](managed-application-overview.md).

W tym artykule opisano, jak MSP, niezależnego dostawcy oprogramowania lub SI publikowanie aplikacji w portalu Marketplace i były szeroko dostępne dla klientów.

## <a name="prerequisites-for-publishing-a-managed-application"></a>Wymagania wstępne dotyczące publikowania aplikacji zarządzanej

Wymagania wstępne dotyczące listy w witrynie Marketplace:

* Techniczne

    *  Aby uzyskać informacje o podstawowej struktury i składni szablonów usługi Azure Resource Manager, zobacz [szablonów usługi Azure Resource Manager](resource-group-authoring-templates.md).
    *  Aby wyświetlić pełny szablon rozwiązania, zobacz [szablonów Szybki Start Azure](https://azure.microsoft.com/en-us/documentation/templates/) lub [repozytorium szablonów Szybki Start](https://github.com/azure/azure-quickstart-templates).
    *  Informacje o sposobie tworzenia interfejsu dla klientów, którzy wdrażania aplikacji za pośrednictwem portalu Marketplace, zobacz [Utwórz plik definicji interfejsu użytkownika](managed-application-createuidefinition-overview.md).

* Nietechnicznym (wymagania biznesowe)

    *   Firmy lub zależnemu musi znajdować się w danym kraju, w którym sprzedaży są obsługiwane przez witryny Marketplace.
    *   W taki sposób, który jest zgodny z rozliczeń modeli obsługiwanych przez witryny Marketplace muszą mieć licencję na produkt.
    *   Wszystko jest odpowiedzialny za udostępnienie pomocy technicznej dla klientów w sposób uzasadnione ekonomicznie. Obsługa można wolny, płatnej, lub za pośrednictwem społeczności obsługuje.
    *   Wszystko jest odpowiedzialny za Licencjonowanie oprogramowanie oraz wszystkie zależności oprogramowania innych firm.
    *   Należy podać zawartość, która spełnia kryteria dla Twojej oferty był wyświetlany w witrynie Marketplace oraz w portalu Azure.
    *   Musisz zaakceptować postanowienia Umowy o Azure Marketplace udział zasad i wydawcy.
    *   Musisz zaakceptować warunki użytkowania, zasady zachowania poufności informacji firmy Microsoft i certyfikowane umowę programu Microsoft Azure.

## <a name="create-a-new-azure-application-offer"></a>Tworzenie nowej oferty aplikacji Azure

Po spełniają wymagania wstępne, możesz utworzyć ofertę zarządzanej aplikacji. Spójrzmy szybki przegląd oferty i jednostki SKU.

### <a name="offer"></a>Oferta

Oferta dla aplikacji zarządzanej odpowiada klasie produktu oferty od wydawcy. Jeśli masz nowy typ rozwiązania/aplikacji, które mają być dostępne w witrynie Marketplace go skonfigurować, jako nowego oferty. Oferta jest kolekcją jednostki SKU. Co oferta zostanie wyświetlony jako własny element w witrynie Marketplace.

### <a name="sku"></a>SKU

Jednostka SKU jest najmniejszej jednostki oferowane oferty. SKU w tej samej klasie produktu (Oferta) umożliwia rozróżnianie między:

* Różne funkcje, które są obsługiwane.
* Określa, czy oferty jest zarządzane lub niezarządzane.
* Modele rozliczeń, które są obsługiwane.

Jednostka SKU jest wyświetlany w obszarze oferta nadrzędnego w witrynie Marketplace. Wygląda na to jako własny element oferowane w portalu Azure.

### <a name="set-up-an-offer"></a>Konfigurowanie oferty

1. Zaloguj się do [portalu dla partnerów chmury](https://cloudpartner.azure.com/).

2. W okienku nawigacji po lewej stronie wybierz **+ nowe oferty** > **aplikacji Azure**.

    ![Nowa oferta](./media/managed-application-author-marketplace/newOffer.png)

3. Wypełnianie formularzy, które pojawiają się po lewej stronie w **edytor** widoku. Wymagane pola są oznaczone czerwoną gwiazdką (*).

    ![Ustawienia oferty](./media/managed-application-author-marketplace/newOffer_OfferSettings.png)

    Cztery główne formularze są używane do tworzenia zarządzanej aplikacji:

    a. Ustawienia oferty

    b. Jednostki SKU

    c. Portal Marketplace

    d. Pomoc techniczna

Formularze są opisane bardziej szczegółowo w poniższych sekcjach.

## <a name="offer-settings-form"></a>Formularz Ustawienia oferty
Ten formularz podstawowy do określenia ustawień oferty.

1. Wypełnij **oferują ustawienia** formularza. Różne pola to:

    a. **Identyfikator oferty**: ten unikatowy identyfikator identyfikuje oferta w profilu wydawcy. Ten identyfikator jest widoczny w adresach URL produktu, szablony usługi Resource Manager i raporty rozliczeń. Go mogą się składać tylko małe znaki alfanumeryczne i łączniki (-). Identyfikator nie może kończyć się łącznikiem. Jego ograniczone do maksymalnie 50 znaków. Po ofertę odbywa się na żywo, to pole jest zablokowane.

    b. **Identyfikator wydawcy**: Użyj tej listy rozwijanej, aby wybrać profil wydawcy, którą chcesz opublikować tej oferty, w obszarze. Po ofertę odbywa się na żywo, to pole jest zablokowane.

    c. **Nazwa**: Ta nazwa wyświetlana dla danej oferty pojawia się w witrynie Marketplace oraz w portalu. Może mieć maksymalnie 50 znaków. Obejmują rozpoznawalną nazwę markę produktu. Nie zawierają nazwę swojej firmy, chyba że jest sposób jest ona sprzedawane. Ta oferta jest marketingu na własne witryny sieci Web, upewnij się, że nazwa jest dokładnie sposobu jej wyświetlania w witrynie sieci Web.

2. Wybierz **zapisać** Aby zapisać postęp. 

## <a name="skus-form"></a>Formularz jednostki SKU
Następnym krokiem jest można dodać jednostki SKU dla danej oferty.

1. Wybierz **jednostki SKU** > **nowe jednostki SKU**. 

    ![Wybierz nowe jednostki SKU](./media/managed-application-author-marketplace/newOffer_skus.png)

2. Wprowadź **identyfikator jednostki SKU**. Identyfikator jednostki SKU to unikatowy identyfikator dla jednostki SKU oferta. Ten identyfikator jest widoczny w adresach URL produktu, szablony usługi Resource Manager i raporty rozliczeń. Go mogą się składać tylko małe znaki alfanumeryczne i łączniki (-). Identyfikator nie może kończyć się kreską i ograniczone do maksymalnie 50 znaków. Po ofertę odbywa się na żywo, to pole jest zablokowane. Może mieć wiele jednostek SKU oferta. Należy jednostki SKU dla każdego obrazu, który chcesz opublikować.

3. Wypełnianie **szczegóły jednostki SKU** sekcji w następującej postaci:

    ![Podaj nowe jednostki SKU](./media/managed-application-author-marketplace/newOffer_newsku.png)

    Wypełnij następujące pola:
    
    a. **Tytuł**: Podaj tytuł tej jednostki SKU. Ten tytuł pojawi się w galerii dla tego elementu.

    b. **Podsumowanie**: Podaj krótkie podsumowanie dla tej wersji produktu. Ten tekst jest wyświetlany pod tytułem.

    c. **Opis elementu**: Podaj szczegółowy opis jednostki SKU.

    d. **Typ jednostki SKU**: dozwolone wartości **zarządzanej aplikacji** i **szablony rozwiązań**. W tym przypadku wybierz **zarządzanej aplikacji**.

4. Wypełnianie **informacje szczegółowe dotyczące pakietu** sekcji w następującej postaci:

    ![Pakiet](./media/managed-application-author-marketplace/newOffer_newsku_package.png)

    Wypełnij następujące pola:

    a. **Bieżąca wersja**: wprowadź wersję pakietu przekazywania. Powinna być w formacie `{number}.{number}.{number}{number}`.

    b. **Wybierz plik pakietu**: ten pakiet zawiera następujące pliki skompresowane do pliku zip:
    * **applianceMainTemplate.json**: plik szablonu wdrożenia, który służy do wdrażania rozwiązania/aplikacji. Aby uzyskać informacje o sposobach tworzenia wdrożenia pliki szablonów, zobacz [Tworzenie pierwszego szablonu usługi Azure Resource Manager](resource-manager-create-first-template.md).
    * **appliancecreateUIDefinition.json**: ten plik jest używany w portalu Azure, aby wygenerować interfejs użytkownika, który jest używany do udostępniania tego rozwiązania/aplikacji. Aby uzyskać więcej informacji, zobacz [wprowadzenie CreateUiDefinition](managed-application-createuidefinition-overview.md).
    * **mainTemplate.json**: ten plik szablonu zawiera tylko zasobu Microsoft.Solution/appliances. Plik mainTemplate zawiera następujące właściwości:

        *  **rodzaj**: Użyj **Marketplace** zarządzanych aplikacji w witrynie Marketplace.
        *  **ManagedResourceGroupId**: jest to grupa zasobów w subskrypcji klienta wdrożonym wszystkie zasoby zdefiniowane w applianceMainTemplate.json.
        *  **PublisherPackageId**: ten ciąg unikatowo identyfikujący pakietu. Podaj wartość w formacie `{publisherId}.{OfferId}.{SKUID}.{PackageVersion}`.

Uzyskaj **identyfikator oferty** i **identyfikator wydawcy** z portalu publikowania, jak pokazano na poniższej ilustracji:

![Identyfikator oferty](./media/managed-application-author-marketplace/UniqueString_pubid_offerid.png)
        
Uzyskaj **identyfikator jednostki SKU**, jak pokazano na poniższej ilustracji:

![IDENTYFIKATOR JEDNOSTKI SKU](./media/managed-application-author-marketplace/UniqueString_skuid.png)
        
Uzyskiwanie pakietu **wersji**, jak pokazano na poniższej ilustracji:

![Wersja pakietu](./media/managed-application-author-marketplace/UniqueString_packageversion.png)
    
  W poprzednich przykładach, wartość w oparciu **PublisherPackageId** jest `azureappliance-test.ravmanagedapptest.ravpreviewmanagedsku.1.0.0`.

  Przykładowe mainTemplate.json:

  ```json
  {
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
      "storageAccountNamePrefix": {
        "type": "string",
        "metadata": {
          "description": "Specify the name of the storage account"
        }
      },
      "storageAccountType": {
        "type": "string"
      }
    },
    "variables": {
      "managedResourceGroup": "[concat(resourceGroup().id,uniquestring(resourceGroup().id))]"
    },
    "resources": [{
      "type": "Microsoft.Solutions/appliances",
      "apiVersion": "2016-09-01-preview",
      "name": "[concat(parameters('storageAccountNamePrefix'), '-', 'managed')]",
      "location": "[resourceGroup().location]",
      "kind": "marketplace",
      "properties": {
        "managedResourceGroupId": "[variables('managedResourceGroup')]",
        "PublisherPackageId":"azureappliancetest.ravmanagedapptest.ravpreviewmanagedsku.1.0.0",
        "parameters": {
          "storageAccountName": {
            "value": "[parameters('storageAccountNamePrefix')]"
          },
          "storageAccountType": {
            "value": "[parameters('storageAccountType')]"
          }
        }
      }
    }],
    "outputs": {

    }
  }
  ```

Ten pakiet powinien zawierać wszystkie zagnieżdżone szablony lub skryptów, które są wymagane do udostępnienia pomyślnie tej aplikacji. MainTemplate.json, applianceMainTemplate.json i applianceCreateUIDefinition.json pliki muszą być obecne w folderze głównym.

* **Autoryzacje**: Ta właściwość określa, kto dostępu i poziom dostępu do zasobów w subskrypcji klientów. Wydawca służy do zarządzania aplikacją w imieniu klienta.
* **PrincipalId**: Ta właściwość jest identyfikator użytkownika, grupy użytkowników lub aplikacji, która ma pewne uprawnienia do zasobów w subskrypcji klienta usługi Azure Active Directory (Azure AD). Definicja roli Opisuje uprawnienia. 
* **Definicja roli**: Ta właściwość jest lista wszystkich kontroli dostępu opartej na rolach (RBAC) role wbudowane dostarczane przez usługę Azure AD. Możesz wybrać rolę, która jest najbardziej odpowiednia na potrzeby zarządzania zasobami w imieniu klienta.

Można dodać wiele zezwolenia. Zaleca się utworzenie grupy użytkowników usługi AD i określić jej identyfikator w **PrincipalId**. W ten sposób można dodać więcej użytkowników do grupy użytkowników, bez konieczności aktualizacji jednostki SKU.

Aby uzyskać więcej informacji o RBAC, zobacz [wprowadzenie RBAC w portalu Azure](../active-directory/role-based-access-control-what-is.md).

## <a name="marketplace-form"></a>Formularz Marketplace

Formularz Marketplace wprowadza się pola, które są widoczne na [portalu Azure Marketplace](https://azuremarketplace.microsoft.com) i na [portalu Azure](https://portal.azure.com/).

### <a name="preview-subscription-ids"></a>Identyfikatory subskrypcji w wersji zapoznawczej

Wprowadź listę identyfikatorów, które mogą uzyskać dostęp do oferty po opublikowaniu subskrypcji platformy Azure. Te subskrypcje wymienione biały można użyć do testowania przeglądanego oferty, przed wprowadzeniem go na żywo. Można kompilować białą listę maksymalnie 100 subskrypcji w portalu dla partnerów.

### <a name="suggested-categories"></a>Kategorii sugerowanych

Wybierz kategorie do pięciu z Twoją ofertę może być najlepiej skojarzony z listy. Te kategorie są używane do mapowania kategorie produktów, które są dostępne w Twojej oferty [portalu Azure Marketplace](https://azuremarketplace.microsoft.com) i [portalu Azure](https://portal.azure.com/).

#### <a name="azure-marketplace"></a>Azure Marketplace

Podsumowanie aplikacji zarządzanej wyświetla następujące pola:

![Podsumowanie witryny Marketplace](./media/managed-application-author-marketplace/publishvm10.png)

**Omówienie** karcie dla aplikacji zarządzanej wyświetla następujące pola:

![Omówienie portalu Marketplace](./media/managed-application-author-marketplace/publishvm11.png)

**Planów + cennik** karcie dla aplikacji zarządzanej wyświetla następujące pola:

![Plany Marketplace](./media/managed-application-author-marketplace/publishvm15.png)

#### <a name="azure-portal"></a>Azure Portal

Podsumowanie aplikacji zarządzanej wyświetla następujące pola:

![Portal podsumowania](./media/managed-application-author-marketplace/publishvm12.png)

Omówienie aplikacji zarządzanej wyświetla następujące pola:

![Przegląd portalu](./media/managed-application-author-marketplace/publishvm13.png)

#### <a name="logo-guidelines"></a>Wytycznych dotyczących logo

Skorzystaj z następujących wskazówek dla dowolnego logo, które można przekazać w portalu dla partnerów chmury:

*   Projekt platformy Azure ma paletę kolorów proste. Ogranicz liczbę podstawowy i pomocniczy kolorów w logo.
*   Kolorów motywu portalu są białe i czarne. Nie używaj kolorów te jako kolor tła dla logo. Użyj kolor, który sprawia, że logo jest widoczne w portalu. Zaleca się proste kolorów podstawowych. *Jeśli używasz przezroczyste tło, upewnij się, logo i tekst są białe, czarne lub niebieski.*
*   Nie używaj gradientu tła na logo.
*   Nie należy umieszczać na logo, nawet Twoja firma lub marką tekstu. Wygląd i działanie logo powinny być płaski i uniknąć gradienty.
*   Upewnij się, że nie jest rozciągana logo.

#### <a name="hero-logo"></a>Logo bohater

Logo bohater jest opcjonalna. Wydawcy można zrezygnować z przekazać logo bohater. Po przekazaniu ikona bohater nie można usunąć. W tym czasie partnera musi postępuj zgodnie z wytycznymi Marketplace, bohater ikon.

Skorzystaj z następujących wskazówek bohater ikony logo:

*   Nazwa wyświetlana wydawca, tytuł planu i długie Podsumowanie oferty są wyświetlane w biały. W związku z tym nie należy używać jasny kolor tła ikon bohater. Czarne, białe lub przezroczyste tło nie jest dozwolona dla bohater ikon.
*   Po oferty jest wyświetlana, wydawcy Nazwa wyświetlana, tytuł planu, długie Podsumowanie oferty i **Utwórz** przycisk programowo osadzone wewnątrz bohater logo. W rezultacie nie wprowadź tekst, podczas projektowania logo bohater. Pozostaw puste miejsce po prawej stronie, ponieważ tekst stanowi programowo, w tym miejscu. Puste miejsce na tekst powinien być 415 x 100 pikseli po prawej stronie. Jest on przesunięcia 370 pikseli z lewej strony.

    ![Przykład logo bohater](./media/managed-application-author-marketplace/publishvm14.png)

## <a name="support-form"></a>Obsługa formularza

Wypełnianie **obsługuje** formularza z obsługą kontaktuje się z Twojej firmy. Te informacje mogą inżynierii, kontaktów i kontaktów pomocy technicznej klienta.

## <a name="publish-an-offer"></a>Publikowanie oferty

Po wypełnieniu wszystkie sekcje wybierz **publikowania** można uruchomić procesu, który udostępnia klientom ofertę.

## <a name="next-steps"></a>Następne kroki

* Aby obejrzeć wprowadzenie do aplikacji zarządzanych, zobacz [Omówienie aplikacji zarządzanych](managed-application-overview.md).
* Aby dowiedzieć się, jak korzystanie z zarządzanych aplikacji z portalu Marketplace, zobacz [korzystać z platformy Azure zarządzanych aplikacji w witrynie Marketplace](managed-application-consume-marketplace.md).
* Aby dowiedzieć się, jak publikowanie aplikacji zarządzanych katalogu usług, zobacz [Utwórz i publikowanie aplikacji katalogu usług zarządzanych](managed-application-publishing.md).
* Aby dowiedzieć się, jak korzystanie z aplikacji zarządzanych katalogu usług, zobacz [korzystać z aplikacji zarządzanych katalogu usług](managed-application-consumption.md).
