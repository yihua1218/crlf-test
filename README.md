# CRLF 和 LF 轉換與忽略測試指南

本指南將說明如何在 Git 中測試 CRLF 和 LF 的轉換與忽略功能。

在軟件開發中，不同操作系統使用不同的行尾符號。Windows 使用 CRLF (Carriage Return + Line Feed)，而 Unix/Linux 和 macOS 則使用 LF (Line Feed)。這可能會導致跨平台開發時出現行尾符號不一致的問題。

Git 提供了 `core.autocrlf` 配置來自動處理行尾符號轉換：

- `true`：在檢出代碼時將 LF 轉換為 CRLF，在提交代碼時將 CRLF 轉換為 LF。
- `input`：在提交代碼時將 CRLF 轉換為 LF，但檢出代碼時不做任何轉換。
- `false`：不進行任何行尾符號轉換。

通過設置這些配置，您可以確保在不同操作系統之間協作時，行尾符號保持一致，從而避免潛在的問題。

## 前置準備

1. 安裝 Git。
2. 準備一個測試目錄，例如 `/D:/crlf-test/`。

## 設置 Git 配置

在開始測試之前，您需要設置 Git 的配置來控制行尾轉換行為。

### 全局配置

```sh
git config --global core.autocrlf true  # 自動將 LF 轉換為 CRLF
git config --global core.autocrlf input # 只將 CRLF 轉換為 LF
git config --global core.autocrlf false # 不進行任何轉換
```

### 本地配置

在您的測試目錄中，您可以設置本地配置來覆蓋全局配置：

```sh
cd /D:/crlf-test/
git init
git config core.autocrlf true  # 自動將 LF 轉換為 CRLF
```

## 測試步驟

1. 創建一個包含 LF 行尾的文件：

```sh
echo -e "Line1\nLine2\nLine3" > lf-file.txt
```

2. 創建一個包含 CRLF 行尾的文件：

```sh
echo -e "Line1\r\nLine2\r\nLine3" > crlf-file.txt
```

3. 將文件添加到 Git 並提交：

```sh
git add lf-file.txt crlf-file.txt
git commit -m "Add LF and CRLF files"
```

4. 檢查文件的行尾格式：

```sh
git checkout .
```

5. 查看文件內容以確認行尾轉換：

```sh
cat lf-file.txt
cat crlf-file.txt
```

## 忽略行尾轉換

如果您希望 Git 忽略特定文件的行尾轉換，可以使用 `.gitattributes` 文件：

```sh
echo "*.txt text eol=lf" > .gitattributes
```

這將強制所有 `.txt` 文件使用 LF 行尾。

## 忽略行尾符號差異

如果您希望在 diff 中忽略 CRLF 和 LF 的差異，可以設置 Git 的配置來忽略行尾符號的變化：

### 全局配置

```sh
git config --global core.whitespace cr-at-eol
```

### 本地配置

在您的測試目錄中，您可以設置本地配置來覆蓋全局配置：

```sh
cd /D:/crlf-test/
git config core.whitespace cr-at-eol
```

這將告訴 Git 在 diff 時忽略行尾符號的差異。

## 測試步驟

1. 創建一個包含 LF 行尾的文件：

```sh
echo -e "Line1\nLine2\nLine3" > lf-file.txt
```

2. 創建一個包含 CRLF 行尾的文件：

```sh
echo -e "Line1\r\nLine2\r\nLine3" > crlf-file.txt
```

3. 將文件添加到 Git 並提交：

```sh
git add lf-file.txt crlf-file.txt
git commit -m "Add LF and CRLF files"
```

4. 修改文件內容並提交：

```sh
echo "New Line" >> lf-file.txt
echo "New Line" >> crlf-file.txt
git add lf-file.txt crlf-file.txt
git commit -m "Update LF and CRLF files"
```

5. 查看 diff 以確認行尾符號差異被忽略：

```sh
git diff HEAD~1 HEAD
```

## 結論

通過上述步驟，您可以測試 Git 中 CRLF 和 LF 的轉換與忽略功能，並根據需要進行配置。
