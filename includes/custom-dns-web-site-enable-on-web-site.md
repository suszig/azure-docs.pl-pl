ドメイン名のレコードが反映されたら、それを Web アプリに関連付ける必要があります。 Web ブラウザーを使用してドメイン名を有効にするには、次の手順を使用します。

> [AZURE.NOTE] DNS システムを経由して反映されるまで、前の手順で作成された CNAME レコードの時間がかかることができます。 CNAME が反映されるまで、Web アプリに対してドメイン名を追加することはできません。 A レコードを使用している場合は、まで、web アプリに A レコードのドメイン名を追加できません、 **awverify** 前の手順で作成された CNAME レコードが反映されます。
>
> <a href="http://www.digwebinterface.com/">http://www.digwebinterface.com/</a> などのサービスを使用すると、CNAME を利用できるかどうかを確認できます。

1. ブラウザーで開く、 [Azure 管理ポータル](https://portal.azure.com)します。

2.  **Web Apps** ] タブで、選択に、web アプリの名前をクリックして **設定**, 、し、[ **カスタム ドメインと SSL**

    ![](./media/custom-dns-web-site/dncmntask-cname-6.png)

3.  **カスタム ドメインと SSL** ブレードで、をクリックして **外部ドメインの表示**します。

    ![](./media/custom-dns-web-site/dncmntask-cname-7.png)

4. 使用して、 **ドメイン名** ボックスにこの web アプリに関連付けるドメイン名を入力します。

    ![](./media/custom-dns-web-site/dncmntask-cname-8.png)

5. クリックして **保存** ドメイン名の構成を保存します。

    構成が完了したら、カスタム ドメイン名に一覧表示、 **ドメイン名** web アプリのセクションです。

この時点でブラウザーにカスタム ドメイン名を入力して、対象の Web アプリに正常にアクセスできることを確認できます。


