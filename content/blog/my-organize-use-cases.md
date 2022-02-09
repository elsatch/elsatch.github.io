---
title: "My Organize tool use cases"
description: "meta description"
image: "images/post/organize-use-cases.png"
date: 2022-02-09T14:00:00+02:00
categories: ["python", "organize-tool"]
type: "featured" # available types: [featured/regular]
draft: false
---

{{ $js := resources.Get "mod/mermaidjs/mermaid.min.js" }}
{{ $secureJS := $js | resources.Fingerprint "sha512" }}
<script src="{{ $secureJS.Permalink }}" integrity="{{ $secureJS.Data.Integrity }}"></script>
<script>
  var config = {
    startOnLoad: true,
    flowchart: {
      useMaxWidth: true,
      htmlLabels: true,
      curve: "cardinal",
    },
    theme: "neutral",
    securityLevel: "strict",
  };

  mermaid.initialize(config);
</script>

#### Current structure

```mermaid
graph LR;
    id1([Local file])
    id2([Local NAS])
    id3([Google Drive])
    id1 --> id2;
    id1 --> id3;
```

#### Semi automated processes

Sorting my pictures

#### Interactive disk organization

Exploring from terminal

Photo credits: Photo by Sharon McCutcheon on Unsplash
  