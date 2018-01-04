---
title: "Z sygnatury dostępu Współdzielonego przy użyciu usługi Azure CDN | Dokumentacja firmy Microsoft"
description: 
services: cdn
documentationcenter: 
author: dksimpson
manager: 
editor: 
ms.assetid: 
ms.service: cdn
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/14/2017
ms.author: rli v-deasim
ms.openlocfilehash: de30f4319be75362131f8c8ad71aad57b0528f05
ms.sourcegitcommit: 3f33787645e890ff3b73c4b3a28d90d5f814e46c
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 01/03/2018
---
# <a name="using-azure-cdn-with-sas"></a>Z sygnatury dostępu Współdzielonego przy użyciu usługi Azure CDN

Możesz udostępniać zawartość z kontenera magazynu konta magazynu, można zabezpieczyć, jak użytkownicy mają dostęp do plików, przyznając prywatny dostęp do kontenera magazynu. W przeciwnym razie są dostępne przez każdy użytkownik, który zna adres URL kontenera magazynu, dla których udzielono dostępu publicznego. Aby chronić konto magazynu, którym zezwolono sieci dostarczania zawartości (CDN), aby uzyskać dostęp do, funkcja dostępu sygnatury dostępu Współdzielonego z usługi Azure storage można przyznać ograniczony dostęp do magazynu prywatne kontenery.

Sygnatury dostępu Współdzielonego jest identyfikator URI, że przyznaje ograniczone prawa dostępu do zasobów usługi Azure Storage bez narażania klucz konta. Sygnatury dostępu Współdzielonego zapewnia klientom, którzy nie ufasz kluczem konta magazynu, ale do których chcesz delegować dostęp do niektórych zasobów konta magazynu. Rozkładając identyfikator URI sygnatury dostępu współdzielonego dla tych klientów, przyznano im dostęp do zasobu w określonym okresie czasu.
 
Sygnatury dostępu Współdzielonego można zdefiniować różnych parametrów dostępu do obiektu blob, takie jak czas rozpoczęcia i wygaśnięcia, uprawnienia (odczyt/zapis) oraz zakresów adresów IP. W tym artykule opisano sposób użycia SAS w połączeniu z usługą Azure CDN. Aby uzyskać więcej informacji na temat sygnatury dostępu Współdzielonego, łącznie ze sposobem tworzenia go i parametru opcje, zobacz [używanie udostępnionych sygnatur dostępu (SAS)](https://docs.microsoft.com/azure/storage/common/storage-dotnet-shared-access-signature-part-1).

## <a name="setting-up-azure-cdn-to-work-with-storage-sas"></a>Konfigurowanie usługi Azure CDN do pracy z magazynem SAS
Zaleca się trzech opcji korzystania z usługi Azure CDN przy użyciu sygnatury dostępu Współdzielonego. Wszystkie opcje założono, że utworzono już działającego SAS (patrz wymagania wstępne). 
 
### <a name="prerequisites"></a>Wymagania wstępne
Aby rozpocząć, Utwórz konto magazynu, a następnie Generowanie sygnatury dostępu Współdzielonego dla zawartości. Możesz wygenerować dwa rodzaje sygnatur dostępu przechowywanej: sygnatury dostępu Współdzielonego usługi lub kontem sygnatury dostępu Współdzielonego. Aby uzyskać więcej informacji, zobacz [rodzaje sygnatur dostępu współdzielonego](https://docs.microsoft.com/azure/storage/common/storage-dotnet-shared-access-signature-part-1#types-of-shared-access-signatures).

Po wygenerowaniu sygnatury dostępu Współdzielonego, można uzyskać dostępu do pliku magazynu obiektów blob przy użyciu adresu URL, który ma następujący format:`https://<account>.blob.core.windows.net/<folder>/<file>?sv=<SAS_TOKEN>`
 
Na przykład:
 ```
https://democdnstorage1.blob.core.windows.net/container1/sasblob.txt?sv=2017-04-17&ss=b&srt=co&sp=r&se=2038-01-02T21:30:49Z&st=2018-01-02T13:30:49Z&spr=https&sig=QehoetQFWUEd1lhU5iOMGrHBmE727xYAbKJl5ohSiWI%3D
```

Aby uzyskać więcej informacji na temat ustawienie parametrów, zobacz [zagadnienia dotyczące parametru SAS](#sas-parameter-considerations) i [parametrów podpisu dostępu współużytkowanego](https://docs.microsoft.com/azure/storage/common/storage-dotnet-shared-access-signature-part-1#shared-access-signature-parameters).

![Ustawienia sieci CDN SAS](./media/cdn-sas-storage-support/cdn-sas-settings.png)

### <a name="option-1-using-sas-with-pass-through-to-blob-storage-from-the-cdn"></a>Opcja 1: Przy użyciu sygnatury dostępu Współdzielonego z przekazywanego do obiektu blob magazynu z sieci CDN

Ta opcja jest najprostszą i używa tylko jednego tokenu sygnatury dostępu Współdzielonego, który jest przekazywany z sieci CDN do serwera pochodzenia. Nie jest obsługiwany przez **Azure CDN from Verizon**, profilów wersjach Standard i Premium i **Azure CDN from Akamai**. 
 
1. Wybierz punkt końcowy, kliknij przycisk **buforowanie reguły**, a następnie wybierz pozycję **Buforuj każdy unikatowy adres URL** z **buforowania ciągu kwerendy** listy.

    ![Reguły buforowania CDN](./media/cdn-sas-storage-support/cdn-caching-rules.png)

2. Po skonfigurowaniu SAS na koncie magazynu za pomocą tokenu sygnatury dostępu Współdzielonego adres URL sieci CDN w warstwie dostępu do tego pliku. 
   
   Wynikowa adres URL ma następujący format:`https://<endpoint>.azureedge.net/<folder>/<file>?sv=<SAS_TOKEN>`

   Na przykład:   
   ```
   https://demoendpoint.azureedge.net/test/demo.jpg/?sv=2017-04-17&ss=b&srt=c&sp=r&se=2027-12-19T17:35:58Z&st=2017-12-19T09:35:58Z&spr=https&sig=kquaXsAuCLXomN7R00b8CYM13UpDbAHcsRfGOW3Du1M%3D
   ```
   
3. Dostosowywanie czas trwania pamięci podręcznej, za pomocą reguł buforowania lub dodając `Cache-Control` nagłówków w źródle. Ponieważ usługa CDN traktuje tokenu sygnatury dostępu Współdzielonego jako ciąg zapytania zwykły, jako najlepsze rozwiązanie należy skonfigurować czas buforowania, wygaśnięcia czasu wygaśnięcia SAS lub wcześniejsze. W przeciwnym razie, jeśli plik jest buforowana przez dłuższy czas nie jest aktywne skojarzenia zabezpieczeń, to plik może być dostępny z serwera pochodzenia CDN, po upływie czasu wygaśnięcia sygnatury dostępu Współdzielonego. Jeśli dzieje się tak, a chcesz uczynić plików w pamięci podręcznej jest niedostępny, należy wykonać operację przeczyszczania na plik, aby usunąć go z pamięci podręcznej. Informacje o ustawianiu czas buforowania na CDN, zobacz [kontroli Azure Content Delivery Network zachowanie buforowania z buforowaniem reguły](cdn-caching-rules.md).

### <a name="option-2-hidden-cdn-security-token-using-rewrite-rule"></a>Opcja 2: Ukryta tokenu zabezpieczającego CDN przy użyciu reguły ponownego zapisywania
 
Po wybraniu tej opcji można zabezpieczyć magazynu obiektów blob ze źródła — wersja bez konieczności tokenu sygnatury dostępu Współdzielonego dla użytkownika CDN. Możesz użyć tej opcji, jeśli nie ma potrzeby ograniczenia dostępu do określonego pliku, ale chcesz uniemożliwić użytkownikom dostęp do źródła magazynu bezpośrednio, aby skrócić czas odciążania CDN. Ta opcja jest dostępna tylko w przypadku **Azure CDN Premium from Verizon** profilów. 
 
1. Użyj [aparatu reguł](cdn-rules-engine.md) do utworzenia reguły ponowne zapisywanie adresów URL. Nowe zasady potrwać około 90 minut propagacji.

   ![Przycisk Zarządzaj CDN](./media/cdn-sas-storage-support/cdn-manage-btn.png)

   ![Przycisk aparatu reguł CDN](./media/cdn-sas-storage-support/cdn-rules-engine-btn.png)

   Ta przykładowa reguła ponowne zapisywanie adresów URL ma następujący wzorców:
   
   Źródło:   
   `/test/demo.jpg`
   
   Miejsce docelowe:   
   `/test/demo.jpg?sv=2017-04-17&ss=b&srt=c&sp=r&se=2027-12-19T17:35:58Z&st=2017-12-19T09:35:58Z&spr=https&sig=kquaXsAuCLXomN7R00b8CYM13UpDbAHcsRfGOW3Du1M%3D`

   ![Ponowne zapisywanie adresów URL w sieci CDN reguły](./media/cdn-sas-storage-support/cdn-url-rewrite-rule.png)
 
2. Dostęp do plików w sieci CDN bez tokenu sygnatury dostępu Współdzielonego, w następującym formacie:`https://<endpoint>.azureedge.net/<folder>/<file>`
 
   Na przykład:   
   `https://demoendpoint.azureedge.net/test/demo.jpg`
       
   Należy pamiętać, że każdy użytkownik, niezależnie od tego, czy używają tokenu sygnatury dostępu Współdzielonego, mogą uzyskiwać dostęp do punktu końcowego usługi CDN. 

3. Dostosowywanie czas trwania pamięci podręcznej, za pomocą reguł buforowania lub dodając `Cache-Control` nagłówków w źródle. Ponieważ usługa CDN traktuje tokenu sygnatury dostępu Współdzielonego jako ciąg zapytania zwykły, jako najlepsze rozwiązanie należy skonfigurować czas buforowania, wygaśnięcia czasu wygaśnięcia SAS lub wcześniejsze. W przeciwnym razie, jeśli plik jest buforowana przez dłuższy czas nie jest aktywne skojarzenia zabezpieczeń, to plik może być dostępny z serwera pochodzenia CDN, po upływie czasu wygaśnięcia sygnatury dostępu Współdzielonego. Jeśli dzieje się tak, a chcesz uczynić plików w pamięci podręcznej jest niedostępny, należy wykonać operację przeczyszczania na plik, aby usunąć go z pamięci podręcznej. Informacje o ustawianiu czas buforowania na CDN, zobacz [kontroli Azure Content Delivery Network zachowanie buforowania z buforowaniem reguły](cdn-caching-rules.md).

### <a name="option-3-using-cdn-security-token-authentication-with-a-rewrite-rule"></a>Opcja 3: Przy użyciu uwierzytelniania tokenu zabezpieczeń CDN przy użyciu reguły ponownego zapisywania

Ta opcja jest bezpieczne i dostosowania. Aby używać uwierzytelniania tokenu zabezpieczeń CDN, musisz mieć **Azure CDN Premium from Verizon** profilu. Dostęp klienta jest oparty na parametry zabezpieczeń ustawione w sieci CDN tokenu zabezpieczającego. Jednak jeśli sygnatury dostępu Współdzielonego staje się nieprawidłowy, sieć CDN będzie mógł ponownie sprawdź poprawność zawartości z serwera pochodzenia.

1. [Utwórz token zabezpieczenia sieci CDN](https://docs.microsoft.com/azure/cdn/cdn-token-auth#setting-up-token-authentication) i Aktywuj ją przy użyciu aparatu reguł dla punktu końcowego CDN oraz ścieżkę, gdzie użytkownicy mogą uzyskać dostępu do pliku.

   Adres URL SAS ma następujący format:   
   `https://<endpoint>.azureedge.net/<folder>/<file>?sv=<SAS_TOKEN>`
 
   Na przykład:   
   ```
   https://demoendpoint.azureedge.net/test/demo.jpg?sv=2017-04-17&ss=b&srt=c&sp=r&se=2027-12-19T17:35:58Z&st=2017-12-19T09:35:58Z&spr=https&sig=kquaXsAuCLXomN7R00b8CYM13UpDbAHcsRfGOW3Du1M%3D
   ```
       
   Parametr opcje uwierzytelniania tokenu zabezpieczenia sieci CDN w warstwie są inne niż opcje parametru tokenu sygnatury dostępu Współdzielonego. Jeśli chcesz użyć podczas tworzenia tokenu zabezpieczającego CDN czas wygaśnięcia, ustaw ją na taką samą wartość jak czas wygaśnięcia tokenu sygnatury dostępu Współdzielonego. Daje to gwarancję, że czas wygaśnięcia jest atrybutem wartości prognozowanych. 
 
2. Użyj [aparatu reguł](cdn-rules-engine.md) Aby utworzyć regułę ponowne zapisywanie adresów URL, aby włączyć tokenu dostępu do wszystkich obiektów blob w kontenerze. Nowe zasady potrwać około 90 minut propagacji.

   Ta przykładowa reguła ponowne zapisywanie adresów URL ma następujący wzorców:
   
   Źródło:   
   `/test/demo.jpg`
   
   Miejsce docelowe:   
   `/test/demo.jpg?sv=2017-04-17&ss=b&srt=c&sp=r&se=2027-12-19T17:35:58Z&st=2017-12-19T09:35:58Z&spr=https&sig=kquaXsAuCLXomN7R00b8CYM13UpDbAHcsRfGOW3Du1M%3D`

   ![Ponowne zapisywanie adresów URL w sieci CDN reguły](./media/cdn-sas-storage-support/cdn-url-rewrite-rule.png)

3. Po odnowieniu sygnatury dostępu Współdzielonego, aktualizowanie reguły ponowne zapisywanie adresów Url do użycia nowego tokenu sygnatury dostępu Współdzielonego. 

## <a name="sas-parameter-considerations"></a>Zagadnienia dotyczące parametru SAS

Ponieważ SAS parametry nie są widoczne dla CDN, sieć CDN nie można zmienić jego zachowania dotyczącego dostarczania na ich podstawie. Ograniczenia parametrów zdefiniowanych mają zastosowanie tylko w odpowiedzi na żądania, które CDN sprawia, że na serwerze źródłowym, nie dla żądania od klienta do sieci CDN. Ta różnica jest istotnym elementem do rozważenia po ustawieniu parametrów sygnatury dostępu Współdzielonego. Jeśli te zaawansowane możliwości są wymagane i używasz [opcja 3](#option-3-using-cdn-security-token-authentication-with-a-rewrite-rule), ustaw odpowiednie ograniczenia w sieci CDN tokenu zabezpieczającego.

| Nazwa parametru SAS | Opis |
| --- | --- |
| Uruchamianie | Czas, jaki CDN można rozpocząć dostępu do pliku obiektu blob. Z powodu zegara pochylanie (nadejściu sygnał zegara w różnym czasie różnych składników), wybierz czas wcześniej 15 minut, jeśli chcesz zasobów, które mają być dostępne natychmiast. |
| Koniec | Czas, po upływie którego CDN już nie można uzyskać dostępu do pliku obiektu blob. Wcześniej buforowanych plików w sieci CDN są nadal dostępne. Aby kontrolować czas wygaśnięcia pliku, ustawić czas wygaśnięcia odpowiednie dla tokenu zabezpieczającego CDN lub przeczyszczania elementu zawartości. |
| Dozwolonych adresów IP | Opcjonalny. Jeśli używasz **Azure CDN from Verizon**, ustaw ten parametr, do zakresów określonych w [Azure CDN from zakresów adresów IP serwera krawędzi Verizon](https://msdn.microsoft.com/library/mt757330.aspx). Jeśli używasz **Azure CDN from Akamai**, nie można ustawić parametru zakresy IP, ponieważ nie są statyczne adresy IP.|
| Dozwolone protokoły | Następujące protokoły dozwoloną dla żądania dotyczącego przy użyciu sygnatury dostępu Współdzielonego konta. Ustawienie HTTPS jest zalecane.|

## <a name="see-also"></a>Zobacz także
- [Przy użyciu sygnatury dostępu współdzielonego (SAS)](https://docs.microsoft.com/azure/storage/common/storage-dotnet-shared-access-signature-part-1)
- [Udostępnione sygnatur dostępu, część 2: Tworzenie i sygnatury dostępu Współdzielonego za pomocą magazynu obiektów Blob](https://docs.microsoft.com/azure/storage/blobs/storage-dotnet-shared-access-signature-part-2)
- [Zabezpieczanie zasobów Azure Content Delivery Network z tokenu uwierzytelniania](https://docs.microsoft.com/azure/cdn/cdn-token-auth)
