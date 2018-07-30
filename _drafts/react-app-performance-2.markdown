---
layout: post
comments: true
title:  "React App Performance 2"
tagline: "Code Splitting"
date:   2018-07-16 07:26:00
author: "José Del Valle"
categories: main
tags:
- React
- Performance
- Webpack
---

## Code Splitting

### Dependencies 
npm install react-loadable --save-dev

npm install babel-preset-stage-3 --save-dev

### On to babelrc

```javascript
{
    "presets": [
        ["env", {
            "targets": {
                "browsers": ["last 2 versions"]
            }
        }] ,
        "stage-2",
        "stage-3",
        "react" 
    ],
    "plugins": [
        "transform-react-jsx-img-import",
        "transform-class-properties",
        ["lodash", { "id":["lodash", "recompose"] }]
    ]
}
```

### Webpack config



### Loadable components


### How the bundles look now

### Load times