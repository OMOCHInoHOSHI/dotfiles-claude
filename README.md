## このリポジトリについて
.claudeのグローバル設定の一部を共有するためのリポジトリ。


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


