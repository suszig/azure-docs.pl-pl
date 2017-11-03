---
title: "Konfigurowanie protokołu SSL dla usługi w chmurze (klasyczne) | Dokumentacja firmy Microsoft"
description: "Dowiedz się, jak określić punkt końcowy HTTPS dla roli sieci web i jak można przekazać certyfikatu SSL do zabezpieczania aplikacji."
services: cloud-services
documentationcenter: .net
author: Thraka
manager: timlt
editor: 
ms.assetid: 4cbb7f38-7994-454d-b4f0-7259b558c766
ms.service: cloud-services
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/14/2016
ms.author: adegeo
ms.openlocfilehash: 7df2371f64f0d8a9fabc0df37c17d4dfbc47cd7e
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/11/2017
---
# <a name="configuring-ssl-for-an-application-in-azure"></a>Konfigurowanie protokołu SSL dla aplikacji na platformie Azure
> [!div class="op_single_selector"]
> * [Witryna Azure Portal](cloud-services-configure-ssl-certificate-portal.md)
> * [Klasyczna witryna Azure Portal](cloud-services-configure-ssl-certificate.md)
> 
> 

Szyfrowanie SSL (Secure Socket Layer) to najczęściej stosowana metoda zabezpieczania danych wysyłanych przez Internet. W ramach tego często wykonywanego zadania omówiono sposób określania punktu końcowego HTTPS dla roli sieci Web oraz przekazywania certyfikatu SSL w celu zabezpieczenia aplikacji.

> [!NOTE]
> Procedury w tym zadaniu mają zastosowanie do usług Azure Cloud Services; dla usług aplikacji, zobacz [to](../app-service/app-service-web-tutorial-custom-ssl.md) artykułu.
> 
> 

To zadanie używa wdrożenia produkcyjnego. Informacji na temat używania tymczasowej wdrożenia znajduje się na końcu tego tematu.

Odczyt [to](cloud-services-how-to-create-deploy.md) najpierw artykułu, jeśli nie utworzono jeszcze usługi w chmurze.

## <a name="step-1-get-an-ssl-certificate"></a>Krok 1: Uzyskiwanie certyfikatu SSL.
Aby skonfigurować protokół SSL dla aplikacji, należy najpierw uzyskać certyfikat SSL, który został podpisany przez urząd certyfikacji, zaufanych innej firmy, który wystawia certyfikaty w tym celu. Jeśli nie masz już jeden, należy uzyskać go z firmą, która sprzedaje certyfikatów SSL.

Certyfikat musi spełniać następujące wymagania dotyczące certyfikatów SSL na platformie Azure:

* Certyfikat musi zawierać klucz prywatny.
* Certyfikat musi zostać utworzony dla wymiany kluczy, można eksportować do pliku wymiany informacji osobistych (pfx).
* Nazwa podmiotu certyfikatu musi odpowiadać domeny używane do uzyskania dostępu do usługi w chmurze. Nie można uzyskać certyfikatu SSL z urzędu certyfikacji (CA) dla domeny cloudapp.net. Należy uzyskać niestandardowej nazwy domeny do użycia podczas połączenia się z usługą. Podczas żądania certyfikatu z urzędu certyfikacji, nazwa podmiotu certyfikatu musi odpowiadać nazwie domeny niestandardowej, umożliwiające dostęp do aplikacji. Na przykład jeśli nazwą domeny niestandardowej jest **contoso.com** może zażądać certyfikatu od urzędu certyfikacji dla ***. contoso.com** lub **www.contoso.com**.
* Certyfikat należy użyć co najmniej 2048-bitowego szyfrowania.

Do celów testowych możesz [utworzyć](cloud-services-certs-create.md) i użycia certyfikatu z podpisem własnym. Certyfikatu z podpisem własnym nie jest uwierzytelniony za pośrednictwem urzędu certyfikacji i może używać domeny cloudapp.net jako adres URL witryny sieci Web. Na przykład następujące zadanie używa certyfikatu z podpisem własnym jest nazwa pospolita (CN) użyta w certyfikacie **sslexample.cloudapp.net**.

Następnie musi zawierać informacje o certyfikacie w definicji usługi oraz pliki konfiguracji usługi.

## <a name="step-2-modify-the-service-definition-and-configuration-files"></a>Krok 2: Modyfikowanie plików definicji i konfigurację usługi
Aplikacji musi być skonfigurowana do używania certyfikatu, a punkt końcowy HTTPS muszą zostać dodane. W związku z tym definicji usługi i pliki konfiguracyjne usługi muszą zostać zaktualizowane.

1. W środowisku projektowania, otwórz plik definicji usługi (CSDEF), Dodaj **certyfikaty** sekcji w **sieć Web** sekcji i obejmują następujące informacje dotyczące certyfikatu (i certyfikaty pośrednie):
   
    ```xml
    <WebRole name="CertificateTesting" vmsize="Small">
    ...
        <Certificates>
            <Certificate name="SampleCertificate" 
                        storeLocation="LocalMachine" 
                        storeName="My"
                        permissionLevel="limitedOrElevated" />
            <!-- IMPORTANT! Unless your certificate is either
            self-signed or signed directly by the CA root, you
            must include all the intermediate certificates
            here. You must list them here, even if they are
            not bound to any endpoints. Failing to list any of
            the intermediate certificates may cause hard-to-reproduce
            interoperability problems on some clients.-->
            <Certificate name="CAForSampleCertificate"
                        storeLocation="LocalMachine"
                        storeName="CA"
                        permissionLevel="limitedOrElevated" />
        </Certificates>
    ...
    </WebRole>
    ```
   
   **Certyfikaty** sekcja definiuje nazwę naszych certyfikatu, jego lokalizację i nazwę magazynu, w którym znajduje się.
   
   Uprawnienia (`permisionLevel` atrybut) może być ustawione na jedną z następujących wartości:
   
   | Wartość uprawnienia | Opis |
   | --- | --- |
   | limitedOrElevated |**(Ustawienie domyślne)**  Wszystkie procesy roli można uzyskać dostępu do klucza prywatnego. |
   | z podwyższonym poziomem uprawnień |Tylko procesów z podwyższonym poziomem uprawnień można uzyskać dostępu do klucza prywatnego. |
2. W pliku definicji usługi, dodać **InputEndpoint** w elemencie **punkty końcowe** sekcji, aby włączyć protokół HTTPS:
   
    ```xml
    <WebRole name="CertificateTesting" vmsize="Small">
    ...
        <Endpoints>
            <InputEndpoint name="HttpsIn" protocol="https" port="443" 
                certificate="SampleCertificate" />
        </Endpoints>
    ...
    </WebRole>
    ```

3. W pliku definicji usługi, dodać **powiązanie** w elemencie **witryny** sekcji. W tej sekcji dodaje powiązanie HTTPS do mapowania punktu końcowego do swojej witryny:
   
    ```xml   
    <WebRole name="CertificateTesting" vmsize="Small">
    ...
        <Sites>
            <Site name="Web">
                <Bindings>
                    <Binding name="HttpsIn" endpointName="HttpsIn" />
                </Bindings>
            </Site>
        </Sites>
    ...
    </WebRole>
    ```
   
   Wszystkie wymagane zmiany w pliku definicji usługi zostały ukończone, ale nadal konieczne jest dodanie informacji o certyfikacie w pliku konfiguracji usługi.
4. W pliku konfiguracji usługi (CSCFG), ServiceConfiguration.Cloud.cscfg, Dodaj **certyfikaty** sekcji w **roli** sekcji, zastępując wartość odcisku palca przykładowych pokazano poniżej, z którego certyfikatu:
   
    ```xml   
    <Role name="Deployment">
    ...
        <Certificates>
            <Certificate name="SampleCertificate" 
                thumbprint="9427befa18ec6865a9ebdc79d4c38de50e6316ff" 
                thumbprintAlgorithm="sha1" />
            <Certificate name="CAForSampleCertificate"
                thumbprint="79d4c38de50e6316ff9427befa18ec6865a9ebdc" 
                thumbprintAlgorithm="sha1" />
        </Certificates>
    ...
    </Role>
    ```

(W poprzednim przykładzie użyto **sha1** dla algorytmu odcisk palca. Określ wartość odpowiednią dla algorytmu odcisk palca certyfikatu).

Teraz, definicji usługi i pliki konfiguracji usługi zostały zaktualizowane, pakiet wdrożenia do przekazywania do platformy Azure. Jeśli używasz **cspack**, nie używaj **/generateConfigurationFile** Flaga, jako który zastępuje wstawiono informacji o certyfikacie.

## <a name="step-3-upload-a-certificate"></a>Krok 3: Przekaż certyfikat
Pakiet wdrażania została zaktualizowana do używania certyfikatu, a dodano punkt końcowy HTTPS. Możesz teraz przekazać certyfikatów i pakietów do platformy Azure z klasycznego portalu Azure.

1. Zaloguj się do [klasycznego portalu Azure][Azure classic portal]. 
2. Kliknij przycisk **usługi w chmurze** w okienku nawigacji po lewej stronie.
3. Kliknij usługę w chmurze żądany.
4. Kliknij przycisk **certyfikaty** kartę.
   
    ![Kliknij kartę certyfikaty](./media/cloud-services-configure-ssl-certificate/click-cert.png)

5. Kliknij przycisk **Przekaż**.
   
    ![Upload](./media/cloud-services-configure-ssl-certificate/upload-button.png)
    
6. Podaj **pliku**, **hasło**, następnie kliknij przycisk **Complete** (znacznikiem wyboru).

## <a name="step-4-connect-to-the-role-instance-by-using-https"></a>Krok 4: Łączenie z wystąpienia roli przy użyciu protokołu HTTPS
Teraz, że wdrożenie jest uruchomiona na platformie Azure, można połączyć się przy użyciu protokołu HTTPS.

1. W klasycznym portalu Azure, wybierz wdrożenie, a następnie kliknij łącze w obszarze **adres URL witryny**.
   
   ![Określić adres URL witryny][2]
2. W przeglądarce sieci web Zmień łącze do użycia **https** zamiast **http**, a następnie odwiedź stronę.
   
   > [!NOTE]
   > Jeśli używasz certyfikatu z podpisem własnym, po przejściu do punktu końcowego protokołu HTTPS, który został skojarzony z certyfikatu z podpisem własnym może zostać wyświetlony błąd certyfikatu w przeglądarce. Przy użyciu certyfikatu podpisanego przez zaufany urząd certyfikacji eliminuje ten problem. w międzyczasie możesz zignorować błąd. (Inną opcją jest można dodać certyfikatu z podpisem własnym do magazynu certyfikatów urzędu zaufanego certyfikatu użytkownika).
   > 
   > 
   
   ![Przykład witryny sieci web protokołu SSL][3]

Jeśli chcesz używać protokołu SSL dla tymczasowej wdrożenia zamiast wdrożenia produkcyjnego, należy najpierw ustalić adresu URL używany do wdrażania przejściowego. Wdrażanie usługi w chmurze do środowiska pomostowego, bez uwzględniania certyfikatu lub żadnych informacji o certyfikacie. Po wdrożeniu, można określić adres URL na podstawie identyfikatora GUID, która jest wyświetlana w klasycznym portalu Azure w **adres URL witryny** pola. Utwórz certyfikat z nazwa pospolita (CN) taki sam adres URL na podstawie identyfikatora GUID (na przykład **32818777-6e77-4ced-a8fc-57609d404462.cloudapp.net**). Użyj klasycznego portalu Azure można dodać certyfikatu do usługi w chmurze przemieszczane. Następnie dodaj informacje o certyfikacie do pliki CSDEF i CSCFG, ponownie utworzyć pakiet aplikacji i aktualizacji przemieszczanego wdrożenia do użycia nowego pakietu.

## <a name="next-steps"></a>Następne kroki
* [Konfiguracja ogólna usługi w chmurze](cloud-services-how-to-configure.md).
* Dowiedz się, jak [wdrażania usługi w chmurze](cloud-services-how-to-create-deploy.md).
* Skonfiguruj [niestandardowej nazwy domeny](cloud-services-custom-domain-name.md).
* [Usługi w chmurze zarządzanie](cloud-services-how-to-manage.md).

[Azure classic portal]: http://manage.windowsazure.com
[0]: ./media/cloud-services-configure-ssl-certificate/CreateCloudService.png
[1]: ./media/cloud-services-configure-ssl-certificate/AddCertificate.png
[2]: ./media/cloud-services-configure-ssl-certificate/CopyURL.png
[3]: ./media/cloud-services-configure-ssl-certificate/SSLCloudService.png
[4]: ./media/cloud-services-configure-ssl-certificate/AddCertificateComplete.png  
