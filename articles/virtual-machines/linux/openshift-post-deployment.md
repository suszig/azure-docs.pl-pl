---
title: "OpenShift na zadania wdrażania Azure Post | Dokumentacja firmy Microsoft"
description: "Zadania wdrażania OpenShift Post"
services: virtual-machines-linux
documentationcenter: virtual-machines
author: haroldw
manager: najoshi
editor: 
tags: azure-resource-manager
ms.assetid: 
ms.service: virtual-machines-linux
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 
ms.author: haroldw
ms.openlocfilehash: 12e6785358f5f412326418b0c64eeaeabdaa3b5f
ms.sourcegitcommit: b979d446ccbe0224109f71b3948d6235eb04a967
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/25/2017
---
# <a name="post-deployment-tasks"></a>Zadania po wdrożeniu

Po wdrożeniu klastra OpenShift są dodatkowe elementy, które można skonfigurować. W tym artykule opisano następujące czynności:

- Konfigurowanie rejestracji jednokrotnej przy użyciu usługi Azure Active Directory (AAD)
- Skonfiguruj OMS do monitorowania OpenShift
- Konfigurowanie metryki i rejestrowanie

## <a name="single-sign-on-using-aad"></a>Rejestracja jednokrotna przy użyciu usługi AAD

Aby można było używać usługi AAD do uwierzytelniania, najpierw należy utworzyć rejestracji aplikacji Azure AD. Ten proces będzie obejmować należy wykonać dwie czynności — Tworzenie rejestracji aplikacji, a następnie skonfigurować uprawnienia.

### <a name="create-app-registration"></a>Tworzenie rejestracji aplikacji

Aby utworzyć rejestracji aplikacji i graficznego interfejsu użytkownika (Portal), aby ustawić uprawnienia użyjemy wiersza polecenia platformy Azure. Aby utworzyć rejestracji aplikacji, będą potrzebne pięciu sztuk informacji.

- Nazwa wyświetlana: Nazwa rejestracji aplikacji (przykład: OCPAzureAD)
- Strona główna: Adres URL konsoli OpenShift (przykład: https://masterdns343khhde.westus.cloudapp.azure.com:8443/konsoli)
- Identyfikatora URI: Adres URL konsoli OpenShift (przykład: https://masterdns343khhde.westus.cloudapp.azure.com:8443/konsoli)
- Adres URL odpowiedzi: Wzorca publiczny adres URL i nazwa rejestracji aplikacji (przykład: https://masterdns343khhde.westus.cloudapp.azure.com:8443/oauth2callback/OCPAzureAD)
- Hasło: Bezpieczne hasło (Użyj silnego hasła)

Poniższy przykład tworzy rejestracji aplikacji, korzystając z informacji z powyżej.

```azurecli
az ad app create --display-name OCPAzureAD --homepage https://masterdns343khhde.westus.cloudapp.azure.com:8443/console --reply-urls https://masterdns343khhde.westus.cloudapp.azure.com:8443/oauth2callback/hwocpadint --identifier-uris https://masterdns343khhde.westus.cloudapp.azure.com:8443/console --password {Strong Password}
```

Jeśli zakończyło się pomyślnie, możesz uzyskać dane wyjściowe JSON podobne do:

```json
{
  "appId": "12345678-ca3c-427b-9a04-ab12345cd678",
  "appPermissions": null,
  "availableToOtherTenants": false,
  "displayName": "OCPAzureAD",
  "homepage": "https://masterdns343khhde.westus.cloudapp.azure.com:8443/console",
  "identifierUris": [
    "https://masterdns343khhde.westus.cloudapp.azure.com:8443/console"
  ],
  "objectId": "62cd74c9-42bb-4b9f-b2b5-b6ee88991c80",
  "objectType": "Application",
  "replyUrls": [
    "https://masterdns343khhde.westus.cloudapp.azure.com:8443/oauth2callback/OCPAzureAD"
  ]
}
```

Zwróć uwagę na właściwość appId zwrócił polecenia dla kolejnych kroków.

W **portalu Azure**:

1.  Wybierz **usługi Azure Active Directory** --> **rejestracji aplikacji**
2.  Wyszukiwanie rejestracji aplikacji (przykład: OCPAzureAD)
3.  W wynikach kliknij przycisk rejestracji aplikacji
4.  W bloku ustawienia, wybierz **wymagane uprawnienia**
5.  W bloku wymagane uprawnienia, kliknij przycisk **Dodaj**

  ![Rejestracja aplikacji](media/openshift-post-deployment/app-registration.png)

6.  Kliknij krok 1: Wybierz interfejsu API, a następnie kliknij przycisk **Windows Azure Active Directory (Microsoft.Azure.ActiveDirectory)** i kliknij przycisk **wybierz** u dołu.

  ![Rejestracja aplikacji interfejsu API](media/openshift-post-deployment/app-registration-select-api.png)

7.  W kroku 2: Wybierz uprawnienia, wybierz pozycję **Zaloguj się i odczytuj profil użytkownika** w obszarze **delegowane uprawnienia** i kliknij przycisk **wybierz**

  ![Dostęp do rejestracji aplikacji](media/openshift-post-deployment/app-registration-access.png)

8.  Kliknij przycisk **gotowe**

### <a name="configure-openshift-for-azure-ad-authentication"></a>Konfigurowanie OpenShift uwierzytelniania usługi Azure AD

Aby skonfigurować OpenShift przy użyciu usługi Azure AD jako dostawcy uwierzytelniania **/etc/origin/master/master-config.yaml** należy edytować plik we wszystkich węzłach wzorca.

Identyfikator dzierżawcy można znaleźć przy użyciu interfejsu wiersza polecenia następujące polecenie:

```azurecli
az account show
```

W pliku yaml programu Znajdź następujące wiersze:

```yaml
oauthConfig:
  assetPublicURL: https://masterdns343khhde.westus.cloudapp.azure.com:8443/console/
  grantConfig:
    method: auto
  identityProviders:
  - challenge: true
    login: true
    mappingMethod: claim
    name: htpasswd_auth
    provider:
      apiVersion: v1
      file: /etc/origin/master/htpasswd
      kind: HTPasswdPasswordIdentityProvider
```

Wstaw następujące wiersze natychmiast po wierszu powyżej:

```yaml
  - name: <App Registration Name>
    challenge: false
    login: true
    mappingMethod: claim
    provider:
      apiVersion: v1
      kind: OpenIDIdentityProvider
      clientID: <appId>
      clientSecret: <Strong Password>
      claims:
        id:
        - sub
        preferredUsername:
        - unique_name
        name:
        - name
        email:
        - email
      urls:
        authorize: https://login.microsoftonline.com/<tenant Id>/oauth2/authorize
        token: https://login.microsoftonline.com/<tenant Id>/oauth2/token
```

Identyfikator dzierżawcy można znaleźć przy użyciu interfejsu wiersza polecenia następujące polecenie:```az account show```

Uruchom ponownie usługi OpenShift Master we wszystkich węzłach wzorca

**OpenShift Origin**

```bash
sudo systemctl restart origin-master-api
sudo systemctl restart origin-master-controllers
```

**Platforma kontenera OpenShift z wieloma wzorcami**

```bash
sudo systemctl restart atomic-openshift-master-api
sudo systemctl restart atomic-openshift-master-controllers
```

**Platforma kontenera OpenShift z pojedynczym wzorcem**

```bash
sudo systemctl restart atomic-openshift-master
```

W konsoli OpenShift teraz zostaną wyświetlone dwie opcje dla uwierzytelniania - htpasswd_auth i **[rejestracji aplikacji]**.

## <a name="monitor-openshift-with-oms"></a>Monitor OpenShift z OMS

Monitorowanie OpenShift z OMS można osiągnąć przy użyciu jednej z dwóch opcji - instalacji Agent pakietu OMS na hoście maszyny Wirtualnej lub kontener OMS. Ten artykuł zawiera instrukcje dotyczące wdrażania kontenera OMS.

## <a name="create-an-openshift-project-for-oms-and-set-user-access"></a>Tworzenie projektu OpenShift dla OMS i ustaw dostępu użytkownika

```bash
oadm new-project omslogging --node-selector='zone=default'
oc project omslogging
oc create serviceaccount omsagent
oadm policy add-cluster-role-to-user cluster-reader system:serviceaccount:omslogging:omsagent
oadm policy add-scc-to-user privileged system:serviceaccount:omslogging:omsagent
```

## <a name="create-daemon-set-yaml-file"></a>Tworzenie pliku yaml programu zestawu demon

Utwórz plik o nazwie ocp omsagent.yml.

```yaml
apiVersion: extensions/v1beta1
kind: DaemonSet
metadata:
  name: oms
spec:
  selector:
    matchLabels:
      name: omsagent
  template:
    metadata:
      labels:
        name: omsagent
        agentVersion: 1.4.0-45
        dockerProviderVersion: 10.0.0-25
    spec:
      nodeSelector:
        zone: default
      serviceAccount: omsagent
      containers:
      - image: "microsoft/oms"
        imagePullPolicy: Always
        name: omsagent
        securityContext:
          privileged: true
        ports:
        - containerPort: 25225
          protocol: TCP
        - containerPort: 25224
          protocol: UDP
        volumeMounts:
        - mountPath: /var/run/docker.sock
          name: docker-sock
        - mountPath: /etc/omsagent-secret
          name: omsagent-secret
          readOnly: true
        livenessProbe:
          exec:
            command:
              - /bin/bash
              - -c
              - ps -ef | grep omsagent | grep -v "grep"
          initialDelaySeconds: 60
          periodSeconds: 60
      volumes:
      - name: docker-sock
        hostPath:
          path: /var/run/docker.sock
      - name: omsagent-secret
        secret:
         secretName: omsagent-secret
````

## <a name="create-secret-yaml-file"></a>Utwórz plik tajny yaml programu

Aby utworzyć plik tajny yaml programu, będą potrzebne informacje - OMS identyfikator i klucz udostępniony obszar roboczy OMS. 

Przykładowy plik ocp secret.yml 

```yaml
apiVersion: v1
kind: Secret
metadata:
  name: omsagent-secret
data:
  WSID: wsid_data
  KEY: key_data
```

Zamień wsid_data z Base64 kodowany identyfikator obszaru roboczego OMS i Zastąp key_data z Base64 kodowany klucz udostępniony obszar roboczy OMS.

```bash
wsid_data='11111111-abcd-1111-abcd-111111111111'
key_data='My Strong Password'
echo $wsid_data | base64 | tr -d '\n'
echo $key_data | base64 | tr -d '\n'
```

## <a name="create-secret-and-daemon-set"></a>Utwórz klucz tajny i demon zestawu

Wdróż plik za pomocą tajny

```bash
oc create -f ocp-secret.yml
```

Wdrożenie zestawu demon Agent pakietu OMS

```bash
oc create -f ocp-omsagent.yml
```

## <a name="configure-metrics-and-logging"></a>Konfiguruj rejestrowanie i metryki

Szablon Menedżera zasobów OpenShift kontenera platformy (OCP) zawiera parametry wejściowe włączenie metryk i rejestrowania. Nie ma szablonu OpenShift kontenera platformy Marketplace oferują i OpenShift Origin Resource Manager.

Jeśli użyto szablonu OCP Resource Manager i metryki i rejestrowanie nie zostały włączone na czas instalacji lub użyto oferta portalu Marketplace OCP, te można łatwo włączyć po fakcie. Jeśli przy użyciu szablonu usługi Resource Manager pochodzenia OpenShift niektórych wstępne pracy jest wymagana.

### <a name="openshift-origin-template-pre-work"></a>Pracy przed pochodzenia OpenShift szablonu

SSH używa portu 2200 pierwszego węzła głównego

Przykład

```bash
ssh -p 2200 clusteradmin@masterdnsixpdkehd3h.eastus.cloudapp.azure.com 
```

Edytuj **pliku /etc/ansible/hosts** i dodaj następujące wiersze po sekcji dostawcy tożsamości (# włączyć HTPasswdPasswordIdentityProvider)

```yaml
# Setup metrics
openshift_hosted_metrics_deploy=false
openshift_metrics_cassandra_storage_type=dynamic
openshift_metrics_start_cluster=true
openshift_metrics_hawkular_nodeselector={"type":"infra"}
openshift_metrics_cassandra_nodeselector={"type":"infra"}
openshift_metrics_heapster_nodeselector={"type":"infra"}
openshift_hosted_metrics_public_url=https://metrics.$ROUTING/hawkular/metrics

# Setup logging
openshift_hosted_logging_deploy=false
openshift_hosted_logging_storage_kind=dynamic
openshift_logging_fluentd_nodeselector={"logging":"true"}
openshift_logging_es_nodeselector={"type":"infra"}
openshift_logging_kibana_nodeselector={"type":"infra"}
openshift_logging_curator_nodeselector={"type":"infra"}
openshift_master_logging_public_url=https://kibana.$ROUTING
```

Zamień $ROUTING ciąg używany do **openshift_master_default_subdomain** opcji, w tym samym **/etc/ansible/hosts** pliku.

### <a name="azure-cloud-provider-in-use"></a>Dostawca usługi Azure Cloud w użyciu

Na pierwszy węzeł główny (źródło) lub bastionu węzła (OCP), protokół SSH za pomocą poświadczeń podana podczas wdrażania. Należy wydać następujące polecenie:

```bash
ansible-playbook /usr/share/ansible/openshift-ansible/playbooks/byo/openshift-cluster/openshift-metrics.yml \
-e openshift_metrics_install_metrics=True \
-e openshift_metrics_cassandra_storage_type=dynamic

ansible-playbook /usr/share/ansible/openshift-ansible/playbooks/byo/openshift-cluster/openshift-logging.yml \
-e openshift_logging_install_logging=True \
-e openshift_hosted_logging_storage_kind=dynamic
```

### <a name="azure-cloud-provider-not-in-use"></a>Dostawca usługi Azure Cloud nieużywany

Na pierwszy węzeł główny (źródło) lub bastionu węzła (OCP), protokół SSH za pomocą poświadczeń podana podczas wdrażania. Należy wydać następujące polecenie:

```bash
ansible-playbook /usr/share/ansible/openshift-ansible/playbooks/byo/openshift-cluster/openshift-metrics.yml \
-e openshift_metrics_install_metrics=True 

ansible-playbook /usr/share/ansible/openshift-ansible/playbooks/byo/openshift-cluster/openshift-logging.yml \
-e openshift_logging_install_logging=True 
```

## <a name="next-steps"></a>Następne kroki

- [Rozpoczynanie pracy z platformą kontenera OpenShift](https://docs.openshift.com/container-platform/3.6/getting_started/index.html)
- [Wprowadzenie do platformy OpenShift Origin](https://docs.openshift.org/latest/getting_started/index.html)