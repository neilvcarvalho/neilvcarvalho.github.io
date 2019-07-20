---
layout: post
title:  "Usando AWS Lambda e IFTTT para dormir melhor"
date:   2019-07-19 21:37:00
categories: ruby aws lambda ifttt
---

Nesses dias eu percebi que, nessa época do ano, o nascer do Sol aqui em Americana está bem próximo das 7 da manhã. 6:48 da manhã, pra ser mais preciso. E 7h é a hora que meu despertador está configurado. Dormir com a janela aberta, sendo lentamente despertado pelo Sol e depois só recebendo o último empurrãozinho do despertador tem deixado meu começo de dia muito mais agradável.

Então, tive uma ideia: e se eu configurar meu despertador _sempre_ para o nascer do Sol? E tivesse também acesso fácil ao melhor horário para dormir? Busquei uma solução no IFTTT e não encontrei nada do jeito que eu queria. Então resolvi fazer a minha. Vamos lá pra receita:

Ingredientes:
- Uma conta na AWS
- [Uma chave de API no OpenWeather](https://openweathermap.org/api)
- [Uma chave de API do IFTTT](https://ifttt.com/maker_webhooks)
- Um app do IFTTT instalado no celular

### Passo 1: Criando um applet no IFTTT
_TL/DR sobre o IFTTT: É um serviço que permite a conexão de dois serviços diferentes. Você pode configurar toda foto onde você é marcado no Facebook para ser enviada por e-mail, por exemplo._

IFTTT significa _**if** **t**his, **t**hen **t**hat_. E é exatamente assim que os applets são criados.

Criei um applet em que o _this_ é _Webhooks_. Isso precisa de uma chave de API, que pode ser criada [no site deles](https://ifttt.com/maker_webhooks). Chamei meu evento de _tomorrows_sunrise_, e é isso a configuração do _this_.

Já o _that_ é o _Notifications_. Os webhooks do IFTTT podem receber até três parâmetros: `Value1`, `Value2` e `Value3`. Eles serão passados pela função Lambda, descrita em seguida. Ah, como vou enviar `Value1` como a hora do nascer do Sol e `Value2` como a melhor hora para dormir, minha mensagem de notificação ficou exatamente:
```
Prepare-se para dormir às {{Value2}}. O sol vai nascer às {{Value1}}.
```

### Passo 2: Criando a função do Lambda
_TL/DR sobre o Lambda: É um serviço da Amazon onde você pode programar funções e plugar em outros serviços. Você não precisa se preocupar com o servidor e será cobrado apenas pelos segundos em que a função executar. Há uma quantidade mensal que é gratuita, e essa função está com uma enorme folga._

O propósito da função do Lambda é ser executada todos os dias, às 20h, ver a hora do nascer do Sol no OpenWeather e finalmente passar o bastão pro IFTTT completar o serviço.

O _trigger_ da função é o CloudWatch Events. Criei uma regra do tipo _Schedule expression_, cofigurado assim:
```
cron(0 23 * * ? *)
```

Essa expressão de cronjob vai executar todos os dias, às 23:00 UTC, que equivale às 20:00 no horário de Brasília.

Finalmente, a função em Ruby:
```ruby
require 'json'
require 'net/http'

IFTTT_KEY          = "SUA_CHAVE_DO_IFTTT"
CITY_ID            = 3472343 # Este é o id de Americana-SP - busque em openweathermap.org
OPENWEATHERMAP_KEY = "SUA_CHAVE_DO_OPENWEATHERMAP"
URL_IFTTT          = URI("https://maker.ifttt.com/trigger/tomorrows_sunrise/with/key/#{IFTTT_KEY}")
URL_WEATHER        = URI("https://api.openweathermap.org/data/2.5/weather?id=#{CITY_ID}&appid=#{OPENWEATHERMAP_KEY}")
TIMEZONE           = "-03:00"
SLEEPING_TIME      = 8*60*60 # 8 horas, em segundos

def lambda_handler(event:, context:)
    json          = JSON.parse(Net::HTTP.get(URL_WEATHER))
    sunrise       = json["sys"]["sunrise"]
    time_to_sleep = sunrise - SLEEPING_TIME

    formatted_sunrise       = Time.at(sunrise).getlocal(TIMEZONE).strftime("%H:%M")
    formatted_time_to_sleep = Time.at(time_to_sleep).getlocal(TIMEZONE).strftime("%H:%M")

    response = Net::HTTP.post_form(URL_IFTTT, value1: formatted_sunrise, value2: formatted_time_to_sleep)

    { statusCode: 200, body: response.body }
end
```

E é isso. Pretendo melhorar a notificação, enviando também a previsão do tempo pro dia seguinte. Ou simplesmente utilizarei uma receita já pronta do IFTTT, com uma nova notificação.