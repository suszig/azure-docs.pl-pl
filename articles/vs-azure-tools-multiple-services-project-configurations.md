---
title: "Konfigurowanie projektu platformy Azure przy użyciu konfiguracji z wieloma usługi | Dokumentacja firmy Microsoft"
description: "Dowiedz się, jak skonfigurować projekt usługi w chmurze platformy Azure, zmieniając pliki ServiceDefinition.csdef i pliku ServiceConfiguration.cscfg."
services: visual-studio-online
documentationcenter: na
author: kraigb
manager: ghogen
editor: 
ms.assetid: a4fb79ed-384f-4183-9f74-5cac257206b9
ms.service: multiple
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 11/11/2016
ms.author: kraigb
ms.openlocfilehash: deb69101e855bcad56b9212736c52ace72631f0a
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 08/29/2017
---
# <a name="configuring-your-azure-project-using-multiple-service-configurations"></a>Konfigurowanie projektu platformy Azure przy użyciu wielu konfiguracji usługi
Projekt usługi w chmurze Azure zawiera dwa pliki konfiguracji: ServiceDefinition.csdef i pliku ServiceConfiguration.cscfg. Te pliki są spakować z aplikacją usługi chmury Azure i wdrażane na platformie Azure.

* **ServiceDefinition.csdef** zawierający metadane, które jest wymagane przez środowiska platformy Azure na potrzeby aplikacji usługi chmury, łącznie z ról, które zawiera. Ten plik zawiera także ustawienia konfiguracji, które są stosowane do wszystkich wystąpień. Te ustawienia konfiguracji mogą być odczytywane w czasie wykonywania za pomocą usługi hostingu środowisko uruchomieniowe interfejsu API Azure. Nie można zaktualizować tego pliku, gdy usługa jest uruchomiona na platformie Azure.
* **Pliku ServiceConfiguration.cscfg** plik ustawia wartości ustawień konfiguracji określonych w pliku definicji usługi i określa liczbę wystąpień, aby uruchomić dla każdej roli. Ten plik może zostać zaktualizowana uruchomionej usługi w chmurze na platformie Azure.

Azure Tools dla programu Microsoft Visual Studio Podaj strony właściwości, których można użyć do skonfigurowania ustawień konfiguracji przechowywanych w tych plikach. Aby uzyskać dostęp do strony właściwości, kliknij dwukrotnie odwołanie do roli poniżej projekt usługi w chmurze platformy Azure w Eksploratorze rozwiązań, lub kliknij prawym przyciskiem myszy odwołanie do roli i wybierz polecenie **właściwości**, jak pokazano na poniższej ilustracji.

![VS_Solution_Explorer_Roles_Properties](./media/vs-azure-tools-multiple-services-project-configurations/IC784076.png)

Informacje o podstawowej schematów dla definicji usługi i pliki konfiguracji usługi znajdują się w temacie [odwołanie do schematu](https://msdn.microsoft.com/library/azure/dd179398.aspx). Aby uzyskać więcej informacji na temat konfiguracji usługi, zobacz [sposobu skonfigurowania usługi w chmurze](cloud-services/cloud-services-how-to-configure.md).

## <a name="configuring-role-properties"></a>Konfigurowanie właściwości roli
Strony właściwości dla roli sieci web i roli proces roboczy są podobne, chociaż występują niewielkie różnice, wskazano w poniższych sekcjach.

Z **buforowanie** strony, można skonfigurować buforowanie usług platformy Azure.

### <a name="configuration-page"></a>Na stronie konfiguracji
Na **konfiguracji** stronę, można ustawić następujące właściwości:

**Wystąpienia**

Ustaw **wystąpienia** count — właściwość liczby wystąpień usługi powinno być ono uruchomione dla tej roli.

Ustaw **rozmiar maszyny Wirtualnej** właściwości **dodatkowe małych**, **małych**, **średni**, **duży**, lub **bardzo duży**.  Aby uzyskać więcej informacji, zobacz [rozmiary dla usług w chmurze](cloud-services/cloud-services-sizes-specs.md).

**Akcja uruchamiania** (rola sieci Web tylko)

Ustaw tę właściwość, aby określić programu Visual Studio należy uruchomić przeglądarki sieci web dla punktów końcowych HTTP lub punktów końcowych HTTPS lub zarówno po rozpoczęciu debugowania.

Opcja punkt końcowy HTTPS jest dostępna tylko wtedy, gdy punkt końcowy HTTPS został już zdefiniowany dla roli użytkownika. Można zdefiniować punkt końcowy HTTPS **punkty końcowe** strony właściwości.

Jeśli masz już dodany punkt końcowy HTTPS, opcja punkt końcowy HTTPS jest domyślnie włączona i Visual Studio spowoduje uruchomienie przeglądarki dla tego punktu końcowego, po rozpoczęciu debugowania, oprócz przeglądarki dla punktu końcowego HTTP. Przy założeniu, że obie opcje uruchamiania są włączone.

**Diagnostyka**

Domyślnie diagnostyki jest włączona dla roli sieci Web. Konto projektu i magazynu usługi chmury Azure są skonfigurowane do korzystania z emulatora magazynu lokalnego. Gdy wszystko będzie gotowe do wdrożenia na platformie Azure, można wybrać przycisk konstruktora (**...** ) można zaktualizować konta magazynu do użycia usługi Azure storage w chmurze. Można przenieść danych diagnostycznych na konto magazynu na żądanie lub automatycznie zaplanowanymi interwałami. Aby uzyskać więcej informacji na temat diagnostycznych platformy Azure, zobacz [Włączanie diagnostyki w usług Azure Cloud Services i maszyn wirtualnych](cloud-services/cloud-services-dotnet-diagnostics.md).

## <a name="settings-page"></a>Ustawienia strony
Na **ustawienia** strony, można dodać ustawienia konfiguracji dla usługi. Ustawienia konfiguracji są pary nazwa wartość. Kodu działającego w roli można odczytać wartości ustawień konfiguracji w czasie wykonywania za pomocą klasy udostępniane przez [biblioteki zarządzane Azure](http://go.microsoft.com/fwlink?LinkID=171026). W szczególności [GetConfigurationSettingValue](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleenvironment.getconfigurationsettingvalue.aspx) metoda zwraca wartość o nazwie ustawienia w czasie wykonywania.

### <a name="configuring-a-connection-string-to-a-storage-account"></a>Konfigurowanie ciągu połączenia z kontem magazynu
Ciąg połączenia jest ustawienie konfiguracji, które zawiera informacje o połączenia i uwierzytelniania dla emulatora magazynu lub na koncie magazynu platformy Azure. Zawsze, gdy kod muszą uzyskać dostęp do danych usługi Azure storage — oznacza to, obiektów blob, kolejki lub tabeli danych — od kodu działającego w roli, trzeba będzie zdefiniowanie ciągu połączenia dla tego konta magazynu.

Parametry połączenia, które wskazuje konta magazynu platformy Azure musi mieć format zdefiniowany. Aby dowiedzieć się, jak utworzyć parametry połączenia, zobacz [skonfigurować parametry połączenia magazynu Azure](storage/common/storage-configure-connection-string.md).

Gdy wszystko będzie gotowe do testowania usługi w odniesieniu do usług Azure storage lub gdy wszystko będzie gotowe do wdrożenia usługi w chmurze na platformie Azure, można zmienić wartość wszelkie parametry połączenia, aby wskazywały na koncie magazynu platformy Azure. Wybierz (**...** ), wybierz pozycję **wprowadź poświadczenia konta magazynu**. Wprowadź informacje o Twoim koncie, która zawiera nazwę konta i klucz konta. W **parametry połączenia konta magazynu** okno dialogowe, można również wskazać, czy chcesz używać protokołu HTTPS domyślne punkty końcowe (opcja domyślna), domyślne punkty końcowe HTTP lub niestandardowe punkty końcowe. Możesz użyć niestandardowe punkty końcowe, jeśli zarejestrowano niestandardowej nazwy domeny dla usługi, zgodnie z opisem w [Konfigurowanie niestandardowej nazwy domeny dla danych obiektów blob na koncie magazynu Azure](storage/blobs/storage-custom-domain-name.md).

> [!IMPORTANT]
> Należy zmodyfikować parametry połączenia, aby wskazać konto magazynu platformy Azure przed wdrożeniem usługi. Można to zrobić, może spowodować roli użytkownika nie można uruchomić lub mieć inicjowania zajęty i zatrzymywanie.
> 
> 

## <a name="endpoints-page"></a>Strona punkty końcowe
Rola proces roboczy może mieć dowolną liczbę punktów końcowych HTTP, HTTPS lub TCP. Punkty końcowe można wejściowych punktów końcowych, które są dostępne dla klientów zewnętrznych, lub wewnętrznych punktów końcowych, które są dostępne dla innych ról uruchomionych w usłudze.

* Aby udostępnić punkt końcowy HTTP klientami zewnętrznymi i przeglądarki sieci Web, Zmień typ punktu końcowego jako danych wejściowych i określ nazwę i numer portu publicznego.
* Aby udostępnić punkt końcowy HTTPS klientów zewnętrznych i przeglądarki sieci Web, należy zmienić typ punktu końcowego na **wejściowych**, a następnie określ nazwę, numeru portu publicznego i nazwę certyfikatu zarządzania.
  
    Należy pamiętać, że zanim można będzie określić certyfikat zarządzania, należy zdefiniować certyfikat na **certyfikaty** strony właściwości.
* Aby udostępnić punkt końcowy dla wewnętrznego dostępu przez inne role w usłudze w chmurze, Zmień typ punktu końcowego do wewnętrznego i określ nazwę i możliwe prywatnych portów dla tego punktu końcowego.

## <a name="local-storage-page"></a>Strona magazynu lokalnego
Można użyć **Magazyn lokalny** strony właściwości do rezerwowania co najmniej jeden zasób lokalny magazyn dla roli. Zasób Magazyn lokalny jest zastrzeżony katalogu w systemie plików maszyny wirtualnej platformy Azure, w którym jest uruchomione wystąpienie roli.

## <a name="certificates-page"></a>Strona Certyfikaty
Na **certyfikaty** strony, certyfikaty można skojarzyć z roli użytkownika. Certyfikaty, które można dodać może służyć do konfigurowania punktów końcowych HTTPS na **punkty końcowe** strony właściwości.

**Certyfikaty** strony właściwości dodaje informacje o certyfikatach do konfiguracji usługi. Należy pamiętać, że certyfikaty nie są dostarczane z usługą; Certyfikaty musisz je oddzielnie na platformie Azure za pośrednictwem [klasycznego portalu Azure](http://go.microsoft.com/fwlink/?LinkID=213885).

Aby skojarzyć certyfikatu z roli użytkownika, należy podać nazwę certyfikatu. Nazwa ta będzie używana do odwoływania się do certyfikatu podczas konfigurowania punktu końcowego HTTPS **punkty końcowe** strony właściwości. Następnie określ, czy magazyn certyfikatów jest **komputera lokalnego** lub **bieżącego użytkownika** i nazwę magazynu. Na koniec wprowadź odcisk palca certyfikatu. Jeśli certyfikat znajduje się w bieżącym User\Personal magazynu (Mój), możesz wprowadzić odcisk palca certyfikatu, wybierając certyfikat z listy wypełnione. Jeśli znajduje się ona w innej lokalizacji, należy ręcznie wprowadź wartość odcisku palca.

Po dodaniu certyfikatu z magazynu certyfikatów, wszelkie certyfikaty pośrednie są automatycznie dodawane do ustawienia konfiguracji. Te certyfikaty pośrednie, należy również przekazać Azure Aby poprawnie skonfigurować usługi dla protokołu SSL.

Wszystkie certyfikaty zarządzania skojarzonych z usługą dotyczą usługi tylko wtedy, gdy jest uruchomiona w chmurze. Gdy usługa działa w środowisku programistycznym lokalnego, używa to standardowy certyfikat, który jest zarządzany przez emulator obliczeń.

## <a name="configuring-the-azure-cloud-service-project"></a>Konfigurowanie projektu usługi w chmurze Azure
Aby skonfigurować ustawienia stosowane do projektu usługi w chmurze Azure całego, najpierw otwórz menu skrótów dla tego węzła projektu, a następnie wybierz polecenie Właściwości, aby otworzyć stron jej właściwości. W poniższej tabeli przedstawiono te strony właściwości.

| Strony właściwości | Opis |
| --- | --- |
| Aplikacja |Na tej stronie można wyświetlić informacje o wersji narzędzi Azure, używa tego projektu usługi w chmurze, którą można uaktualnić do wersji bieżącej narzędzi. |
| Zdarzenia kompilacji |Na tej stronie można ustawić zdarzenia prekompilacyjnego i mające miejsce po kompilacji. |
| Opracowywanie zawartości |Na tej stronie można określić instrukcje konfiguracji kompilacji i warunków, w których są uruchamiane wszystkie zdarzenia postkompilacyjnego. |
| Sieć Web |Na tej stronie można skonfigurować ustawienia, które odnoszą się do serwera sieci web. |

