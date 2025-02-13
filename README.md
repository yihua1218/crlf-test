# CRLF 和 LF 轉換與忽略測試指南

本指南將說明如何在 Git 中測試 CRLF 和 LF 的轉換與忽略功能。

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

## 結論

通過上述步驟，您可以測試 Git 中 CRLF 和 LF 的轉換與忽略功能，並根據需要進行配置。
