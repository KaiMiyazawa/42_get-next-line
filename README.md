# get_next_line

A 42 School project that implements a function to read lines from a file descriptor one at a time.

## 概要

`get_next_line`は、ファイルディスクリプタから一行ずつ読み取る関数を実装するプロジェクトです。この関数は、ファイルの終端に達するまで、呼び出されるたびに次の行を返します。

## 機能

- **基本実装**: 単一ファイルディスクリプタから行を読み取り
- **ボーナス実装**: 複数のファイルディスクリプタから同時に行を読み取り
- メモリ効率的な実装
- 任意のバッファサイズでの動作
- エラーハンドリング

## ファイル構成

### 基本実装
- `get_next_line.c` - メイン関数の実装
- `get_next_line.h` - ヘッダファイル
- `get_next_line_utils.c` - ユーティリティ関数

### ボーナス実装
- `get_next_line_bonus.c` - 複数FD対応のメイン関数
- `get_next_line_bonus.h` - ボーナス用ヘッダファイル
- `get_next_line_utils_bonus.c` - ボーナス用ユーティリティ関数

## コンパイル方法

```bash
# 基本バージョン
gcc -Wall -Wextra -Werror -D BUFFER_SIZE=42 get_next_line.c get_next_line_utils.c

# ボーナスバージョン
gcc -Wall -Wextra -Werror -D BUFFER_SIZE=42 get_next_line_bonus.c get_next_line_utils_bonus.c
```

## 使用方法

```c
#include "get_next_line.h"

int main(void)
{
    int fd;
    char *line;
    
    fd = open("test.txt", O_RDONLY);
    while ((line = get_next_line(fd)) != NULL)
    {
        printf("%s", line);
        free(line);
    }
    close(fd);
    return (0);
}
```

## 主要関数

### `char *get_next_line(int fd)`
- **引数**: ファイルディスクリプタ
- **戻り値**: 読み取った行（改行文字含む）、EOF到達時またはエラー時はNULL
- **説明**: ファイルから次の行を読み取って返す

### ユーティリティ関数
- `ft_calloc()` - メモリ確保と初期化
- `ft_strlen()` - 文字列長計算
- `ft_strchr()` - 文字検索
- `ft_strjoin()` - 文字列結合
- `ft_strlcat()` - 安全な文字列連結

## 実装の特徴

### 基本版の特徴
- 単一の静的変数を使用してバッファを管理
- シンプルな実装で理解しやすい

### ボーナス版の特徴
- 静的配列 `save[OPEN_MAX]` を使用
- 複数のファイルディスクリプタを同時に処理可能
- 各FDごとに独立したバッファを維持

## バッファサイズ

`BUFFER_SIZE`マクロでバッファサイズを制御できます：
- デフォルト: 1024バイト
- コンパイル時に `-D BUFFER_SIZE=値` で変更可能

## エラーハンドリング

以下の場合にNULLを返します：
- 無効なファイルディスクリプタ（fd < 0 または fd >= OPEN_MAX）
- 無効なバッファサイズ（BUFFER_SIZE <= 0）
- メモリ確保失敗
- 読み取りエラー

## メモリ管理

- 動的メモリ確保を適切に管理
- メモリリークを防ぐための適切なfree処理
- 呼び出し元が返された文字列を解放する責任

## 作成者

miyazawa.kai.0823

## 更新履歴

- 2023/12/18: 最終更新
- 2023/05/31: プロジェクト開始