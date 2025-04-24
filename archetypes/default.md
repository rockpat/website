---
title: '{{ replace .File.ContentBaseName "-" " " | title }}'
date: {{ now.Format "2006-01-02" }}
image: images/2025-thumbs/{{ .File.ContentBaseName }}.png
tags: []
draft: true
---


## Video

{{< youtube "$VIDEO_URL" >}}
