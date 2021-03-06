---
title: Azure SQL Database でのテーブル監査、TDS リダイレクト、IP エンドポイント | Microsoft Docs
description: Azure SQL Database でテーブル監査を実装する場合の、監査、TDS リダイレクト、IP エンドポイント変更について説明します。
services: sql-database
author: giladm
manager: craigg
ms.service: sql-database
ms.custom: security
ms.topic: article
ms.date: 04/01/2018
ms.author: giladm
ms.openlocfilehash: d1114d6c5073aa6e60d6fb700989fb3d205ee914
ms.sourcegitcommit: 3a4ebcb58192f5bf7969482393090cb356294399
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/06/2018
---
# <a name="sql-database----downlevel-clients-support-and-ip-endpoint-changes-for-table-auditing"></a>SQL Database - テーブル監査のためのダウンレベル クライアントのサポートと IP エンドポイントの変更

> [!IMPORTANT]
> このドキュメントの内容は、**現在は非推奨**となっているテーブル監査についてのみ適用されます。<br>
> 新しい [BLOB 監査](sql-database-auditing.md)をご使用ください。BLOB 監査では、ダウンレベル クライアントの接続文字列の変更が**不要**です。 BLOB 監査に関する追加情報については、「[SQL Database 監査の使用](sql-database-auditing.md)」をご覧ください。

[データベース監査](sql-database-auditing.md) は TDS リダイレクションに対応する SQL クライアントと自動的に連動します。 なお、Blob 監査メソッドを使用している場合、リダイレクトは適用されません。

## <a id="subheading-1"></a>ダウンレベル クライアントのサポート
TDS 7.4 を実装するクライアントもリダイレクトをサポートします。 この例外には一部のリダイレクション機能に対応していない JDBC 4.0 とリダイレクションが実装されていない Tedious for Node.JS があります。

「ダウンレベル クライアント」、つまり、TDS バージョンが 7.3 以前のクライアントの場合、接続文字列のサーバー FQDN を変更する必要があります。

接続文字列の元のサーバー FQDN: <*server name*>.database.windows.net

接続文字列の変更後のサーバー FQDN: <*server name*>.database.**secure**.windows.net

「ダウンレベル クライアント」には次が含まれます。

* .NET 4.0 以前
* ODBC 10.0 以前
* JDBC (JDBC は TDS 7.4 対応ですが、一部の TDS リダイレクション機能に対応していません。)
* Tedious (Node.JS 用)

**注記:** 上のサーバー FDQN 変更は SQL サーバー レベル監査ポリシーの適用にも役に立ちます。データベースごとの構成が必要ありません (一時的な軽減)。

## <a id="subheading-2"></a>監査を有効にしたときの IP エンドポイントの変更
テーブル監査を有効にすると、データベースの IP エンドポイントが変更されます。 ファイアウォールを厳密に設定している場合は、この変更に従ってファイアウォールの設定を更新してください。

データベースの新しい IP エンドポイントは、データベース リージョンによって異なります。

| データベース リージョン | 使用可能な IP エンドポイント |
| --- | --- |
| 中国 (北部) |139.217.29.176、139.217.28.254 |
| 中国 (東部) |42.159.245.65、42.159.246.245 |
| オーストラリア東部 |104.210.91.32、40.126.244.159、191.239.64.60、40.126.255.94 |
| オーストラリア南東部 |191.239.184.223、40.127.85.81、191.239.161.83、40.127.81.130 |
| ブラジル南部 |104.41.44.161、104.41.62.230、23.97.99.54、104.41.59.191 |
| 米国中央部 |104.43.255.70、40.83.14.7、23.99.128.244、40.83.15.176 |
| 米国中部 EUAP |52.180.178.16, 52.180.176.190 |
| 東アジア |23.99.125.133、13.75.40.42、23.97.71.138、13.94.43.245 |
| 米国東部 2 |104.209.141.31、104.208.238.177、191.237.131.51、104.208.235.50 |
| 米国東部 |23.96.107.223、104.41.150.122、23.96.38.170、104.41.146.44 |
| 米国東部 EUAP |52.225.190.86, 52.225.191.187 |
| インド中部 |104.211.98.219、104.211.103.71 |
| インド南部 |104.211.227.102、104.211.225.157 |
| インド西部 |104.211.161.152、104.211.162.21 |
| 東日本 |104.41.179.1、40.115.253.81、23.102.64.207、40.115.250.196 |
| 西日本 |104.214.140.140、104.214.146.31、191.233.32.34、104.214.146.198 |
| 米国中北部 |191.236.155.178、23.96.192.130、23.96.177.169、23.96.193.231 |
| 北ヨーロッパ |104.41.209.221、40.85.139.245、137.116.251.66、40.85.142.176 |
| 米国中南部 |191.238.184.128、40.84.190.84、23.102.160.153、40.84.186.66 |
| 東南アジア |104.215.198.156、13.76.252.200、23.97.51.109、13.76.252.113 |
| 西ヨーロッパ |104.40.230.120、13.80.23.64、137.117.171.161、13.80.8.37、104.47.167.215、40.118.56.193、104.40.176.73、40.118.56.20 |
| 米国西部 |191.236.123.146、138.91.163.240、168.62.194.148、23.99.6.91 |
| 米国西部 2 |13.66.224.156, 13.66.227.8 |
| 米国中西部 |52.161.29.186, 52.161.27.213 |
| カナダ中部 |13.88.248.106, 13.88.248.110 |
| カナダ東部 |40.86.227.82, 40.86.225.194 |
| 英国北部 |13.87.101.18, 13.87.100.232 |
| 英国南部 2 |13.87.32.202, 13.87.32.226 |
