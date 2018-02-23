---
title: "Azure AD Connect, kondycję i rozporządzenia ochrony danych ogólne | Dokumentacja firmy Microsoft"
description: "Ten dokument zawiera opis sposobu uzyskiwania GDPR zgodności z programem Azure AD Connect."
services: active-directory
documentationcenter: 
author: billmath
manager: mtillman
editor: 
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/18/2018
ms.author: billmath
ms.openlocfilehash: b9a0b9027bbead00300040186e453933b3a7f46b
ms.sourcegitcommit: d87b039e13a5f8df1ee9d82a727e6bc04715c341
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 02/21/2018
---
# <a name="gdpr-compliance-and-azure-ad-connect-health"></a>GDPR zgodności i Azure AD Connect Health 

[Rozporządzenia ogólne ochrony danych (GDPR)](http://ec.europa.eu/justice/data-protection/reform/index_en.htm) jest prawo ochrona i prywatność danych Unii Europejskiej (UE). GDPR nakłada nowe zasady dotyczące firmy, agencji rządowych, z systemem innym niż zysków i innych organizacji, które oferują towarów i usług do osób w UE, lub że zbieranie i analizowanie danych powiązane mieszkańców Unii Europejskiej. 

Usług i produktów firmy Microsoft dostępnych dzisiaj ułatwić spełniają wymagania GDPR. Dowiedz się więcej o zasady Privacy firmy Microsoft pod adresem [Centrum zaufania](https://www.microsoft.com/trustcenter).

Azure AD Connect Health monitoruje usługi infrastruktury i synchronizacji tożsamości lokalnych. Udostępnia również należy insights i powierzchni alerty. Firma Microsoft dba o zgodności GDPR przez usługi w chmurze podczas wymuszania rozpoczęcia 2018 maja oraz w celu zapewnienia związanych z GDPR gwarancji, w umownym zobowiązań. 

>[!NOTE] 
> W tym artykule opisano zgodność GDPR w Azure AD Connect Health. Aby uzyskać informacje na temat zgodności GDPR w programie Azure AD Connect, zobacz [GDPR zgodności i Azure AD Connect](../../active-directory/connect/active-directory-aadconnect-gdpr.md).

## <a name="gdpr-classification"></a>GDPR klasyfikacji
Azure AD Connect Health należące do **procesora danych** Kategoria klasyfikacji GDPR. Jako potok procesor danych z usługi udostępnia usługi przetwarzania danych i klucza użytkowników końcowych. Azure AD Connect Health nie generuje danych użytkownika i nie ma niezależnego kontroli nad jakie dane osobiste są zbierane i sposobie ich użycia. Pobieranie danych, agregacji, analizy i raportowania w programie Azure AD Connect Health są oparte na istniejących danych lokalnych. 

## <a name="data-retention-policy"></a>Zasady przechowywania danych
Azure AD Connect Health nie generować raporty, wykonywać analizy lub wgląd ponad 30 dni. W związku z tym Azure AD Connect Health nie przechowywania, przetwarzania lub zachować wszystkie dane poza 30 dni. Ten projekt jest zgodne z przepisami GDPR przepisy dotyczące zgodności ochrony prywatności firmy Microsoft, a zasady przechowywania danych usługi Azure AD. 
 
## <a name="disable-data-collection-and-monitoring-in-azure-ad-connect-health"></a>Wyłączanie zbierania danych i monitoring w programie Azure AD Connect Health
Azure AD Connect Health umożliwia zatrzymać zbieranie danych dla poszczególnych serwerów monitorowanych lub wystąpienie monitorowanej usługi. Na przykład można zatrzymać zbieranie danych dla poszczególnych serwerów usług AD FS (Active Directory Federation Services), które są monitorowane przy użyciu usługi Azure AD Connect Health. Można również zatrzymać zbieranie danych dla całego wystąpienia usług AD FS, która jest monitorowana przy użyciu usługi Azure AD Connect Health. Gdy wybierzesz to zrobić, odpowiadające serwery są usuwane z portalu Azure AD Connect Health po zatrzymaniu zbierania danych. 

>[!IMPORTANT]
> Musisz uprawnienia Administrator globalny usługi Azure AD lub roli współautora w RBAC delete monitorowane serwery z usługi Azure AD Connect Health.
>
> Usuwanie serwera lub wystąpienie usługi z usługi Azure AD Connect Health nie jest nieodwracalne. 

### <a name="what-to-expect"></a>Czego można oczekiwać?
Po zatrzymaniu zbierania danych i monitorowania dla monitorowanych pojedynczy serwer lub wystąpienie monitorowanej usługi, należy uwzględnić następujące informacje:

- Po usunięciu wystąpienia monitorowanej usługi, wystąpienie jest usuwane z usługi Azure AD Connect Health monitorowania listy usług w portalu. 
- Po usunięciu monitorowany serwer lub wystąpienie monitorowanej usługi agenta programu Health nie zostaje odinstalowany lub usunąć z serwerów. Agent kondycji został skonfigurowany z wysyłania danych do usługi Azure AD Connect Health. Należy ręcznie odinstalować agenta programu Health na wcześniej monitorowanych serwerach.
- Jeśli nie odinstalowano agenta programu Health przed wykonaniem tej czynności, na serwerach powiązanych z agentem programu Health mogą pojawić się zdarzenia błędów.
- Wszystkie dane należącego do wystąpienia monitorowanej usługi zostaną usunięte zgodnie z zasadami przechowywania danych programu Microsoft Azure.

### <a name="disable-data-collection-and-monitoring-for-a-monitored-server"></a>Wyłączanie zbierania danych i monitorowania na monitorowanym serwerze
Zobacz [jak usuwanie serwera z usługi Azure AD Connect Health](active-directory-aadconnect-health-operations.md#to-delete-a-server-from-the-azure-ad-connect-health-service).

### <a name="disable-data-collection-and-monitoring-for-an-instance-of-a-monitored-service"></a>Wyłączanie zbierania danych i monitorowania dla wystąpienia usługi monitorowane
Zobacz [jak usunąć wystąpienie usługi z usługi Azure AD Connect Health](active-directory-aadconnect-health-operations.md#delete-a-service-instance-from-azure-ad-connect-health-service).


## <a name="re-enable-data-collection-and-monitoring-in-azure-ad-connect-health"></a>Ponowne włączenie funkcji zbierania danych i monitoring w programie Azure AD Connect Health
Aby ponownie włączyć monitoring w programie Azure AD Connect Health do celów wcześniej usuniętych monitorowanej usługi, należy odinstalować i [ponownie zainstalować agenta programu health](active-directory-aadconnect-health-agent-install.md) na wszystkich serwerach.


## <a name="next-steps"></a>Kolejne kroki
* [Przejrzyj zasady Privacy firmy Microsoft w Centrum zaufania](https://www.microsoft.com/trustcenter)
* [Azure AD Connect i GDPR](../../active-directory/connect/active-directory-aadconnect-gdpr.md)
* [Operacje w programie Azure AD Connect Health](active-directory-aadconnect-health-operations.md)
