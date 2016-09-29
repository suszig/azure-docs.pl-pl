<properties
    pageTitle="Uzyskiwanie dostępu do usługi Key Vault za zaporą | Microsoft Azure"
    description="Dowiedz się, jak uzyskać dostęp do usługi Key Vault z aplikacji znajdującej się za zaporą"
    services="key-vault"
    documentationCenter=""
    authors="amitbapat"
    manager="mbaldwin"
    tags="azure-resource-manager"/>

<tags
    ms.service="key-vault"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="hero-article"
    ms.date="09/13/2016"
    ms.author="ambapat"/>


# Uzyskiwanie dostępu do usługi Key Vault za zaporą
### Pytanie: Moja aplikacja kliencka magazynu kluczy musi znajdować się za zaporą. Które porty/hosty/adresy IP należy otworzyć, aby umożliwić dostęp do magazynu kluczy?

Aby uzyskać dostęp do magazynu kluczy, aplikacja kliencka magazynu kluczy musi być w stanie uzyskać dostęp do wielu punktów końcowych dla różnych funkcji.

- Uwierzytelnianie (za pośrednictwem usługi Azure Active Directory)
- Zarządzanie usługą Key Vault (w tym tworzenie/odczyt/aktualizowanie/usuwanie oraz ustawianie zasad dostępu) za pośrednictwem usługi Azure Resource Manager
- Uzyskiwanie dostępu do obiektów (kluczy i wpisów tajnych) przechowywanych w magazynie kluczy i zarządzanie tymi obiektami przechodzi przez punkt końcowy specyficzny dla magazynu kluczy (np. [https://nazwamagazynu.vault.azure.net](https://yourvaultname.vault.azure.net)).  

W zależności od konfiguracji i środowiska istnieją pewne odstępstwa.   

## Porty

Cały ruch do magazynu kluczy dla wszystkich 3 funkcji (uwierzytelnianie, zarządzanie i dostęp do płaszczyzny danych) przechodzi przez protokół HTTPS: port 443. Jednak w przypadku listy CRL może czasami wystąpić ruch w protokole HTTP (port 80). Klienci obsługujący protokół OCSP nie powinni docierać do listy CRL, ale czasami mogą dotrzeć do pliku [http://cdp1.public-trust.com/CRL/Omniroot2025.crl](http://cdp1.public-trust.com/CRL/Omniroot2025.crl).  

## Uwierzytelnianie

W celu uwierzytelnienia aplikacja kliencka usługi Key Vault będzie musiała uzyskać dostęp do punktów końcowych usługi Azure Active Directory. Użyty punkt końcowy zależy od konfiguracji dzierżawy usługi AAD oraz typu nazwy głównej — nazwa główna użytkownika, nazwa główna usługi i typ konta, np. konto Microsoft lub identyfikator organizacji.  

| Typ nazwy głównej | Punkt końcowy:port |
|----------------|---------------|
| Użytkownik korzystający z konta Microsoft<br> (np. użytkownik@hotmail.com) | **Globalny:**<br> login.microsoftonline.com:443<br><br> **Chińska wersja platformy Azure:**<br> login.chinacloudapi.cn:443<br><br>**Wersja platformy Azure dla administracji USA:**<br> login-us.microsoftonline.com:443<br><br>**Niemiecka wersja platformy Azure:**<br> login.microsoftonline.de:443<br><br> i <br>login.live.com:443   |
| Nazwa główna użytkownika/usługi przy użyciu identyfikatora organizacji wraz z usługą AAD (np. użytkownik@contoso.com) | **Globalny:**<br> login.microsoftonline.com:443<br><br> **Chińska wersja platformy Azure:**<br> login.chinacloudapi.cn:443<br><br>**Wersja platformy Azure dla administracji USA:**<br> login-us.microsoftonline.com:443<br><br>**Niemiecka wersja platformy Azure:**<br> login.microsoftonline.de:443 |
| Nazwa główna użytkownika/usługi przy użyciu identyfikatora organizacji i usług ADFS lub innego federacyjnego punktu końcowego (np. użytkownik@contoso.com) | Wszystkie powyższe punkty końcowe dla identyfikatora organizacji oraz usług ADFS lub innych federacyjnych punktów końcowych |

Istnieją inne możliwe złożone scenariusze. Aby uzyskać dodatkowe informacje, zapoznaj się z artykułami [Azure Active Directory Authentication Flow](/documentation/articles/active-directory-authentication-scenarios/) (Przepływ uwierzytelniania w usłudze Azure Active Directory), [Integrowanie aplikacji w usłudze Azure Active Directory](/documentation/articles/active-directory-integrating-applications/) i [Protokoły uwierzytelniania usługi Active Directory](https://msdn.microsoft.com/library/azure/dn151124.aspx).  

## Zarządzanie usługą Key Vault

Aby zarządzać usługą Key Vault (akcje CRUD i ustawianie zasad dostępu), aplikacja kliencka magazynu kluczy musi uzyskać dostęp do punktu końcowego usługi Azure Resource Manager.  

| Typ operacji | Punkt końcowy:port |
|----------------|---------------|
| Operacje warstwy kontroli usługi Key Vault<br> za pośrednictwem usługi Azure Resource Manager | **Globalny:**<br> management.azure.com:443<br><br> **Chińska wersja platformy Azure:**<br> management.chinacloudapi.cn:443<br><br> **Wersja platformy Azure dla administracji USA:**<br> management.usgovcloudapi.net:443<br><br> **Niemiecka wersja platformy Azure:**<br> management.microsoftazure.de:443 |
| Interfejs API programu Graph usługi Azure Active Directory | **Globalny:**<br> graph.windows.net:443<br><br> **Chińska wersja platformy Azure:**<br> graph.chinacloudapi.cn:443<br><br> **Wersja platformy Azure dla administracji USA:**<br> graph.windows.net:443<br><br> **Niemiecka wersja platformy Azure:**<br> graph.cloudapi.de:443 |

## Operacje usługi Key Vault

We wszystkich przypadkach operacji kryptograficznych na obiektach (kluczach i wpisach tajnych) oraz zarządzania tymi obiektami klient magazynu kluczy musi uzyskać dostęp do punktu końcowego magazynu kluczy. Sufiks DNS punktu końcowego jest inny w zależności od lokalizacji usługi Key Vault. Format punktu końcowego usługi Key Vault wygląda następująco: <nazwa magazynu >. <specyficzny dla regionu sufiks dns> zgodnie z opisem w poniższej tabeli.  

| Typ operacji | Punkt końcowy:port |
|----------------|---------------|
| Operacje usługi Key Vault, takie jak operacje kryptograficzne na kluczach, tworzenie/odczytywanie/aktualizowanie/usuwanie kluczy i wpisów tajnych, ustawianie/pobieranie tagów i innych atrybutów obiektów magazynu kluczy (klucze/wpisy tajne)     | **Globalny:**<br> &lt;vault-name&gt;.vault.azure.net:443<br><br> **Chińska wersja platformy Azure:**<br> &lt;vault-name&gt;.vault.azure.cn:443<br><br> **Wersja platformy Azure dla administracji USA:**<br> &lt;vault-name&gt;.vault.usgovcloudapi.net:443<br><br> **Niemiecka wersja platformy Azure:**<br> &lt;vault-name&gt;.vault.microsoftazure.de:443 |

## Zakresy adresów IP

Z kolei usługa Key Vault używa innych zasobów platformy Azure, takich jak infrastruktura PaaS, dlatego nie jest możliwe podanie konkretnego zakresu adresów IP, których będą używać punkty końcowe magazynu kluczy w określonym czasie. Jeśli jednak zapora obsługuje tylko zakresy adresów IP, zapoznaj się z dokumentem [Microsoft Azure Datacenter IP Ranges](https://www.microsoft.com/download/details.aspx?id=41653) (Zakresy adresów IP centrów danych platformy Microsoft Azure).   W przypadku uwierzytelniania i tożsamości (Azure Active Directory) aplikacja musi być w stanie nawiązać połączenie z punktami końcowymi opisanymi w artykule [Authentication and identity Addresses](https://support.office.com/article/Office-365-URLs-and-IP-address-ranges-8548a211-3fe7-47cb-abb1-355ea5aa88a2) (Adresy uwierzytelniania i tożsamości).

## Następne kroki

- Jeśli masz pytania dotyczące usługi Key Vault, odwiedź [forum usługi Azure Key Vault](https://social.msdn.microsoft.com/forums/azure/home?forum=AzureKeyVault)



<!--HONumber=Sep16_HO3-->


