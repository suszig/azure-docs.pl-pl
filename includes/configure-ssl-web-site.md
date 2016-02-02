この記事では、Azure App Service で web アプリに対する HTTPS を構成する方法を示します。 クライアント証明書認証を網羅していません方法については、次を参照してください。 [方法を構成する TLS 相互認証を Web Apps](../articles/app-service-web/app-service-web-configure-tls-mutual-auth.md)します。

既定では、Azure は既に \*.azurewebsites.net ドメインのワイルドカード証明書を使用してアプリケーションの HTTP を使用します。 カスタム ドメインを構成する予定がない場合は、既定の HTTPS 証明書を利用できます。 ただし、 [すべてのワイルドカード ドメイン](https://casecurity.org/2014/02/26/pros-and-cons-of-single-domain-multi-domain-and-wildcard-certificates/), 、独自の証明書にカスタム ドメインを使用する場合ほど安全ではありません。

このドキュメントの残りの部分がなど、カスタム ドメインの HTTPS を有効にする方法の詳細を提供 **contoso.com**, 、**www.contoso.com**, 、または **\*.contoso.com**

<a name="bkmk_domainname"></a>
## カスタム ドメインに対して SSL を有効にする

など、カスタム ドメインの HTTPS を有効にする **contoso.com**, 、まず [Azure App Service でカスタム ドメイン名の構成](../articles/app-service-web/web-sites-custom-domain-name.md)します。 次に、以下の手順を実行します。

1. [SSL 証明書を取得します。](#bkmk_getcert)
2. [Standard 価格レベルを構成します。](#bkmk_standardmode)
2. [アプリの SSL を構成します。](#bkmk_configuressl)
3. [アプリで SSL を強制](#bkmk_enforce) (省略可能)

この記事のどの時点でもその他のヘルプを必要がある場合で Azure のエキスパートに連絡することができます [MSDN Azure とスタック オーバーフロー フォーラム](http://azure.microsoft.com/support/forums/)します。 または、Azure サポート インシデントを送信できます。 移動して、 [Azure サポート サイト](http://azure.microsoft.com/support/options/) ] をクリック **サポートにお問い合わせ**します。

<a name="bkmk_getcert"></a>
## 1.SSL 証明書を取得する

SSL 証明書を要求する前に、その証明書により、どのドメイン名をセキュリティで保護するかを最初に決定する必要があります。 この結果、どのような種類の証明書を取得する必要があるかが決まります。 **contoso.com** または **www.contoso.com** のように、ただ 1 つのドメイン名を保護する必要がある場合は、基本的な証明書で十分です。 などの複数のドメイン名をセキュリティで保護する必要があるかどうかは **contoso.com**, 、**www.contoso.com**, 、および **mail.contoso.com**, を入手できる、 [ワイルドカード証明書](http://en.wikipedia.org/wiki/Wildcard_certificate), 、または証明書を [サブジェクト代替名](http://en.wikipedia.org/wiki/SubjectAltName) (subjectAltName)。

によって App Service で使用される SSL 証明書を署名する必要があります、 [証明機関](http://en.wikipedia.org/wiki/Certificate_authority) (CA)。 まだ SSL 証明書がない場合は、SSL 証明書を発行する会社から取得する必要があります。 証明機関の一覧は、次を参照してください。 [Windows および Windows Phone 8 SSL ルート証明書プログラム (メンバー Ca) の ][cas] 、Microsoft TechNet Wiki にします。

証明書は、Azure における SSL 証明書の次の要件を満たす必要があります。

* 証明書は秘密キーを含む必要があります。
* 証明書はキー交換のために作成され、Personal Information Exchange (.pfx) ファイルにエクスポートできる必要があります。
* 証明書のサブジェクト名は、アプリへのアクセスに使用されるドメインと一致する必要があります。 この証明書で複数のドメインを扱う場合は、前に説明したように、ワイルドカードの値を使用するか、subjectAltName の値を指定する必要があります。
* 証明書では、2,048 ビット以上の暗号化を使用する必要があります。
* プライベート CA サーバーから発行された証明書は、Azure App Service ではサポートされません。

Azure App Service で使用する SSL 証明書を取得するには、証明書署名要求 (CSR) を証明機関に送信し、その後、受け取った証明書で .pfx ファイルを生成します。 これは好みのツールを使用して実行できます。 証明書を取得する一般的な方法には、以下の数種類があります。

- [Certreq.exe を使用した証明書を取得します。](#bkmk_certreq)
- [IIS マネージャーを使用して証明書を取得します。](#bkmk_iismgr)
- [OpenSSL を使用した証明書を取得します。](#bkmk_openssl)
- [OpenSSL を使用した SubjectAltName 証明書を取得します。](#bkmk_subjectaltname)
- [自己署名証明書の生成 (テスト目的専用)](#bkmk_selfsigned)

> [AZURE.NOTE] 入力を求められます手順をで、 **共通名**, など `www.contoso.com`します。 この値は、ワイルドカード証明書は、\*.domainname (たとえば、\*. contoso.com)。 ワイルドカード名 (\*.contoso.com など) とルート ドメイン名 (contoso.com など) の両方をサポートする必要がある場合、ワイルドカードの subjectAltName 証明書を使用できます。
>
> Azure App Service は楕円曲線暗号 (ECC) 証明書をサポートしています。ただし、この証明書は比較的新しいため、正しい手順で CSR を作成するには証明機関の協力が必要です。

取得する必要がありますも * *[中間証明書](http://en.wikipedia.org/wiki/Intermediate_certificate_authorities)* * (チェーン証明とも呼ばれます)、CA が使用している場合。 中間証明書を使用すると、"チェーンされていない証明書" を使用する場合よりセキュリティが強化されると見なされるため、CA で一般的に使用されています。 中間証明書は、多くの場合、CA の Web サイトから個別にダウンロードする形で提供されています。 この記事では、中間証明書が、アプリにアップロードされた証明書にマージされることを確認する手順を説明します。

<a name="bkmk_certreq"></a>
### Certreq.exe を使用した証明書の取得 (Windows のみ)

Certreq.exe は、証明書の要求を作成するための Windows ユーティリティです。 Windows XP または Windows Server 2000 以降の Windows 基本インストールの一部であり、最近の Windows システムで使用できます。 Certreq.exe を使用して SSL 証明書を取得するには、次のステップを使用します。

1. **メモ帳**を開き、次の内容を含む新しい文書を作成します。 置換 **mysite.com** [件名] 行、アプリのカスタム ドメイン名を使用します。 たとえば、Subject = "CN=www.contoso.com" と記述します。

     [NewRequest]
     Subject = "CN=mysite.com"
     Exportable = TRUE
     KeyLength = 2048
     KeySpec = 1
     KeyUsage = 0xA0
     MachineKeySet = True
     ProviderName = "Microsoft RSA SChannel Cryptographic Provider"
     ProviderType = 12
    
     [EnhancedKeyUsageExtension]
     OID=1.3.6.1.5.5.7.3.1

 使用可能なその他のオプションと同様に、上記で指定したオプションの詳細については、次を参照してください。、 [Certreq リファレンス ドキュメント](http://technet.microsoft.com/library/cc725793.aspx)します。

2. テキスト ファイル **myrequest.txt** という名前で保存します。

3. **スタート画面**または **[スタート] メニュー**で、**cmd.exe** を実行します。

4. コマンド プロンプトで次のコマンドを使用して、証明書要求ファイルを作成します。

        certreq -new \path\to\myrequest.txt \path\to\create\myrequest.csr

    ステップ 1 で作成した **myrequest.txt** ファイルに対応するパスと、**myrequest.csr** ファイルを作成するときに使用するパスを指定します。

5. SSL 証明書を取得するために、**myrequest.csr** を証明機関に送信します。 この送信には、ファイルをアップロードすること、またはメモ帳でファイルを開き、Web フォームに内容を貼り付ける作業が含まれる可能性があります。

    証明機関の一覧は、次を参照してください。 [Windows および Windows Phone 8 SSL ルート証明書プログラム (メンバー Ca) の ][cas] 、Microsoft TechNet Wiki にします。

6. 証明機関から証明書 (.CER) ファイルが提供された後、要求を生成したときに使用しコンピューターにこのファイルを保存してから、次のコマンドを使用して要求を受け入れ、証明書の生成プロセスを完了します。

        certreq -accept -user mycert.cer

    この例では、証明機関から受け取った **mycert.cer** 証明書を使用して、証明書の署名を完了します。 ファイルは作成されません。代わりに、証明書が Windows 証明書ストアに格納されます。

6. CA が中間証明書を使用している場合、次のステップで証明書をエクスポートする前に、それらの中間証明書をインストールする必要があります。 通常、これらの証明書は CA から個別のダウンロードとして提供されており、Web サーバーの種類に応じていくつかの形式で提供されています。 Microsoft IIS 用に提供されているバージョンを選択します。

    証明書をダウンロードした後、エクスプローラーで証明書を右クリックし、**[証明書のインストール]** をクリックします。 **証明書のインポート ウィザード**で既定値を使用し、インポートが完了するまで、**[次]** のクリックを続けます。

7. 証明書ストアから証明書をエクスポートするには、**スタート画面**または **[スタート] メニュー**から **certmgr.msc** を実行します。 **証明書マネージャー**が表示された時点で、**[個人用]** フォルダーを展開し、**[証明書]** をクリックします。 **[発行先]** フィールドで、証明書を要求したカスタム ドメイン名に対応するエントリを見つけます。 **[発行者]** フィールドに、この証明書に関して使用した証明機関が表示されます。

    ![証明書マネージャーに関するイメージをここに挿入します][certmgr]

9. 証明書を右クリックし、**[すべてのタスク]** をクリックしてから **[エクスポート]** をクリックします。 **証明書のエクスポート ウィザード**で、**[次へ]** をクリックしてから **[はい、秘密キーをエクスポートします]** をクリックします。 **[次へ]** をクリックします。

    ![秘密キーをエクスポートします][certwiz1]

10. **[個人情報の交換 - PKCS #12]**、**[証明書チェーン内にすべての証明書を含める]**、および **[すべての拡張プロパティをエクスポートする]** を選択します。 **[次へ]** をクリックします。

    ![すべての証明書と拡張プロパティを含める][certwiz2]

11. **[パスワード]** をクリックし、パスワードの入力と確認入力を行います。 **[次へ]** をクリックします。

    ![パスワードの指定][certwiz3]

12. エクスポートした証明書を格納するファイル名とパスを指定します。 ファイル名には、**.pfx** という拡張子を付ける必要があります。 **[次へ]** をクリックしてプロセスを完了します。

    ![ファイル パスを指定する][certwiz4]

ここで、Azure App Service でアプリにエクスポートした PFX ファイルをアップロードすることができます。

<a name="bkmk_openssl"></a>
### OpenSSL を使用した証明書の取得

1. コマンド ライン、bash、またはターミナル セッションから次を入力し、秘密キーと証明書署名要求を生成します:

        openssl req -new -nodes -keyout myserver.key -out server.csr -newkey rsa:2048

2. メッセージが表示されたら、適切な情報を入力します。 次に例を示します。

     Country Name (2 letter code)
     State or Province Name (full name) []: Washington
     Locality Name (eg, city) []: Redmond
     Organization Name (eg, company) []: Microsoft
     Organizational Unit Name (eg, section) []: Azure
     Common Name (eg, YOUR name) []: www.microsoft.com
     Email Address []:
    
     Please enter the following 'extra' attributes to be sent with your certificate request
    
     A challenge password []:

 この処理を完了すると、**myserver.key** および **server.csr** の 2 つのファイルが生成されます。 **server.csr** には、証明書署名要求が含まれます。

3. SSL 証明書を取得するために、CSR を証明機関に送信します。 証明機関の一覧は、次を参照してください。 [Windows および Windows Phone 8 SSL ルート証明書プログラム (メンバー Ca) の ][cas] 、Microsoft TechNet Wiki にします。

4. CA からの証明書を入手した場合、**myserver.crt** という名前のファイルに保存します。 CA がテキスト形式の証明書を提供した場合、**myserver.crt** ファイルに証明書のテキストを貼り付けるだけです。 ファイルの内容をテキスト エディターで表示すると、次のようになります。

        -----BEGIN CERTIFICATE-----
        MIIDJDCCAgwCCQCpCY4o1LBQuzANBgkqhkiG9w0BAQUFADBUMQswCQYDVQQGEwJV
        UzELMAkGA1UECBMCV0ExEDAOBgNVBAcTB1JlZG1vbmQxEDAOBgNVBAsTB0NvbnRv
        c28xFDASBgNVBAMTC2NvbnRvc28uY29tMB4XDTE0MDExNjE1MzIyM1oXDTE1MDEx
        NjE1MzIyM1owVDELMAkGA1UEBhMCVVMxCzAJBgNVBAgTAldBMRAwDgYDVQQHEwdS
        ZWRtb25kMRAwDgYDVQQLEwdDb250b3NvMRQwEgYDVQQDEwtjb250b3NvLmNvbTCC
        ASIwDQYJKoZIhvcNAQEBBQADggEPADCCAQoCggEBAN96hBX5EDgULtWkCRK7DMM3
        enae1LT9fXqGlbA7ScFvFivGvOLEqEPD//eLGsf15OYHFOQHK1hwgyfXa9sEDPMT
        3AsF3iWyF7FiEoR/qV6LdKjeQicJ2cXjGwf3G5vPoIaYifI5r0lhgOUqBxzaBDZ4
        xMgCh2yv7NavI17BHlWyQo90gS2X5glYGRhzY/fGp10BeUEgIs3Se0kQfBQOFUYb
        ktA6802lod5K0OxlQy4Oc8kfxTDf8AF2SPQ6BL7xxWrNl/Q2DuEEemjuMnLNxmeA
        Ik2+6Z6+WdvJoRxqHhleoL8ftOpWR20ToiZXCPo+fcmLod4ejsG5qjBlztVY4qsC
        AwEAATANBgkqhkiG9w0BAQUFAAOCAQEAVcM9AeeNFv2li69qBZLGDuK0NDHD3zhK
        Y0nDkqucgjE2QKUuvVSPodz8qwHnKoPwnSrTn8CRjW1gFq5qWEO50dGWgyLR8Wy1
        F69DYsEzodG+shv/G+vHJZg9QzutsJTB/Q8OoUCSnQS1PSPZP7RbvDV9b7Gx+gtg
        7kQ55j3A5vOrpI8N9CwdPuimtu6X8Ylw9ejWZsnyy0FMeOPpK3WTkDMxwwGxkU3Y
        lCRTzkv6vnHrlYQxyBLOSafCB1RWinN/slcWSLHADB6R+HeMiVKkFpooT+ghtii1
        A9PdUQIhK9bdaFicXPBYZ6AgNVuGtfwyuS5V6ucm7RE6+qf+QjXNFg==
        -----END CERTIFICATE-----

    ファイルを保存します。

5. コマンド ライン、bash、またはターミナル セッションから、次のコマンドを使用して、**myserver.key** と **myserver.crt** を、Azure App Service で必要とされる形式である **myserver.pfx** に変換します。

        openssl pkcs12 -export -out myserver.pfx -inkey myserver.key -in myserver.crt

    メッセージが表示されたら、パスワードを入力して .pfx ファイルをセキュリティ保護します。
    > [AZURE.NOTE] CA が中間証明書を使用する場合、次のステップで証明書をエクスポートする前に、それらの中間証明書をインストールする必要があります。 通常、これらの証明書は CA から個別のダウンロードとして提供されており、Web サーバーの種類に応じていくつかの形式で提供されています。 PEM ファイル (ファイル拡張子は .pem) の形で提供されているバージョンを選択します。
    >
    > 次のコマンドは、**intermediate-cets.pem** ファイルに格納されている中間証明書を含む .pfx ファイルを作成する方法を示しています。  
    >
    >
    `````
    openssl pkcs12 -chain -export -out myserver.pfx -inkey myserver.key -in myserver.crt -certfile intermediate-cets.pem
    `````

    このコマンドを実行すると、Azure App Service での使用に適した **myserver.pfx** ファイルが生成されます。

<a name="bkmk_iismgr"></a>
### IIS マネージャーを使用した証明書の取得

IIS マネージャーに慣れている場合は、IIS マネージャーを使用して、Azure App Service で使用できる証明書を生成することができます。

1. IIS マネージャーを使用して、証明機関に送信する CSR を生成します。 CSR を生成する方法の詳細については、次を参照してください。 [(IIS 7) ][iiscsr]します。

2. SSL 証明書を取得するために、CSR を証明機関に送信します。 証明機関の一覧は、次を参照してください。 [Windows および Windows Phone 8 SSL ルート証明書プログラム (メンバー Ca) の ][cas] 、Microsoft TechNet Wiki にします。

3. 証明機関ベンダーにより提供された証明書で CSR を完了します。 CSR の完了の詳細については、次を参照してください。 [(IIS 7) ][installcertiis]します。

4. CA が中間証明書を使用する場合、次のステップで証明書をエクスポートする前に、それらの中間証明書をインストールする必要があります。 通常、これらの証明書は CA から個別のダウンロードとして提供されており、Web サーバーの種類に応じていくつかの形式で提供されています。 Microsoft IIS 用に提供されているバージョンを選択します。

    証明書をダウンロードした後、エクスプローラーで証明書を右クリックし、**[証明書のインストール]** をクリックします。 **証明書のインポート ウィザード**で既定値を使用し、インポートが完了するまで、**[次]** のクリックを続けます。

4. IIS マネージャから証明書のエクスポート、証明書をエクスポートする方法の詳細については、「 [(IIS 7) ][exportcertiis]します。 エクスポートされたファイルは、後の手順でアプリで使用するために Azure にアップロードするために使用されます。
    > [AZURE.NOTE] エクスポート プロセス中に、オプションを選択することを確認する <strong>はい、秘密キーをエクスポート</strong>します。 これにより、エクスポートされる証明書に秘密キーが含まれます。

    > [AZURE.NOTE] エクスポート プロセスでは、**[証明のパスにあるすべての証明書を含める]** オプション と **[すべての拡張プロパティをエクスポートする]** オプションを必ず選択してください。 これにより、エクスポートされる証明書にすべての中間証明書が含まれます。

<a name="bkmk_subjectaltname"></a>
### OpenSSL を使用した SubjectAltName 証明書の取得

OpenSSL を使用して、1 つの証明書で複数のドメイン名をサポートするために SubjectAltName 拡張機能を使用する証明書要求を作成できます。ただし、その場合は構成ファイルが必要になります。 次のステップでは、構成ファイルの作成と構成ファイルを使用した証明書の要求について説明します。

1. __sancert.cnf__ という名前の新しいファイルを作成し、このファイルの内容として次のコードを使用します。

        # -------------- BEGIN custom sancert.cnf -----
        HOME = .
        oid_section = new_oids
        [ new_oids ]
        [ req ]
        default_days = 730
        distinguished_name = req_distinguished_name
        encrypt_key = no
        string_mask = nombstr
        req_extensions = v3_req # Extensions to add to certificate request
        [ req_distinguished_name ]
        countryName = Country Name (2 letter code)
        countryName_default =
        stateOrProvinceName = State or Province Name (full name)
        stateOrProvinceName_default =
        localityName = Locality Name (eg, city)
        localityName_default =
        organizationalUnitName  = Organizational Unit Name (eg, section)
        organizationalUnitName_default  =
        commonName              = Your common name (eg, domain name)
        commonName_default      = www.mydomain.com
        commonName_max = 64
        [ v3_req ]
        subjectAltName=DNS:ftp.mydomain.com,DNS:blog.mydomain.com,DNS:*.mydomain.com
        # -------------- END custom sancert.cnf -----

    "subjectAltName" で始まる行に注意してください。 現在示されているドメイン名は、共通名に加えてサポートする必要があるドメイン名に置き換えてください。 次に例を示します。

        subjectAltName=DNS:sales.contoso.com,DNS:support.contoso.com,DNS:fabrikam.com

    commonName_default フィールドを変更する必要はありません。次のステップのいずれかで、共通名を入力するように求められるためです。

2. __sancert.cnf__ ファイルを保存します。

1. sancert.cnf 構成ファイルを使用して、秘密キーと証明書署名要求を生成します。 bash またはターミナル セッションから、次のコマンドを使用します。

        openssl req -new -nodes -keyout myserver.key -out server.csr -newkey rsa:2048 -config sancert.cnf

2. メッセージが表示されたら、適切な情報を入力します。 次に例を示します。

        Country Name (2 letter code) []: US
        State or Province Name (full name) []: Washington
        Locality Name (eg, city) []: Redmond
        Organizational Unit Name (eg, section) []: Azure
        Your common name (eg, domain name) []: www.microsoft.com

    この処理を完了すると、**myserver.key** および **server.csr** の 2 つのファイルが生成されます。 **server.csr** には、証明書署名要求が含まれます。

3. SSL 証明書を取得するために、CSR を証明機関に送信します。 証明機関の一覧は、次を参照してください。 [Windows および Windows Phone 8 SSL ルート証明書プログラム (メンバー Ca) の ][cas] 、Microsoft TechNet Wiki にします。

4. CA からの証明書を入手した場合、**myserver.crt** という名前のファイルに保存します。 CA がテキスト形式の証明書を提供した場合、**myserver.crt** ファイルに証明書のテキストを貼り付けるだけです。 ファイルの内容をテキスト エディターで表示すると、次のようになります。

        -----BEGIN CERTIFICATE-----
        MIIDJDCCAgwCCQCpCY4o1LBQuzANBgkqhkiG9w0BAQUFADBUMQswCQYDVQQGEwJV
        UzELMAkGA1UECBMCV0ExEDAOBgNVBAcTB1JlZG1vbmQxEDAOBgNVBAsTB0NvbnRv
        c28xFDASBgNVBAMTC2NvbnRvc28uY29tMB4XDTE0MDExNjE1MzIyM1oXDTE1MDEx
        NjE1MzIyM1owVDELMAkGA1UEBhMCVVMxCzAJBgNVBAgTAldBMRAwDgYDVQQHEwdS
        ZWRtb25kMRAwDgYDVQQLEwdDb250b3NvMRQwEgYDVQQDEwtjb250b3NvLmNvbTCC
        ASIwDQYJKoZIhvcNAQEBBQADggEPADCCAQoCggEBAN96hBX5EDgULtWkCRK7DMM3
        enae1LT9fXqGlbA7ScFvFivGvOLEqEPD//eLGsf15OYHFOQHK1hwgyfXa9sEDPMT
        3AsF3iWyF7FiEoR/qV6LdKjeQicJ2cXjGwf3G5vPoIaYifI5r0lhgOUqBxzaBDZ4
        xMgCh2yv7NavI17BHlWyQo90gS2X5glYGRhzY/fGp10BeUEgIs3Se0kQfBQOFUYb
        ktA6802lod5K0OxlQy4Oc8kfxTDf8AF2SPQ6BL7xxWrNl/Q2DuEEemjuMnLNxmeA
        Ik2+6Z6+WdvJoRxqHhleoL8ftOpWR20ToiZXCPo+fcmLod4ejsG5qjBlztVY4qsC
        AwEAATANBgkqhkiG9w0BAQUFAAOCAQEAVcM9AeeNFv2li69qBZLGDuK0NDHD3zhK
        Y0nDkqucgjE2QKUuvVSPodz8qwHnKoPwnSrTn8CRjW1gFq5qWEO50dGWgyLR8Wy1
        F69DYsEzodG+shv/G+vHJZg9QzutsJTB/Q8OoUCSnQS1PSPZP7RbvDV9b7Gx+gtg
        7kQ55j3A5vOrpI8N9CwdPuimtu6X8Ylw9ejWZsnyy0FMeOPpK3WTkDMxwwGxkU3Y
        lCRTzkv6vnHrlYQxyBLOSafCB1RWinN/slcWSLHADB6R+HeMiVKkFpooT+ghtii1
        A9PdUQIhK9bdaFicXPBYZ6AgNVuGtfwyuS5V6ucm7RE6+qf+QjXNFg==
        -----END CERTIFICATE-----

    ファイルを保存します。

5. コマンド ライン、bash、またはターミナル セッションから、次のコマンドを使用して、**myserver.key** と **myserver.crt** を、Azure App Service で必要とされる形式である **myserver.pfx** に変換します。

        openssl pkcs12 -export -out myserver.pfx -inkey myserver.key -in myserver.crt

    メッセージが表示されたら、パスワードを入力して .pfx ファイルをセキュリティ保護します。
    > [AZURE.NOTE] CA が中間証明書を使用する場合、次のステップで証明書をエクスポートする前に、それらの中間証明書をインストールする必要があります。 通常、これらの証明書は CA から個別のダウンロードとして提供されており、Web サーバーの種類に応じていくつかの形式で提供されています。 PEM ファイル (ファイル拡張子は .pem) の形で提供されているバージョンを選択します。  
    >
    > 次のコマンドは、**intermediate-cets.pem** ファイルに格納されている中間証明書を含む .pfx ファイルを作成する方法を示しています。  
    >
    >
    `````
    openssl pkcs12 -chain -export -out myserver.pfx -inkey myserver.key -in myserver.crt -certfile intermediate-cets.pem
    `````

    このコマンドを実行すると、Azure App Service での使用に適した **myserver.pfx** ファイルが生成されます。

<a name="bkmk_selfsigned"></a>
### 自己署名証明書を生成する (テスト目的専用)

テスト用の証明書を取得し、信頼された CA からの証明書の実際の購入は、運用開始時に行いたい場合もあります。 この場合、自己署名証明書が有効です。 自己署名証明書では、自身が証明機関 (CA) となり、証明書を作成および署名できます。 この証明書は、アプリのセキュリティ保護に使用できますが、ほとんどのブラウザーでに信頼された CA 証明書の署名がないと、アプリケーションにアクセスしたときにエラーが返されます。 ブラウザーによっては、アプリを表示することができるように拒否もあります。

- [Makecert を使用した自己署名証明書の生成します。](#bkmk_ssmakecert)
- [OpenSSL を使用した自己署名証明書の生成します。](#bkmk_ssopenssl)

<a name="bkmk_ssmakecert"></a>
#### makecert を使用した自己署名証明書の生成

Visual Studio がインストールされている Windows システムからテスト証明書を作成する手順は次のとおりです:

1. **[スタート] メニュー**または**スタート画面**で、「**開発者コマンド プロンプト**」を検索します。 最後に、**[開発者コマンド プロンプト]** を右クリックし、**[管理者として実行]** をクリックします。

    [ユーザー アカウント制御] ダイアログ ボックスが表示された場合、**[はい]** をクリックして続行します。

2. [開発者のコマンド プロンプト] で、新しい自己署名証明書を作成するために次のコマンドを使用します。 置き換える必要があります **serverdnsname** 、アプリの DNS とします。

        makecert -r -pe -b 01/01/2013 -e 01/01/2014 -eku 1.3.6.1.5.5.7.3.1 -ss My -n CN=serverdnsname -sky exchange -sp "Microsoft RSA SChannel Cryptographic Provider" -sy 12 -len 2048

    このコマンドが作成する証明書は、2013 年 1 月 1 日から 2014 年 1 月 1 日まで有効で、CurrentUser の証明書ストア内の位置に格納されます。

3. **[スタート] メニュー**または**スタート画面**で、「**Windows PowerShell**」を検索し、このアプリケーションを開始します。

4. Windows PowerShell プロンプトで、以前に作成された証明書をエクスポートするために、次のコマンドを使用します:

        $mypwd = ConvertTo-SecureString -String "password" -Force -AsPlainText
        get-childitem cert:\currentuser\my -dnsname serverdnsname | export-pfxcertificate -filepath file-to-export-to.pfx -password $mypwd

    これは $mypwd にセキュリティ保護された文字列として指定したパスワードを格納し、**dnsname** パラメーターによって指定された DNS 名を使用して証明書を検索し、**filepath** パラメーターに指定されたファイルにエクスポートします。 パスワードを含むセキュリティ保護された文字列は、エクスポートしたファイルをセキュリティ保護するために使用されます。

<a name="bkmk_ssopenssl"></a>
#### OpenSSL を使用した自己署名証明書の生成

1. **serverauth.cnf** いう名前の新しいドキュメントを作成し、このファイルの内容として次を使用します。

     [ req ]
     default_bits           = 2048
     default_keyfile        = privkey.pem
     distinguished_name     = req_distinguished_name
     attributes             = req_attributes
     x509_extensions        = v3_ca
    
     [ req_distinguished_name ]
     countryName         = Country Name (2 letter code)
     countryName_min         = 2
     countryName_max         = 2
     stateOrProvinceName     = State or Province Name (full name)
     localityName            = Locality Name (eg, city)
     0.organizationName      = Organization Name (eg, company)
     organizationalUnitName      = Organizational Unit Name (eg, section)
     commonName          = Common Name (eg, your app's domain name)
     commonName_max          = 64
     emailAddress            = Email Address
     emailAddress_max        = 40
    
     [ req_attributes ]
     challengePassword       = A challenge password
     challengePassword_min       = 4
     challengePassword_max       = 20
    
     [ v3_ca ]
      subjectKeyIdentifier=hash
      authorityKeyIdentifier=keyid:always,issuer:always
      basicConstraints = CA:false
      keyUsage=nonRepudiation, digitalSignature, keyEncipherment
      extendedKeyUsage = serverAuth

 これは、Azure App Service で使用できる SSL 証明書の生成に必要な構成設定を指定します。

2. コマンド ライン、bash、またはターミナル セッションから次を入力し、新しい自己署名証明書を生成します:

        openssl req -x509 -nodes -days 365 -newkey rsa:2048 -keyout myserver.key -out myserver.crt -config serverauth.cnf

    これは **serverauth.cnf** ファイルで指定された構成設定を使用して、新しい証明書を作成します。

3. 証明書をエクスポートします。Azure App Service でアプリにアップロード可能な PFX ファイルは、次のコマンドを使用します。

        openssl pkcs12 -export -out myserver.pfx -inkey myserver.key -in myserver.crt

    メッセージが表示されたら、パスワードを入力して .pfx ファイルをセキュリティ保護します。

    **Myserver.pfx** これによって生成されたテストのためにアプリをセキュリティで保護するコマンドを使用することができます。

<a name="bkmk_standardmode"></a>
## 2.Standard 価格レベルを構成する

カスタム ドメインはのみに HTTPS を有効にすると、 **標準** Azure App Service での層です。 App Service プランを **Standard** レベルに切り替えるには、次の手順を実行します。
> [AZURE.NOTE] アプリを切り替える前に、 **Free** レベルから **標準** 層を削除して、サブスクリプションの場所に設定されている使用、請求期間が終了する前に、制限に達した場合に、アプリが使用できなくなるをおそれするそれ以外の場合。 詳細については、共有と **標準** 層は、「 [[価格] の料金詳細][pricing]します。

1.  ブラウザーで開く、 [Azure ポータル](http://go.microsoft.com/fwlink/?LinkId=529715)します。
2.  ページの左側にある **[参照]** オプションをクリックします。
3.  **[Web Apps]** ブレードをクリックします。
4.  アプリの名前をクリックします。
5.  **[要点]** ページで **[設定]** をクリックします。
6.  クリックして **スケール**
    ![[スケール] タブ][scale]
7.  **スケール** 」をクリックして、App Service プランのモードを設定 **選択**します。
    ![The Pricing tier][sslreserved]
    > [AZURE.NOTE] 「のスケールの構成 web アプリ '< アプリ名 >' が失敗しました」エラーが発生する場合は、詳細情報を表示、詳細ボタンを使用することができます。 「十分な使用可能な標準インスタンス サーバーをこの要求を満たすために」が表示される場合があります。 エラーがあります。 このエラーが発生した場合にお問い合わせください [Azure サポート](/support/options/)します。

<a name="bkmk_configuressl"></a>
## 3.アプリで SSL を構成する

このセクションの手順を実行する前にはアプリにカスタム ドメイン名が関連付けられたがする必要があります。 詳細については、次を参照してください。 [[customdomain] の web アプリのカスタム ドメイン名の構成][customdomain]します。

1.  ブラウザーで開く、 [Azure 管理ポータル](https://portal.azure.com)します。
2.  ページの左側にある **[参照]** オプションをクリックします。
3.  **[Web Apps]** ブレードをクリックします。
4.  アプリの名前をクリックします。
5.  **[要点]** ページで **[設定]** をクリックします。
6.  **[カスタム ドメインと SSL]** をクリックします。
    ![The config tab][sslconfig]
7.  **[証明書]** セクションで、**[アップロード]** をクリックします。
8.  **[証明書をアップロードします]** ダイアログを使用して、IIS マネージャーまたは OpenSSL によって既に作成されている .pfx 証明書ファイルを選択します。 .pfx ファイルをセキュリティ保護するために使用されたパスワードがある場合、それを指定します。 最後に、クリックして、 **保存** 、証明書をアップロードします。
    ![ssl upload][ssluploadcert]
9. **[SSL の設定]** タブの **[SSL バインド]** セクションで、ドロップダウン リストから、SSL で保護するドメイン名と使用する証明書を選択します。 使用するかどうかを選択することも [Server Name Indication ][sni] (SNI) または IP ベースの SSL。

    ![SSL のバインディング][sslbindings]

    * IP ベースの SSL は、サーバーの専用パブリック IP アドレスをドメイン名にマッピングすることによって、証明書をドメイン名に関連付けします。 各ドメイン名 (contoso.com、fabricam.com など) が必要です。 専用の IP アドレス、サービスに関連付けられています。 これは SSL 証明書と Web サーバーを関連付ける従来の方式です。

    * SNI ベースの SSL は SSL の拡張と [トランスポート層セキュリティ ][tls] (TLS) ドメインごとに個別のセキュリティ証明書と同じ IP アドレスを共有する複数のドメインを利用できます。 最新のブラウザー (Internet Explorer、Chrome、Firefox、および Opera を含む) のほとんどが SNI をサポートしていますが、古いブラウザーには、SNI をサポートしていないものもあります。 SNI の詳細については、次を参照してください。、 [Server Name Indication ][sni] Wikipedia の記事です。

10. 変更を保存して SSL を有効にするには、**[保存]** をクリックします。

> [AZURE.NOTE] **[IP ベースの SSL]** を選択し、カスタム ドメインが A レコードを使用して構成されている場合は、次の追加手順を実行する必要があります。
>
> 1.構成した後は、IP ベースの SSL バインド、アプリに専用の IP アドレスが割り当てられます。 この IP アドレスを確認することができます、 **ダッシュ ボード** アプリのページで、 **概要** セクションです。 このアドレスは、**仮想 IP アドレス**として示されます。
>    
>     ![Virtual IP address](./media/configure-ssl-web-site/staticip.png)
>    
>     この IP アドレスは、ドメイン用の A レコードを構成するために以前使用した仮想 IP アドレスとは異なります。 SNI ベースの SSL を使用するように構成する場合、または SSL を使用するように構成しない場合は、このエントリに対してアドレスは表示されません。
>
> 2.ドメイン名レジストラーから提供されるツールを使用して、前の手順の IP アドレスを指定するようにカスタム ドメイン名用の A レコードを変更します。


この時点を使用してアプリを参照してください。 できなければならない `HTTPS://` の代わりに `HTTP://` 証明書が正しく構成されていることを確認します。

<a name="bkmk_enforce"></a>
## 4.アプリにおける HTTPS を適用します。

Azure App Service は HTTPS を適用*しません*。 訪問者には、引き続き HTTP では、アプリのセキュリティが損なわれる可能性を使用して、アプリがアクセスできることです。 アプリの HTTPS を適用する場合は、使用、 **URL 書き換え** モジュールです。 URL 書き換えモジュールは Azure App Service に組み込まれており、それを使用して、受信した要求をアプリケーションに渡す前に要求に適用されるルールを定義できます。 **このモジュールは、Azure がサポートするプログラミング言語で記述されたアプリケーションで使用できます。**
> [AZURE.NOTE] .NET MVC アプリケーションを使用する必要があります、 [RequireHttps](http://msdn.microsoft.com/library/system.web.mvc.requirehttpsattribute.aspx) URL 書き換えの代わりにフィルターします。 RequireHttps の使用方法の詳細については、次を参照してください。 [web アプリにセキュリティで保護された ASP.NET MVC 5 アプリケーションをデプロイ](../articles/app-service-web/web-sites-dotnet-deploy-aspnet-mvc-app-membership-oauth-sql-database.md)します。
>
> 他のプログラミング言語とフレームワークを使用して要求をプログラム的にリダイレクトする方法の詳細については、該当するテクノロジのドキュメントを参照してください。

URL 書き換えルールは、アプリケーションのルートに格納されている **web.config** ファイルに定義されます。 次の例には、すべての受信トラフィックに HTTPS の使用を強制する基本的な URL 書き換えルールが含まれています。

<a name="example"></a>**Web.config ファイルの URL 書き換えの例**

    <?xml version="1.0" encoding="UTF-8"?>
    <configuration>
      <system.webServer>
        <rewrite>
          <rules>
            <rule name="Force HTTPS" enabled="true">
              <match url="(.*)" ignoreCase="false" />
              <conditions>
                <add input="{HTTPS}" pattern="off" />
              </conditions>
              <action type="Redirect" url="https://{HTTP_HOST}/{R:1}" appendQueryString="true" redirectType="Permanent" />
            </rule>
          </rules>
        </rewrite>
      </system.webServer>
    </configuration>

このルールは、ユーザーが HTTP を使用してページを要求したときに HTTP 状態コード 301 (永続的なリダイレクト) を返すことで動作します。 301 は、訪問者が要求した URL と同じ URL へ要求をリダイレクトしますが、要求の HTTP 部分は HTTPS で置き換えられます。 たとえば、HTTP://contoso.com は、HTTPS://contoso.com にリダイレクトされます。
> [AZURE.NOTE] アプリケーションが記述されている場合  **Node.js**, 、**PHP**, 、**Python Django**, 、または **Java**, 、おそらく、web.config ファイルは含まれません。 ただし、**Node.js**、**Python Django**、**Java** はすべて、実際には、Azure App Service 上でホストされた場合に web.config を使用します。このファイルは、デプロイ時に自動的に作成されるため、目にすることはありません。 アプリケーションの一部としてこのファイルを含めると、Azure が自動的に生成したファイルは上書きされます。

### .NET

.NET アプリケーションの場合、アプリケーションの web.config ファイルを変更し、追加、 **< rewrite >** 」の「、 [例](#example) に **< システムです。Web サーバー >** セクションです。

Web.config ファイルが既に含まれている場合、 **< rewrite >** セクションで、追加、 **< 規則 >** から、 [例](#example) の最初のエントリとして、 **< 規則 >** セクションです。

### PHP

PHP アプリケーションの場合は、単に保存、 [例](#example) 、アプリケーションのルートで web.config ファイルとして再展開、アプリへのアプリケーションです。

### Node.js、Python Django、および Java

Node.js、Python Django、および Java アプリケーションの場合、web.config ファイルがまだ提供されていない場合は自動的に作成されますが、デプロイ時に作成されるため、サーバー上にのみ存在します。 自動生成されたファイルには、Azure にアプリケーションのホスト方法を伝える設定が含まれます。

取得して、アプリケーションから自動生成されたファイルを変更して、次の手順に従います。

1. FTP を使用してファイルをダウンロード (を参照してください [FTP や診断ログの収集ファイルのアップロード/ダウンロード](http://blogs.msdn.com/b/avkashchauhan/archive/2012/06/19/windows-azure-website-uploading-downloading-files-over-ftp-and-collecting-diagnostics-logs.aspx))します。

2. それをアプリケーションのルートに追加します。

3. 次の情報を使用して書き換えルールを追加します。

    * **Node.js と Python Django**

        Node.js および Python Django アプリケーションは既に用に生成された web.config ファイル、 **< rewrite >** セクションが含まれている **< 規則 >** アプリの正しく機能するために必要なエントリです。 HTTPS を使用するアプリには、追加、 **< 規則 >** の例の最初のエントリとして、 **< 規則 >** セクションです。 これにより、その他のルールは未変更のままで、HTTPS が強制的に使用されます。

    * **Java**

        Apache Tomcat を使用している Java アプリケーションの web.config ファイルには **&lt;rewrite&gt;** セクションが含まれていないため、例の **&lt;rewrite&gt;** セクションを **<system.webServer>** セクションに追加する必要があります。

4. プロジェクト (更新された web.config を含む) を Azure に再度デプロイします。

HTTPS を強制的に使用する書き換えルールを伴う web.config をデプロイすると、すぐに有効となり、すべての要求が HTTPS にリダイレクトされます。

IIS URL 書き換えモジュールの詳細については、次を参照してください。、 [URL 書き換え](http://www.iis.net/downloads/microsoft/url-rewrite) ドキュメントです。

## その他のリソース

- [Microsoft Azure トラスト センター](/support/trust-center/security/)
- [Azure の Web サイトでのロックを解除する構成オプション](/blog/2014/01/28/more-to-explore-configuration-options-unlocked-in-windows-azure-web-sites/)
- [診断ログの記録を有効にします。](../articles/app-service-web/web-sites-enable-diagnostic-log.md)
- [Azure App Service で web アプリを構成します。](../articles/app-service-web/web-sites-configure.md)
- [Azure 管理ポータル](https://manage.windowsazure.com)

>[AZURE.NOTE] 場合は、Azure アカウントがサインアップする前に Azure App Service の使用を開始するには、 [App Service の試用](http://go.microsoft.com/fwlink/?LinkId=523751), 、App Service で有効期間の短いスターター アプリをすぐに作成する場所です。 このサービスの利用にあたり、クレジット カードは必要ありません。契約も必要ありません。

## 変更内容

* Web サイトから App Service への変更のガイドを参照してください: [Azure App Service と既存の Azure サービスへの影響](http://go.microsoft.com/fwlink/?LinkId=529714)
* 古いポータルから新しいポータルへの変更ガイドについては、次を参照してください: [プレビュー ポータル内の移動に関するリファレンス](http://go.microsoft.com/fwlink/?LinkId=529715)。


[customdomain]: ../articles/app-service-web/web-sites-custom-domain-name.md 
[iiscsr]: http://technet.microsoft.com/library/cc732906(WS.10).aspx 
[cas]: http://go.microsoft.com/fwlink/?LinkID=269988 
[installcertiis]: http://technet.microsoft.com/library/cc771816(WS.10).aspx 
[exportcertiis]: http://technet.microsoft.com/library/cc731386(WS.10).aspx 
[openssl]: http://www.openssl.org/ 
[portal]: https://manage.windowsazure.com/ 
[tls]: http://en.wikipedia.org/wiki/Transport_Layer_Security 
[staticip]: ./media/configure-ssl-web-site/staticip.png 
[website]: ./media/configure-ssl-web-site/sslwebsite.png 
[scale]: ./media/configure-ssl-web-site/sslscale.png 
[standard]: ./media/configure-ssl-web-site/sslreserved.png 
[pricing]: /pricing/details/ 
[configure]: ./media/configure-ssl-web-site/sslconfig.png 
[uploadcert]: ./media/configure-ssl-web-site/ssluploadcert.png 
[uploadcertdlg]: ./media/configure-ssl-web-site/ssluploaddlg.png 
[sslbindings]: ./media/configure-ssl-web-site/sslbindings.png 
[sni]: http://en.wikipedia.org/wiki/Server_Name_Indication 
[certmgr]: ./media/configure-ssl-web-site/waws-certmgr.png 
[certwiz1]: ./media/configure-ssl-web-site/waws-certwiz1.png 
[certwiz2]: ./media/configure-ssl-web-site/waws-certwiz2.png 
[certwiz3]: ./media/configure-ssl-web-site/waws-certwiz3.png 
[certwiz4]: ./media/configure-ssl-web-site/waws-certwiz4.png 

