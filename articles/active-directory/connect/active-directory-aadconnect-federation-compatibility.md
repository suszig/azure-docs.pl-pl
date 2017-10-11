---
title: "Lista zgodności usługi Azure AD z usługami federacyjnymi"
description: "Ta strona zawiera dostawców tożsamości innych firm, które mogą służyć do implementowania rejestracji jednokrotnej."
services: active-directory
documentationcenter: 
author: billmath
manager: femila
editor: curtand
ms.assetid: 22c8693e-8915-446d-b383-27e9587988ec
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/13/2017
ms.author: billmath
ms.openlocfilehash: bce5867017647764546d872d97943d5d4f01f2d2
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 08/03/2017
---
# <a name="azure-ad-federation-compatibility-list"></a>Lista zgodności usługi Azure AD z usługami federacyjnymi
Usługa Azure Active Directory zapewnia jednokrotnego na i rozszerzone zabezpieczenia dostępu do aplikacji dla usługi Office 365 i innych usług Online firmy Microsoft dla implementacji tylko w chmurze i hybrydowa bez konieczności dowolnego rozwiązania innej firmy niż Microsoft. Usługi Office 365, takich jak większość usług Online firmy Microsoft, jest zintegrowany z usługą Azure Active Directory usług katalogowych, uwierzytelniania i autoryzacji. Usługa Azure Active Directory udostępnia jednokrotnego do tysięcy aplikacji SaaS i lokalnej aplikacji sieci web. Zobacz galerii aplikacji usługi Azure Active Directory dla obsługiwanych aplikacji SaaS.

W przypadku organizacji, które zainwestowały w rozwiązaniach federacyjnej innych niż Microsoft w tym temacie zawiera wskazówki dotyczące konfigurowania rejestracji jednokrotnej dla użytkowników usługi Active Directory systemu Windows Server z usługami Online firmy Microsoft przy użyciu dostawcy tożsamości innych firm z "Azure Active Directory federation zgodności"poniższej listy. 

![](./media/active-directory-aadconnect-federation-compatibility/oxford2.jpg)   
[Grupa komputerów Oxford](http://oxfordcomputergroup.com/), innych firm, w imieniu firmy Microsoft, przetestowane jednej funkcji logowania jednokrotnego przy użyciu dostawcy tożsamości innych firm z zestawem typowe przypadki użycia z usługą Azure Active Directory.

Informacji na temat jak wprowadzasz dostawcy tożsamości innych firm przedstawione w tym miejscu, skontaktuj się z Oxford grupy komputerów w [ idp@oxfordcomputergroup.com ](mailto:idp@oxfordcomputergroup.com).

> [!IMPORTANT]
> Grupy komputerów Oxford przetestowane tylko funkcje federacyjnego tych pojedynczego scenariuszy logowania jednokrotnego. Grupa komputerów Oxford nie wykonał badań, synchronizacja, uwierzytelnianie dwuskładnikowe, itp. składniki te pojedynczego scenariusze logowania jednokrotnego.
> 
> Korzystanie z logowania za pomocą alternatywnego Identyfikatora do głównej nazwy użytkownika również nie został przetestowany w tym programie.
> 
> 

* [Azure Active Directory](#azure-active-directory)
* [AuthAnvil logowania jednokrotnego 4.5](#authanvil-single-sign-on-45)
* [BIG-IP z wersja Menedżera zasad dostępu BIG-IP 11.3 x — 11, 6 x](#big-ip-with-access-policy-manager-big-ip-ver-113x--116x) 
* [BitGlass](#bitglass)
* [Chmura bezpiecznego urzędu certyfikacji](#ca-secure-cloud) 
* [Urząd certyfikacji SiteMinder 12.52](#ca-siteminder-1252-sp1-cumulative-release-4) 
* [Centrify](#centrify) 
* [Dell jeden v7.1 Menedżer dostępu do tożsamości chmury](#dell-one-identity-cloud-access-manager-v71) 
* [Uwierzytelnianie złożone DigitalPersona](#digitalpersona-composite-authentication)
* [Menedżer tożsamości 6.2.2 federacyjnych IBM Tivoli](#ibm-tivoli-federated-identity-manager-622) 
* [IceWall Federation w wersji 3.0](#icewall-federation-version-30) 
* [Memority](#memority)
* [Menedżer dostępu do NetIQ 4.x](#netiq-access-manager-4x) 
* [Usługi okta](#okta) 
* [OneLogin](#onelogin) 
* [Usługi federacyjne serwera wirtualnego tożsamości optymalne IDM](#optimal-idm-virtual-identity-server-federation-services) 
* [PingFederate 6.11, 7.2, 8.x](#pingfederate-611-72-8x)
* [RadiantOne CFS 3.0](#radiantone-cfs-30) 
* [Sailpoint IdentityNow](#sailpoint-identitynow)
* [SecureAuth IdP 7.2.0](#secureauth-idp-720) 
* [Zaloguj się i przejdź 5.3](#signgo-53) 
* [Brama usługi Online SoftBank technologii](#softbank)
* [Obszar roboczy VMware co](#vmware-workspace-one)



> [!IMPORTANT]
> Ponieważ te produkty innych firm, firma Microsoft nie zapewnia obsługi wdrażania, konfiguracji, rozwiązywania problemów i najlepsze rozwiązania w zakresie, itp. problemów i pytania dotyczące tych dostawców tożsamości. Do obsługi i pytania dotyczące tych dostawców tożsamości skontaktuj się bezpośrednio z obsługiwanych stron trzecich.
> 
> Ci dostawcy tożsamości innych firm zostały przetestowane na współdziałanie z usługami w chmurze firmy Microsoft tylko protokoły WS-Trust i WS-Federation. Testowanie nie obejmują przy użyciu protokołu SAML.
> 


## <a name="azure-active-directory"></a>Usługa Azure Active Directory

Poniżej przedstawiono scenariusz macierz obsługi dla tego działania logowania jednokrotnego: 

| Klient | Pomoc techniczna | Wyjątki |
| --- | --- | --- |
| Oparte na sieci Web klientów, takich jak dostęp w sieci Web programu Exchange i SharePoint Online |Obsługiwane |Brak |
| Rozbudowane aplikacje klienckie, takie jak Lync subskrypcji pakietu Office, CRM |Obsługiwane |Brak |
| Bogate w wiadomości e-mail klientów, takich jak Outlook i programu ActiveSync |Obsługiwane |Brak |
| Nowoczesne aplikacje przy użyciu biblioteki ADAL, takich jak pakiet Office 2016 |Obsługiwane |Brak |

Aby uzyskać więcej informacji o korzystaniu z usługi Azure Active Directory z usługami AD FS, zobacz [Active Directory Federation Services (ADFS)](active-directory-aadconnect-get-started-custom.md#configuring-federation-with-ad-fs).

Aby uzyskać więcej informacji o korzystaniu z usługi Azure Active Directory z synchronizacji haseł zobacz [Azure AD Connect](active-directory-aadconnect.md).

## <a name="authanvil-single-sign-on-45"></a>AuthAnvil logowania jednokrotnego 4.5

Poniżej przedstawiono scenariusz macierzy obsługi tego pojedynczego jednokrotnego:

| Klient | Pomoc techniczna | Wyjątki |
| --- | --- | --- |
| Oparte na sieci Web klientów, takich jak dostęp w sieci Web programu Exchange i SharePoint Online |Obsługiwane |Zintegrowane uwierzytelnianie systemu Windows nie jest obsługiwana. |
| Rozbudowane aplikacje klienckie, takie jak Lync subskrypcji pakietu Office, CRM |Obsługiwane |Zintegrowane uwierzytelnianie systemu Windows nie jest obsługiwana. |
| Bogate w wiadomości e-mail klientów, takich jak Outlook i programu ActiveSync |Obsługiwane |Brak |

Aby uzyskać więcej informacji, zobacz [AuthAnvil rejestracji jednokrotnej.](https://help.scorpionsoft.com/entries/26538603-How-can-I-Configure-Single-Sign-On-for-Office-365-).


## <a name="big-ip-with-access-policy-manager-big-ip-ver-113x--116x"></a>BIG-IP z wersja Menedżera zasad dostępu BIG-IP 11.3 x — 11, 6 x

Poniżej przedstawiono scenariusz macierzy obsługi tego pojedynczego jednokrotnego: 

| Klient | Pomoc techniczna | Wyjątki |
| --- | --- | --- |
| Oparte na sieci Web klientów, takich jak dostęp w sieci Web programu Exchange i SharePoint Online |Obsługiwane |Brak |
| Rozbudowane aplikacje klienckie, takie jak Lync subskrypcji pakietu Office, CRM |Nieobsługiwane |Nieobsługiwane |
| Bogate w wiadomości e-mail klientów, takich jak Outlook i programu ActiveSync |Obsługiwane |Brak |

Aby uzyskać więcej informacji na temat Menedżera zasad dostępu BIG-IP, zobacz [Menedżera zasad dostępu BIG-IP.](https://f5.com/products/modules/access-policy-manager) 

Instrukcje Menedżera zasad dostępu BIG-IP na temat sposobu konfigurowania tego STS w celu zapewnienia jednego środowisko logowania użytkowników Active Directory, Pobierz plik pdf [BIG-IP](http://www.f5.com/pdf/deployment-guides/microsoft-office-365-idp-dg.pdf).

## <a name="bitglass"></a>BitGlass

Poniżej przedstawiono scenariusz macierzy obsługi tego pojedynczego jednokrotnego:

| Klient | Pomoc techniczna | Wyjątki |
| --- | --- | --- |
| Oparte na sieci Web klientów, takich jak dostęp w sieci Web programu Exchange i SharePoint Online |Obsługiwane |Zintegrowane uwierzytelnianie systemu Windows nie jest obsługiwana. |
| Rozbudowane aplikacje klienckie, takie jak Lync subskrypcji pakietu Office, CRM |Obsługiwane |Zintegrowane uwierzytelnianie systemu Windows nie jest obsługiwana. |
| Bogate w wiadomości e-mail klientów, takich jak Outlook i programu ActiveSync |Obsługiwane |Brak |

Aby uzyskać więcej informacji na temat BitGlass zobacz [BitGlass](http://www.bitglass.com).

## <a name="ca-secure-cloud"></a>Chmura bezpiecznego urzędu certyfikacji

Poniżej przedstawiono scenariusz macierzy obsługi tego pojedynczego jednokrotnego:

| Klient | Pomoc techniczna | Wyjątki |
| --- | --- | --- |
| Oparte na sieci Web klientów, takich jak dostęp w sieci Web programu Exchange i SharePoint Online |Obsługiwane |Zintegrowane uwierzytelnianie systemu Windows nie jest obsługiwana. |
| Rozbudowane aplikacje klienckie, takie jak Lync subskrypcji pakietu Office, CRM |Obsługiwane |Zintegrowane uwierzytelnianie systemu Windows nie jest obsługiwana. |
| Bogate w wiadomości e-mail klientów, takich jak Outlook i programu ActiveSync |Obsługiwane |Brak |

Aby uzyskać więcej informacji o chmurze Secure urzędu certyfikacji, zobacz [urzędu certyfikacji Secure Cloud](http://www.ca.com/us/products/security-as-a-service.aspx).

## <a name="ca-siteminder-1252-sp1-cumulative-release-4"></a>Urząd certyfikacji SiteMinder 12.52 SP1 zbiorczą w wersji 4

Poniżej przedstawiono scenariusz macierzy obsługi tego pojedynczego jednokrotnego: 

| Klient | Pomoc techniczna | Wyjątki |
| --- | --- | --- |
| Oparte na sieci Web klientów, takich jak dostęp w sieci Web programu Exchange i SharePoint Online |Obsługiwane |Brak |
| Rozbudowane aplikacje klienckie, takie jak Lync subskrypcji pakietu Office, CRM |Obsługiwane |Brak |
| Bogate w wiadomości e-mail klientów, takich jak Outlook i programu ActiveSync |Obsługiwane |Brak |

Aby uzyskać więcej informacji na temat SiteMinder urzędu certyfikacji, zobacz [federacyjnego SiteMinder urzędu certyfikacji](http://www.ca.com/us/products/ca-single-sign-on.html). 

## <a name="centrify"></a>Centrify

Poniżej przedstawiono scenariusz macierzy obsługi tego pojedynczego jednokrotnego:

| Klient | Pomoc techniczna | Wyjątki |
| --- | --- | --- |
| Oparte na sieci Web klientów, takich jak dostęp w sieci Web programu Exchange i SharePoint Online |Obsługiwane |Brak |
| Rozbudowane aplikacje klienckie, takie jak Lync subskrypcji pakietu Office, CRM |Obsługiwane |Brak |
| Bogate w wiadomości e-mail klientów, takich jak Outlook i programu ActiveSync |Obsługiwane |Kontrola dostępu klienta nie jest obsługiwana. |

Aby uzyskać więcej informacji o Centrify, zobacz [Centrify](http://www.centrify.com/cloud/apps/single-sign-on-for-office-365.asp).

## <a name="dell-one-identity-cloud-access-manager-v71"></a>Dell jeden v7.1 Menedżer dostępu do tożsamości chmury

Poniżej przedstawiono scenariusz macierzy obsługi tego pojedynczego jednokrotnego:

| Klient | Pomoc techniczna | Wyjątki |
| --- | --- | --- |
| Oparte na sieci Web klientów, takich jak dostęp w sieci Web programu Exchange i SharePoint Online |Obsługiwane |Brak |
| Rozbudowane aplikacje klienckie, takie jak Lync subskrypcji pakietu Office, CRM |Obsługiwane |Brak |
| Bogate w wiadomości e-mail klientów, takich jak Outlook i programu ActiveSync |Obsługiwane |Brak |

Aby uzyskać więcej informacji o Dell jednej tożsamości chmury Menedżera dostępu, zobacz [Menedżer dostępu do chmury co tożsamości Dell](http://software.dell.com/products/cloud-access-manager).

 Aby uzyskać instrukcje dotyczące konfigurowania tej usługi STS zapewnienie jednej środowisko logowania do sieci użytkowników usługi Office 365, zobacz [skonfigurowania użytkowników usługi Office 365](http://documents.software.dell.com/dell-one-identity-cloud-access-manager/7.1/how-to-configure-microsoft-office-365). 

## <a name="digitalpersona-composite-authentication"></a>Uwierzytelnianie złożone DigitalPersona  

Poniżej przedstawiono scenariusz macierzy obsługi tego pojedynczego jednokrotnego:

| Klient | Pomoc techniczna | Wyjątki |
| --- | --- | --- |
| Oparte na sieci Web klientów, takich jak dostęp w sieci Web programu Exchange i SharePoint Online |Obsługiwane |Zintegrowane uwierzytelnianie systemu Windows nie jest obsługiwana.|
| Rozbudowane aplikacje klienckie, takie jak Lync subskrypcji pakietu Office, CRM |Obsługiwane |Zintegrowane uwierzytelnianie systemu Windows nie jest obsługiwana.|
| Bogate w wiadomości e-mail klientów, takich jak Outlook i programu ActiveSync |Obsługiwane |Brak |

Aby uzyskać więcej informacji, zobacz [uwierzytelniania złożonego DigitalPersona](http://www.crossmatch.com/uploadedFiles/Support/Reference_Material/DigitalPersona-Office-365-Deployment-Guide.pdf).


## <a name="ibm-tivoli-federated-identity-manager-622"></a>Menedżer tożsamości 6.2.2 federacyjnych IBM Tivoli

Poniżej przedstawiono scenariusz macierzy obsługi tego pojedynczego jednokrotnego: 

| Klient | Pomoc techniczna | Wyjątki |
| --- | --- | --- |
| Oparte na sieci Web klientów, takich jak dostęp w sieci Web programu Exchange i SharePoint Online |Obsługiwane |Brak |
| Rozbudowane aplikacje klienckie, takie jak Lync subskrypcji pakietu Office, CRM |Obsługiwane |Brak |
| Bogate w wiadomości e-mail klientów, takich jak Outlook i programu ActiveSync |Obsługiwane |Brak |

Aby uzyskać więcej informacji na temat IBM Tivoli federacyjnych Identity Manager, zobacz [Menedżer dostępu do zabezpieczeń IBM for Microsoft Applications](http://www-01.ibm.com/support/docview.wss?uid=swg24029517).

## <a name="icewall-federation-version-30"></a>IceWall Federation w wersji 3.0

Poniżej przedstawiono scenariusz macierzy obsługi tego pojedynczego jednokrotnego:

| Klient | Pomoc techniczna | Wyjątki |
| --- | --- | --- |
| Oparte na sieci Web klientów, takich jak dostęp w sieci Web programu Exchange i SharePoint Online |Obsługiwane |Zintegrowane uwierzytelnianie systemu Windows nie jest obsługiwana. |
| Rozbudowane aplikacje klienckie, takie jak Lync subskrypcji pakietu Office, CRM |Obsługiwane |Zintegrowane uwierzytelnianie systemu Windows nie jest obsługiwana. |
| Bogate w wiadomości e-mail klientów, takich jak Outlook i programu ActiveSync |Obsługiwane |Brak |

Aby uzyskać więcej informacji na temat IceWall federacyjnego, zobacz [IceWall Federation w wersji 3.0](http://h50146.www5.hp.com/products/software/security/icewall/eng/federation/) i [IceWall federacji z usługą Office 365](http://h50146.www5.hp.com/products/software/security/icewall/federation/office365.html).

## <a name="memority"></a>Memority

Poniżej przedstawiono scenariusz macierz obsługi dla tego działania logowania jednokrotnego: 

| Klient | Pomoc techniczna | Wyjątki |
| --- | --- | --- |
| Oparte na sieci Web klientów, takich jak dostęp w sieci Web programu Exchange i SharePoint Online |Obsługiwane |Brak |
| Rozbudowane aplikacje klienckie, takie jak Lync subskrypcji pakietu Office, CRM |Obsługiwane |Brak |
| Bogate w wiadomości e-mail klientów, takich jak Outlook i programu ActiveSync |Obsługiwane |Brak |

Aby uzyskać więcej informacji o używaniu Memority zobacz [Memority](http://www.memority.com).


## <a name="netiq-access-manager-4x"></a>Menedżer dostępu do NetIQ 4.x

Poniżej przedstawiono scenariusz macierzy obsługi tego pojedynczego jednokrotnego:

| Klient | Pomoc techniczna | Wyjątki |
| --- | --- | --- |
| Oparte na sieci Web klientów, takich jak dostęp w sieci Web programu Exchange i SharePoint Online |Obsługiwane |Brak|
| Rozbudowane aplikacje klienckie, takie jak Lync subskrypcji pakietu Office, CRM |Obsługiwane |Brak|
| Bogate w wiadomości e-mail klientów, takich jak Outlook i programu ActiveSync |Obsługiwane |Brak |

Aby uzyskać więcej informacji, zobacz [Menedżer dostępu do NetIQ](https://www.netiq.com/documentation/access-manager-43/admin/data/b65ogn0.html#b12iqp0m).

## <a name="okta"></a>Usługi okta

Poniżej przedstawiono scenariusz macierzy obsługi tego pojedynczego jednokrotnego: 

| Klient | Pomoc techniczna | Wyjątki |
| --- | --- | --- |
| Oparte na sieci Web klientów, takich jak dostęp w sieci Web programu Exchange i SharePoint Online |Obsługiwane |Zintegrowane uwierzytelnianie systemu Windows wymaga instalacji dodatkowy serwer sieci web i aplikacji usługi Okta. |
| Rozbudowane aplikacje klienckie, takie jak Lync subskrypcji pakietu Office, CRM |Obsługiwane |Zintegrowane uwierzytelnianie systemu Windows |
| Bogate w wiadomości e-mail klientów, takich jak Outlook i programu ActiveSync |Obsługiwane |Brak |

Aby uzyskać więcej informacji na temat usługi Okta, zobacz [usługi Okta](https://www.okta.com/).

## <a name="onelogin"></a>OneLogin

Poniżej przedstawiono scenariusz macierzy obsługi tego pojedynczego jednokrotnego: 

| Klient | Pomoc techniczna | Wyjątki |
| --- | --- | --- |
| Oparte na sieci Web klientów, takich jak dostęp w sieci Web programu Exchange i SharePoint Online |Obsługiwane |Zintegrowane uwierzytelnianie systemu Windows |
| Rozbudowane aplikacje klienckie, takie jak Lync subskrypcji pakietu Office, CRM |Obsługiwane |Zintegrowane uwierzytelnianie systemu Windows |
| Bogate w wiadomości e-mail klientów, takich jak Outlook i programu ActiveSync |Obsługiwane |Brak |

Aby uzyskać więcej informacji o OneLogin, zobacz [OneLogin](https://www.onelogin.com/).

## <a name="optimal-idm-virtual-identity-server-federation-services"></a>Usługi federacyjne serwera wirtualnego tożsamości optymalne IDM

Poniżej przedstawiono Scenariusz ten pojedynczy jednokrotnego macierz obsługi:

| Klient | Pomoc techniczna | Wyjątki |
| --- | --- | --- |
| Oparte na sieci Web klientów, takich jak dostęp w sieci Web programu Exchange i SharePoint Online |Obsługiwane |Brak |
| Rozbudowane aplikacje klienckie, takie jak Lync subskrypcji pakietu Office, CRM |Obsługiwane |Zintegrowane uwierzytelnianie systemu Windows |
| Bogate w wiadomości e-mail klientów, takich jak Outlook i programu ActiveSync |Obsługiwane |

Dla więcej informacji na temat dostępu klienta zasady zobacz [ograniczanie dostępu do pakietu Office 365 usługi na podstawie lokalizacji klienta](https://technet.microsoft.com/library/hh526961.aspx).





## <a name="pingfederate-611-72-8x"></a>PingFederate 6.11, 7.2, 8.x

Poniżej przedstawiono scenariusz macierzy obsługi tego pojedynczego jednokrotnego:

| Klient | Pomoc techniczna | Wyjątki |
| --- | --- | --- |
| Oparte na sieci Web klientów, takich jak dostęp w sieci Web programu Exchange i SharePoint Online |Obsługiwane |Brak |
| Rozbudowane aplikacje klienckie, takie jak Lync subskrypcji pakietu Office, CRM |Obsługiwane |Brak |
| Bogate w wiadomości e-mail klientów, takich jak Outlook i programu ActiveSync |Obsługiwane |Brak |

PingFederate instrukcje dotyczące konfigurowania tej usługi STS zapewnienie jednej środowisko logowania jednokrotnego dla użytkowników usługi Active Directory Zobacz jedną z następujących czynności: 

- [PingFederate 6.11](http://go.microsoft.com/fwlink/?LinkID=266321)
- [PingFederate 7.2](http://documentation.pingidentity.com/display/PF72/PingFederate+7.2)
- [PingFederate 8.x](http://documentation.pingidentity.com/display/PFS/SSO+to+Office+365+Introduction)

## <a name="radiantone-cfs-30"></a>RadiantOne CFS 3.0

Poniżej przedstawiono scenariusz macierzy obsługi tego pojedynczego jednokrotnego: 

| Klient | Pomoc techniczna | Wyjątki |
| --- | --- | --- |
| Oparte na sieci Web klientów, takich jak dostęp w sieci Web programu Exchange i SharePoint Online |Obsługiwane |Brak |
| Rozbudowane aplikacje klienckie, takie jak Lync subskrypcji pakietu Office, CRM |Obsługiwane |Zintegrowane uwierzytelnianie systemu Windows |
| Bogate w wiadomości e-mail klientów, takich jak Outlook i programu ActiveSync |Obsługiwane |Brak |

Aby uzyskać więcej informacji na temat RadiantOne CFS, zobacz [RadiantOne CFS](http://www.radiantlogic.com/products/radiantone-cfs/).

## <a name="sailpoint-identitynow"></a>Sailpoint IdentityNow

Poniżej przedstawiono scenariusz macierzy obsługi tego pojedynczego jednokrotnego:

| Klient | Pomoc techniczna | Wyjątki |
| --- | --- | --- |
| Oparte na sieci Web klientów, takich jak dostęp w sieci Web programu Exchange i SharePoint Online |Obsługiwane |Zintegrowane uwierzytelnianie systemu Windows nie jest obsługiwana. |
| Rozbudowane aplikacje klienckie, takie jak Lync subskrypcji pakietu Office, CRM |Obsługiwane |Zintegrowane uwierzytelnianie systemu Windows nie jest obsługiwana. |
| Bogate w wiadomości e-mail klientów, takich jak Outlook i programu ActiveSync |Obsługiwane |Brak |

Aby uzyskać więcej informacji, zobacz [Sailpoint IdentityNow](https://www.sailpoint.com/idaas-identity-as-a-service-identitynow/).

## <a name="secureauth-idp-720"></a>SecureAuth IdP 7.2.0

Poniżej przedstawiono scenariusz macierzy obsługi tego pojedynczego jednokrotnego: 

| Klient | Pomoc techniczna | Wyjątki |
| --- | --- | --- |
| Oparte na sieci Web klientów, takich jak dostęp w sieci Web programu Exchange i SharePoint Online |Obsługiwane |Brak |
| Rozbudowane aplikacje klienckie, takie jak Lync subskrypcji pakietu Office, CRM |Obsługiwane |Brak |
| Bogate w wiadomości e-mail klientów, takich jak Outlook i programu ActiveSync |Obsługiwane |Brak |

Aby uzyskać więcej informacji o SecureAuth, zobacz [SecureAuth IdP](http://go.microsoft.com/?linkid=9845293).














## <a name="signgo-53"></a>Zaloguj się i przejdź 5.3

Poniżej przedstawiono scenariusz macierzy obsługi tego pojedynczego jednokrotnego:

| Klient | Pomoc techniczna | Wyjątki |
| --- | --- | --- |
| Oparte na sieci Web klientów, takich jak dostęp w sieci Web programu Exchange i SharePoint Online |Obsługiwane |Kontrakty protokołu Kerberos, obsługiwany |
| Rozbudowane aplikacje klienckie, takie jak Lync subskrypcji pakietu Office, CRM |Obsługiwane |Brak |
| Bogate w wiadomości e-mail klientów, takich jak Outlook i programu ActiveSync |Obsługiwane |Brak |

Znak & go 5.3 obsługuje uwierzytelnianie Kerberos za pomocą konfiguracji kontrakt protokołu Kerberos.  Aby uzyskać pomoc w przypadku tej konfiguracji, skontaktuj się z Ilex lub wyświetlić przewodnik konfiguracji [znak & go](http://www.ilex-international.com/docs/sign&go_wsfederation_en.pdf)

## <a name="softbank-technology-online-service-gate"></a>Brama usługi Online SoftBank technologii

Poniżej przedstawiono scenariusz macierzy obsługi tego pojedynczego jednokrotnego:

| Klient | Pomoc techniczna | Wyjątki |
| --- | --- | --- |
| Oparte na sieci Web klientów, takich jak dostęp w sieci Web programu Exchange i SharePoint Online |Obsługiwane |Zintegrowane uwierzytelnianie systemu Windows nie jest obsługiwana. |
| Rozbudowane aplikacje klienckie, takie jak Lync subskrypcji pakietu Office, CRM |Obsługiwane |Zintegrowane uwierzytelnianie systemu Windows nie jest obsługiwana. |
| Bogate w wiadomości e-mail klientów, takich jak Outlook i programu ActiveSync |Obsługiwane |Brak |

Aby uzyskać więcej informacji na temat technologii SoftBank Online bramy usługi zobacz [Softbank](https://www.softbanktech.jp/service/list/osg-pro-ent/)

## <a name="vmware-workspace-one"></a>Obszar roboczy VMware co

Poniżej przedstawiono scenariusz macierzy obsługi tego pojedynczego jednokrotnego:

| Klient | Pomoc techniczna | Wyjątki |
| --- | --- | --- |
| Oparte na sieci Web klientów, takich jak dostęp w sieci Web programu Exchange i SharePoint Online |Obsługiwane |Zintegrowane uwierzytelnianie systemu Windows nie jest obsługiwana. |
| Rozbudowane aplikacje klienckie, takie jak Lync subskrypcji pakietu Office, CRM |Obsługiwane |Zintegrowane uwierzytelnianie systemu Windows nie jest obsługiwana. |
| Bogate w wiadomości e-mail klientów, takich jak Outlook i programu ActiveSync |Obsługiwane |Brak |

Aby uzyskać więcej informacji na temat, zobacz [jednego obszaru roboczego VMware](http://www.vmware.com/pdf/vidm-office365-saml.pdf)

