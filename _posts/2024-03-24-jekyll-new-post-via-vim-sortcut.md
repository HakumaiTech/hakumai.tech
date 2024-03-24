---
layout: post
title: "vim ショートカットで jekyll の記事作成"
date: 2024-03-24 13:46:00 +0900
categories: ['vim']
---

vim `.jk` コマンド入力で `_posts/` 配下に新規記事を作成したい。 <br/>
`.vimrc` に以下のコードを追記すればOK

```vim
function! JekyllNewPost()
    let title = input('Post title: ')
    " タイトルからファイル名を生成（スペースをハイフンに置き換え、小文字に変換）
    let filename = tolower(substitute(title, ' ', '-', 'g'))
    " 現在の日付を YYYY-MM-DD の形式で取得
    let date = strftime('%Y-%m-%d')
    " ファイルパスを生成（_posts ディレクトリに配置）
    let filepath = '_posts/' . date . '-' . filename . '.md'
    " 新規ファイルを開く
    exec 'e ' . filepath
    " ファイルの先頭にフロントマターを挿入
    call append(0, ['---', 'layout: post', 'title: "' . title . '"', 'date: ' . date . ' 00:00:00 +0900', 'categories: ', '---', ''])
    " 挿入した後に2行目にカーソルを移動（編集を開始する場所）
    exec 'normal! 2G'
endfunction

" コマンド 'JekyllNewPost' を作成して関数を呼び出す
nnoremap .jk :call JekyllNewPost()<CR>
```

