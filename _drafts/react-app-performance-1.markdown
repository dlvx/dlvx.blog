---
layout: post
comments: true
title:  "React App Performance 1"
tagline: "Bundles, file sizes and loadtimes"
date:   2018-07-03 07:26:00
author: "José Del Valle"
categories: main
tags:
- React
- Performance
- Webpack
---

## Generated Bundles

Current In Development

| Bundle | Type | Size | Load Time |
|-------|--------|---------|---------|
| main.bundle.js | scripts | 15,974kb - 3.7mb (browswer) | 643ms / 32ms |

Current In Production

| Bundle | Type | Size | Load Time |
|-------|--------|---------|---------|
| main.bundle.js | scripts | 2,027kb - 1.8mb (browser) | 2.41s / 325ms -- 4.10s / 237ms -- 6.85s  |
| main.bundle.js.map | scripts | 11,290kb | |
| main.css | styles | 908kb - 806kb (browser) | 1.91s / 174ms -- 2.76s / 190ms -- 4.92s / 196ms |
| main.css.map | styles | 1kb | |

Development after chunking

| Bundle | Type | Size | Load Time |
|-------|--------|---------|---------|
| app.bundle.js | scripts | 1,936kb - 1.5mb (browser) | 457ms / 28ms |
| vendor.bundle.js | scripts | 15,193kb - 1.0mb (browser) | 226ms / 25ms |
| style.css | styles | 450kb | 89ms / 25ms |
| elmiron.css | styles | 302kb | 13ms / 9ms |
| keytruda.css | styles | 310kb | 13ms / 9ms |
| remedy.css | styles | 304kb | 14ms / 10ms |
| remedy-v2.css | styles | 306kb | 13ms / 9ms |
| shingrix-branded.css | styles | 300kb | 21ms / 17ms |
| shingrix-unbranded.css | styles | 300kb | 23ms / 18ms |
| trumenba-branded.css | styles | 304kb | 14ms / 10ms |
| trumenba-unbranded.css | styles | 307kb | 14ms / 10ms |
| zerbaxa.css | styles | 304kb | 14 ms / 9ms |

Production after chunking

| Bundle | Type | Size | Load Time |
|-------|--------|---------|---------|
| app.bundle.js | scripts | 330kb | 1.02s / 515ms -- 2.35s / 1.33s |
| app.bundle.js.map | scripts | 1,905kb | |
| vendor.bundle.js | scripts | 2.2mb | 2.86s / 318ms -- 6.59s / 941ms |
| vendor.bundle.js.map | scripts | 14,680kb | |
| style.css | styles | 438kb | 1.95s / 198ms -- 4.63s / 294ms -- 6.64s / 399ms |
| elmiron.css | styles | 242kb | |
| keytruda.css | styles | 249kb | |
| remedy.css | styles | 243kb | 780ms / 436ms |
| remedy-v2.css | styles | 246kb | |
| shingrix-branded.css | styles | 241kb | 439ms / 434ms |
| shingrix-unbranded.css | styles | 241kb | |
| trumenba-branded.css | styles | 243kb | 552ms / 216ms -- 614ms / 406ms |
| trumenba-unbranded.css | styles | 246kb | 763ms / 255ms -- 1.09s / 240ms -- 1.24s / 726ms |
| zerbaxa.css | styles | 244kb | |

And 1kb css.map for each theme


JS Bundles in Development after chunking and reducing dependencies

| Bundle | Type | Size | Load Time |
|-------|--------|---------|---------|
| app.bundle.js | scripts | 454kb |  |
| vendor.bundle.js | scripts | 3.32mb |  |

JS Bundles in Production after chunking and reducing dependencies

| Bundle | Type | Size | Load Time |
|-------|--------|---------|---------|
| app.bundle.js | scripts | 454kb |  |
| vendor.bundle.js | scripts | 3.02mb |  |



We went from:
main.bundle.js 2,027kb / main.bundle.js.map 11,290kb
main.css 806kb

To: 
vendor.bundle.js 1,305kb / vendor.bundle.js.map 8,434kb
app.bundle.js 215kb / app.bundle.js.map 996kb
TOTAL SCRIPTS = 1.52mb / 74.98% - Reduced by 25%

style.css 439kb
trumenba-branded.css 243kb   
TOTAL STYLES = 682kb / 84.61% - Reduced by 15.39%