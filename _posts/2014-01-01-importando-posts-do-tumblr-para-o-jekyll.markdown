---
layout: post
title:  "Importando posts do Tumblr para o Jekyll"
date:   2014-02-01 18:53;00
categories: jekyll tumblr importacao
---

Foi trabalhoso, mas consegui.

Ao começar, parecia que seria mais simples. A
[página do Jekyll](http://jekyllrb.com) tem um guia explicando como fazer a
importação para vários engines e sites de blog. O Tumblr tava lá, então parecia
que metade do trabalho já tava feito.

Basicamente, eu precisava instalar a gem `jekyll-import`. Instalei. A
documentação me sugeria esse comando:

{% highlight bash %}
$ ruby -rubygems -e 'require "jekyll-import";
    JekyllImport::Importers::Tumblr.run({
      "url"            => "http://myblog.tumblr.com",
      "format"         => "html", # or "md"
      "grab_images"    => false,  # whether to download images as well.
      "add_highlights" => false,  # whether to wrap code blocks (indented 4 spaces) in a Liquid "highlight" tag
      "rewrite_urls"   => false   # whether to write pages that redirect from the old Tumblr paths to the new Jekyll paths
    })'
{% endhighlight %}

Logo, alterei as opções `format` pra `md`, `add_highlights` pra `true` e
`rewrite_urls` também para `true`. Rodei o comando e _drumroll please_
ocorreu uma exceção.

Conferi o código e o problema estava na opção `add_highlights`, que tentava
chamar um método não definido. "Ah, não tinha tanto código no blog, tô com
preguiça de consertar a gem", pensei, e removi a opção.

Yey, rodou. Abro o blog e meh, perdeu toda a formatação, todos os links, ficou
só o texto. Não era isso que eu queria. Desfiz a importação e refiz em HTML.

Aí sim, funcionou. Todos os posts do Tumblr já estão nesse blog, e no máximo
terei que corrigir links para outras postagens, pois o `rewrite_urls` não
funcionou.