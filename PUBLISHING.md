# 新しい作品・新しい版を上げる手順

このリポジトリは public です。ソースコードは入れず、説明（README）と画像だけを置きます。
ゲーム本体の zip は git に入れず、Releases に添付します（1ファイル2GBまで。git は100MBで拒否され、履歴も重くなるため）。

## ルール

| 項目 | 決め方 | 例 |
|---|---|---|
| フォルダー名 | ゲーム名を小文字英数字で | `aetheredge/` |
| タグ | `フォルダー名-vバージョン` | `aetheredge-v0.1` |
| zip 名 | `ゲーム名-版-vバージョン.zip` | `AetherEdge-exhibition-v0.1.zip` |
| 画像 | `images/フォルダー名-内容.png` | `images/aetheredge-title.png` |

タグの頭にゲーム名を付けるのは、複数の作品の版が Releases で混ざらないようにするためです。

## 手順（約15分）

1. ゲームをビルドする（各作品のリポジトリの手順で）
2. 配布しない物を消してから zip にする
   - Unity: `*_BackUpThisFolder_ButDontShipItWithYourGame`（デバッグ情報）、`*_BurstDebugInformation_DoNotShip`
   - その他: 開発用のログや設定ファイル、`.pdb`
   - zip の中は「ゲーム名のフォルダー1つ」にする（展開した時にファイルが散らばらないように）
   ```powershell
   Compress-Archive -Path .\AetherEdge -DestinationPath .\AetherEdge-exhibition-v0.1.zip
   ```
3. zip を別のフォルダーに展開し、exe が起動することを確かめる
4. 新しい作品なら `フォルダー名/README.md` を作る（`aetheredge/README.md` を写して使う: 紹介・ダウンロード表・動作環境・操作）
5. ルートの `README.md` のゲーム一覧に1行足す。新しい版なら「最新版」とリンクを書き換え、作品の README のダウンロード表にも1行足す
6. コミットして push する
   ```bash
   git add -A && git commit -m "作品名 v0.2 を追加" && git push
   ```
7. Release を作って zip を添付する
   ```bash
   gh release create aetheredge-v0.2 AetherEdge-v0.2.zip --title "AETHER EDGE v0.2" --notes "変更点を箇条書きで"
   ```
8. ログアウトしたブラウザー（シークレットウィンドウ）で README のリンクを開き、zip をダウンロードできることを確かめる

## 上げる前のチェック

- [ ] zip にソースコード・APIキー・パスワード・個人情報が入っていない
- [ ] 使った素材（フォント・音・画像）のライセンスが再配布を許している。必要ならライセンス文を zip に同梱する（AETHER EDGE はフォント DotGothic16 が OFL）
- [ ] 展開した zip で起動できた

## コードを読まれにくくする（任意）

Unity の既定（Mono）でビルドすると、同梱される `Assembly-CSharp.dll` から無料のツールでほぼ元の C# を復元できます。
読まれにくくしたい場合は IL2CPP でビルドします。

1. Unity Hub でその Unity の「Windows Build Support (IL2CPP)」モジュールを追加する
2. Visual Studio Installer で「C++ によるデスクトップ開発」を入れる
3. Player Settings → Scripting Backend を IL2CPP にする
