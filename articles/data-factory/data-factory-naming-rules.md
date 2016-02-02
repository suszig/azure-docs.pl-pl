<properties 
    pageTitle="Data Factory - 名前付け規則 | Microsoft Azure" 
    description="Data Factory エンティティの名前付け規則について説明します。" 
    services="data-factory" 
    documentationCenter="" 
    authors="spelluru" 
    manager="jhubbard" 
    editor="monicar"/>

<tags 
    ms.service="data-factory" 
    ms.workload="data-services" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="10/15/2015" 
    ms.author="spelluru"/>


# Azure Data Factory - 名前付け規則

次の表に、Data Factory アーティファクトの名前付け規則を示します。



 名前| 名前の一意性| 検証チェック
:--- | :-------------- | :----------------
 Data Factory| Microsoft Azure 全体で一意です。名前の大文字と小文字を区別されません。つまり、MyDF と mydf は同じデータ ファクトリを表します。| <ul><li>各データ ファクトリは、ただ 1 つの Azure サブスクリプションに関連付けられます。</li><li>オブジェクト名はアルファベットまたは数字で始める必要があるあり、英字、数字、またはダッシュ (-) 文字を含めることができます</li>。<li>すぐに前にで、アルファベットまたは数字が続く必要がありますすべてのダッシュ (-) 文字コンテナーの名前に連続するダッシュ文字は使用できません。</li>。<li>名前が 3 ~ 63 文字にすることができます。</li></ul>
 リンクされたサービス/テーブル/パイプライン| データ ファクトリ内で一意です。名前の大文字と小文字は区別されません。| <ul><li>テーブル名の文字の最大数: 260 です。</li><li>オブジェクト名は、文字、数字、またはアンダー スコア (_) で開始する必要があります</li>。<li>次の文字は使用できません:"です"、"+","?"、"/"、"<”, ”>","*"、"%"、"&"、":","\\"。</li></ul>
 リソース グループ| Microsoft Azure 全体で一意です。名前の大文字と小文字は区別されません。| <ul><li>最大文字数: 1000年です。</li><li>名は、英字、数字、および次の文字を含めることができます。"-"、"_",","と"です。"です。</li></ul>




