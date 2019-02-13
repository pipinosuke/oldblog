---
layout: post
title: TOC(Table Of Contents)を自動生成する
category: ["技術"]
description: "GithubMarkdownが対応していない、目次（TOC）を自動生成できるツールの紹介です"
image: programming.png
date: 2019-01-04T09:03:48+09:00
---

普段はMarkdown方式で記事を書いている僕。基本的に[TOC]とすることで目次を自動生成してくれるのだが、GithubMarkdownはなぜか対応していない。何かいい方法はないかとググってみたら**[github-markdown-toc.go](https://github.com/ekalinin/github-markdown-toc.go)**なるツールを見つけた。自動生成できるようなので紹介と使い方のメモ。

## インストール
``` bash
brew install github-markdown-toc
```

## 使い方
[こちら](https://github.com/ekalinin/github-markdown-toc.go)に説明があるのだが、一応メモ。

## まとめ
記事のページにこういう目次があることによってSEO対策にもなるし、読者にとっても読みやすくなるのでこういうツールがあると助かります。
