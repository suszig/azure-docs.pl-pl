---
title: "Wdrażanie aplikacji sieci web przy użyciu szablonu - DB rozwiązania Cosmos Azure | Dokumentacja firmy Microsoft"
description: "Informacje o sposobie wdrażania konta bazy danych Azure rozwiązania Cosmos, aplikacje sieci Web usługi aplikacji Azure i przykładowej aplikacji sieci web przy użyciu szablonu usługi Azure Resource Manager."
services: cosmos-db, app-service\web
author: mimig1
manager: jhubbard
editor: monicar
documentationcenter: 
ms.assetid: 087d8786-1155-42c7-924b-0eaba5a8b3e0
ms.service: cosmos-db
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/08/2016
ms.author: mimig
ms.custom: mvc
ms.openlocfilehash: 7ceb4bf97c29a18d6879af55615eea46037c51ce
ms.sourcegitcommit: 7136d06474dd20bb8ef6a821c8d7e31edf3a2820
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 12/05/2017
---
# <a name="deploy-azure-cosmos-db-and-azure-app-service-web-apps-using-an-azure-resource-manager-template"></a>Wdrożenie bazy danych Azure rozwiązania Cosmos i Azure Web Apps App Service przy użyciu szablonu usługi Azure Resource Manager
Ten samouczek pokazuje, jak za pomocą szablonu usługi Azure Resource Manager można wdrożyć oraz integracji [bazy danych programu Microsoft Azure rozwiązania Cosmos](https://azure.microsoft.com/services/cosmos-db/), [usłudze Azure App Service](http://go.microsoft.com/fwlink/?LinkId=529714) aplikacji sieci web i przykładowej aplikacji sieci web.

Korzystanie z szablonów usługi Azure Resource Manager, można łatwo automatyzować wdrażania i konfigurowania zasobów platformy Azure.  W tym samouczku pokazano, jak wdrożyć aplikację sieci web i automatycznie skonfigurować parametry połączenia konta bazy danych Azure rozwiązania Cosmos.

Po ukończeniu tego samouczka, będzie mógł odpowiedzieć na następujące pytania:  

* Jak używać szablonu usługi Azure Resource Manager do wdrażania i integracji konto bazy danych rozwiązania Cosmos Azure i aplikacji sieci web w usłudze Azure App Service?
* Jak używać szablonu usługi Azure Resource Manager do wdrażania i integracji konta bazy danych rozwiązania Cosmos platformy Azure, aplikacji sieci web w aplikacji usługi sieci Web aplikacji i aplikację Webdeploy?

<a id="Prerequisites"></a>

## <a name="prerequisites"></a>Wymagania wstępne
> [!TIP]
> W tym samouczku nie zakłada się doświadczenie z szablonów usługi Azure Resource Manager lub JSON, powinien, którą chcesz zmodyfikować opcje wdrażania i do którego istnieje odwołanie szablony następnie wiedzę na temat każdego z tych obszarów będzie wymagane.
> 
> 

Przed wykonaniem instrukcji zawartych w tym samouczku, upewnij się, że masz następujące czynności:

* Subskrypcja platformy Azure. Azure to platforma subskrypcji.  Aby uzyskać więcej informacji na temat uzyskiwania subskrypcji, zobacz [opcje zakupu](https://azure.microsoft.com/pricing/purchase-options/), [oferty Członkowskie](https://azure.microsoft.com/pricing/member-offers/), lub [bezpłatnej wersji próbnej](https://azure.microsoft.com/pricing/free-trial/).

## <a id="CreateDB"></a>Krok 1: Pobierz pliki szablonów
Zacznijmy od pobierania plików szablonów, które będą używane w tym samouczku.

1. Pobierz [Tworzenie konta bazy danych Azure rozwiązania Cosmos, aplikacje sieci Web i wdrażanie przykładowej aplikacji demonstracyjnej](https://portalcontent.blob.core.windows.net/samples/DocDBWebsiteTodo.json) szablonu do folderu lokalnego (np. C:\Azure rozwiązania Cosmos DBTemplates). Ten szablon zostanie wdrożona, konto bazy danych Azure rozwiązania Cosmos, aplikację usługi aplikacji sieci web i aplikacji sieci web.  Zostanie ona automatycznie skonfigurowana aplikacji sieci web do nawiązania połączenia konta bazy danych Azure rozwiązania Cosmos.
2. Pobierz [Tworzenie konta bazy danych Azure rozwiązania Cosmos i przykładowej aplikacji sieci Web](https://portalcontent.blob.core.windows.net/samples/DocDBWebSite.json) szablonu do folderu lokalnego (np. C:\Azure rozwiązania Cosmos DBTemplates). Ten szablon wdroży konto bazy danych Azure rozwiązania Cosmos aplikacji sieci web usługi aplikacji i spowoduje jej modyfikacji ustawień aplikacji witryny można łatwo ujawnienie informacji połączenia bazy danych rozwiązania Cosmos Azure, ale nie zawiera aplikacji sieci web.  

<a id="Build"></a>

## <a name="step-2-deploy-the-azure-cosmos-db-account-app-service-web-app-and-demo-application-sample"></a>Krok 2: Wdrażanie konto bazy danych rozwiązania Cosmos Azure, aplikacji usługi sieci web aplikacji i pokaz aplikacji przykładowych
Teraz wdróżmy naszych pierwszego szablonu.

> [!TIP]
> Szablon nie sprawdzić, czy nazwa aplikacji sieci web i nazwę konta bazy danych rozwiązania Cosmos Azure wprowadzono poniżej są) prawidłowe i (b) dostępne.  Zdecydowanie zaleca się, aby zweryfikować dostępności nazwy, które zamierzasz dostarczyć przed przesłaniem wdrożenia.
> 
> 

1. Zaloguj się do [Azure Portal](https://portal.azure.com), kliknij przycisk Nowy i wyszukaj "Wdrażanie szablonu".
    ![Zrzut ekranu przedstawiający wdrażanie szablonu interfejsu użytkownika](./media/create-website/TemplateDeployment1.png)
2. Wybierz element wdrożenia szablonu, a następnie kliknij przycisk **Utwórz** ![zrzut ekranu przedstawiający wdrażanie szablonu interfejsu użytkownika](./media/create-website/TemplateDeployment2.png)
3. Kliknij przycisk **Edytuj szablon**, a następnie wklej zawartość pliku szablonu DocDBWebsiteTodo.json i kliknij przycisk **zapisać**.
   ![Zrzut ekranu przedstawiający wdrażanie szablonu interfejsu użytkownika](./media/create-website/TemplateDeployment3.png)
4. Kliknij przycisk **Edytuj parametry**, podaj wartości dla poszczególnych obowiązkowe parametry i kliknij przycisk **OK**.  Dostępne są następujące parametry:
   
   1. Nazwa witryny: Określa nazwę aplikacji sieci web usługi aplikacji i jest używany do tworzenia adres URL, który umożliwia dostęp do aplikacji sieci web (np. Jeśli określisz "mydemodocdbwebapp", a następnie adres URL, za pomocą której będą uzyskiwać dostęp do aplikacji sieci web będzie mydemodocdbwebapp.azurewebsites.net).
   2. HOSTINGPLANNAME: Określa nazwę hostingu planu usługi aplikacji, aby utworzyć.
   3. Lokalizacja: Określa lokalizacji platformy Azure, w której chcesz utworzyć zasobów aplikacji sieci web i bazy danych Azure rozwiązania Cosmos.
   4. DATABASEACCOUNTNAME: Określa nazwę konta Azure DB rozwiązania Cosmos do utworzenia.   
      
      ![Zrzut ekranu przedstawiający wdrażanie szablonu interfejsu użytkownika](./media/create-website/TemplateDeployment4.png)
5. Wybierz istniejącą grupę zasobów lub podaj nazwę, aby utworzyć nową grupę zasobów, a następnie wybierz lokalizację dla grupy zasobów.

    ![Zrzut ekranu przedstawiający wdrażanie szablonu interfejsu użytkownika](./media/create-website/TemplateDeployment5.png)
6. Kliknij przycisk **Przejrzyj postanowienia prawne**, **zakupu**, a następnie kliknij przycisk **Utwórz** aby rozpocząć wdrażanie.  Wybierz **Przypnij do pulpitu nawigacyjnego** , wynikowy wdrożenie jest widoczny na stronie głównej portalu Azure.
   ![Zrzut ekranu przedstawiający wdrażanie szablonu interfejsu użytkownika](./media/create-website/TemplateDeployment6.png)
7. Po zakończeniu wdrożenia spowoduje otwarcie bloku grupy zasobów.
   ![Zrzut ekranu przedstawiający blok grupa zasobów](./media/create-website/TemplateDeployment7.png)  
8. Korzystanie z aplikacji, po prostu przejdź do adresu URL aplikacji sieci web (w powyższym przykładzie adres URL jest http://mydemodocdbwebapp.azurewebsites.net).  Zostanie wyświetlony w następującej aplikacji sieci web:
   
   ![Przykładowa aplikacja Todo](./media/create-website/image2.png)
9. Przejdź dalej i Utwórz kilka zadań w aplikacji sieci web, a następnie wróć do bloku grupy zasobów w portalu Azure. Kliknij zasób konta bazy danych Azure rozwiązania Cosmos na liście zasobów, a następnie kliknij przycisk **Eksploratora zapytań**.
    ![Zrzut ekranu przedstawiający podsumowania obiektywu z aplikacją sieci web wyróżnione](./media/create-website/TemplateDeployment8.png)  
10. Uruchom zapytanie domyślne "Wybierz * c" i przejrzyj wyniki.  Zwróć uwagę, że zapytanie ma pobrać reprezentacja JSON zadań do wykonania, utworzony w kroku 7 powyżej.  Możesz eksperymentować zapytania; na przykład, spróbuj uruchomić SELECT * z c WHERE c.isComplete = true, aby zwrócić wszystkie elementy todo, które zostały oznaczone jako zakończone.
    
    ![Zrzut ekranu przedstawiający Eksploratora zapytań i wyniki bloków, wyświetlanie wyników zapytania](./media/create-website/image5.png)
11. Możesz także Eksploruj obsługi portalu Azure DB rozwiązania Cosmos lub zmodyfikować przykładowej aplikacji Todo.  Jeśli wszystko jest gotowe, wdróżmy innego szablonu.

<a id="Build"></a> 

## <a name="step-3-deploy-the-document-account-and-web-app-sample"></a>Krok 3: Wdrażanie przykładowej aplikacji sieci web i konta dokumentu
Teraz wdróżmy naszych drugiego formularza.  Ten szablon jest przydaje się do wyświetlania, jak możesz można wstrzyknąć bazy danych Azure rozwiązania Cosmos połączenia informacje takie jak punkt końcowy konta i klucz główny do aplikacji sieci web jako ustawień aplikacji lub niestandardowe parametry połączenia. Na przykład być może masz własnych aplikacji sieci web, który chcesz wdrożyć przy użyciu konta bazy danych Azure rozwiązania Cosmos i ma informacje o połączeniu automatycznie wypełniane podczas wdrażania.

> [!TIP]
> Szablon nie sprawdzić, czy nazwa aplikacji sieci web i nazwę konta bazy danych rozwiązania Cosmos Azure wprowadzono poniżej są) prawidłowe i (b) dostępne.  Zdecydowanie zaleca się, aby zweryfikować dostępności nazwy, które zamierzasz dostarczyć przed przesłaniem wdrożenia.
> 
> 

1. W [Azure Portal](https://portal.azure.com), kliknij przycisk Nowy i wyszukaj "Wdrażanie szablonu".
    ![Zrzut ekranu przedstawiający wdrażanie szablonu interfejsu użytkownika](./media/create-website/TemplateDeployment1.png)
2. Wybierz element wdrożenia szablonu, a następnie kliknij przycisk **Utwórz** ![zrzut ekranu przedstawiający wdrażanie szablonu interfejsu użytkownika](./media/create-website/TemplateDeployment2.png)
3. Kliknij przycisk **Edytuj szablon**, a następnie wklej zawartość pliku szablonu DocDBWebSite.json i kliknij przycisk **zapisać**.
   ![Zrzut ekranu przedstawiający wdrażanie szablonu interfejsu użytkownika](./media/create-website/TemplateDeployment3.png)
4. Kliknij przycisk **Edytuj parametry**, podaj wartości dla poszczególnych obowiązkowe parametry i kliknij przycisk **OK**.  Dostępne są następujące parametry:
   
   1. Nazwa witryny: Określa nazwę aplikacji sieci web usługi aplikacji i jest używany do tworzenia adres URL, który umożliwia dostęp do aplikacji sieci web (np. Jeśli określisz "mydemodocdbwebapp", a następnie adres URL, za pomocą której będą uzyskiwać dostęp do aplikacji sieci web będzie mydemodocdbwebapp.azurewebsites.net).
   2. HOSTINGPLANNAME: Określa nazwę hostingu planu usługi aplikacji, aby utworzyć.
   3. Lokalizacja: Określa lokalizacji platformy Azure, w której chcesz utworzyć zasobów aplikacji sieci web i bazy danych Azure rozwiązania Cosmos.
   4. DATABASEACCOUNTNAME: Określa nazwę konta Azure DB rozwiązania Cosmos do utworzenia.   
      
      ![Zrzut ekranu przedstawiający wdrażanie szablonu interfejsu użytkownika](./media/create-website/TemplateDeployment4.png)
5. Wybierz istniejącą grupę zasobów lub podaj nazwę, aby utworzyć nową grupę zasobów, a następnie wybierz lokalizację dla grupy zasobów.

    ![Zrzut ekranu przedstawiający wdrażanie szablonu interfejsu użytkownika](./media/create-website/TemplateDeployment5.png)
6. Kliknij przycisk **Przejrzyj postanowienia prawne**, **zakupu**, a następnie kliknij przycisk **Utwórz** aby rozpocząć wdrażanie.  Wybierz **Przypnij do pulpitu nawigacyjnego** , wynikowy wdrożenie jest widoczny na stronie głównej portalu Azure.
   ![Zrzut ekranu przedstawiający wdrażanie szablonu interfejsu użytkownika](./media/create-website/TemplateDeployment6.png)
7. Po zakończeniu wdrożenia spowoduje otwarcie bloku grupy zasobów.
   ![Zrzut ekranu przedstawiający blok grupa zasobów](./media/create-website/TemplateDeployment7.png)  
8. Kliknij zasób aplikacji sieci Web na liście zasobów, a następnie kliknij przycisk **ustawienia aplikacji** ![zrzut ekranu przedstawiający grupy zasobów](./media/create-website/TemplateDeployment9.png)  
9. Należy zwrócić uwagę, jak istnieją ustawienia aplikacji dla punktu końcowego bazy danych Azure rozwiązania Cosmos i każdy klucz główny bazy danych Azure rozwiązania Cosmos.

    ![Zrzut ekranu przedstawiający ustawień aplikacji](./media/create-website/TemplateDeployment10.png)  
10. Możesz także kontynuować eksploracji portalu Azure lub użyj jednego z naszych Azure DB rozwiązania Cosmos [przykłady](http://go.microsoft.com/fwlink/?LinkID=402386) do tworzenia aplikacji bazy danych Azure rozwiązania Cosmos.

<a name="NextSteps"></a>

## <a name="next-steps"></a>Następne kroki
Gratulacje! Wdrożeniu Azure rozwiązania Cosmos bazy danych, aplikacji sieci web usługi aplikacji i przykładowej aplikacji sieci web przy użyciu szablonów usługi Azure Resource Manager.

* Aby dowiedzieć się więcej o usłudze Azure DB rozwiązania Cosmos, kliknij przycisk [tutaj](http://azure.com/docdb).
* Aby dowiedzieć się więcej o aplikacjach sieci Web usługi aplikacji Azure, kliknij przycisk [tutaj](http://go.microsoft.com/fwlink/?LinkId=325362).
* Aby dowiedzieć się więcej na temat szablonów usługi Azure Resource Manager, kliknij przycisk [tutaj](https://msdn.microsoft.com/library/azure/dn790549.aspx).

## <a name="whats-changed"></a>Co zostało zmienione
* Przewodnik dotyczący przejścia od usługi Witryny sieci Web do usługi App Service można znaleźć w temacie [Azure App Service and Its Impact on Existing Azure Services](http://go.microsoft.com/fwlink/?LinkId=529714) (Usługa Azure App Service i jej wpływ na istniejące usługi platformy Azure).

> [!NOTE]
> Jeśli chcesz zacząć korzystać z usługi Azure App Service przed utworzeniem konta platformy Azure, przejdź do artykułu [Try App Service](http://go.microsoft.com/fwlink/?LinkId=523751) (Wypróbuj usługę App Service), w którym wyjaśniono, jak od razu utworzyć początkową aplikację sieci Web o krótkim okresie istnienia w usłudze App Service. Bez kart kredytowych i bez zobowiązań.
> 
> 

