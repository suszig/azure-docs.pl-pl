---
title: "Konfigurowanie niestandardowej nazwy domeny usług w chmurze | Dokumentacja firmy Microsoft"
description: "Dowiedz się, jak udostępnić aplikacji Azure lub dane na domenę niestandardową Konfigurowanie ustawień DNS."
services: cloud-services
documentationcenter: .net
author: Thraka
manager: timlt
editor: 
ms.assetid: 6a62c2b7-ea47-4cce-9d6a-0cca38357f42
ms.service: cloud-services
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/05/2017
ms.author: adegeo
ms.openlocfilehash: b61f6aad7cf974ce25baf944e342284b02ea0048
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/11/2017
---
# <a name="configuring-a-custom-domain-name-for-an-azure-cloud-service"></a>Konfigurowanie niestandardowej nazwy domeny dla usługi w chmurze Azure
> [!div class="op_single_selector"]
> * [Witryna Azure Portal](cloud-services-custom-domain-name-portal.md)
> * [Klasyczna witryna Azure Portal](cloud-services-custom-domain-name.md)
> 
> 

Podczas tworzenia usługi w chmurze Azure przypisuje go do poddomeny cloudapp.net. Na przykład jeśli usługi w chmurze ma nazwę "contoso", użytkownikom będzie można uzyskać dostępu do aplikacji przy użyciu adresu URL, takie jak http://contoso.cloudapp.net. Azure przypisuje wirtualny adres IP.

Można jednak również ujawniać aplikacji na własną nazwę domeny, np. contoso.com. W tym artykule wyjaśniono, jak zarezerwować lub skonfigurowania niestandardowej nazwy domeny dla ról sieci web usługi w chmurze.

Czy można już zrozumieć, jakie CNAME i są? [Skok poza wyjaśnienie](#add-a-cname-record-for-your-custom-domain).

> [!NOTE]
> Pobrać, przechodząc szybciej! Użyj platformy Azure [z przewodnikiem wskazówki](http://support.microsoft.com/kb/2990804). Powoduje skojarzenie niestandardowej nazwy domeny i zabezpieczania komunikacji (SSL) z usług Azure Cloud Services lub witryny sieci Web Azure bardzo proste.
> 
> 

<p/>

> [!NOTE]
> Procedury w tym zadaniu mają zastosowanie do usługi w chmurze Azure. Dla usług aplikacji, zobacz [to](../app-service/app-service-web-tutorial-custom-domain.md). W przypadku kont magazynu, zobacz [to](../storage/blobs/storage-custom-domain-name.md).
> 
> 

## <a name="understand-cname-and-a-records"></a>Zrozumienie rekordów CNAME i A
CNAME (lub rekordy aliasu) i rejestruje zarówno pozwalają na nazwę domeny można skojarzyć z określonym serwerem (lub usługi, w tym przypadku) jednak one działać inaczej. Istnieją także niektóre zagadnień, używając rekordy z usługami w chmurze Azure, które należy wziąć pod uwagę przed podjęciem decyzji, który ma zostać użyty.

### <a name="cname-or-alias-record"></a>Rekord CNAME lub Alias
Rekord CNAME mapuje *określonych* domeny, takie jak **contoso.com** lub **www.contoso.com**, nazwę kanoniczną domeny. W takim przypadku nazwa kanoniczna domeny jest **.cloudapp [moja_aplikacja] .net** nazwy domeny platformy Azure hostowanej aplikacji. Po utworzeniu nazwy CNAME tworzy alias dla **.cloudapp [moja_aplikacja] .net**. Wpis CNAME rozwiąże adres IP Twojego **.cloudapp [moja_aplikacja] .net** usługi automatycznie, więc jeśli zmieni adres IP usługi w chmurze, nie trzeba podejmować żadnych działań.

> [!NOTE]
> Niektóre rejestratorów domeny umożliwiają tylko mapy poddomen przy użyciu rekordu CNAME, np. www.contoso.com i nie nazwy głównych, np. contoso.com. Aby uzyskać więcej informacji na rekordy CNAME, zobacz w dokumentacji dostarczonej przez rejestratora, [wpis Wikipedia rekordu CNAME](http://en.wikipedia.org/wiki/CNAME_record), lub [nazwy domen IETF - wdrażania i specyfikację](http://tools.ietf.org/html/rfc1035) dokumentu.
> 
> 

### <a name="a-record"></a>Rekord
Rekord A mapuje domeny, takie jak **contoso.com** lub **www.contoso.com**, *lub domena symboli wieloznacznych* takich jak  **\*. contoso.com**, adres IP. W przypadku usługi w chmurze Azure, wirtualnego adresu IP usługi. Dlatego Największą zaletą rekord A za pośrednictwem rekordu CNAME mają jeden wpis, który używa symboli wieloznacznych, takich jak \* **. contoso.com**, który będzie obsługiwać żądania wielu domen podrzędnych takich jak  **mail.contoso.com**, **login.contoso.com**, lub **www.contso.com**.

> [!NOTE]
> Ponieważ rekord jest zamapowana na statycznego adresu IP, nie może automatycznie rozwiązać zmian adres IP usługi w chmurze. Adres IP używany przez usługi w chmurze jest przydzielony przy pierwszym uruchomieniu wdrożenia do pustego gniazda (produkcyjnego lub przemieszczania.) Po usunięciu wdrożenia gniazda adres IP został wydany przez Azure i wszystkich przyszłych wdrożeń do gniazda, należy podać nowy adres IP.
> 
> Wygodnie adres IP miejsca danego wdrożenia (środowisko produkcyjne lub tymczasowe) jest trwały podczas wymiany między przemieszczania i wdrożeń produkcyjnych lub uaktualnianie w miejscu istniejącego wdrożenia. Aby uzyskać więcej informacji dotyczących wykonywania tych akcji, zobacz [sposobu zarządzania usługami w chmurze](cloud-services-how-to-manage.md).
> 
> 

## <a name="add-a-cname-record-for-your-custom-domain"></a>Dodaj rekord CNAME dla domeny niestandardowej
Aby utworzyć rekord CNAME, możesz należy dodać nowy wpis w tabeli DNS dla domeny niestandardowej przy użyciu narzędzi dostarczonych przez rejestratora. Każdy rejestrator ma podobne, lecz nieco inne metody określania rekord CNAME, ale pojęcia są takie same.

1. Użyj jednej z tych metod, aby znaleźć **. cloudapp.net** nazwy domeny przypisany do usługi w chmurze.
   
   * Zaloguj się do [klasycznego portalu Azure], wybierz usługi w chmurze, wybierz **pulpitu nawigacyjnego**, a następnie znajdź **adres URL witryny** wpis w **szybkiego dostępu**sekcji.
     
       ![sekcja szybkiego dostępu przedstawiający adres URL witryny][csurl]
     
       **LUB**  
   * Instalowanie i konfigurowanie [programu Azure Powershell](/powershell/azure/overview), a następnie użyj następującego polecenia:
     
       ```powershell
       Get-AzureDeployment -ServiceName yourservicename | Select Url
       ```
     
     Zapisz nazwę domeny używaną w adresie URL zwrócone przez każdej z metod, ponieważ będzie potrzebny podczas tworzenia rekordu CNAME.
2. Zaloguj się w witrynie rejestratora DNS i przejdź do strony zarządzania DNS. Wyszukaj łącza lub obszarów witryny oznaczone jako **nazwy domeny**, **DNS**, lub **nazwy serwera zarządzania**.
3. Teraz można znaleźć, gdzie wybierz lub wprowadź CNAME w. Może być konieczne wybierz typ rekordu z listy rozwijanej lub przejdź do strony Ustawienia zaawansowane. Należy zwrócić uwagę słowa **CNAME**, **Alias**, lub **poddomen**.
4. Należy również podać domeny lub poddomeny aliasu CNAME, takich jak **www** Jeśli chcesz utworzyć alias **www.customdomain.com**. Jeśli chcesz utworzyć alias dla domeny głównej, może być wymieniona jako "**@**" symbol w narzędziach DNS rejestratora.
5. Następnie należy podać nazwę kanoniczną hosta, która jest aplikacji **cloudapp.net** domeny w tym przypadku.

Na przykład następujący rekord CNAME przekazuje cały ruch z **www.contoso.com** do **contoso.cloudapp.net**, nazwę domeny niestandardowej wdrożonej aplikacji:

| Nazwa aliasu/hosta/domeny podrzędnej | Canonical domeny |
| --- | --- |
| www |contoso.cloudapp.NET |

Obiekt odwiedzający **www.contoso.com** nie będzie widoczna wartość true, hosta (contoso.cloudapp.net), więc procesu przekazywania jest niewidoczny dla użytkownika końcowego.

> [!NOTE]
> W powyższym przykładzie dotyczy tylko ruchu na **www** poddomeny. Ponieważ rekordy CNAME nie można używać symboli wieloznacznych, należy utworzyć jeden CNAME dla każdej domeny/poddomeny. Aby skierować ruch z domen podrzędnych, takich jak \*. contoso.com, adres cloudapp.net możesz skonfigurować **adres URL przekierowania** lub **adres URL do przodu** wpis w ustawieniach DNS lub Utwórz rekord A.
> 
> 

## <a name="add-an-a-record-for-your-custom-domain"></a>Dodawanie rekordu A dla domeny niestandardowej
Aby utworzyć rekord A, możesz znaleźć wirtualnego adresu IP usługi w chmurze. Następnie dodaj nowy wpis w tabeli DNS dla domeny niestandardowej przy użyciu narzędzi dostarczonych przez rejestratora. Każdy rejestrator ma podobne, lecz nieco inne metody określania rekord A, ale pojęcia są takie same.

1. Użyj jednej z następujących metod, aby uzyskać adres IP usługi w chmurze.
   
   * Zaloguj się do [klasycznego portalu Azure], wybierz usługi w chmurze, wybierz **pulpitu nawigacyjnego**, a następnie znajdź **adres publiczny wirtualnego adresu IP (VIP)** wpis w  **szybkiego dostępu** sekcji.
     
       ![Wyświetlanie adres VIP sekcji szybkiego dostępu][vip]
     
       **LUB**  
   * Instalowanie i konfigurowanie [programu Azure Powershell](/powershell/azure/overview), a następnie użyj następującego polecenia:
     
       ```powershell
       get-azurevm -servicename yourservicename | get-azureendpoint -VM {$_.VM} | select Vip
       ```
     
     Jeśli masz wiele punktów końcowych skojarzonych z usługi w chmurze, otrzymasz wiele wierszy zawierających adres IP, ale wszystkie ten sam adres powinien być wyświetlany.
     
     Zapisz adres IP, ponieważ będzie potrzebny podczas tworzenia rekordu A.
2. Zaloguj się w witrynie rejestratora DNS i przejdź do strony zarządzania DNS. Wyszukaj łącza lub obszarów witryny oznaczone jako **nazwy domeny**, **DNS**, lub **nazwy serwera zarządzania**.
3. Teraz można znaleźć, gdzie wybierz lub wprowadź rekordu. Może być konieczne wybierz typ rekordu z listy rozwijanej lub przejdź do strony Ustawienia zaawansowane.
4. Wybierz lub wprowadź domenę lub poddomen, który będzie używany przez ten rekord. Na przykład wybierz **www** Jeśli chcesz utworzyć alias **www.customdomain.com**. Jeśli chcesz utworzyć wpis symboli wieloznacznych dla wszystkich domen podrzędnych, wprowadź "__*__". Wszystkich domen podrzędnych będzie on zawierał takie jak **mail.customdomain.com**, **login.customdomain.com**, i **www.customdomain.com**.
   
    Jeśli chcesz utworzyć rekordu A dla domeny głównej, może być wymieniona jako "**@**" symbol w narzędziach DNS rejestratora.
5. Wprowadź adres IP usługi w chmurze w polu podana. Skojarzenie wpis domeny używane w rekordzie przy użyciu adresu IP wdrożenia usługi chmury.

Na przykład następujące rekord przekazuje cały ruch z **contoso.com** do **137.135.70.239**, adres IP wdrożonej aplikacji:

| Nazwa hosta/domeny podrzędnej | Adres IP |
| --- | --- |
| @ |137.135.70.239 |

W tym przykładzie pokazano tworzenie rekordu A dla domeny głównej. Jeśli chcesz utworzyć wpis symboli wieloznacznych, aby pokrywał wszystkich domen podrzędnych, należy wprowadzić "__*__" jako poddomeny.

> [!WARNING]
> Adresy IP na platformie Azure są dynamiczne domyślnie. Prawdopodobnie warto użyć [zastrzeżony adres IP](../virtual-network/virtual-networks-reserved-public-ip.md) aby upewnić się, czy adres IP nie ulega zmianie.
> 
> 

## <a name="next-steps"></a>Następne kroki
* [Jak zarządzać usługi w chmurze](cloud-services-how-to-manage.md)
* [Jak zamapować zawartość usługi CDN na domenę niestandardową](../cdn/cdn-map-content-to-custom-domain.md)
* [Konfiguracja ogólna usługi w chmurze](cloud-services-how-to-configure.md).
* Dowiedz się, jak [wdrażania usługi w chmurze](cloud-services-how-to-create-deploy.md).
* Skonfiguruj [certyfikaty ssl](cloud-services-configure-ssl-certificate.md).

[Expose Your Application on a Custom Domain]: #access-app
[Add a CNAME Record for Your Custom Domain]: #add-cname
[Expose Your Data on a Custom Domain]: #access-data
[VIP swaps]: http://msdn.microsoft.com/library/ee517253.aspx
[Create a CNAME record that associates the subdomain with the storage account]: #create-cname
[klasycznego portalu Azure]: https://manage.windowsazure.com
[Validate Custom Domain dialog box]: http://i.msdn.microsoft.com/dynimg/IC544437.jpg
[vip]: ./media/cloud-services-custom-domain-name/csvip.png
[csurl]: ./media/cloud-services-custom-domain-name/csurl.png
