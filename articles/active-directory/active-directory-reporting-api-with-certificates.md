---
title: "Pobieranie danych przy użyciu interfejsu API raportowania usługi Azure AD z certyfikatami | Microsoft Docs"
description: "Opis sposobu użycia interfejsu API raportowania usługi Azure AD przy użyciu poświadczeń certyfikatu w celu pobrania danych z katalogów bez interwencji użytkownika."
services: active-directory
documentationcenter: 
author: ramical
writer: v-lorisc
manager: kannar
ms.assetid: 
ms.service: active-directory
ms.workload: infrastructure-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 09/08/2017
ms.author: ramical
ms.translationtype: Human Translation
ms.sourcegitcommit: 07635b0eb4650f0c30898ea1600697dacb33477c
ms.openlocfilehash: c1345dcda6e52267a8037ffd7207e6bc3b0d3b31
ms.contentlocale: pl-pl
ms.lasthandoff: 03/28/2017

---
# <a name="get-data-using-the-azure-ad-reporting-api-with-certificates"></a>Pobieranie danych przy użyciu interfejsu API raportowania usługi Azure AD z certyfikatami
Ten artykuł zawiera opis sposobu użycia interfejsu API raportowania usługi Azure AD przy użyciu poświadczeń certyfikatu w celu pobrania danych z katalogów bez interwencji użytkownika. 

## <a name="use-the-azure-ad-reporting-api"></a>Zastosowanie interfejsu API raportowania usługi Azure AD 
Interfejs API raportowania usługi Azure AD wymaga wykonania następujących kroków:
 *  Instalacja wymagań wstępnych
 *  Skonfigurowanie certyfikatu w aplikacji
 *  Pobranie tokenu dostępu
 *  Wykorzystanie tokenu dostępu do wywołania interfejsu API programu Graph

Aby uzyskać informacje o kodzie źródłowym, zobacz [Leverage Report API Module](https://github.com/AzureAD/azure-activedirectory-powershell/tree/gh-pages/Modules/AzureADUtils) (Wykorzystanie modułu interfejsu API raportowania). 

### <a name="install-prerequisites"></a>Instalacja wymagań wstępnych
Konieczne jest zainstalowanie programu Azure AD PowerShell V2 i modułu AzureADUtils.

1. Pobierz i zainstaluj program Azure AD Powershell V2, postępując zgodnie z instrukcjami w temacie [Azure Active Directory PowerShell](https://github.com/Azure/azure-docs-powershell-azuread/blob/master/Azure AD Cmdlets/AzureAD/index.md).
2. Pobierz moduł Azure AD Utils z lokalizacji [AzureAD/azure-activedirectory-powershell](https://github.com/AzureAD/azure-activedirectory-powershell/blob/gh-pages/Modules/AzureADUtils/AzureADUtils.psm1). 
  Ten moduł zapewnia kilka poleceń cmdlet narzędzi, w tym:
   * Najnowszą wersję bibliotek ADAL korzystających z narzędzia Nuget
   * Tokeny dostępu użytkownika, kluczy aplikacji i certyfikatów korzystających z bibliotek ADAL
   * Stronicowane wyniki obsługi interfejsu API programu Graph

**Aby zainstalować moduł Azure AD Utils:**

1. Utwórz katalog w celu zapisania modułu narzędzi (na przykład c:\azureAD) i pobierz moduł z serwisu GitHub.
2. Otwórz sesję programu PowerShell i przejdź do utworzonego właśnie katalogu. 
3. Zaimportuj moduł i zainstaluj go w ścieżce modułu programu PowerShell za pomocą polecenia cmdlet Install-AzureADUtilsModule. 

Sesja powinna wyglądać podobnie do tego ekranu:

  ![Windows Powershell](./media/active-directory-report-api-with-certificates/windows-powershell.png)

### <a name="set-the-certificate-in-your-app"></a>Skonfigurowanie certyfikatu w aplikacji
1. Jeśli masz już aplikację, pobierz jej identyfikator obiektu z witryny Azure Portal. 

  ![Portal Azure](./media/active-directory-report-api-with-certificates/azure-portal.png)

2. Otwórz sesję programu PowerShell i nawiąż połączenie z usługą Azure AD za pomocą polecenia cmdlet Connect-AzureAD.

  ![Portal Azure](./media/active-directory-report-api-with-certificates/connect-azuaread-cmdlet.png)

3. Użyj polecenia cmdlet New-AzureADApplicationCertificateCredential z modułu AzureADUtils, aby dodać do niego poświadczenie certyfikatu. 

>[!Note]
>Należy podać zarejestrowany wcześniej identyfikator obiektu aplikacji, a także obiekt certyfikatu (służy do tego polecenie Cert: drive).
>


  ![Portal Azure](./media/active-directory-report-api-with-certificates/add-certificate-credential.png)
  
### <a name="get-an-access-token"></a>Pobranie tokenu dostępu

Aby uzyskać token dostępu, użyj polecenia cmdlet Get-AzureADGraphAPIAccessTokenFromCert z modułu AzureADUtils. 

>[!NOTE]
>Należy użyć identyfikatora aplikacji zamiast identyfikatora obiektu użytego w ostatniej sekcji.
>

 ![Portal Azure](./media/active-directory-report-api-with-certificates/application-id.png)

### <a name="use-the-access-token-to-call-the-graph-api"></a>Wykorzystanie tokenu dostępu do wywołania interfejsu API programu Graph

Teraz można utworzyć skrypt. Poniżej zamieszczono przykład korzystający z polecenia Invoke-AzureADGraphAPIQuery z modułu AzureADUtils. To polecenie cmdlet obsługuje wyniki wielostronicowe, a następnie wysyła te wyniki do potoku programu PowerShell. 

 ![Portal Azure](./media/active-directory-report-api-with-certificates/script-completed.png)

Teraz można przystąpić do eksportowania wyników do formatu CSV i zapisu w systemie SIEM. Skrypt można również opakować w zaplanowane zadanie, aby okresowo uzyskiwać dane usługi Azure AD od dzierżawcy bez konieczności przechowywania kluczy aplikacji w kodzie źródłowym. 

## <a name="next-steps"></a>Następne kroki
[The fundamentals of Azure identity management](https://docs.microsoft.com/en-us/azure/active-directory/fundamentals-identity) (Podstawy zarządzania tożsamościami w usłudze Azure)<br>




