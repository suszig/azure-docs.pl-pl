---
title: "Ochrona danych osobowych podczas przesyłania przy użyciu szyfrowania na platformie Azure | Dokumentacja firmy Microsoft"
description: "informacje dotyczące szyfrowania Azure do ochrony danych osobowych mogą być pomocne w wysiłków w celu zachowania zgodności z ogólnych danych ochrony rozporządzenia (GDPR)."
services: security
documentationcenter: na
author: Barclayn
manager: MBaldwin
editor: TomSh
ms.assetid: 
ms.service: security
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 03/06/2018
ms.author: barclayn
ms.custom: 
ms.openlocfilehash: 6975358d40206a497a53de16731d16ef374db905
ms.sourcegitcommit: 168426c3545eae6287febecc8804b1035171c048
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/08/2018
---
# <a name="azure-encryption-technologies-protect-personal-data-in-transit-with-encryption"></a>Technologii szyfrowania Azure: ochrony danych osobowych podczas przesyłania przy użyciu szyfrowania

Ten artykuł pomoże zrozumieć i użyć technologii szyfrowania Azure do zabezpieczania danych podczas przesyłania. Ochrona prywatności danych osobowych podczas przekazywania przez sieć jest integralną część strategii zabezpieczeń obrony zabezpieczeń wielowarstwowy. Szyfrowanie podczas przesyłania zaprojektowano w celu zapobieżenia osoba atakująca przechwytuje transmisji mogli wyświetlić lub użyć danych. Informacje zawarte w tym artykule mogą być użyteczne podczas próby organizacji są zgodne z ogólnych danych ochrony rozporządzenia (GDPR).

## <a name="scenario"></a>Scenariusz

Firma rejs dużych, siedzibą w Stanach Zjednoczonych rozwija operacjach oferowanie trasy w DS, Adriatyku i Bałtyckiego mórz, jak również brytyjskich. Aby zapewnić obsługę tych działań, ustawił mniejszych rejs wiersze na podstawie we Włoszech, Niemczech, Dania i Zjednoczone Królestwo 

Przez firmę Microsoft Azure do przechowywania danych firmowych w chmurze. Dotyczy to również dane osobowe, takich jak nazwy, adresy, numery telefonów i informacje o karcie kredytowej z jej klientów globalnych. Zawiera także tradycyjnych zasobów ludzkich informacje takie jak adresy, numery telefonów, numery identyfikacyjne podatku i inne informacje dotyczące pracowników firmy we wszystkich lokalizacjach. Wiersz rejs zachowuje również dużej bazy danych elementów członkowskich programu osób trzecich i lojalność zawierający informacje osobiste, aby śledzić relacje z bieżących i starszych klientów.

Dane osobowe klientów została wprowadzona w bazie danych w oddziałach firmy, jak i z agentów podróży na całym świecie. Dokumenty zawierające informacje o kliencie są przesyłane przez sieć do magazynu Azure.

## <a name="problem-statement"></a>Opis problemu

Firmy muszą chronić prywatność danych osobowych pracowników i klientów, jest przesyłane do i z usług Azure.

## <a name="company-goal"></a>Celem firmy

Celem firmy, aby upewnić się, że dane osobowe są szyfrowane, gdy poza dysku. Jeśli osoby nieupoważnione przechwycenia danych osobowych wyłączyć dysk, musi należeć do formularza, który będzie renderowany go nie można go odczytać. Zastosowanie szyfrowania powinna być łatwa lub całkowicie niewidoczne dla użytkowników i administratorów.

## <a name="solutions"></a>Rozwiązania

Usługi Azure zapewniają wiele narzędzia i technologie, aby pomóc w ochronie danych osobowych podczas przesyłania.

### <a name="azure-storage"></a>Azure Storage

Dane przechowywane w chmurze musi przesyłane z klientem, które mogą być fizycznie umieszczony w dowolnym miejscu na świecie, centrum danych Azure. Po pobraniu przez użytkowników dane przesyłane ponownie, w przeciwnym kierunku. Dane są przesyłane za pośrednictwem publicznego Internetu jest zawsze na ryzyko przechwycenia przez osoby atakujące. Należy chronić prywatność danych osobowych za pomocą szyfrowania na poziomie transportu zabezpieczyć przesyłane między lokacjami.

Protokół HTTPS udostępnia kanał komunikacyjny bezpieczne, zaszyfrowane za pośrednictwem Internetu. Dostęp do obiektów w usłudze Azure Storage i podczas wywoływania interfejsów API REST używanego protokołu HTTPS. Wymusić użycie protokołu HTTPS, korzystając z [sygnatury dostępu współdzielonego](https://docs.microsoft.com/azure/storage/storage-dotnet-shared-access-signature-part-1) (SAS), aby delegować dostęp do obiektów usługi Azure Storage. Istnieją dwa typy sygnatury dostępu Współdzielonego: sygnatury dostępu Współdzielonego usługi i konto sygnatury dostępu Współdzielonego.

#### <a name="how-do-i-construct-a-service-sas"></a>Jak utworzyć sygnatury dostępu Współdzielonego usługi?

Sygnatury dostępu Współdzielonego usługi deleguje dostęp do zasobów tylko w jednej z usług magazynu (usługa blob, kolejki, tabeli lub pliku). Do utworzenia sygnatury dostępu Współdzielonego usługi, wykonaj następujące czynności:

1. Określ pola wersji podpisem

2. Określ zasób podpisem (obiektów Blob i tylko usługa plików)

3. Określ parametry zapytania, aby zastąpić nagłówki odpowiedzi (tylko usługi obiektów Blob i plików)

4. Określ nazwę tabeli (tylko usługa tabel)

5. Określ zasady dostępu

6. Określ okres ważności sygnatury

8. Określ uprawnienia

9. Określ adres IP lub zakres adresów IP

10. Określ protokół HTTP

11. Określ zakres dostępu do tabeli

12. Określ identyfikator podpisem

13. Określ podpisu

Aby uzyskać szczegółowe instrukcje, zobacz [konstruowania sygnatury dostępu Współdzielonego usługi](https://docs.microsoft.com/rest/api/storageservices/Constructing-a-Service-SAS?redirectedfrom=MSDN).

#### <a name="how-do-i-construct-an-account-sas"></a>Jak utworzyć sygnatury dostępu Współdzielonego konta?

Sygnatury dostępu Współdzielonego konta deleguje dostęp do zasobów w co najmniej jednej usługi magazynu. Możesz również delegować dostęp do operacji odczytu, zapisu i usuwania dla kontenerów obiektów blob, tabel, kolejek i udziałów plików, co jest niedozwolone w wypadku sygnatury dostępu współdzielonego usługi. Konstrukcja SAS konta jest podobny do sygnatury dostępu Współdzielonego usługi. Aby uzyskać szczegółowe instrukcje, zobacz [konstruowania SAS konta.](https://docs.microsoft.com/rest/api/storageservices/Constructing-an-Account-SAS?redirectedfrom=MSDN)

#### <a name="how-do-i-enforce-https-when-calling-rest-apis"></a>Jak wymusić HTTPS podczas wywoływania interfejsów API REST?

Aby wymusić użycie protokołu HTTPS podczas wywoływania interfejsów API REST do dostępu do obiektów na kontach magazynu, należy włączyć bezpieczny Transfer wymagane dla konta magazynu. 

1. W portalu Azure wybierz **Utwórz konto magazynu**, lub wybierz istniejące konto magazynu, **ustawienia** , a następnie **konfiguracji**.

2. W obszarze **bezpieczny Transfer wymagane**, wybierz pozycję **włączone**.

![Tworzenie konta magazynu](media/protect-personal-data-in-transit-encryption/create-storage-account.png)

Aby uzyskać szczegółowe instrukcje, łącznie ze sposobem programowo włączyć bezpieczny Transfer wymaganego, zobacz [wymaga bezpiecznego transferu](https://docs.microsoft.com/azure/storage/storage-require-secure-transfer).

#### <a name="how-do-i-encrypt-data-in-azure-file-storage"></a>Jak zaszyfrować danych w usłudze magazyn plików Azure?

Szyfrowanie danych przesyłanych z [Azure File Storage](https://docs.microsoft.com/azure/storage/storage-file-how-to-use-files-portal), można użyć protokołu SMB 3.x z systemu Windows 8, 8.1 i 10 i Windows Server 2012 R2 i Windows Server 2016. Usługa pliki Azure jest używana, każde połączenie bez szyfrowania nie powiedzie się po włączeniu "Bezpieczny transfer wymagane". W tym scenariuszy przy użyciu protokołu SMB 2.1, SMB 3.0 bez szyfrowania i niektórych odmian klient protokołu SMB w systemie Linux.

#### <a name="azure-client-side-encryption"></a>Azure Client-Side Encryption

Inną opcją w przypadku ochrony danych osobowych, gdy są przesyłane między aplikacji klienckiej i usługi Azure Storage jest [szyfrowania po stronie klienta](https://docs.microsoft.com/azure/storage/storage-client-side-encryption). Dane są szyfrowane przed przesyłane do usługi Azure Storage, a podczas pobierania danych z usługi Azure Storage, dane zostaną odszyfrowane po odebraniu po stronie klienta.

### <a name="azure-site-to-site-vpn"></a>Azure VPN lokacja lokacja

Efektywny sposób ochrony danych osobowych przesyłane między siecią firmową lub użytkownika i sieć wirtualna platformy Azure jest użycie [lokacja lokacja](https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-howto-site-to-site-resource-manager-portal) lub [punkt lokacja](https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-howto-point-to-site-resource-manager-portal) wirtualnej sieci prywatnej (VPN). Połączenie VPN tworzy bezpieczny tunel zaszyfrowanych przez Internet.

#### <a name="how-do-i-create-a-site-to-site-vpn-connection"></a>Jak utworzyć połączenie VPN lokacja lokacja?

Sieć VPN lokacja lokacja łączy wielu użytkowników w sieci firmowej do platformy Azure. Aby utworzyć połączenie lokacja lokacja, w portalu Azure, wykonaj następujące czynności:

1. Utwórz sieć wirtualną.

2. Określ serwer DNS.

3. Utwórz podsieć bramy.

4. Utwórz bramę sieci VPN. 

    ![](media/protect-personal-data-in-transit-encryption/vpn-step-01.png)

5. Utwórz bramę sieci lokalnej.

    ![](media/protect-personal-data-in-transit-encryption/vpn-step-02.png)

6. Skonfiguruj urządzenie sieci VPN.

7. Utwórz połączenie sieci VPN.

    ![](media/protect-personal-data-in-transit-encryption/vpn-step-03.png)

8. Sprawdź połączenie sieci VPN.

Aby uzyskać bardziej szczegółowe instrukcje dotyczące sposobu tworzenia połączenia lokacja lokacja w portalu Azure Zobacz [Utwórz połączenie lokacja-lokacja w portalu Azure.] (https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-howto-site-to-site-resource-manager-portal)

#### <a name="how-do-i-create-a-point-to-site-vpn-connection"></a>Jak utworzyć połączenie VPN punkt lokacja?

Sieć VPN punkt-lokacja tworzy bezpieczne połączenie z komputera klienckiego do sieci wirtualnej. Jest to przydatne, jeśli chcesz połączyć się z platformy Azure z lokalizacji zdalnej, takich jak z domu lub konferencji w hotelach Centrum. Aby utworzyć połączenie punkt lokacja w portalu Azure

1. Utwórz sieć wirtualną.

2. Dodaj podsieć bramy.

3. Określ serwer DNS. (opcjonalnie)

4. Utwórz bramę sieci wirtualnej.

5. Generowanie certyfikatów.

6. Dodaj pulę adresów klienta.

7. Przekazywanie danych certyfikatu publicznego certyfikatu głównego.

8. Generowanie i zainstaluj pakiet konfiguracji klienta sieci VPN.

9. Zainstaluj certyfikat wyeksportowany klienta.

10. Połączenia z platformą Azure.

11. Weryfikowanie połączenia.

Aby uzyskać szczegółowe instrukcje, zobacz [skonfigurować połączenie punkt-lokacja sieci wirtualnej przy użyciu uwierzytelniania certyfikatów: Azure Portal.](https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-howto-point-to-site-resource-manager-portal)

### <a name="ssltls"></a>SSL/TLS

Firma Microsoft zaleca używanie protokołów SSL/TLS do wymiany danych w różnych lokalizacjach. Organizacje, które chce używać [ExpressRoute](https://docs.microsoft.com/azure/expressroute/) przenoszenia dużych zestawów danych za pośrednictwem dedykowanej sieci WAN o dużej szybkości łącza można również zaszyfrować dane na poziomie aplikacji przy użyciu protokołu SSL/TLS lub innymi protokołami dla dodano ochronę.

### <a name="encryption-by-default"></a>Szyfrowanie domyślnie

Firma Microsoft używa szyfrowania, aby chronić dane przesyłane między klientami i usług w chmurze Azure. Jeśli użytkownik korzysta z usługi Azure Storage za pośrednictwem portalu Azure, wszystkich transakcji jest realizowana za pośrednictwem protokołu HTTPS.

[Transport Layer Security](https://en.wikipedia.org/wiki/Transport_Layer_Security) (TLS) to protokół, który centrach danych firmy Microsoft podejmie próbę negocjowania systemach klientów łączących się z usługami w chmurze firmy Microsoft. TLS zapewnia silne uwierzytelnianie, poufność komunikatów i integralność (umożliwia wykrywanie naruszeniu wiadomości, zatrzymania oraz sfałszowaniem), współdziałanie, elastyczność algorytmów, łatwość wdrażania i używania.

[Udoskonalenia utajnienia](https://en.wikipedia.org/wiki/Forward_secrecy) (przekazywania PFS) jest również zastosować, dzięki czemu każdego połączenia między systemy klienckie klientów i usługi w chmurze firmy Microsoft w klucze unikatowe. Połączenia usług chmurowych firmy Microsoft również korzystać na podstawie RSA 2048 bitowego szyfrowania długości kluczy. Kombinacja protokołu TLS, długości kluczy RSA 2048-bitowych, i PFS utrudnia bardziej ktoś do przechwycenia i dostępu do danych, które są przesyłane między usługami w chmurze firmy Microsoft i klientów.

Przesyłane dane są zawsze szyfrowane w [Data Lake Store] (https://docs.microsoft.com/azure/data-lake-store/data-lake-store-security-overview). Oprócz tego, że dane są szyfrowane przed zapisaniem na nośniku trwałym, są również zawsze zabezpieczane podczas przesyłania przy użyciu protokołu HTTPS. Protokół HTTPS jest jedynym protokołem obsługiwanym przez interfejsy REST usługi Data Lake Store.

## <a name="summary"></a>Podsumowanie

Firmy mogą wykonywać jego celem ochrony danych osobowych i prywatności takich danych, wymuszając połączenia HTTPS z usługi Azure Storage, przy użyciu sygnatury dostępu współdzielonego i umożliwiające bezpieczny Transfer wymagane dla kont magazynu. Mogą one również chronić dane osobiste przy użyciu połączenia protokołu SMB 3.0 i implementowanie szyfrowania po stronie klienta. Połączenia sieci VPN lokacja lokacja z sieci firmowej do sieci wirtualnej platformy Azure i połączeń sieci VPN punkt lokacja z poszczególnych użytkowników będzie tworzenia bezpiecznego tunelu za pośrednictwem której można bezpiecznie przesyłane dane osobowe. Rozwiązania szyfrowania domyślna firmy Microsoft będzie jeszcze lepiej chronić prywatność danych osobowych.

## <a name="next-steps"></a>Kolejne kroki

- [Dobre praktyki dotyczące zabezpieczeń danych platformy Azure i szyfrowania](https://docs.microsoft.com/azure/security/azure-security-data-encryption-best-practices)

- [Planowanie i projektowanie dla usługi VPN Gateway](https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-plan-design)

- [VPN Gateway — często zadawane pytania](https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-vpn-faq)

- [Kupowanie i konfigurowanie certyfikatu SSL usługi aplikacji Azure](https://docs.microsoft.com/azure/app-service/web-sites-purchase-ssl-web-site)
