+++
title = "go-fuzzyfinderのプレビューの高さを調節する"
description = "go-fuzzyfinderのプレビューの高さを調節する"
date = "2020-08-02"
tags = ["Go"]
slug = "go-fuzzyfinder"
+++

Goで簡単にfuzzy-finderのインターフェイスを用意してくれるライブラリ
[go-fuzzyfinder](https://github.com/ktr0731/go-fuzzyfinder)がある。

READMEに書かれているように書くと簡単動作を確認できるのだが、以下のように上の方に表示される。

![](/images/fuzzy.png)


下の方に表示したいときは、以下のように書くといい。

<!--more-->



```go
type Track struct {
	Name      string
	AlbumName string
	Artist    string
}

var tracks = []Track{
	{"foo", "album1", "artist1"},
	{"bar", "album1", "artist1"},
	{"foo", "album2", "artist1"},
	{"baz", "album2", "artist2"},
	{"baz", "album3", "artist2"},
}

func main() {
	spew.Dump("test")
	idx, err := fuzzyfinder.FindMulti(
		tracks,
		func(i int) string {
			return tracks[i].Name
		},
		fuzzyfinder.WithPreviewWindow(func(i, w, h int) string {
			if i == -1 {
				return ""
			}
			down := h - 10
			if down < 1 {
				down = 0
			}
			return fmt.Sprintf(strings.Repeat("\n", down)+"Track: %s (%s)\nAlbum: %s",
				tracks[i].Name,
				tracks[i].Artist,
				tracks[i].AlbumName)
		}))
	if err != nil {
		log.Fatal(err)
	}
	fmt.Printf("selected: %v\n", idx)
}
```

ここでいうhで高さが得られるため、それを使って単純に改行を下にズラしたいだけ入れるだけだ。
ターミナルの高さが低いときはズラさなくてもいいため、改行を入れないように条件分岐する。
