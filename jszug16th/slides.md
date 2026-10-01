---
layout: title
theme: custom
theme-file: ./themes/ms-modern/theme.css
deck: JAZUG 16th Short Session
title: GitHub Copilot SDK × Microsoft Foundry
---

# GitHub Copilot SDK Agent を Foundry にデプロイ

### BYOK で Foundry のモデル deployment を指定

**JAZUG 16th Short Session**

<!--
目安: 0:30
Copilot SDKのagentハーネスは維持し、BYOKでMicrosoft Foundryのモデルdeploymentを推論先に指定します。Hosted Agentとして実行する構成とデモを紹介します。
-->

---

## Copilot SDK と BYOK

- Copilot SDK は、アプリケーションから agent のハーネスを利用する SDK
- BYOK では、モデル provider と endpoint を SDK の session に設定
- `model` には Foundry の **model deployment 名**を指定
- `/openai/v1/` endpoint では provider type は `openai`

<!--
目安: 1:15
BYOKはGitHub Copilot認証を推論経路に使わず、設定したモデルproviderを利用します。Foundryではカタログ上のモデル名ではなく、Foundryリソースで作成したdeployment名をmodelとして指定します。Responses API対応は選択モデルとruntimeで確認します。
-->

---

## 構成要素と役割

| コンポーネント | このデモでの役割 |
|---|---|
| GitHub Copilot SDK | session、tool、agent harness |
| BYOK provider 設定 | Foundry endpoint と deployment 名を指定 |
| Microsoft Agent Framework | SDK agent を `AIAgent` として構成 |
| Foundry Hosted Agent | agent container と endpoint を管理 |
| Aspire | Foundry リソースと agent のデプロイを記述 |

推論要求は Copilot SDK から Foundry model endpoint へ送る。

<!--
目安: 1:15
SDKのBYOK設定とMAFのagent抽象化は別の役割です。MAF providerが採用バージョンでSDKのprovider設定をどう受け取るかは、実装時に確認します。
-->

---

## DEMO 1｜ローカル実行

**質問：** 「実行環境を教えて」

1. Foundry deployment を指定した Copilot SDK agent に入力
2. agent がシェル実行ツールを選択して環境を確認
3. ツールの実行結果を用いて回答

**ツール実行の許可対象：** デモに必要な操作に限定

<!--
目安: 2:15（説明0:30 + デモ1:45）
同じFoundry deploymentを推論先にした状態で、組み込みシェルツールを使う流れを示します。ローカルの認証は開発用Entra IDまたはAPI keyを使い、資格情報はソースやイメージに含めません。
-->

---

## Hosted Agent の実行とモデル認証

- Foundry Agent Service が agent container と endpoint を管理
- BYOK token provider が使う実行 principal を特定
- keyless 認証では `https://ai.azure.com/.default` の Entra token を使用
- endpoint と deployment に対応する推論 RBAC を確認

<!--
目安: 1:15
ここでは二つの認証を区別します。クライアントからHosted Agent endpointへの認証と、Hosted Agent内のBYOK providerからモデルendpointへの認証です。後者はtoken providerが実際に使うprincipalを確認します。Foundry projectのmanaged identityにproject endpoint用のFoundry Userロールがあることは、BYOKの直接endpoint呼び出しに使う別principalの権限を意味しません。`/openai/v1/` のkeyless推論ではscopeは `https://ai.azure.com/.default` です。必要なロールはモデルとendpointにより異なり、OpenAIモデル専用なら `Cognitive Services OpenAI User`、より広いFoundryモデルの推論では `Cognitive Services User` または `Foundry User` が候補です。選択deploymentの要件を確認します。
-->

---

## DEMO 2｜Aspire で Azure にデプロイ

```text
Copilot SDK agent
  └─ BYOK: Foundry endpoint + deployment 名
       ↓ Microsoft Agent Framework
Hosted Agent adapter
       ↓ Aspire で構成・デプロイ
Microsoft Foundry Hosted Agent
```

<!--
目安: 1:15
Aspireを使った構成とデプロイを示します。AspireはBYOKのprovider設定、実行時の認証トークン供給、ツールの安全性を自動で決めるものではありません。デプロイ済み環境を使う場合は設定箇所だけ短く説明します。
-->

---

## DEMO 2｜Hosted Agent の実行

- Hosted Agent endpoint にリクエスト
- ローカルと同じ Foundry deployment による応答を確認
- session 実行と tool call を確認

<!--
目安: 1:30
デプロイしたagentにローカルと同じ入力を与え、推論先がFoundry deploymentであること、ツール実行と応答を確認します。デプロイ操作に時間がかかる場合は、事前にデプロイしたendpointを使います。
-->

---

## まとめ

- Copilot SDK の BYOK で Foundry model deployment を推論先に指定
- Microsoft Agent Framework で SDK agent を構成
- Aspire を用いて Foundry Hosted Agent としてデプロイ

<!--
目安: 0:45
Copilot SDKのagent harnessを維持しながら、推論先をMicrosoft Foundryのmodel deploymentとして明示できます。ローカルとHosted Agentの両方で、deployment名、endpoint、実行IDの権限を確認することが重要です。全体で約10分です。
-->
