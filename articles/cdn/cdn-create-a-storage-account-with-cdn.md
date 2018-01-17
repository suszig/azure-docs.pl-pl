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
ms.date: 01/04/2018
ms.author: mazha
ms.openlocfilehash: 022071f7825cb9184bd4c815c09e1c202a0a6f91
ms.sourcegitcommit: 384d2ec82214e8af0fc4891f9f840fb7cf89ef59
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 01/16/2018
---
# <a name="integrate-an-azure-storage-account-with-azure-cdn"></a>Integracja z usługą Azure CDN konta magazynu platformy Azure
Azure Content Delivery Network (CDN) można włączyć do pamięci podręcznej zawartości z usługi Azure storage. Usługi Azure CDN oferuje deweloperom globalne rozwiązanie umożliwiające dostarczanie zawartości wysokiej przepustowości. Można go pamięci podręcznej obiekty BLOB i zawartości statycznej wystąpień obliczeń w węzłach fizycznych Stanów Zjednoczonych, Europa, Azji, Australia i Azji.

## <a name="step-1-create-a-storage-account"></a>Krok 1: Utwórz konto magazynu
Poniższa procedura umożliwia utworzenie nowego konta magazynu dla subskrypcji platformy Azure. Konto magazynu zapewnia dostęp do usług magazynu Azure. Konto magazynu reprezentuje najwyższy poziom przestrzeni nazw do uzyskiwania dostępu do poszczególnych składników usługi Azure Storage: magazynu obiektów Blob platformy Azure, kolejki i tabeli. Aby uzyskać więcej informacji, zobacz [wprowadzenie do usługi Microsoft Azure Storage](../storage/common/storage-introduction.md).

Aby utworzyć konto magazynu, musi być administratorem usługi albo coadministrator subskrypcji skojarzonych.

> [!NOTE]
> Można utworzyć konta magazynu, w tym portalu Azure i programu PowerShell, można użyć kilku metod. Ten samouczek pokazuje, jak korzystać z portalu Azure.   
> 

**Aby utworzyć konto magazynu dla subskrypcji platformy Azure**

1. Zaloguj się w witrynie [Azure Portal](https://portal.azure.com).
2. W lewym górnym rogu, wybierz **Utwórz zasób**. W **nowy** okienku wybierz **magazynu**, a następnie wybierz **konta magazynu — obiekt blob, plików, tabeli, kolejki**.
    
    **Utworzyć konto magazynu** pojawi się okienko.   

    ![Utwórz okienko konta magazynu](./media/cdn-create-a-storage-account-with-cdn/cdn-create-new-storage-account.png)

3. W **nazwa** wprowadź nazwę domeny podrzędnej. Ten wpis może zawierać 3 do 24 małych liter i cyfr.
   
    Ta wartość używana jako nazwa hosta w identyfikatorze URI, który jest wykorzystywana do adresowania obiektów blob, kolejki lub tabeli zasobów dla subskrypcji. Aby rozwiązać zasobu kontenera w magazynie obiektów Blob, użyj identyfikatora URI w następującym formacie:
   
    http://*&lt;StorageAcountLabel&gt;*.blob.core.windows.net/*&lt;mycontainer&gt;*

    gdzie  *&lt;StorageAccountLabel&gt;*  odwołuje się do wartości wprowadzonej w **nazwa** pole.
   
    > [!IMPORTANT]    
    > Etykieta adresu URL tworzy poddomenę tego konta magazynu identyfikatora URI i musi być unikatowa wśród wszystkich usług hostowanych na platformie Azure.
   
    Ta wartość jest także używana jako nazwa konta magazynu w portalu lub gdy uzyskujesz dostęp do tego konta programowo.
    
4. Użyj wartości domyślnych dla **modelu wdrażania**, **konta rodzaju**, **wydajności**, i **replikacji**. 
    
5. Aby uzyskać **subskrypcji**, wybierz subskrypcję do użycia z kontem magazynu.
    
6. Aby uzyskać **grupy zasobów**wybierz lub Utwórz grupę zasobów. Aby uzyskać informacje na temat grup zasobów, zobacz [Omówienie usługi Azure Resource Manager](../azure-resource-manager/resource-group-overview.md#resource-groups).
    
7. Aby uzyskać **lokalizacji**, wybierz lokalizację dla konta magazynu.
    
8. Wybierz pozycję **Utwórz**. Proces tworzenia konta magazynu może potrwać kilka minut.

## <a name="step-2-enable-cdn-for-the-storage-account"></a>Krok 2: Włączanie CDN dla konta magazynu

CDN dla konta magazynu można włączyć bezpośrednio z konta magazynu. 

1. Wybierz konto magazynu z pulpitu nawigacyjnego, a następnie wybierz **Azure CDN** w lewym okienku. Jeśli **Azure CDN** przycisk nie jest od razu widoczne, możesz wprowadzić sieci CDN w **wyszukiwania** pole w lewym okienku.
    
    **Azure Content Delivery Network** pojawi się okienko.

    ![Utwórz punkt końcowy CDN](./media/cdn-create-a-storage-account-with-cdn/cdn-storage-new-endpoint-creation.png)
    
2. Utwórz nowy punkt końcowy, wprowadzając wymagane informacje:
    - **Profil CDN**: Utwórz nowy profil CDN lub użyj istniejącego profilu CDN.
    - **Warstwa cenowa**: Wybierz warstwy cenowej tylko wtedy, gdy tworzysz profil CDN.
    - **Nazwa punktu końcowego CDN**: Wprowadź nazwę punktu końcowego CDN.

    > [!TIP]
    > Domyślnie nowy punkt końcowy CDN używa nazwy hosta konta magazynu co serwer pochodzenia.

3. Wybierz pozycję **Utwórz**. Po utworzeniu punktu końcowego, pojawi się na liście punktu końcowego.

    ![Magazyn nowy punkt końcowy CDN](./media/cdn-create-a-storage-account-with-cdn/cdn-storage-new-endpoint-list.png)

> [!NOTE]
> Jeśli chcesz określić zaawansowane ustawienia konfiguracji dla punktu końcowego CDN, takie jak typ optymalizacji, możesz użyć [rozszerzenia usługi Azure CDN](cdn-create-new-endpoint.md#create-a-new-cdn-endpoint) można utworzyć punktu końcowego usługi CDN lub profilu CDN.

## <a name="step-3-enable-additional-cdn-features"></a>Krok 3: Włącz dodatkowe funkcje CDN

Z konta magazynu **Azure CDN** okienku, wybierz z listy, aby otworzyć okienko konfiguracji CDN punktu końcowego CDN. W celu dostarczania, na przykład kompresji, ciąg zapytania i filtrowanie replikacji geograficznej — warstwa można włączyć dodatkowe funkcje CDN. Można również dodać mapowania niestandardową domenę do punktu końcowego CDN i włączyć domeny niestandardowej HTTPS.
    
![Konfiguracja punktu końcowego CDN magazynu](./media/cdn-create-a-storage-account-with-cdn/cdn-storage-endpoint-configuration.png)

## <a name="step-4-access-cdn-content"></a>Krok 4: Dostęp do zawartości w sieci CDN
Aby uzyskać dostęp do zawartości w pamięci podręcznej w sieci CDN, użyj adresu URL CDN, w portalu. Adres pamięci podręcznej blob ma następujący format:

http://<*EndpointName*\>.azureedge.net/<*myPublicContainer*\>/<*BlobName*\>

> [!NOTE]
> Po włączeniu sieci CDN w warstwie dostępu do konta magazynu, wszystkie obiekty publicznie dostępnych kwalifikują się do buforowania krawędzi CDN. Jeśli zmodyfikujesz obiekt, który jest aktualnie w pamięci podręcznej w sieci CDN nową zawartość nie będzie dostępne za pośrednictwem sieci CDN dopóki CDN odświeża jego zawartości, po wygaśnięciu okresu czasu wygaśnięcia do zawartości w pamięci podręcznej.

## <a name="step-5-remove-content-from-the-cdn"></a>Krok 5: Usuń zawartość z sieci CDN
Jeśli nie chcesz już pamięci podręcznej obiektu w usłudze Azure CDN, można wykonać jedną z następujących czynności:

* Utwórz prywatne kontenera zamiast publicznego. Aby uzyskać więcej informacji, zobacz [Zarządzanie anonimowy dostęp do odczytu do kontenerów i obiektów blob](../storage/blobs/storage-manage-access-to-resources.md).
* Wyłącz lub Usuń punkt końcowy CDN przy użyciu portalu Azure.
* Zmodyfikuj usługi hostowanej nie będzie odpowiadać na żądania dla obiekt.

Obiekt, który jest już w pamięci podręcznej Azure CDN pozostaje pamięci podręcznej aż do zakończenia okresu czasu wygaśnięcia dla obiekt lub punkt końcowy jest wyczyszczone. Po wygaśnięciu okresu time-to-live Azure CDN sprawdza, czy punkt końcowy CDN jest nadal ważny, i obiekt jest nadal anonimowo dostępne. Jeśli nie, obiekt już nie będą buforowane.

## <a name="additional-resources"></a>Zasoby dodatkowe
* [Dodawanie domeny niestandardowej do punktu końcowego usługi CDN](cdn-map-content-to-custom-domain.md)
* [Skonfiguruj protokół HTTPS na domenę niestandardową Azure CDN](cdn-custom-ssl.md)

