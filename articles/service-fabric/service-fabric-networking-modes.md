---
title: "Konfigurowanie sieci tryby usługi kontenera platformy Azure Service Fabric | Dokumentacja firmy Microsoft"
description: "Dowiedz się, jak można skonfigurować różne tryby sieci, które obsługuje sieć szkieletowa usług Azure."
services: service-fabric
documentationcenter: .net
author: mani-ramaswamy
manager: timlt
editor: 
ms.assetid: d552c8cd-67d1-45e8-91dc-871853f44fc6
ms.service: service-fabric
ms.devlang: dotNet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 8/9/2017
ms.author: subramar
ms.openlocfilehash: 855e315f66858210875039f91f7f05055ff7d9b9
ms.sourcegitcommit: 6a22af82b88674cd029387f6cedf0fb9f8830afd
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/11/2017
---
# <a name="service-fabric-container-networking-modes"></a>Tryby sieci kontenera sieci szkieletowej usług

Domyślny tryb sieci oferowanych w klastrze usługi sieć szkieletowa usług dla usługi kontenera jest `nat` trybu sieci. Z `nat` sieci trybie mających więcej niż jedna usługa kontenery nasłuchiwanie wyniki do tego samego portu błędy wdrożenia. Uruchomione kilka usług tego nasłuchiwania na tym samym porcie, obsługuje sieci szkieletowej usług `Open` trybu sieciowego (w wersji 5.7 lub nowszej). Z `Open` sieci trybu każdej usługi kontenera pobiera przypisywany dynamicznie adres IP wewnętrznie stosowanie wielu usług do nasłuchiwania do tego samego portu.   

W związku z tym z pojedynczego typu usługi z punktem końcowym statyczne zdefiniowane w manifeście usługi, nowych usług mogą być tworzone i usunąć bez błędów wdrażania przy użyciu `Open` trybu sieci. Podobnie, co może używać tego samego `docker-compose.yml` plik z portu statycznego mapowania do tworzenia wielu usług.

Za pomocą IP dynamicznie przypisywanych do odnajdywania usługi nie jest zalecane od czasu zmiany adresu IP, gdy Usługa uruchamia się ponownie lub przenoszone do innego węzła. Należy używać tylko **Usługa nazewnictwa sieci szkieletowej** lub **usługi DNS** dla potrzeb odnajdywania usług. 


> [!WARNING]
> Tylko łącznie 4096 adresy IP są dozwolone dla sieci wirtualnej na platformie Azure. W związku z tym sumę liczby węzłów i numer wystąpienia usługi kontenera (z `Open` sieci) nie może przekroczyć 4096 w ramach sieci wirtualnej. W przypadku takich scenariuszy wysokiej gęstości `nat` zalecany jest tryb sieci.
>

## <a name="setting-up-open-networking-mode"></a>Konfigurowanie trybu otwartego sieci

1. Konfigurowanie szablonów usługi Azure Resource Manager przez włączenie usługi DNS i dostawca IP w obszarze `fabricSettings`. 

    ```json
    "fabricSettings": [
                {
                    "name": "DnsService",
                    "parameters": [
                       {
                            "name": "IsEnabled",
                            "value": "true"
                      }
                    ]
                },
                {
                    "name": "Hosting",
                    "parameters": [
                      { 
                            "name": "IPProviderEnabled",
                            "value": "true"
                      }
                    ]
                },
                {
                    "name":  "Trace/Etw", 
                    "parameters": [
                    {
                            "name": "Level",
                            "value": "5"
                    }
                    ]
                },
                {
                    "name": "Setup",
                    "parameters": [
                    {
                            "name": "ContainerNetworkSetup",
                            "value": "true"
                    }
                    ]
                }
            ],
    ```

2. Konfigurowanie sekcji profilu sieciowego umożliwiają wielu adresów IP, które mają być skonfigurowane na każdym węźle klastra. Poniższy przykład powoduje ustawienie pięć adresów IP na węzeł (w związku z tym można masz pięć wystąpień usługi nasłuchiwanie portów w każdym węźle) dla klastra usługi sieć szkieletowa usług systemu Windows i Linux.

    ```json
    "variables": {
        "nicName": "NIC",
        "vmName": "vm",
        "virtualNetworkName": "VNet",
        "vnetID": "[resourceId('Microsoft.Network/virtualNetworks',variables('virtualNetworkName'))]",
        "vmNodeType0Name": "[toLower(concat('NT1', variables('vmName')))]",
        "subnet0Name": "Subnet-0",
        "subnet0Prefix": "10.0.0.0/24",
        "subnet0Ref": "[concat(variables('vnetID'),'/subnets/',variables('subnet0Name'))]",
        "lbID0": "[resourceId('Microsoft.Network/loadBalancers',concat('LB','-', parameters('clusterName'),'-',variables('vmNodeType0Name')))]",
        "lbIPConfig0": "[concat(variables('lbID0'),'/frontendIPConfigurations/LoadBalancerIPConfig')]",
        "lbPoolID0": "[concat(variables('lbID0'),'/backendAddressPools/LoadBalancerBEAddressPool')]",
        "lbProbeID0": "[concat(variables('lbID0'),'/probes/FabricGatewayProbe')]",
        "lbHttpProbeID0": "[concat(variables('lbID0'),'/probes/FabricHttpGatewayProbe')]",
        "lbNatPoolID0": "[concat(variables('lbID0'),'/inboundNatPools/LoadBalancerBEAddressNatPool')]"
    }
    "networkProfile": {
                "networkInterfaceConfigurations": [
                  {
                    "name": "[concat(parameters('nicName'), '-0')]",
                    "properties": {
                      "ipConfigurations": [
                        {
                          "name": "[concat(parameters('nicName'),'-',0)]",
                          "properties": {
                            "primary": "true",
                            "loadBalancerBackendAddressPools": [
                              {
                                "id": "[variables('lbPoolID0')]"
                              }
                            ],
                            "loadBalancerInboundNatPools": [
                              {
                                "id": "[variables('lbNatPoolID0')]"
                              }
                            ],
                            "subnet": {
                              "id": "[variables('subnet0Ref')]"
                            }
                          }
                        },
                        {
                          "name": "[concat(parameters('nicName'),'-', 1)]",
                          "properties": {
                            "primary": "false",
                            "subnet": {
                              "id": "[variables('subnet0Ref')]"
                            }
                          }
                        },
                        {
                          "name": "[concat(parameters('nicName'),'-', 2)]",
                          "properties": {
                            "primary": "false",
                            "subnet": {
                              "id": "[variables('subnet0Ref')]"
                            }
                          }
                        },
                        {
                          "name": "[concat(parameters('nicName'),'-', 3)]",
                          "properties": {
                            "primary": "false",
                            "subnet": {
                              "id": "[variables('subnet0Ref')]"
                            }
                          }
                        },
                        {
                          "name": "[concat(parameters('nicName'),'-', 4)]",
                          "properties": {
                            "primary": "false",
                            "subnet": {
                              "id": "[variables('subnet0Ref')]"
                            }
                          }
                        },
                        {
                          "name": "[concat(parameters('nicName'),'-', 5)]",
                          "properties": {
                            "primary": "false",
                            "subnet": {
                              "id": "[variables('subnet0Ref')]"
                            }
                          }
                        }
                      ],
                      "primary": true
                    }
                  }
                ]
              }
   ```
 

3. Tylko klastrów systemu Windows należy skonfigurować reguły NSG otwierania portu UDP i 53 sieci wirtualnej z następującymi wartościami:

   | Priorytet |    Nazwa    |    Element źródłowy      |  Element docelowy   |   Usługa    | Akcja |
   |:--------:|:----------:|:--------------:|:--------------:|:------------:|:------:|
   |     2000 | Custom_Dns | VirtualNetwork | VirtualNetwork | DNS (UDP/53) | Zezwalaj  |


4. Określ tryb sieci w manifeście aplikacji dla każdej usługi `<NetworkConfig NetworkType="Open">`.  Tryb `Open` powoduje Usługa pobierania dedykowany adres IP. Jeśli tryb nie jest określony, domyślnie przyjmowana do podstawowego `nat` tryb. W związku z tym w poniższym przykładzie manifestu `NodeContainerServicePackage1` i `NodeContainerServicePackage2` można każdego nasłuchiwania do tego samego portu (obie te usługi są nasłuchiwanie `Endpoint1`). Gdy `Open` tryb sieci jest seryjnych, `PortBinding` configs nie może zostać określony.

    ```xml
    <?xml version="1.0" encoding="UTF-8"?>
    <ApplicationManifest ApplicationTypeName="NodeJsApp" ApplicationTypeVersion="1.0" xmlns="http://schemas.microsoft.com/2011/01/fabric" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance">
      <Description>Calculator Application</Description>
      <Parameters>
        <Parameter Name="ServiceInstanceCount" DefaultValue="3"></Parameter>
        <Parameter Name="MyCpuShares" DefaultValue="3"></Parameter>
      </Parameters>
      <ServiceManifestImport>
        <ServiceManifestRef ServiceManifestName="NodeContainerServicePackage1" ServiceManifestVersion="1.0"/>
        <Policies>
          <ContainerHostPolicies CodePackageRef="NodeContainerService1.Code" Isolation="hyperv">
           <NetworkConfig NetworkType="Open"/>
          </ContainerHostPolicies>
        </Policies>
      </ServiceManifestImport>
      <ServiceManifestImport>
        <ServiceManifestRef ServiceManifestName="NodeContainerServicePackage2" ServiceManifestVersion="1.0"/>
        <Policies>
          <ContainerHostPolicies CodePackageRef="NodeContainerService2.Code" Isolation="default">
            <NetworkConfig NetworkType="Open"/>
          </ContainerHostPolicies>
        </Policies>
      </ServiceManifestImport>
    </ApplicationManifest>
    ```
Można mieszać i dopasowywać różne tryby sieciowych w usługach w ramach aplikacji w klastrze systemu Windows. W związku z tym niektóre usługi mogą mieć na `Open` tryb i niektóre na `nat` trybu sieci. Gdy usługa jest skonfigurowana z `nat`, port jest nasłuchiwanie muszą być unikatowe. Mieszanie tryby sieci dla różnych usług nie jest obsługiwana w systemie Linux klastrów. 


## <a name="next-steps"></a>Następne kroki
W tym artykule przedstawiono dotyczące tryby oferowane przez sieć szkieletowa usług sieciowych.  

* [Model aplikacji sieci szkieletowej usług](service-fabric-application-model.md)
* [Zasoby manifestu usługi sieci szkieletowej usług](https://docs.microsoft.com/en-us/azure/service-fabric/service-fabric-service-manifest-resources)
* [Wdrażanie kontenera systemu Windows w sieci szkieletowej usług w systemie Windows Server 2016](service-fabric-get-started-containers.md)
* [Wdrażanie kontenera Docker sieci szkieletowej usług w systemie Linux](service-fabric-get-started-containers-linux.md)
