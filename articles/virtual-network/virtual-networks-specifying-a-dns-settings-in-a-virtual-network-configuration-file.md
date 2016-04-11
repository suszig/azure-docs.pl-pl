<properties 
   pageTitle="Virtual Network 構成ファイルでの DNS 設定の指定 | Microsoft Azure"
   description="仮想ネットワーク構成ファイルを使用して仮想ネットワークの DNS サーバーの設定を変更する方法"
   services="virtual-network"
   documentationCenter="na"
   authors="joaoma"
   manager="jdial"
   editor="tysonn" />
<tags 
   ms.service="virtual-network"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="08/25/2015"
   ms.author="joaoma" />

# 仮想ネットワーク構成ファイルでの DNS 設定の指定

ネットワーク構成ファイルは、ドメイン ネーム システム (DNS) 設定を指定するのに使用できる 2 つの要素を持つ: **DnsServers** と **DnsServerRef**します。 それぞれの IP アドレスを指定して DNS サーバーの一覧を追加し、参照名を **DnsServers** 要素。 使用して、 **DnsServerRef** 、DnsServers 要素から DNS サーバー エントリは、仮想ネットワーク内の別のネットワーク サイトの使用を指定する要素。

>[AZURE.IMPORTANT] ネットワーク構成ファイルを構成する方法については、次を参照してください。 [ネットワーク構成ファイルを使用して仮想ネットワークを構成](virtual-networks-using-network-configuration-file.md)します。 ネットワーク構成ファイルに含まれる各要素については、次を参照してください。 [Azure 仮想ネットワーク構成スキーマ](https://msdn.microsoft.com/library/azure/jj157100.aspx)します。

ネットワーク構成ファイルは、次の要素を含むことができます。 各要素のタイトルは、要素値の設定に関する追加情報を提供するページにリンクされています。

[Dns 要素](http://go.microsoft.com/fwlink/?LinkId=248093)

    <Dns>
      <DnsServers>
        <DnsServer name="ID1" IPAddress="IPAddress1" />
        <DnsServer name="ID2" IPAddress="IPAddress2" />
        <DnsServer name="ID3" IPAddress="IPAddress3" />
      </DnsServers>
    </Dns>

>[AZURE.WARNING]  **名前** 属性、 **DnsServer** 要素は、参照用としてのみ使用、 **DnsServerRef** 要素。 DNS サーバーのホスト名を表してはいません。 各 **DnsServer** 属性の値は、Microsoft Azure サブスクリプション全体にわたって一意でなければなりません

[VirtualNetworkSites 要素](http://go.microsoft.com/fwlink/?LinkId=248093)

    <DnsServersRef>
      <DnsServerRef name="ID1" />
      <DnsServerRef name="ID2" />
      <DnsServerRef name="ID3" />
    </DnsServersRef>

>[AZURE.NOTE] Virtualnetworksites 要素に対してこの設定を指定するために、定義しなければなりません以前 DNS 要素で。 DnsServerRef *名前* Virtual Network Sites で要素は dns サーバーの DNS 要素で指定された名前の値を参照する *名前*します。

## 次のステップ

[ネットワーク構成ファイルを使用した仮想ネットワークの構成](virtual-networks-using-network-configuration-file.md)

[Azure Virtual Network の構成スキーマ](http://go.microsoft.com/fwlink/?LinkId=248093)

[Azure サービス構成スキーマ](https://msdn.microsoft.com/library/windowsazure/ee758710)

