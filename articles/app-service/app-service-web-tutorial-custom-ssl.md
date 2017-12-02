---
title: "Powiąż istniejący certyfikat SSL niestandardowych do aplikacji sieci Web platformy Azure | Dokumentacja firmy Microsoft"
description: "Dowiedz się powiązać niestandardowego certyfikatu SSL do aplikacji sieci web, zaplecza aplikacji mobilnej lub aplikacji interfejsu API w usłudze Azure App Service."
services: app-service\web
documentationcenter: nodejs
author: cephalin
manager: erikre
editor: 
ms.assetid: 5d5bf588-b0bb-4c6d-8840-1b609cfb5750
ms.service: app-service-web
ms.workload: web
ms.tgt_pltfrm: na
ms.devlang: nodejs
ms.topic: tutorial
ms.date: 11/30/2017
ms.author: cephalin
ms.custom: mvc
ms.openlocfilehash: f69bc731b2858c338d7f7b4d347e7107a0f4eeed
ms.sourcegitcommit: be0d1aaed5c0bbd9224e2011165c5515bfa8306c
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 12/01/2017
---
# <a name="bind-an-existing-custom-ssl-certificate-to-azure-web-apps"></a>Powiąż istniejący certyfikat SSL niestandardowych do aplikacji sieci Web Azure

Aplikacje sieci Web platformy Azure oferuje wysoce skalowalną, własnym poprawiania usługi hosta sieci web. Ten samouczek pokazuje, jak można powiązać certyfikat SSL niestandardowych zakupionego od zaufanego urzędu certyfikacji do [Azure Web Apps](app-service-web-overview.md). Po zakończeniu, będzie można uzyskać dostępu do aplikacji sieci web w punkcie końcowym HTTPS z niestandardowej domeny DNS.

![Aplikacja sieci Web z niestandardowego certyfikatu SSL](./media/app-service-web-tutorial-custom-ssl/app-with-custom-ssl.png)

Ten samouczek zawiera informacje na temat wykonywania następujących czynności:

> [!div class="checklist"]
> * Uaktualnij warstwę cenową aplikacji
> * Powiązać niestandardowego certyfikatu SSL z usługi aplikacji
> * Wymuszanie protokołu HTTPS dla aplikacji
> * Powiązania certyfikatu SSL za pomocą skryptów automatyzacji

> [!NOTE]
> Jeśli potrzebujesz niestandardowego certyfikatu SSL, możesz pobrać go w portalu Azure bezpośrednio i powiązać go z aplikacji sieci web. Postępuj zgodnie z [certyfikaty usługi aplikacji — samouczek](web-sites-purchase-ssl-web-site.md).

## <a name="prerequisites"></a>Wymagania wstępne

W celu ukończenia tego samouczka:

- [Utwórz aplikację usługi aplikacji](/azure/app-service/)
- [Mapowanie niestandardową nazwę DNS do aplikacji sieci web](app-service-web-tutorial-custom-domain.md)
- Uzyskanie certyfikatu SSL z zaufanego urzędu certyfikacji
- Klucz prywatny, używanego do podpisywania żądań certyfikatów SSL

<a name="requirements"></a>

### <a name="requirements-for-your-ssl-certificate"></a>Wymagania dotyczące certyfikatu SSL

Aby użyć certyfikatu w usłudze App Service, certyfikat musi spełniać następujące wymagania:

* Podpisane przez zaufany urząd certyfikacji
* Eksportowane jako chronionego hasłem pliku PFX
* Zawiera klucz prywatny co najmniej 2048 bitów długo
* Zawiera wszystkie certyfikaty pośrednie w łańcuchu certyfikatów

> [!NOTE]
> **Certyfikaty krzywa Cryptography (ECC) eliptycznej** może współpracować z usługi aplikacji, ale nie są objęte w tym artykule. Współpraca z urzędu certyfikacji na kolejnych kroków w celu utworzenia certyfikatów ECC.

## <a name="prepare-your-web-app"></a>Przygotowanie aplikacji sieci web

Do powiązania niestandardowego certyfikatu SSL do aplikacji sieci web z [planu usługi aplikacji](https://azure.microsoft.com/pricing/details/app-service/) musi znajdować się w **podstawowe**, **standardowe**, lub **Premium** warstwy. W tym kroku należy upewnić się, że aplikacja sieci web jest w obsługiwanym warstwy cenowej.

### <a name="log-in-to-azure"></a>Zaloguj się do platformy Azure.

Otwórz [portal Azure](https://portal.azure.com).

### <a name="navigate-to-your-web-app"></a>Przejdź do aplikacji sieci web

Z menu po lewej stronie kliknij **usługi aplikacji**, a następnie kliknij nazwę aplikacji sieci web.

![Wybierz aplikację sieci web](./media/app-service-web-tutorial-custom-ssl/select-app.png)

Jest na stronie Zarządzanie aplikacji sieci web.  

### <a name="check-the-pricing-tier"></a>Sprawdź warstwę cenową

W obszarze nawigacji po lewej stronie strony aplikacji sieci web, przewiń do **ustawienia** a następnie wybierz opcję **skalowanie w górę (plan usługi App Service)**.

![Skalowanie w pionie menu](./media/app-service-web-tutorial-custom-ssl/scale-up-menu.png)

Upewnij się, że aplikacja sieci web nie znajduje się w **wolne** lub **Shared** warstwy. Warstwa bieżąca aplikacja sieci web jest wyróżniony ciemny niebieskie pole.

![Sprawdź warstwę cenową](./media/app-service-web-tutorial-custom-ssl/check-pricing-tier.png)

Niestandardowe SSL nie jest obsługiwany w **wolne** lub **Shared** warstwy. Jeśli potrzebujesz skalowanie w górę, wykonaj kroki opisane w następnej sekcji. W przeciwnym razie Zamknij **wybierz warstwę cenową** strony i przejść [przekazywanie i powiązać certyfikatu SSL](#upload).

### <a name="scale-up-your-app-service-plan"></a>Skalowanie w górę plan usługi aplikacji

Wybierz jedną z **podstawowe**, **standardowe**, lub **Premium** warstw.

Kliknij pozycję **Wybierz**.

![Wybierz warstwę cenową](./media/app-service-web-tutorial-custom-ssl/choose-pricing-tier.png)

Gdy zostanie wyświetlone następujące powiadomienie, zakończeniu operacji skalowania.

![Skalowanie w górę powiadomień](./media/app-service-web-tutorial-custom-ssl/scale-notification.png)

<a name="upload"></a>

## <a name="bind-your-ssl-certificate"></a>Powiąż certyfikat protokołu SSL

Wszystko jest gotowe do przekazania certyfikatu SSL do aplikacji sieci web.

### <a name="merge-intermediate-certificates"></a>Certyfikaty pośrednie scalenia

Urzędu certyfikacji zawiera wiele certyfikatów w łańcuchu certyfikatów, należy scalić certyfikaty w kolejności. 

Aby to zrobić, Otwórz każdy z certyfikatów, zostanie wyświetlony w edytorze tekstów. 

Utwórz plik certyfikatu scalone, nazywany _mergedcertificate.crt_. W edytorze tekstów skopiuj zawartość każdy z certyfikatów do tego pliku. Kolejność certyfikaty należy stosować kolejności w łańcuchu certyfikatów, począwszy od certyfikatu i kończąc certyfikatu głównego. Wygląda następująco:

```
-----BEGIN CERTIFICATE-----
<your entire Base64 encoded SSL certificate>
-----END CERTIFICATE-----

-----BEGIN CERTIFICATE-----
<The entire Base64 encoded intermediate certificate 1>
-----END CERTIFICATE-----

-----BEGIN CERTIFICATE-----
<The entire Base64 encoded intermediate certificate 2>
-----END CERTIFICATE-----

-----BEGIN CERTIFICATE-----
<The entire Base64 encoded root certificate>
-----END CERTIFICATE-----
```

### <a name="export-certificate-to-pfx"></a>Wyeksportuj certyfikat PFX

Eksportowanie scalonych certyfikatu SSL z kluczem prywatnym, który żądania certyfikatu został wygenerowany z.

Jeśli generowany jest żądanie certyfikatu przy użyciu biblioteki OpenSSL, został utworzony plik klucza prywatnego. Aby wyeksportować certyfikat do PFX, uruchom następujące polecenie. Zastąp symbole zastępcze  _&lt;plików kluczy prywatnych >_ i  _&lt;scalić — plik certyfikatu >_ z ścieżki do klucza prywatnego i certyfikatu scalony plik.

```bash
openssl pkcs12 -export -out myserver.pfx -inkey <private-key-file> -in <merged-certificate-file>  
```

Po wyświetleniu monitu, należy określić hasło eksportu. To hasło będzie używany podczas przekazywania certyfikatu SSL w usłudze App Service później.

Jeśli używasz usług IIS lub _Certreq.exe_ do wygenerowania żądania certyfikatu, zainstalować certyfikat na komputerze lokalnym, a następnie [wyeksportuj certyfikat PFX](https://technet.microsoft.com/library/cc754329(v=ws.11).aspx).

### <a name="upload-your-ssl-certificate"></a>Przekaż certyfikat protokołu SSL

Aby przekazać certyfikat SSL, kliknij przycisk **certyfikaty SSL** na lewym pasku nawigacyjnym aplikacji sieci web.

Kliknij przycisk **Przekaż certyfikat**. 

W **plik certyfikatu PFX**, wybierz plik w formacie PFX. W **hasło certyfikatu**, wpisz hasło, które utworzono podczas eksportowania pliku PFX.

Kliknij pozycję **Przekaż**.

![Przekazywanie certyfikatu](./media/app-service-web-tutorial-custom-ssl/upload-certificate-private1.png)

Po zakończeniu przekazywania certyfikatu usługi aplikacji był w **certyfikaty SSL** strony.

![Przekazany certyfikat](./media/app-service-web-tutorial-custom-ssl/certificate-uploaded.png)

### <a name="bind-your-ssl-certificate"></a>Powiąż certyfikat protokołu SSL

W **powiązania SSL** kliknij **dodać powiązanie**.

W **Dodaj powiązanie SSL** Użyj listę rozwijaną, aby wybrać nazwę domeny do zabezpieczania i certyfikat do użycia.

> [!NOTE]
> Jeśli zostały przekazane certyfikat, ale nie ma nazwy domeny w **Hostname** listy rozwijanej, spróbuj odświeżyć stronę przeglądarki.
>
>

W **typu SSL**, wybierz, czy ma być używany  **[oznaczenia nazwy serwera (SNI)](http://en.wikipedia.org/wiki/Server_Name_Indication)**  lub opartych na protokole SSL.

- **SSL opartego na protokole SNI** -powiązania SSL opartego na wiele SNI mogą zostać dodane. Ta opcja umożliwia wielu certyfikatów protokołu SSL do zabezpieczania wielu domen na ten sam adres IP. Większość nowoczesnych przeglądarek (w tym programu Internet Explorer, Chrome, Firefox i Opera) obsługuje SNI (znaleźć bardziej szczegółowe informacje pomocy technicznej przeglądarki na [wskaźnika nazwy serwera](http://wikipedia.org/wiki/Server_Name_Indication)).
- **Oparte na protokole SSL** — mogą być dodawane tylko jednego powiązania SSL opartego na protokole IP. Ta opcja umożliwia tylko jeden certyfikat SSL do zabezpieczania dedykowanych publicznego adresu IP. Aby zabezpieczyć wielu domen, należy zabezpieczyć je wszystkie przy użyciu tego samego certyfikatu SSL. Jest to tradycyjne opcja dla powiązania SSL.

Kliknij przycisk **dodać powiązanie**.

![Powiąż certyfikat protokołu SSL](./media/app-service-web-tutorial-custom-ssl/bind-certificate.png)

Po zakończeniu przekazywania certyfikatu usługi aplikacji był w **powiązania SSL** sekcje.

![Certyfikat powiązany z aplikacji sieci web](./media/app-service-web-tutorial-custom-ssl/certificate-bound.png)

## <a name="remap-a-record-for-ip-ssl"></a>Ponowne mapowanie rekord dla protokołu SSL z adresu IP

Jeśli nie używasz opartych na protokole SSL w aplikacji sieci web, przejdź do [HTTPS testu dla domeny niestandardowej](#test).

Domyślnie aplikacja sieci web używa udostępnionego publicznego adresu IP. Gdy Powiąż certyfikat z SSL opartego na protokole IP, usługi aplikacji — tworzy nowy, dedykowany adres IP dla aplikacji sieci web.

Jeśli rekord A zamapowaniu do aplikacji sieci web, należy zaktualizować rejestru domeny ten nowy, dedykowany adres IP.

Aplikacja sieci web **domeny niestandardowe** strona zostanie zaktualizowana nowy, dedykowany adres IP. [Skopiuj ten adres IP](app-service-web-tutorial-custom-domain.md#info), następnie [ponownie zamapować rekord A](app-service-web-tutorial-custom-domain.md#map-an-a-record) ten nowy adres IP.

<a name="test"></a>

## <a name="test-https"></a>Test protokołu HTTPS

Teraz pozostało celu jest aby upewnić się, że HTTPS działa dla domeny niestandardowej. W różnych przeglądarkach, przejdź do `https://<your.custom.domain>` aby zobaczyć, służy ona zapasowej swojej aplikacji sieci web.

![Nawigacji w portalu do aplikacji Azure](./media/app-service-web-tutorial-custom-ssl/app-with-custom-ssl.png)

> [!NOTE]
> Jeśli aplikacja sieci web udostępnia certyfikatu błędy sprawdzania poprawności, prawdopodobnie używasz certyfikatu z podpisem własnym.
>
> Jeśli nie jest wielkość liter, mogą być przechowywane poza certyfikaty pośrednie podczas eksportowania certyfikatu do pliku PFX.

<a name="bkmk_enforce"></a>

## <a name="enforce-https"></a>Wymuszanie protokołu HTTPS

Domyślnie każdy użytkownik nadal dostęp do aplikacji sieci web przy użyciu protokołu HTTP. Można przekierować żądania HTTP z portem HTTPS.

Na stronie sieci web aplikacji, w obszarze nawigacji po lewej stronie, wybierz **domen niestandardowych**. Następnie w **HTTPS tylko**, wybierz pozycję **na**.

![Wymuszanie protokołu HTTPS](./media/app-service-web-tutorial-custom-ssl/enforce-https.png)

Po zakończeniu operacji, przejdź do żadnego z adresów URL protokołu HTTP, który aby wskazywała twoją aplikację. Na przykład:

- `http://<app_name>.azurewebsites.net`
- `http://contoso.com`
- `http://www.contoso.com`

## <a name="automate-with-scripts"></a>Zautomatyzować za pomocą skryptów

Można zautomatyzować powiązań SSL dla aplikacji sieci web za pomocą skryptów przy użyciu [interfejsu wiersza polecenia Azure](/cli/azure/install-azure-cli) lub [programu Azure PowerShell](/powershell/azure/overview).

### <a name="azure-cli"></a>Interfejs wiersza polecenia platformy Azure

Polecenie przekazuje wyeksportowanego pliku PFX i pobiera odcisk palca.

```bash
thumbprint=$(az webapp config ssl upload \
    --name <app_name> \
    --resource-group <resource_group_name> \
    --certificate-file <path_to_PFX_file> \
    --certificate-password <PFX_password> \
    --query thumbprint \
    --output tsv)
```

Polecenie dodaje powiązania SSL opartego na protokole SNI, za pomocą odcisku palca z poprzednie polecenie.

```bash
az webapp config ssl bind \
    --name <app_name> \
    --resource-group <resource_group_name>
    --certificate-thumbprint $thumbprint \
    --ssl-type SNI \
```

### <a name="azure-powershell"></a>Azure PowerShell

Polecenie przekazuje wyeksportowanego pliku PFX i dodaje powiązania SSL opartego na SNI.

```PowerShell
New-AzureRmWebAppSSLBinding `
    -WebAppName <app_name> `
    -ResourceGroupName <resource_group_name> `
    -Name <dns_name> `
    -CertificateFilePath <path_to_PFX_file> `
    -CertificatePassword <PFX_password> `
    -SslState SniEnabled
```
## <a name="public-certificates-optional"></a>Certyfikaty publiczne (opcjonalnie)
Możesz przekazać [certyfikaty publiczne](https://blogs.msdn.microsoft.com/appserviceteam/2017/11/01/app-service-certificates-now-supports-public-certificates-cer/) do aplikacji sieci web. Można również używane certyfikaty publiczne dla aplikacji w środowisku usługi aplikacji. Jeśli potrzebujesz do przechowywania certyfikatu w magazynie LocalMachine certyfikatów, należy użyć aplikacji sieci web w środowisku usługi aplikacji. Aby uzyskać więcej informacji, zobacz [jak skonfigurować certyfikaty publiczne do aplikacji sieci Web](https://blogs.msdn.microsoft.com/appserviceteam/2017/11/01/app-service-certificates-now-supports-public-certificates-cer).

![Przekaż certyfikat publiczny](./media/app-service-web-tutorial-custom-ssl/upload-certificate-public1.png)

## <a name="next-steps"></a>Następne kroki

W niniejszym samouczku zawarto informacje na temat wykonywania następujących czynności:

> [!div class="checklist"]
> * Uaktualnij warstwę cenową aplikacji
> * Powiązać niestandardowego certyfikatu SSL z usługi aplikacji
> * Wymuszanie protokołu HTTPS dla aplikacji
> * Powiązania certyfikatu SSL za pomocą skryptów automatyzacji

Przejdź do następnego samouczkiem, aby dowiedzieć się, jak używać usługi Azure Content Delivery Network.

> [!div class="nextstepaction"]
> [Dodawanie sieci dostarczania zawartości (CDN) w usłudze Azure App Service](app-service-web-tutorial-content-delivery-network.md)

Aby uzyskać więcej informacji, zobacz [używają certyfikatu SSL w kodzie aplikacji w usłudze Azure App Service](app-service-web-ssl-cert-load.md).