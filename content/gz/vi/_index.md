---
title: "vi/vim/nvim"
date: 2022-05-09T18:45:00+02:00
#draft: true
author: 'Ran#'
#description: ''

toc: true
collapsible_toc: false
collapsible_changelogs: true

search_hidden: false
math: false
zooming_js: false

ga: true
disqus: true
twitter_cards: false

#code_copy: false
#open_graph: false

url: '/vi/'
slug: 'vi'
aliases: [
    'vi',
    'vim',
    'neovim',
]

#weight: 0
#bookcase_cover_src: ''
#bookcase_cover_src_dark: ''

#type: 'bookcase'

---

Vi

## Copiar todas as linhas con x palabra

`:g/palabra/y A`

Copia todas as linhas que conteñen a palabra *palabra* e meteas no rexistro *a*.
Pode ser pegada con `"ap` ou simplemente `p`

O uso da maiúscula `A` faise para que os contidos copiados de adhiran ó rexistro.
De usar a minúscula `a` soamente a última linha se tería gardado no rexistro.
