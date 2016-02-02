<properties 
   pageTitle="CLI を使用して Azure DNS の DNS レコード セットとレコードを管理する | Microsoft Azure" 
   description="Azure DNS でドメインをホストする際に Azure DNS の DNS レコード セットとレコードを管理します。レコード セットとレコードに対する操作のための CLI コマンドをすべて紹介します。" 
   services="dns" 
   documentationCenter="na" 
   authors="joaoma" 
   manager="carmonm" 
   editor=""/>

<tags
   ms.service="dns"
   ms.devlang="en"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services" 
   ms.date="11/10/2015"
   ms.author="joaoma"/>


# CLI を使用して DNS レコードを管理する方法

> [AZURE.SELECTOR]
- [Azure CLI](dns-operations-recordsets-cli.md)
- [PowerShell](dns-operations-recordsets.md)


このガイドでは、DNS ゾーンのレコード セットとレコードを管理する方法について説明します。
>[AZURE.NOTE] Azure DNS は、Azure リソース マネージャー専用のサービスです。 ASM API はありません。 したがって、"azure config mode arm" コマンドを使用して、リソース マネージャー モードを使用するように Azure CLI を構成するようにする必要があります。

>"エラー: 'dns' は、Azure のコマンドではありません" と表示される場合は、多くの場合、リソース マネージャー モードではなく、ASM モードで Azure CLI を使用していることが原因です。

DNS レコード セットと個々の DNS レコードの違いを理解することは重要です。 レコード セットとは、1 つのゾーン内にある同じ名前、同じ種類のレコードのコレクションです。 詳細については、次を参照してください。 [理解レコードが設定し、記録](dns-getstarted-create-recordset.md#Understanding-record-sets-and-records)します。

## レコード セットの作成

使用してレコード セットを作成、 `azure ネットワークの dns レコード セットを作成` コマンドです。 レコード セット名、ゾーン、Time-to-Live (TTL)、レコードの種類を指定する必要があります。
>[AZURE.NOTE] レコード セット名は、ゾーン名を除いた相対名にする必要があります。 たとえば、"contoso.com" ゾーン内のレコード セット名 "www" によって、完全修飾名 "www.contoso.com" を持つレコード セットが作成されます。

>ゾーンの頂点のレコード セットの場合は、レコード セット名として "@" (引用符を含みます) を使用します。 レコード セットの完全修飾名はゾーン名と同じになります。この場合は "contoso.com" です。

Azure DNS では、A、AAAA、CNAME、MX、NS、SOA、SRV、TXT というレコードの種類がサポートされています。 SOA という種類のレコード セットは、各ゾーンと共に自動的に作成されるため、個別には作成できません。 なお [、SPF レコードの種類は、TXT レコードの種類を使用して SPF レコードを作成している DNS の標準で推奨されていません](http://tools.ietf.org/html/rfc7208#section-3.1)します。

    azure network dns record-set create myresourcegroup contoso.com  www  A --ttl 300

>[AZURE.IMPORTANT] CNAME レコード セットは、同じ名前を持つ他のレコード セットとは共存できません。 たとえば、相対名 "www" を持つ CNAME と相対名 "www" を持つ A レコードを同時に作成することはできません。 ゾーンの頂点 (名前は '@') には、ゾーンの作成時に作成された NS および SOA レコード セットが必ず含まれるため、ゾーンの頂点で CNAME レコード セットを作成することはできません。 これらの制約は、DNS 標準から生じるものであり、Azure DNS の制限事項ではありません。

### ワイルドカード レコード

Azure DNS は、サポート [ワイルドカード レコード](https://en.wikipedia.org/wiki/Wildcard_DNS_record)します。 ワイルドカード レコードは、一致する名前を含むクエリに対してに返されます (非ワイルドカード レコード セットに、より近い一致がない場合)。
>[AZURE.NOTE] ワイルドカード レコード セットを作成するには、レコード セット名を使用して"\*"、または最初のラベルが名"\*"、"\*.foo"などです。

>ワイルドカード レコード セットは、NS と SOA を除くすべてのレコードの種類でサポートされています。

## レコード セットの取得

既存のレコード セットを取得するには使用 `azure ネットワークの dns レコード セットの表示`, 、セット相対名とレコードの種類のリソース グループ、ゾーン名、レコードを指定します。

    azure network dns record-set show myresourcegroup contoso.com www A

## レコード セットの一覧を表示する

DNS ゾーンを使用して、すべてのレコードを一覧表示 `azure ネットワークの dns レコード セットの一覧` コマンド。

### 方法 1

すべてのレコード セットの一覧を表示します。 これにより、名前またはレコードの種類に関係なく、すべてのレコード セットが返されます。

    azure network dns record-set list myresourcegroup contoso.com

### 方法 2

指定したレコードの種類のレコード セットの一覧を表示します。 これにより、指定したレコードの種類 (この場合は A レコード) に一致するすべてのレコード セットが返されます。


    azure network dns record-set list myresourcegroup contoso.com A 

どちらの場合も、リソース グループ名とゾーン名を指定します。

## レコード セットへのレコードの追加

使用してレコード セットにレコードを追加、 `azure ネットワーク dns レコード セットの追加-レコード`します。

レコード セットにレコードを追加するためのパラメーターは、レコード セットの種類によって異なります。たとえば、型 'A' のレコード セットを使用する場合のみことができます、パラメーターを持つレコードを指定する"で、 `< IPv4 アドレス >`"です。

次の例では、1 つのレコードを含む、各種のレコード セットを作成する方法を示します。

### 1 つのレコードを含む A レコード セットの作成

レコード セットを作成するには使用 `azure ネットワークの dns レコード セットを作成`, 、リソース グループ、ゾーン名、レコード セットを指定する相対名、レコードの種類、および時間 (ttl)。

    azure network dns record-set create myresourcegroup  contoso.com "test-a"  A --ttl 300

>[AZURE.NOTE] --ttl パラメーターを定義しないと、値は既定で 4 (秒) になります。


IPv4 の追加、A レコード セットを作成した後を記録するアドレスの設定と `azure ネットワーク dns レコード セットの追加-レコード`:

    azure network dns record-set add-record myresourcegroup contoso.com "test-a" A -a 192.168.1.1 

### 1 つのレコードを含む AAAA レコード セットの作成

    azure network dns record-set create myresourcegroup contoso.com "test-aaaa" AAAA --ttl 300
    
    azure network dns record-set add-record myresourcegroup contoso.com "test-aaaa" AAAA -b "2607:f8b0:4009:1803::1005"

### 1 つのレコードを含む CNAME レコード セットの作成

    azure network dns record-set create -g myresourcegroup contoso.com  "test-cname" CNAME --ttl 300
    
    azure network dns record-set add-record  myresourcegroup contoso.com  test-cname CNAME -c "www.contoso.com"

>[AZURE.NOTE] CNAME レコードでは、単一の文字列値だけを使用できます。 

### 1 つのレコードを含む MX レコード セットの作成

この例では、レコード セット名 "@" を使用してゾーンの頂点 ("contoso.com" など) に MX レコードを作成します。 これは、MX レコードに共通です。

    azure network dns record-set create myresourcegroup contoso.com  "@"  MX --ttl 300
    
    azure network dns record-set add-record -g myresourcegroup contoso.com  "@" MX -e "mail.contoso.com" -f 5

### 1 つのレコードを含む NS レコード セットの作成

    azure network dns record-set create myresourcegroup contoso.com test-ns  NS --ttl 300
    
    azure network dns record-set add-record myresourcegroup  contoso.com  "test-ns" NS -d "ns1.contoso.com" 

### 1 つのレコードを含む SRV レコード セットの作成

ゾーンのルートに SRV レコードを作成する場合は、レコード名に _サービスと _プロトコルを指定するだけです。レコード名に "@" も含める必要はありません。


    azure network dns record-set create myresourcegroup contoso.com "_sip._tls" SRV --ttl 300 
    
    azure network dns record-set add-record myresourcegroup contoso.com  "_sip._tls" SRV -p 0 - w 5 -o 8080 -u "sip.contoso.com" 

### 1 つのレコードを含む TXT レコード セットの作成

    azure network dns record-set create myresourcegroup contoso.com "test-TXT" TXT --ttl 300
    
    azure network dns record-set add-record myresourcegroup contoso.com "test-txt" TXT -x "this is a TXT record" 

## 既存のレコード セットの変更

これを次の例で説明します。

### 既存のレコード セット内のレコードの更新

この例では、別の IP アドレス (1.2.3.4) を既存の A レコード セット (www) に追加します。

    azure network dns record-set add-record  myresourcegroup contoso.com  A
    -a 1.2.3.4
    info:    Executing command network dns record-set add-record
    Record set name: www
    + Looking up the dns zone "contoso.com"
    + Looking up the DNS record set "www"
    + Updating DNS record set "www"
    data:    Id                              : /subscriptions/################################/resourceGroups/myresourcegroup/providers/Microsoft.Network/dnszones/contoso.com/a/www
    data:    Name                            : www  
    data:    Type                            : Microsoft.Network/dnszones/a
    data:    Location                        : global
    data:    TTL                             : 4
    data:    A records:
    data:        IPv4 address                : 192.168.1.1
    data:        IPv4 address                : 1.2.3.4
    data:
    info:    network dns record-set add-record command OK

使用する `azure ネットワーク dns レコード セットの削除レコード` レコード セットから既存の値を削除します。

    azure network dns record-set delete-record myresourcegroup contoso.com www A -a 1.2.3.4
    info:    Executing command network dns record-set delete-record
    + Looking up the DNS record set "www"
    Delete DNS record? [y/n] y
    + Updating DNS record set "www"
    data:    Id                              : /subscriptions/################################/resourceGroups/myresourcegroup/providers/Microsoft.Network/dnszones/contoso.com/A/www
    data:    Name                            : www
    data:    Type                            : Microsoft.Network/dnszones/A
    data:    Location                        : global
    data:    TTL                             : 4
    data:    A records:
    data:        IPv4 address                : 192.168.1.1
    data:
    info:    network dns record-set delete-record command OK

## 既存のレコード セットからのレコードの削除

レコードを使用してセットからレコードを削除できます `azure ネットワーク dns レコード セットの削除レコード` レコードが削除されているすべてのパラメーターで、既存のレコードを完全に一致する必要があることに注意してください。

レコード セットから最後のレコードを削除しても、レコード セットは削除されません。 参照してください [レコード セットの削除](#delete-a-record-set) の下の詳細。


    azure network dns record-set delete-record myresourcegroup contoso.com www A -a 192.168.1.1
    
    azure network dns record-set delete myresourcegroup contoso.com www A

### レコード セットからの AAAA レコードの削除

    azure network dns record-set delete-record myresourcegroup contoso.com test-aaaa  AAAA -b "2607:f8b0:4009:1803::1005"

### レコード セットからの CNAME レコードの削除

    azure network dns record-set delete-record myresourcegroup contoso.com test-cname CNAME -c www.contoso.com

### レコード セットからの MX レコードの削除

    azure network dns record-set delete-record myresourcegroup contoso.com "@" MX -e "mail.contoso.com" -f 5

### レコード セットからの NS レコードの削除

    azure network dns record-set delete-record myresourcegroup contoso.com  "test-ns" NS -d "ns1.contoso.com"

### レコード セットからの SRV レコードの削除

    azure network dns record-set delete-record myresourcegroup contoso.com  "_sip._tls" SRV -p 0 -w 5 -o 8080 -u "sip.contoso.com" 

### レコード セットからの TXT レコードの削除

    azure network dns record-set delete-record myresourcegroup contoso.com  "test-TXT" TXT -x "this is a TXT record"

## レコード セットの削除

レコード セットは、Remove-AzureDnsRecordSet コマンドレットを使用して削除できます。
>[AZURE.NOTE] ゾーンの作成時に自動的に作成される、ゾーンの頂点 (名前は "@") の SOA および NS レコード セットは削除できません。 これらはゾーンの削除時に自動的に削除されます。

次の例では、A レコード セット "test-a" を contoso.com DNS ゾーンから削除します。

    azure network dns record-set delete myresourcegroup contoso.com  "test-a" A 

オプションの "-q" スイッチを使用すると、確認プロンプトが表示されないように設定できます。


## 関連項目

[Azure DNS へのドメインの委任](dns-domain-delegation.md)<BR>
[DNS ゾーンを管理します。](dns-operations-dnszones-cli.md)<BR>
[.NET SDK を使用して操作を自動化します。](dns-sdk.md)






