---
layout: post
title:  "Habilitando a cedilha no teclado internacional no Linux"
date:   2020-10-08 21:00:00
categories: linux
---

Toda vez que instalo uma distribuição nova eu tenho que procurar como resolver o problema da sequência `'` + `c`  que se transforma em um ć, como se eu fosse do leste europeu. Sempre tem a opção de enviar um `Alt Gr` + `,`, mas ela não é nada agradável de usar. Então, vou colocar aqui pra referência futura:

Basta instalar o pacote `ibus` (pelo menos se chama assim no Manjaro), e em seguida, informei no `/etc/environment`:

```
GTK_IM_MODULE=cedilla
```
E reiniciar o computador.

Possível que dê problema em outros ambientes, como QT, mas aí eu vou esperar passar pelo problema pra buscar uma solução.

TL/DR: Instalar `ibus`, informar `GTK_IM_MODULE=cedilla` no `/etc/environment` e reiniciar o computador