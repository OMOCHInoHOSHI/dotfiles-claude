## このリポジトリについて
.claudeのグローバル設定の一部を共有するためのリポジトリ。

| ファイル | 内容 |
| --- | --- |
| `CLAUDE.md` | 全プロジェクト共通の指示 |
| `settings.json` | deny ルール・モデル・effort などの設定（マシン固有部分はフィルタで除外して追跡） |
| `agents/reviewer.md` | 読み取り専用のレビュー用サブエージェント |

サブエージェントは最小限にし、基本はメインの会話（Opus）で進める方針。


## クローンについて

### ディレクトリ名を.claudeでクローンする

```bash
git clone git@github.com:OMOCHInoHOSHI/dotfiles-claude.git ~/.claude 
```

### 既に ~/.claude が存在する場合
```bash
git init
```

```bash
git remote add origin git@github.com:OMOCHInoHOSHI/dotfiles-claude.git
```

```
git fetch origin
```

```
git checkout -b main --track origin/main
```

上書きに注意。

### settings.json のフィルタ設定（クローン後に各マシンで1回）

`settings.json` の `hooks` / `statusLine` / `theme` / `skipDangerousModePermissionPrompt` は Orca などが書き込むマシン固有の値なので、git の clean フィルタでコミット対象から外している。フィルタの定義は `.git/config` にあり共有されないため、クローンしたマシンごとに設定する（要 `jq`）。

```bash
git config filter.strip-local.clean "jq 'del(.hooks, .statusLine, .theme, .skipDangerousModePermissionPrompt)'"
git config filter.strip-local.smudge cat
git config filter.strip-local.required true
```

#### 注意事項

- **フィルタ設定を忘れない**: 未設定のままだと、Orca などが書き込んだ hooks を含む `settings.json` がそのままコミットできてしまう（このリポジトリは PUBLIC）。クローン直後に上の3行を実行する。
- **別端末で Orca を使う場合**: クローン直後の `settings.json` には hooks / statusLine / theme が無い。Orca が不足している自分の hooks を検出して追加する仕組みを持っているので、Orca を起動すれば書き込まれる想定（追加のタイミングが Orca の起動時かエージェント起動時かは未確認）。Orca は自分の hooks だけを入れ替え、他の hooks（wakelock など）は残す。
- **wakelock を使う場合**: hooks は `/opt/homebrew/bin/wakelock` を直接呼ぶため端末ごとの設定になる。その端末に wakelock を入れて `wakelock auto on` を実行する（hooks が追加される想定。未確認）。
- **checkout / pull で除外部分が消える**: `git checkout` や `git pull` で `settings.json` が書き換わると、ローカルの除外部分（Orca / wakelock の hooks など）も消える。Orca は再起動、wakelock は `wakelock auto on` で書き戻す。
