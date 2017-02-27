---
title: Klaster Azure Kubernetes dla systemu Windows | Microsoft Docs
description: "Wdrażanie klastra Kubernetes dla kontenerów systemu Windows i rozpoczynanie z nim pracy w usłudze Azure Container Service"
services: container-service
documentationcenter: 
author: dlepow
manager: timlt
editor: 
tags: acs, azure-container-service, kubernetes
keywords: 
ms.assetid: 
ms.service: container-service
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 02/21/2017
ms.author: dlepow
translationtype: Human Translation
ms.sourcegitcommit: 2a381431acb6436ddd8e13c69b05423a33cd4fa6
ms.openlocfilehash: 010a9a4a9ad0f6f7584b1c9a54e665557078d25b


---

# <a name="get-started-with-windows-containers-in-a-kubernetes-cluster"></a>Rozpoczynanie pracy z kontenerami systemu Windows w klastrze Kubernetes


W tym artykule przedstawiono sposób tworzenia w usłudze Azure Container Service klastra Kubernetes, który zawiera węzły systemu Windows umożliwiające uruchamianie kontenerów systemu Windows. 

> [!NOTE]
> Obsługa kontenerów systemu Windows przy użyciu usługi Kubernetes w usłudze Azure Container Service jest dostępna w wersji zapoznawczej. Do tworzenia klastra Kubernetes z węzłami systemu Windows należy użyć witryny Azure Portal lub szablonu usługi Resource Manager. Ta funkcja nie jest obecnie obsługiwana za pomocą interfejsu wiersza polecenia platformy Azure w wersji 2.0.



Na poniższej ilustracji przedstawiono architekturę klastra Kubernetes w usłudze Azure Container Service z jednym węzłem głównym systemu Linux i dwoma węzłami agenta systemu Windows. 

* Węzeł główny systemu Linux obsługuje interfejs API REST usługi Kubernetes i jest dostępny za pośrednictwem protokołu SSH w porcie 22 lub za pomocą narzędzia `kubectl` w porcie 443. 
* Węzły agenta systemu Windows są zgrupowane w zestawie dostępności platformy Azure i odpowiadają za uruchamianie kontenerów. Węzły systemu Windows są dostępne przez tunel protokołu RDP SSH za pośrednictwem węzła głównego. Reguły usługi Azure Load Balancer są dodawane dynamicznie do klastra w zależności od uwidocznionych usług.


![Obraz przedstawiający klaster Kubernetes na platformie Azure](media/container-service-kubernetes-windows-walkthrough/kubernetes-windows.png)

Wszystkie maszyny wirtualne znajdują w tej samej prywatnej sieci wirtualnej i są dla siebie w pełni dostępne. Wszystkie maszyny wirtualne uruchamiają agenta kubelet, usługę Docker i serwer proxy.

## <a name="prerequisites"></a>Wymagania wstępne


* **Klucz publiczny SSH RSA**: podczas wdrażania za pośrednictwem portalu lub jednego z szablonów szybkiego startu platformy Azure musisz podać klucz publiczny SSH RSA na potrzeby uwierzytelniania na maszynach wirtualnych usługi Azure Container Service. Aby utworzyć klucze RSA Secure Shell (SSH), zobacz wskazówki dla systemów [OS X i Linux](../virtual-machines/virtual-machines-linux-mac-create-ssh-keys.md) lub [Windows](../virtual-machines/virtual-machines-linux-ssh-from-windows.md). 

* **Wpis tajny i identyfikator klienta nazwy głównej usługi**: aby uzyskać więcej informacji i wskazówek, zobacz [About the service principal for a Kubernetes cluster](container-service-kubernetes-service-principal.md) (Informacje o nazwie głównej usługi dla klastra Kubernetes).




## <a name="create-the-cluster"></a>Tworzenie klastra

Do [tworzenia klastra Kubernetes](container-service-deployment.md#create-a-cluster-by-using-the-azure-portal) z węzłami agenta systemu Windows można użyć witryny Azure Portal. 

W okienku **Konfiguracja platformy** w obszarze **Konfiguracja programu Orchestrator** wybierz opcję **Kubernetes — Windows**. 

## <a name="connect-to-the-cluster"></a>Łączenie z klastrem

Użyj narzędzie wiersza polecenia `kubectl`, aby nawiązać połączenie z komputera lokalnego do węzła głównego klastra Kubernetes. Aby zapoznać się z czynnościami instalowania i konfigurowania narzędzia `kubectl`, zobacz [Łączenie z klastrem usługi Azure Container Service](container-service-connect.md#connect-to-a-kubernetes-cluster). Poleceń `kubectl` możesz użyć w celu uzyskania dostępu do interfejsu użytkownika sieci Web usługi Kubernetes oraz w celu tworzenia obciążeń kontenerów systemu Windows i zarządzania nimi.

## <a name="create-your-first-kubernetes-service"></a>Tworzenie pierwszej usługi Kubernetes

Po utworzeniu klastra i nawiązaniu połączenia z narzędziem `kubectl` możesz spróbować uruchomić podstawową aplikację sieci Web systemu Windows i uwidocznić ją w Internecie. W tym przykładzie określasz zasoby kontenera przy użyciu pliku YAML, a następnie tworzysz go za pomocą narzędzia `kubctl apply`.

1. Aby wyświetlić listę węzłów, wpisz ciąg `kubectl get nodes`.  Jeśli chcesz wyświetlić wszystkie szczegóły węzłów, wpisz:  

  ```
  kubectl get nodes -o yaml
  ```

2. Utwórz plik o nazwie `simpleweb.yaml` i skopiuj następujące dane. Ten plik konfiguruje aplikację sieci Web za pomocą podstawowego obrazu systemu operacyjnego Windows Server 2016 Server Core z usługi [Docker Hub](https://hub.docker.com/r/microsoft/windowsservercore/).  

  ```yaml
  apiVersion: v1
  kind: Service
  metadata:
    name: win-webserver
    labels:
      app: win-webserver
  spec:
    ports:
      # the port that this service should serve on
    - port: 80
      targetPort: 80
    selector:
      app: win-webserver
    type: LoadBalancer
  ---
  apiVersion: extensions/v1beta1
  kind: Deployment
  metadata:
    labels:
      app: win-webserver
    name: win-webserver
  spec:
    replicas: 1
    template:
      metadata:
        labels:
          app: win-webserver
        name: win-webserver
      spec:
        containers:
        - name: windowswebserver
          image: microsoft/windowsservercore
          command:
          - powershell.exe
          - -command
          - "<#code used from https://gist.github.com/wagnerandrade/5424431#> ; $$ip = (Get-NetIPAddress | where {$$_.IPAddress -Like '*.*.*.*'})[0].IPAddress ; $$url = 'http://'+$$ip+':80/' ; $$listener = New-Object System.Net.HttpListener ; $$listener.Prefixes.Add($$url) ; $$listener.Start() ; $$callerCounts = @{} ; Write-Host('Listening at {0}...' -f $$url) ; while ($$listener.IsListening) { ;$$context = $$listener.GetContext() ;$$requestUrl = $$context.Request.Url ;$$clientIP = $$context.Request.RemoteEndPoint.Address ;$$response = $$context.Response ;Write-Host '' ;Write-Host('> {0}' -f $$requestUrl) ;  ;$$count = 1 ;$$k=$$callerCounts.Get_Item($$clientIP) ;if ($$k -ne $$null) { $$count += $$k } ;$$callerCounts.Set_Item($$clientIP, $$count) ;$$header='<html><body><H1>Windows Container Web Server</H1>' ;$$callerCountsString='' ;$$callerCounts.Keys | % { $$callerCountsString+='<p>IP {0} callerCount {1} ' -f $$_,$$callerCounts.Item($$_) } ;$$footer='</body></html>' ;$$content='{0}{1}{2}' -f $$header,$$callerCountsString,$$footer ;Write-Output $$content ;$$buffer = [System.Text.Encoding]::UTF8.GetBytes($$content) ;$$response.ContentLength64 = $$buffer.Length ;$$response.OutputStream.Write($$buffer, 0, $$buffer.Length) ;$$response.Close() ;$$responseStatus = $$response.StatusCode ;Write-Host('< {0}' -f $$responseStatus)  } ; "
        nodeSelector:
          beta.kubernetes.io/os: windows
  ```

3. Aby uruchomić aplikację, wpisz:

  ```
  kubectl apply -f simpleweb.yaml
  ```
  
  > [!NOTE] 
  > Konfiguracja obejmuje element `type: LoadBalancer`. To ustawienie powoduje uwidocznienie usługi w Internecie za pośrednictwem usługi Azure Load Balancer. Aby uzyskać więcej informacji, zobacz [Load balance containers in a Kubernetes cluster in Azure Container Service](container-service-kubernetes-load-balancing.md) (Kontenery równoważenia obciążenia w klastrze Kubernetes w usłudze Azure Container Service).
  
4. Aby zweryfikować wdrożenie usługi (co zajmie około 30 sekund), wpisz:

  ```
  kubectl get pods
  ```

5. Aby wyświetlić wewnętrzne i zewnętrzne adresy IP usługi po jej uruchomieniu, wpisz:

  ```
  kubectl get svc
  ``` 

  ![Adresy IP usługi systemu Windows](media/container-service-kubernetes-windows-walkthrough/externalipa.png)

  Dodanie zewnętrznego adresu IP trwa kilka minut. Zanim moduł równoważenia obciążenia skonfiguruje adres zewnętrzny, jest on wyświetlany jako `<pending>`.


6. Po udostępnieniu zewnętrznego adresu IP można uzyskać dostęp do usługi w przeglądarce sieci Web.

  ![Aplikacja serwera systemu Windows w przeglądarce](media/container-service-kubernetes-windows-walkthrough/wincontainerwebserver.png)


## <a name="access-the-windows-nodes"></a>Dostęp do węzłów systemu Windows
Dostęp do węzłów systemu Windows można uzyskać z komputera lokalnego z systemem Windows za pomocą połączenia pulpitu zdalnego. Zalecamy używanie tunelu protokołu RDP SSH za pośrednictwem węzła głównego. 

Istnieje wiele opcji tworzenia tuneli SSH w systemie Windows. W tej sekcji opisano sposób tworzenia tunelu przy użyciu programu PuTTY.

1. [Pobierz program PuTTY](http://www.chiark.greenend.org.uk/~sgtatham/putty/download.html) do systemu Windows.

2. Uruchom aplikację.

3. Wprowadź nazwę hosta złożoną z nazwy użytkownika administratora klastra i publicznej nazwy DNS pierwszego serwera głównego w klastrze. **Nazwa hosta** wygląda podobnie do `adminuser@PublicDNSName`. W polu **Port** wprowadź wartość 22.

    ![Konfiguracja programu PuTTY 1](media/container-service-kubernetes-windows-walkthrough/putty1.png)

4. Wybierz pozycje **SSH > Uwierzytelnianie**. Dodaj ścieżkę do pliku klucza prywatnego (format ppk) na potrzeby uwierzytelniania. Możesz użyć narzędzia takiego jak [PuTTYgen](http://www.chiark.greenend.org.uk/~sgtatham/putty/download.html), aby wygenerować ten plik z klucza SSH użytego do utworzenia klastra.

    ![Konfiguracja programu PuTTY 2](media/container-service-kubernetes-windows-walkthrough/putty2.png)

5. Wybierz pozycje **SSH > Tunele** i skonfiguruj przekazane porty. Ponieważ maszyna lokalna z systemem Windows już używa portu 3389, w celu uzyskania dostępu do węzła 0 systemu Windows i węzła 1 systemu Windows zalecane jest użycie poniższych ustawień. (Używaj tego wzorca również w przypadku dodatkowych węzłów systemu Windows).

  **Węzeł 0 systemu Windows**

  * **Port źródłowy:** 3390
  * **Miejsce docelowe:** 10.240.245.5:3389

  **Węzeł 1 systemu Windows**

  * **Port źródłowy:** 3391
  * **Miejsce docelowe:** 10.240.245.6:3389

  ![Obraz tuneli protokołu RDP systemu Windows](media/container-service-kubernetes-windows-walkthrough/rdptunnels.png)

6. Po zakończeniu kliknij pozycje **Sesja > Zapisz**, aby zapisać konfigurację połączenia.

7. Aby nawiązać połączenie z sesją programu PuTTY, kliknij pozycję **Otwórz**. Ukończ łączenie z węzłem głównym.

8. Uruchom funkcję Podłączanie pulpitu zdalnego. Aby nawiązać połączenie z pierwszym węzłem systemu Windows, dla pozycji **Komputer** ustaw wartość `localhost:3390` i kliknij pozycję **Połącz**. (Aby nawiązać połączenie z drugim węzłem, ustaw wartość `localhost:3390` itd.). Aby zakończyć połączenie, podaj hasło lokalnego administratora systemu Windows skonfigurowane podczas wdrażania.








## <a name="next-steps"></a>Następne kroki

Oto zalecane linki do dalszych informacji na temat usługi Kubernetes:

* Scenariusz [Kubernetes Bootcamp](https://kubernetesbootcamp.github.io/kubernetes-bootcamp/index.html) — pokazuje, jak wdrażać, skalować, aktualizować i debugować aplikacje konteneryzowane.
* [Podręcznik użytkownika rozwiązania Kubernetes](http://kubernetes.io/docs/user-guide/) — zawiera informacje na temat uruchamiania programów w istniejącym klastrze Kubernetes.
* [Przykłady dotyczące rozwiązania Kubernetes](https://github.com/kubernetes/kubernetes/tree/master/examples) — przykłady związane z uruchamianiem prawdziwych aplikacji przy użyciu rozwiązania Kubernetes.


<!--HONumber=Feb17_HO4-->


