<properties
   pageTitle="ARM テンプレートを使用した仮想ネットワークの作成 | Microsoft Azure"
   description="ARM テンプレートを使用した仮想ネットワークの作成方法の説明 | リソース マネージャー"
   services="virtual-network"
   documentationCenter=""
   authors="telmosampaio"
   manager="carmonm"
   editor=""
   tags="azure-resource-manager"/>

<tags
   ms.service="virtual-network"
   ms.devlang="na"
   ms.topic="hero-article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="12/07/2015"
   ms.author="telmos"/>


# ARM テンプレートを使用した仮想ネットワークの作成

[AZURE.INCLUDE [virtual-networks-create-vnet-selectors-arm-include](../../includes/virtual-networks-create-vnet-selectors-arm-include.md)]

[AZURE.INCLUDE [virtual-networks-create-vnet-intro](../../includes/virtual-networks-create-vnet-intro-include.md)]

[AZURE.INCLUDE [azure-arm-classic-important-include](../../includes/azure-arm-classic-important-include.md)] このドキュメントでは、リソース マネージャーの配置モデルを使用した VNet の作成について説明します。 こともできます [従来のデプロイ モデルで仮想ネットワークの作成](virtual-networks-create-vnet-classic-pportal.md)します。

GitHub から既存の ARM テンプレートをダウンロードして変更し、そのテンプレートを GitHub、PowerShell、および Azure CLI からデプロイする方法を説明します。

単に変更を加えず、GitHub から直接 ARM テンプレートを展開する場合にスキップ [github からテンプレートをデプロイ](#deploy-the-arm-template-by-using-click-to-deploy)します。

[AZURE.INCLUDE [virtual-networks-create-vnet-scenario-include](../../includes/virtual-networks-create-vnet-scenario-include.md)]

[AZURE.INCLUDE [virtual-networks-create-vnet-arm-template-include](../../includes/virtual-networks-create-vnet-arm-template-include.md)]

[AZURE.INCLUDE [virtual-networks-create-vnet-arm-template-ps-include](../../includes/virtual-networks-create-vnet-arm-template-ps-include.md)]

[AZURE.INCLUDE [virtual-networks-create-vnet-arm-template-cli-include](../../includes/virtual-networks-create-vnet-arm-template-cli-include.md)]

[AZURE.INCLUDE [virtual-networks-create-vnet-arm-template-click-include](../../includes/virtual-networks-create-vnet-arm-template-click-include.md)]




