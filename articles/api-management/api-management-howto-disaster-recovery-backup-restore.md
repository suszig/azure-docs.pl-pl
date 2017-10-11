---
title: "Za pomocą odzyskiwania po awarii wdrożenie i przywracania kopii zapasowych w usłudze Azure API Management | Dokumentacja firmy Microsoft"
description: "Dowiedz się, jak używać kopii zapasowej i przywracania do odzyskiwania po awarii w usłudze Azure API Management."
services: api-management
documentationcenter: 
author: steved0x
manager: erikre
editor: 
ms.assetid: 6f10be3c-f796-4a6c-bacd-7931b6aa82af
ms.service: api-management
ms.workload: mobile
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/23/2017
ms.author: apimpm
ms.openlocfilehash: 07c0265490cfae733133b6e0c938f90f9b392da4
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 08/18/2017
---
# <a name="how-to-implement-disaster-recovery-using-service-backup-and-restore-in-azure-api-management"></a>Jak zaimplementować odzyskiwania po awarii przy użyciu usługi Kopia zapasowa i przywracanie w usłudze Azure API Management
Przez wybranie publikowanie i zarządzanie nimi swoje interfejsy API za pomocą usługi Azure API Management są korzystanie z wielu odporność na uszkodzenia i możliwości infrastruktury, które w przeciwnym razie byłyby projektowania, wdrożenia i zarządzania nimi. Platforma Azure zmniejsza duża część potencjalnych błędów w ułamku kosztów.

Pozwoli na odzyskanie dostępności problemów wpływających na region, w którym zarządzania infrastrukturą interfejsu API usługi jest obsługiwana, możesz przystąpić do odtworzenia z usługą w innym regionie, w dowolnym momencie. W zależności od dostępności cele i celu czasu odzyskiwania można zarezerwować usługi tworzenia kopii zapasowej w jednym lub więcej regionów, a następnie spróbuj Obsługa swojej konfiguracji i zawartości w synchronizacji z usługą active. Usługa tworzenia kopii zapasowej i funkcja przywracania zapewnia niezbędne bloku konstrukcyjnego Implementowanie strategię odzyskiwania po awarii.

W tym przewodniku przedstawiono sposób uwierzytelniania usługi Azure Resource Manager żądań i wykonywania kopii zapasowej i przywracania swoich wystąpień usługi Zarządzanie interfejsami API.

> [!NOTE]
> Proces tworzenia kopii zapasowych i przywracanie wystąpienia usługi Zarządzanie interfejsami API dla odzyskiwania po awarii można również do replikacji wystąpień usługi Zarządzanie interfejsami API dla scenariuszy, takich jak przemieszczania.
>
> Należy pamiętać, że każda kopia zapasowa wygaśnie po upływie 30 dni. Próba przywrócenia kopii zapasowej, po wygaśnięciu 30-dniowy okres ważności przywracania kończy się niepowodzeniem z `Cannot restore: backup expired` wiadomości.
>
>

## <a name="authenticating-azure-resource-manager-requests"></a>Żądań uwierzytelniania usługi Azure Resource Manager
> [!IMPORTANT]
> Interfejs API REST dla kopii zapasowej i przywracania używa usługi Azure Resource Manager i ma mechanizm uwierzytelniania inny niż interfejsów API REST zarządzania jednostek zarządzanie interfejsami API. W tej sekcji opisano sposób do uwierzytelniania żądań usługi Azure Resource Manager. Aby uzyskać więcej informacji, zobacz [żądań uwierzytelniania usługi Azure Resource Manager](http://msdn.microsoft.com/library/azure/dn790557.aspx).
>
>

Wszystkie zadania, które można wykonywać na zasobów przy użyciu usługi Azure Resource Manager musi zostać uwierzytelniony w usłudze Azure Active Directory, wykonując następujące kroki.

* Dodawanie aplikacji do dzierżawy usługi Azure Active Directory.
* Ustawianie uprawnień dla aplikacji, który został dodany.
* Pobierz token uwierzytelniania żądań do usługi Azure Resource Manager.

Pierwszym krokiem jest, aby utworzyć aplikację usługi Azure Active Directory. Zaloguj się do [klasycznego portalu Azure](http://manage.windowsazure.com/) korzystanie z subskrypcji, który zawiera usługi Zarządzanie interfejsami API wystąpienia, a następnie przejdź do **aplikacji** kartę domyślnej usługi Azure Active Directory.

> [!NOTE]
> Jeśli domyślny katalog Azure Active Directory nie jest widoczna na koncie, skontaktuj się z administratorem subskrypcji platformy Azure można udzielić wymaganych uprawnień do konta.

![Utwórz aplikację usługi Azure Active Directory][api-management-add-aad-application]

Kliknij przycisk **Dodaj**, **Dodaj aplikację moją organizację**i wybierz polecenie **aplikację Native client**. Wprowadź opisową nazwę, a następnie kliknij strzałkę dalej. Wprowadź adres URL symbolu zastępczego, takich jak `http://resources` dla **identyfikator URI przekierowania**, ponieważ jest to wymagane pole, ale nie jest używana wartość później. Kliknij pole wyboru, aby zapisać aplikację.

Po zapisaniu aplikacji, kliknij przycisk **Konfiguruj**, przewiń w dół do **uprawnień dotyczących innych aplikacji** sekcji, a następnie kliknij polecenie **Dodaj aplikację**.

![Dodaj uprawnienia][api-management-aad-permissions-add]

Wybierz **Windows** **interfejs API zarządzania usługami Azure** i kliknij przycisk wyboru, aby dodać aplikację.

![Dodaj uprawnienia][api-management-aad-permissions]

Kliknij przycisk **delegowane uprawnienia** obok nowo dodanego **Windows** **interfejs API zarządzania usługami Azure** aplikacji, pole wyboru dla **dostępu do usługi Azure Service Management (wersja zapoznawcza)**i kliknij przycisk **zapisać**.

![Dodaj uprawnienia][api-management-aad-delegated-permissions]

Przed wywołaniem interfejsów API, które Generowanie kopii zapasowej i przywrócenie go, należy uzyskać token. W poniższym przykładzie użyto [Microsoft.IdentityModel.Clients.ActiveDirectory](https://www.nuget.org/packages/Microsoft.IdentityModel.Clients.ActiveDirectory) pakiet nuget w celu pobrania tokenu.

```c#
using Microsoft.IdentityModel.Clients.ActiveDirectory;
using System;

namespace GetTokenResourceManagerRequests
{
    class Program
    {
        static void Main(string[] args)
        {
            var authenticationContext = new AuthenticationContext("https://login.microsoftonline.com/{tenant id}");
            var result = authenticationContext.AcquireToken("https://management.azure.com/", {application id}, new Uri({redirect uri});

            if (result == null) {
                throw new InvalidOperationException("Failed to obtain the JWT token");
            }

            Console.WriteLine(result.AccessToken);

            Console.ReadLine();
        }
    }
}
```

Zastąp `{tentand id}`, `{application id}`, i `{redirect uri}` z poniższymi instrukcjami.

Zastąp `{tenant id}` o identyfikatorze dzierżawy utworzoną aplikację usługi Azure Active Directory. Identyfikator można uzyskać, klikając **wyświetlić punkty końcowe**.

![Punkty końcowe][api-management-aad-default-directory]

![Punkty końcowe][api-management-endpoint]

Zastąp `{application id}` i `{redirect uri}` przy użyciu **identyfikator klienta** i adres URL z **identyfikator URI przekierowania** sekcji z poziomu aplikacji usługi Azure Active Directory **Konfiguruj** kartę.

![Zasoby][api-management-aad-resources]

Po wartości są określone, przykładowy kod powinien zwrócić token podobny do poniższego przykładu.

![Token][api-management-arm-token]

Przed wywołaniem metody tworzenia kopii zapasowej i przywracania czynności opisane w poniższych sekcjach, ustaw nagłówek żądania autoryzacji dla wywołania REST.

```c#
request.Headers.Add(HttpRequestHeader.Authorization, "Bearer " + token);
```

## <a name="step1"></a>Utwórz kopię zapasową usługi Zarządzanie interfejsami API
Aby utworzyć kopię zapasową problem dotyczący usługi Zarządzanie interfejsami API następujące żądania HTTP:

`POST https://management.azure.com/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.ApiManagement/service/{serviceName}/backup?api-version={api-version}`

Gdzie:

* `subscriptionId`— Identyfikator subskrypcji zawierający usługi Zarządzanie interfejsami API próbujesz do kopii zapasowej
* `resourceGroupName`-ciąg w formie "Api - Domyślnie — {usługi region}" gdzie `service-region` identyfikuje region platformy Azure, gdzie zarządzanie interfejsami API usługi próbujesz kopii zapasowej jest obsługiwany, np.`North-Central-US`
* `serviceName`-Nazwa usługi Zarządzanie interfejsami API tworzysz kopię zapasową określony w momencie jego tworzenia.
* `api-version`-Zamień`2014-02-14`

W treści żądania Określ nazwę konta magazynu Azure docelowej, klucz dostępu, nazwa kontenera obiektów blob i nazwa kopii zapasowej:

```
'{  
    storageAccount : {storage account name for the backup},  
    accessKey : {access key for the account},  
    containerName : {backup container name},  
    backupName : {backup blob name}  
}'
```

Ustaw wartość `Content-Type` nagłówek żądania do `application/json`.

Kopia zapasowa jest operacją wymagającą dużo czasu, który może potrwać kilka minut.  Jeśli żądanie zostało wykonane i zainicjowano proces tworzenia kopii zapasowej otrzymasz `202 Accepted` kod stanu odpowiedzi z `Location` nagłówka.  Upewnij się, "GET" żądania na adres URL w `Location` nagłówka, aby sprawdzić stan operacji. W trakcie tworzenia kopii zapasowej możesz nadal będziesz otrzymywać kodu stanu "202 zaakceptowane". Kod odpowiedzi `200 OK` wskaże, że ukończenie operacji tworzenia kopii zapasowej.

Należy pamiętać następujące ograniczenia, podczas tworzenia kopii zapasowej żądania.

* **Kontener** określony w treści żądania **musi istnieć**.
* Gdy trwa wykonywanie kopii zapasowej możesz **nie powinny podejmować żadnych operacji zarządzania usługi** takich jak jednostki SKU uaktualnienia lub starszą wersję, zmiana nazwy domeny, itp.
* Przywracanie **kopii zapasowej jest gwarantowana tylko przez 30 dni** od momentu jego utworzenia.
* **Dane użycia** używany do tworzenia raportów analizy **nie jest dołączana** w kopii zapasowej. Użyj [interfejsu API REST Azure API Management] [ Azure API Management REST API] okresowo pobrać raportów analizy w bezpiecznym miejscu.
* Częstotliwość, z którym wykonywanie kopii zapasowych usługi będzie miało wpływ na celu punktu odzyskiwania. Aby zminimalizować zaleca się implementowania regularnych kopii zapasowych, a także wykonywanie kopii zapasowych na żądanie po wprowadzeniu istotnych zmian do usługi API Management.
* **Zmiany** wprowadzone w konfiguracji usługi (np. interfejsów API, zasady, wygląd portalu deweloperów) podczas tworzenia kopii zapasowej operacja jest w toku **mogą nie zostać uwzględnione w kopii zapasowej i w związku z tym zostaną utracone**.

## <a name="step2"></a>Przywrócenia usługi Zarządzanie interfejsami API
Aby przywrócić zarządzanie interfejsami API usługi z kopii zapasowej utworzonej wcześniej upewnij następujące żądania HTTP:

`POST https://management.azure.com/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.ApiManagement/service/{serviceName}/restore?api-version={api-version}`

Gdzie:

* `subscriptionId`— Identyfikator subskrypcji zawierający usługi Zarządzanie interfejsami API są przywracanie kopii zapasowej do
* `resourceGroupName`-ciąg w formie "Api - Domyślnie — {usługi region}" gdzie `service-region` identyfikuje region platformy Azure, gdzie jest hostowana usługa API Management przywracania kopii zapasowej do, np.`North-Central-US`
* `serviceName`-Nazwa zarządzanie interfejsami API usługi są przywracane do określonych w momencie jego tworzenia
* `api-version`-Zamień`2014-02-14`

W treści żądania Określ lokalizację pliku kopii zapasowej, tj. Nazwa konta magazynu platformy Azure, klucz dostępu, nazwa kontenera obiektów blob i nazwa kopii zapasowej:

```
'{  
    storageAccount : {storage account name for the backup},  
    accessKey : {access key for the account},  
    containerName : {backup container name},  
    backupName : {backup blob name}  
}'
```

Ustaw wartość `Content-Type` nagłówek żądania do `application/json`.

Przywracanie jest operacją wymagającą dużo czasu, który może potrwać do co najmniej 30 minut, aby zakończyć.  Jeśli żądanie zostało wykonane i zainicjowano proces przywracania otrzymasz `202 Accepted` kod stanu odpowiedzi z `Location` nagłówka.  Upewnij się, "GET" żądania na adres URL w `Location` nagłówka, aby sprawdzić stan operacji. W trakcie przywracania będzie odbierać "202 zaakceptowane" Kod stanu. Kod odpowiedzi `200 OK` będą wskazywać pomyślne zakończenie operacji przywracania.

> [!IMPORTANT]
> **Jednostka SKU** usługi są przywracane do **musi odpowiadać** jednostki SKU usługi kopii zapasowej przywracanej.
>
> **Zmiany** wprowadzone w konfiguracji usługi (np. interfejsów API, zasady, wygląd portalu deweloperów) podczas przywracania operacja jest w toku **mogą zostać zastąpione**.
>
>

## <a name="next-steps"></a>Następne kroki
Zapoznaj się z następujących blogach firmy Microsoft dla dwóch różnych wskazówki dotyczące procesu tworzenia kopii zapasowej/przywracania.

* [Replikowanie usługi Azure API Management kont](https://www.returngis.net/en/2015/06/replicate-azure-api-management-accounts/)
  * Dziękujemy do Gisela dla swojego udziału w tym artykule.
* [Usługi Azure API Management: Tworzenie kopii zapasowej i przywracanie konfiguracji](http://blogs.msdn.com/b/stuartleeks/archive/2015/04/29/azure-api-management-backing-up-and-restoring-configuration.aspx)
  * Podejście szczegółowych przy Stuart jest niezgodny z oficjalnego wytyczne, ale jest bardzo interesujące.

[Backup an API Management service]: #step1
[Restore an API Management service]: #step2


[Azure API Management REST API]: http://msdn.microsoft.com/library/azure/dn781421.aspx

[api-management-add-aad-application]: ./media/api-management-howto-disaster-recovery-backup-restore/api-management-add-aad-application.png

[api-management-aad-permissions]: ./media/api-management-howto-disaster-recovery-backup-restore/api-management-aad-permissions.png
[api-management-aad-permissions-add]: ./media/api-management-howto-disaster-recovery-backup-restore/api-management-aad-permissions-add.png
[api-management-aad-delegated-permissions]: ./media/api-management-howto-disaster-recovery-backup-restore/api-management-aad-delegated-permissions.png
[api-management-aad-default-directory]: ./media/api-management-howto-disaster-recovery-backup-restore/api-management-aad-default-directory.png
[api-management-aad-resources]: ./media/api-management-howto-disaster-recovery-backup-restore/api-management-aad-resources.png
[api-management-arm-token]: ./media/api-management-howto-disaster-recovery-backup-restore/api-management-arm-token.png
[api-management-endpoint]: ./media/api-management-howto-disaster-recovery-backup-restore/api-management-endpoint.png
