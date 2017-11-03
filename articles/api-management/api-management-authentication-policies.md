---
title: "Zasady uwierzytelniania usługi Azure API Management | Dokumentacja firmy Microsoft"
description: "Więcej informacji na temat zasad uwierzytelniania można używać usługi Azure API Management."
services: api-management
documentationcenter: 
author: vladvino
manager: erikre
editor: 
ms.assetid: 061702a7-3a78-472b-a54a-f3b1e332490d
ms.service: api-management
ms.workload: mobile
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/09/2017
ms.author: apimpm
ms.openlocfilehash: 2907c1e4e39f975b4dc4e9382d7258c5d56dbbdc
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/11/2017
---
# <a name="api-management-authentication-policies"></a>Zarządzanie interfejsami API zasady uwierzytelniania
W tym temacie znajdują się informacje na następujące zasady usługi API Management. Aby uzyskać informacje dotyczące dodawania i konfigurowania zasad, zobacz [zasad w usłudze API Management](http://go.microsoft.com/fwlink/?LinkID=398186).  
  
##  <a name="AuthenticationPolicies"></a>Zasady uwierzytelniania  
  
-   [Uwierzytelnianie za pomocą Basic](api-management-authentication-policies.md#Basic) -uwierzytelniania za pomocą usługi wewnętrznej bazy danych przy użyciu uwierzytelniania podstawowego.  
  
-   [Uwierzytelniania za pomocą certyfikatu klienta](api-management-authentication-policies.md#ClientCertificate) -uwierzytelniania za pomocą usługi wewnętrznej bazy danych przy użyciu certyfikatów klienta.  
  
##  <a name="Basic"></a>Uwierzytelniania Basic  
 Użyj `authentication-basic` zasad uwierzytelniania za pomocą usługi wewnętrznej bazy danych przy użyciu uwierzytelniania podstawowego. Ta zasada efektywnie ustawia dla nagłówka HTTP autoryzacji wartość odpowiadającą poświadczenia podane w zasadach.  
  
### <a name="policy-statement"></a>Deklaracja zasad  
  
```xml  
<authentication-basic username="username" password="password" />  
```  
  
### <a name="example"></a>Przykład  
  
```xml  
<authentication-basic username="testuser" password="testpassword" />  
```  
  
### <a name="elements"></a>Elementy  
  
|Nazwa|Opis|Wymagane|  
|----------|-----------------|--------------|  
|Uwierzytelnianie — podstawowy|Element główny.|Tak|  
  
### <a name="attributes"></a>Atrybuty  
  
|Nazwa|Opis|Wymagane|Domyślne|  
|----------|-----------------|--------------|-------------|  
|nazwa użytkownika|Określa nazwę użytkownika, podstawowe poświadczenia.|Tak|Nie dotyczy|  
|hasło|Określa hasło podstawowych poświadczeń.|Tak|Nie dotyczy|  
  
### <a name="usage"></a>Sposób użycia  
 Te zasady służą następujące zasady [sekcje](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#sections) i [zakresy](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#scopes).  
  
-   **Sekcje zasad:** dla ruchu przychodzącego  
  
-   **Zakresy zasad:** interfejsu API  
  
##  <a name="ClientCertificate"></a>Uwierzytelniania za pomocą certyfikatu klienta  
 Użyj `authentication-certificate` zasad uwierzytelniania za pomocą usługi wewnętrznej bazy danych przy użyciu certyfikatu klienta. Ten certyfikat musi być [zainstalowane do interfejsu API zarządzania](http://go.microsoft.com/fwlink/?LinkID=511599) pierwszy i jest identyfikowany przez jego odcisk palca.  
  
### <a name="policy-statement"></a>Deklaracja zasad  
  
```xml  
<authentication-certificate thumbprint="thumbprint" />  
```  
  
### <a name="example"></a>Przykład  
  
```xml  
<authentication-certificate thumbprint="....." />  
```  
  
### <a name="elements"></a>Elementy  
  
|Nazwa|Opis|Wymagane|  
|----------|-----------------|--------------|  
|certyfikat uwierzytelniania|Element główny.|Tak|  
  
### <a name="attributes"></a>Atrybuty  
  
|Nazwa|Opis|Wymagane|Domyślne|  
|----------|-----------------|--------------|-------------|  
|Odcisk palca|Odcisk palca certyfikatu klienta.|Tak|Nie dotyczy|  
  
### <a name="usage"></a>Sposób użycia  
 Te zasady służą następujące zasady [sekcje](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#sections) i [zakresy](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#scopes).  
  
-   **Sekcje zasad:** dla ruchu przychodzącego  
  
-   **Zakresy zasad:** interfejsu API  
  

## <a name="next-steps"></a>Następne kroki
Aby uzyskać więcej informacji, Praca z zasad, zobacz [zasad w usłudze API Management](api-management-howto-policies.md).  