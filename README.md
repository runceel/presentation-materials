# プレゼン資料置き場

GitHub Copilot の presentation canvas で表示するプレゼンテーション資料を保管しています。

## 収録資料

| ファイル | 内容 |
| --- | --- |
| `back.md` | canvas の背面表示に関する資料 |
| `design-adr.md` | AI が設計書を歴史書にする前に |
| `github-copilot-app.md` | GitHub Copilot App の開発ワークフロー |
| `github-copilot-app-2.md` | GitHub Copilot App の紹介 |
| `jat-202607-hiroshima.md` | GitHub Copilot App の canvas 紹介 |
| `kinoko-takenoko.md` | presentation Skill のサンプル |
| `self-introduction.md` | 共通で使う自己紹介スライド |

`design-adr.memo.md` は `design-adr.md` の編集メモです。

## 表示方法

presentation canvas 拡張を導入したプロジェクトで、表示したい Markdown ファイルを指定します。

```text
このリポジトリの design-adr.md に従ってプレゼンしてください。
```

presentation canvas 拡張は次のリポジトリから導入できます。

https://github.com/runceel/github-copilot-app-presentation/tree/main/.github/extensions/presentation

画像は各 Markdown から `/assets/...` で参照します。
