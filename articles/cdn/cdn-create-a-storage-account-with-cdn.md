---
title: "Integracja z usługą Azure CDN konta magazynu platformy Azure | Dokumentacja firmy Microsoft"
description: "Dowiedz się, jak używać usługi Azure Content Delivery Network (CDN) do dostarczania zawartości wysokiej przepustowości przez buforowanie obiektów blob z usługi Magazyn Azure."
services: cdn
documentationcenter: 
author: zhangmanling
manager: erikre
editor: 
ms.assetid: cbc2ff98-916d-4339-8959-622823c5b772
ms.service: cdn
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/23/2017
ms.author: mazha
ms.openlocfilehash: 511076935d06ed0908341044e37069e74530be49
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/11/2017
---
# <a name="integrate-an-azure-storage-account-with-azure-cdn"></a>Integracja z usługą Azure CDN konta magazynu platformy Azure
Z magazynu Azure CDN można włączyć zawartości pamięci podręcznej. Oferuje deweloperom globalne rozwiązanie umożliwiające dostarczanie zawartości wysokiej przepustowości przez buforowanie obiektów blob i zawartości statycznej wystąpień obliczeń w węzłach fizycznych w Stanów Zjednoczonych, Europa, Azja, Australia i Azji.

## <a name="step-1-create-a-storage-account"></a>Krok 1: Utwórz konto magazynu
Poniższa procedura umożliwia utworzenie nowego konta magazynu dla subskrypcji platformy Azure. Konto magazynu zapewnia dostęp do usług magazynu platformy Azure. Konto magazynu reprezentuje najwyższy poziom przestrzeni nazw do uzyskiwania dostępu do poszczególnych składników usługi Azure storage: obiektów Blob usługi, usługi kolejki i usługi tabeli. Aby uzyskać więcej informacji, zapoznaj się [wprowadzenie do usługi Microsoft Azure Storage](../storage/common/storage-introduction.md).

Aby utworzyć konto magazynu, musi być administratorem usługi albo współadministratorem subskrypcji skojarzone.

> [!NOTE]
> Istnieje kilka metod, które służy do tworzenia konta magazynu, w tym portalu Azure i programu Powershell.  W tym samouczku będziemy używać portalu Azure.  
> 
> 

**Aby utworzyć konto magazynu dla subskrypcji platformy Azure**

1. Zaloguj się do [Portalu Azure](https://portal.azure.com).
2. W lewym górnym rogu wybierz **nowy**. W **nowy** zaznacz pozycję **dane i magazyn**, następnie kliknij przycisk **konta magazynu**.
    
    **Utworzyć konto magazynu** zostanie wyświetlony blok.   

    ![Tworzenie konta magazynu][create-new-storage-account]  

3. W **nazwa** wpisz nazwę domeny podrzędnej. Ten wpis może zawierać 3 do 24 małych liter i cyfr.
   
    Ta wartość używana jako nazwa hosta w identyfikatorze URI, który jest wykorzystywana do adresowania obiektów Blob, kolejki lub tabeli zasobów dla subskrypcji. W celu adresu zasobu kontenera w usłudze obiektów Blob, należy użyć identyfikatora URI w następującym formacie, gdzie  *&lt;StorageAccountLabel&gt;*  odnosi się do wartości wpisanych w **wprowadź adres URL**:
   
    http://*&lt;StorageAcountLabel&gt;*.blob.core.windows.net/*&lt;mojkontener&gt;*
   
    **Ważne:** adresu URL etykiety formularze poddomeną identyfikatora URI konta magazynu i musi być unikatowa wśród wszystkich usług hostowanych na platformie Azure.
   
    Ta wartość jest także używana jako nazwa tego konta magazynu w portalu lub podczas uzyskiwania dostępu do tego konta programowo.
4. Pozostaw wartości domyślne dla **modelu wdrażania**, **konta rodzaju**, **wydajności**, i **replikacji**. 
5. Wybierz **subskrypcji** używanego konta magazynu z.
6. Wybierz lub utwórz **grupę zasobów**.  Więcej informacji na temat grup zasobów znajduje się w temacie [Omówienie usługi Azure Resource Manager](../azure-resource-manager/resource-group-overview.md#resource-groups).
7. Wybierz lokalizację dla konta magazynu.
8. Kliknij przycisk **Utwórz**. Proces tworzenia konta magazynu może potrwać kilka minut.

## <a name="step-2-enable-cdn-for-the-storage-account"></a>Krok 2: Włączanie CDN dla konta magazynu

Dzięki integracji najnowsze teraz umożliwia CDN dla konta magazynu bez opuszczania rozszerzenia portalu magazynu. 

1. Wybierz konto magazynu, wyszukaj "CDN" i przewiń w dół w menu nawigacji po lewej stronie, a następnie kliknij przycisk "Azure CDN".
    
    **Azure CDN** zostanie wyświetlony blok.

    ![CDN Włącz nawigacji][cdn-enable-navigation]
    
2. Utwórz nowy punkt końcowy, wprowadzając wymagane informacje
    - **Profil CDN**: można utworzyć nowy lub użyć istniejącego profilu.
    - **Warstwa cenowa**: należy wybrać warstwę cenową, w przypadku utworzenia nowego profilu CDN.
    - **Nazwa punktu końcowego CDN**: Wprowadź nazwę punktu końcowego na wybór.

    > [!TIP]
    > Utworzony punkt końcowy CDN domyślnie używa nazwy hosta konta magazynu jako źródła.

    ! [tworzenie punktu końcowego cdn nowego] [cdn — nowy — utworzenie punktu końcowego]

3. Po utworzeniu nowy punkt końcowy będą widoczne w powyższej listy punktów końcowych.

    ![nowy punkt końcowy sieci CDN w warstwie magazynu][cdn-storage-new-endpoint]

> [!NOTE]
> Można także przejść do rozszerzenia usługi Azure CDN, aby włączyć sieć CDN. [Samouczek](#Tutorial-cdn-create-profile).
> 
> 

[!INCLUDE [cdn-create-profile](../../includes/cdn-create-profile.md)]  

## <a name="step-3-enable-additional-cdn-features"></a>Krok 3: Włącz dodatkowe funkcje CDN

W bloku "Azure CDN" konto magazynu kliknij punkt końcowy CDN z listy, aby otworzyć blok konfiguracji sieci CDN. Można włączyć dodatkowe funkcje sieci CDN w celu dostarczania, na przykład kompresji, ciąg zapytania geograficznie filtrowania. Można również dodać mapowania niestandardową domenę do punktu końcowego CDN i włączyć domeny niestandardowej HTTPS.
    
![Konfiguracja sieci CDN w warstwie magazynu CDN][cdn-storage-cdn-configuration]

## <a name="step-4-access-cdn-content"></a>Krok 4: Dostęp do zawartości w sieci CDN
Aby uzyskać dostęp do zawartości w pamięci podręcznej w sieci CDN, użyj adresu URL CDN, w portalu. Adres pamięci podręcznej blob będzie podobny do następującego:

http://<*EndpointName*\>.azureedge.net/ <*myPublicContainer*\>/<*element BlobName*\>

> [!NOTE]
> Po włączeniu sieci CDN w warstwie dostępu do konta magazynu, wszystkie obiekty publicznie dostępnych kwalifikują się do buforowania krawędzi CDN. Jeśli zmodyfikujesz obiekt, który jest aktualnie w pamięci podręcznej w sieci CDN nową zawartość nie będzie dostępne za pośrednictwem sieci CDN dopóki CDN odświeża jego zawartości, gdy pamięci podręcznej zawartości okres czasu wygaśnięcia.
> 
> 

## <a name="step-5-remove-content-from-the-cdn"></a>Krok 5: Usuń zawartość z sieci CDN
Jeśli nie chcesz już w pamięci podręcznej obiektu w usłudze Azure sieci dostarczania zawartości (CDN), można wykonać jedną z następujących czynności:

* Można utworzyć kontenera prywatnego zamiast publicznego. Aby uzyskać więcej informacji, zobacz [Manage anonymous read access to containers and blobs](../storage/blobs/storage-manage-access-to-resources.md) (Zarządzanie dostępem anonimowym do kontenerów i obiektów blob).
* Można wyłączyć lub usunąć punkt końcowy CDN za pomocą portalu zarządzania.
* Można zmodyfikować usługi hostowanej nie będzie odpowiadać na żądania dla obiekt.

Obiekt w sieci CDN już pamięci podręcznej pozostaje w pamięci podręcznej aż do zakończenia okresu czasu wygaśnięcia dla obiekt lub punkt końcowy jest wyczyszczone. Po wygaśnięciu okresu time-to-live CDN sprawdzi, czy punkt końcowy CDN jest nadal ważny i nadal anonimowo dostępny obiekt. Jeśli nie, obiekt już być buforowane.

## <a name="additional-resources"></a>Dodatkowe zasoby
* [Jak zamapować zawartość usługi CDN na domenę niestandardową](cdn-map-content-to-custom-domain.md)
* [Włącz protokół HTTPS dla domeny niestandardowej](cdn-custom-ssl.md)

[create-new-storage-account]: ./media/cdn-create-a-storage-account-with-cdn/CDN_CreateNewStorageAcct.png
[cdn-enable-navigation]: ./media/cdn-create-a-storage-account-with-cdn/cdn-storage-new-endpoint-creation.png
[cdn-storage-new-endpoint]: ./media/cdn-create-a-storage-account-with-cdn/cdn-storage-new-endpoint-list.png
[cdn-storage-cdn-configuration]: ./media/cdn-create-a-storage-account-with-cdn/cdn-storage-endpoint-configuration.png 
