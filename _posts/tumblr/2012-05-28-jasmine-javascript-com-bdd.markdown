---
layout: post
title: 'Jasmine: Javascript com BDD'
date: '2012-05-28T22:51:00-03:00'
tags:
- desenvolvimento
- não faço a mínima ideia
- javascript
- bdd
- jasmine
tumblr_url: http://neilcarvalho.tumblr.com/post/23970655171/jasmine-javascript-com-bdd
---
Esse é mais um post da série _não faço a mínima ideia_, onde na verdade faço alguma ideia (ao menos do que se trata) mas nunca parei pra estudar a fundo. Até o final do primeiro parágrafo, não fiz nenhuma espécie de pesquisa ou digitei uma linha de _code example_ com Jasmine. E é assim que é pra ser mesmo.

Jasmine é uma ferramenta de BDD para Javascript. Essa é uma linguagem bem filha da puta e chata quando tudo parece bem, mas absolutamente necessária para qualquer coisa internética mais interativa. Não, não conto com coisas pré-históricas como applets Java ou Flash.

Pra quem é familiar com RSpec, Jasmine é moleza. it, describe, beforeEach, spies, tá tudo lá. Só que com a sintaxe do Javascript, mas beleza, tá perdoado.

Partindo daquele exemplo babaquinha da calculadora ultra simples que soma dois números, começaríamos com o seguinte spec:

{% highlight javascript %}
describe('Calculator', function(){
  it('sums 1+1', function(){
    var calculator = new Calculator();
    expect(calculator.sum(1,1)).toBe(2);
  });
});
{% endhighlight %}

Que retorna o seguinte erro:

````
ReferenceError: Can't find variable: Calculator
````

Para resolver esse erro, definimos Calculator:

{% highlight javascript %}
function Calculator() {
}
{% endhighlight %}

Que faz o erro alterar para

````
TypeError: 'undefined' is not a function (evaluating 'calculator.sum(1,1)')
````

Oops, faltou definir sum (com o código mais simples que dá pra fazer):

{% highlight javascript %}
Calculator.prototype.sum = function(num1, num2) {
  return 2;
}
{% endhighlight %}

Yey!


````
Passing 1 spec
````

Uma _cheat sheet_ bem mais detalhada pode ser vista [aqui](http://pivotal.github.com/jasmine/).
