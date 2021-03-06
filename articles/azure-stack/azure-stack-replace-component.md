---
title: "Azure Stack スケール ユニット ノードのハードウェア コンポーネントを交換する | Microsoft Docs"
description: "Azure Stack 統合システムのハードウェア コンポーネントの交換方法について説明します。"
services: azure-stack
documentationcenter: 
author: mattbriggs
manager: femila
editor: 
ms.assetid: c6e036bf-8c80-48b5-b2d2-aa7390c1b7c9
ms.service: azure-stack
ms.workload: na
pms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 1/29/2018
ms.author: mabrigg
ms.openlocfilehash: 7018f0122ab1ef11d64cce8a9adf58419d0e9ba7
ms.sourcegitcommit: 9d317dabf4a5cca13308c50a10349af0e72e1b7e
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 02/01/2018
---
# <a name="replace-a-hardware-component-on-an-azure-stack-scale-unit-node"></a>Azure Stack スケール ユニット ノードのハードウェア コンポーネントを交換する

*適用対象: Azure Stack 統合システム*

この記事では、ホットスワップが可能でないハードウェア コンポーネントを交換するための一般的なプロセスについて説明します。 実際の交換手順は、ご利用の OEM (Original Equipment Manufacturer) ハードウェア ベンダーによって異なります。 Azure Stack 統合システムに特化した詳しい手順については、ベンダーの現場交換可能ユニット (FRU) ドキュメントをご覧ください。

ホットスワップが可能でないコンポーネントには、以下が含まれます。

- CPU*
- メモリ*
- マザーボード/ベースボード管理コントローラー (BMC)/ビデオ カード
- ディスク コントローラー/ホスト バス アダプター (HBA)/バックプレーン
- ネットワーク アダプター (NIC)
- オペレーティング システム ディスク*
- データ ドライブ (ホットスワップをサポートしないドライブ。例: PCI-e アドイン カード)*

* これらのコンポーネントはホットスワップをサポートしている場合がありますが、ベンダーの実装によって異なる可能性があります。 詳しい手順については、OEM ベンダーの FRU ドキュメントを参照してください。

次のフロー図は、ホットスワップが可能でないハードウェア コンポーネントを交換するための一般的な FRU プロセスを示しています。

![コンポーネント交換フローを示すフロー図](media/azure-stack-replace-component/replacecomponentflow.PNG)

* このアクションは、ハードウェアの物理的な状態によっては必須でありません。

** OEM ハードウェア ベンダーがコンポーネントの交換とファームウェア更新を行うかどうかは、サポート契約に応じて異なる可能性があります。

## <a name="review-alert-information"></a>アラート情報を見直す

Azure Stack の正常性および監視システムは、記憶域スペース ダイレクトによって制御されているネットワーク アダプターとデータ ドライブの正常性を追跡します。 その他のハードウェア コンポーネントは追跡しません。 その他のすべてのハードウェア コンポーネントでは、ハードウェア ライフサイクル ホスト上で実行されているベンダー固有のハードウェア監視ソリューションでアラートが発生します。  

## <a name="component-replacement-process"></a>コンポーネント交換プロセス

次の手順は、コンポーネント交換プロセスの概要を示しています。 OEM 提供の FRU マニュアルを参照せずにこれらの手順を実行しないでください。

1. [ドレイン](azure-stack-node-actions.md#scale-unit-node-actions) アクションを使用して、スケール ユニット ノードをメンテナンス モードにします。 このアクションは、ハードウェアの物理的な状態によっては必須でありません。

   > [!NOTE]
   > いずれの場合も、S2D (記憶域スペース ダイレクト) を中断せずに一度にドレインして電源をオフにできるノードは 1 つだけです。

2. スケール ユニット ノードがメンテナンス モードに入ったら、[電源オフ](azure-stack-node-actions.md#scale-unit-node-actions) アクションを使用します。 このアクションは、ハードウェアの物理的な状態によっては必須でありません。

   > [!NOTE]
   > 可能性は低いですが、電源オフ アクションが機能しない場合は、代わりに BMC (Baseboard Management Controller) Web インターフェイスを使用します。

3. 破損したハードウェア コンポーネントを交換します。 OEM ハードウェア ベンダーがコンポーネントの交換を行うかどうかは、サポート契約に応じて異なる場合があります。  
4. ファームウェアを更新します。 ハードウェア ライフサイクル ホストを使用するベンダー固有のファームウェア更新プロセスに従って、交換したハードウェア コンポーネントに承認されたファームウェア レベルが適用されていることを確認します。 OEM ハードウェア ベンダーがこの手順を行うかどうかは、サポート契約に応じて異なる場合があります。  
5. [修復](azure-stack-node-actions.md#scale-unit-node-actions)アクションを使用して、スケール ユニット ノードをスケール ユニットに戻します。
6. 特権エンドポイントを使用して、[仮想ディスクの修復の状態を確認します](azure-stack-replace-disk.md#check-the-status-of-virtual-disk-repair)。 新しいデータ ドライブを使用するストレージ修復ジョブ全体は、システムの負荷と消費される領域に応じて、数時間かかる可能性があります。
7. 修復アクションが完了したら、すべてのアクティブなアラートが自動的に閉じていることを確認します。

## <a name="next-steps"></a>次の手順

- ホットスワップ可能物理ディスクの交換については、[ディスクの交換](azure-stack-replace-disk.md)に関する記事を参照してください。
- 物理ノードを交換する方法については、[スケール ユニット ノードの交換](azure-stack-replace-node.md)に関する記事を参照してください。
