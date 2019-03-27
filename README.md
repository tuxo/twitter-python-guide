# PROJETO - USANDO O TWITTER COM PYTHON
## Por: Arthur Moreira (github.com/reiarthur)

O intuito deste projeto é familiarizar os alunos à linguagem de programação, à sintaxe da linguagem utilizada, e estimular a descoberta e pesquisa de novas funcionalidades. A escolha foi feita baseada na pergunta “Como posso usar Python no dia-a-dia?”.

Criado em 2006, o Twitter é uma rede social de microblogging, que permite atualizações de status de até 140 caracteres. Essas atualizações de status são mais conhecidas como “tweets”. Um usuário pode seguir outro usuário, o que significa que ele está disposto a acompanhar os tweets e a mídia compartilhada pelo colega. Cada usuário tem a sua linha do tempo, onde ele visualiza os tweets dos usuários que ele segue.  

O Twitter disponibiliza uma API REST, isso significa que conseguimos recuperar ou informações vindas do Twitter para a nossa aplicação, além de conseguimos enviar informações da nossa aplicação ao Twitter. 

Leia a especificação deste projeto com atenção, ela lhe guiará a estabelecer a conexão e a autenticação do seu código ao Twitter.  O seu trabalho será criar as funções necessárias para facilitar o uso da API REST do Twitter, além de poder integrá-las às funcionalidades de Python.

Ao final deste projeto, sinta-se livre para continuar estudando, pesquisando e melhorando seu código depois da entrega. Se quiser, você também pode usar outras bibliotecas para adicionar mais funcionalidades!

### PRÉ-REQUISITOS
* Ter o Python 3.5 instalado
* Ter o pip funcionando
* Ter uma conta no twitter, com um número de telefone associado.
* Ter paciência para ler a documentação da [REST API do Twitter](https://dev.twitter.com/rest/public) quando necessário  


### PASSO 1
Abra o prompt de comando, digite 

```python 
pip install requests requests_oauthlib
```

e tecle enter.

A instalação deverá seguir com sucesso. 


### PASSO 2
Crie uma nova aplicação no [Twitter Apps](https://apps.twitter.com/). Escolha o nome e a descrição do seu app. Na opção de website, adicione um placeholder (exemplo: https://www.meuprojetopython.com), pois não usaremos o nosso app em um site, e sim, no interpretador de Python. Aceite os termos e crie o app.


### PASSO 3
Na aba “Keys and Access Tokens”, no painel “Your Access Token”, clique em “Create my access token”. Agora você tem em mãos as 4 cadeias de caracteres necessárias para se autenticar no seu app. 


### PASSO 4
Inicie o seu programa Python com a seguinte linha:

```python
from requests_oauthlib import OAuth1Session
```

Isso importará a biblioteca que possibilita a autenticação ao Twitter. Armazene em variáveis cada uma das chaves conseguidas no passo anterior.

```python
API_KEY = 'sua api key'
API_SECRET = 'sua api secret'
ACCESS_TOKEN = 'sua access token'
ACCESS_TOKEN_SECRET = 'sua access token secret'
```

Agora, vamos armazenar uma sessão em uma variável. É através da sessão que conseguimos usar o resto da API. 

```python
session = OAuth1Session(API_KEY, API_SECRET, ACCESS_TOKEN, ACCESS_TOKEN_SECRET)
```


### PASSO 5
Na [página da API de busca](https://dev.twitter.com/rest/public/search) é possível obter a url acessada para construir uma query de busca. 

**https://api.twitter.com/1.1/search/tweets.json?**

Se quisermos fazer uma consulta para retornar os últimos tweets contendo a palavra “projeto”, podemos adicionar o parâmetro q ao final da url, com o valor do que queremos procurar. 

**https://api.twitter.com/1.1/search/tweets.json?q=projeto**

Então, agora guardaremos essa url em uma variável

```python
url = "https://api.twitter.com/1.1/search/tweets.json?q=projeto"
```

Uma dica: você ainda pode armazenar o termo da busca em outra variável, e usar a interpolação!

```python
url = "https://api.twitter.com/1.1/search/tweets.json?q="
termo = 'projeto'
url = url % termo
```

Se você quiser saber todos os possíveis parâmetros em uma consulta na API de busca, acesse a [documentação referente a busca de tweets](https://dev.twitter.com/rest/reference/get/search/tweets).


### PASSO 6
Agora criaremos uma variável response que irá armazenar o acesso à essa url com a nossa autenticação definida anteriormente.

```python
response = session.get(url)
```

Pronto! Conexão feita. Se imprimirmos ela, veremos o código 200, isto significa que está tudo ok.


### PASSO 7
Para conseguirmos ler os dados armazenados no response, podemos usar o método .text, que nos retorna uma string grandona com várias informações sobre todos os tweets. CUIDADO! Muito provavelmente isto irá travar o seu IDLE, pois a string é realmente muito grande. Para poder transformar essa string em algo que o Python consiga trabalhar melhor, podemos usar a biblioteca json que já vem instalada, e seu método loads.

```python
import json  # Adicionar esta linha no início do código
# bla bla bla some code

tweets = json.loads(response.text) 
```

O método loads vai retornar um dicionário baseado na string gigante. Agora, será que a variável tweets já está armazenando diretamente os tweets?



### PASSO 8
Na verdade a variável tweets até então está armazenando um dicionário de apenas duas chaves: “search_metadata” e “statuses” (Com o método de dicionários .keys() você pode ver isso). Podemos observar a versão “bonita” desse dicionário aqui:

```python
{
    'search_metadata': {
        'refresh_url': '?since_id=890775573047455745&q=python&include_entities=1',
        'since_id_str': '0',
        'since_id': 0,
        'count': 1,
        'completed_in': 0.02,
        'max_id': 890775573047455745,
        'max_id_str': '890775573047455745',
        'next_results': '?max_id=890775573047455744&q=python&count=1&include_entities=1',
        'query': 'python'
    },
    'statuses': [{
        'in_reply_to_user_id_str': None,
        'id': 890775573047455745,
        'is_quote_status': False,
        'retweeted': False,
        'in_reply_to_user_id': None,
        'contributors': None,
        'metadata': {
            'result_type': 'recent',
            'iso_language_code': 'en'
        },
        'place': None,
        'coordinates': None,
        'created_at': 'Fri Jul 28 03:27:01 +0000 2017',
        'retweet_count': 0,
        'favorited': False,
        'entities': {
            'user_mentions': [],
            'hashtags': [{
                'indices': [60, 67],
                'text': 'python'
            }, {
                'indices': [68, 80],
                'text': 'filesystems'
            }],
            'urls': [{
                'indices': [81, 104],
                'display_url': 'goo.gl/C5GYHl',
                'url': 'https://t.co/0qYkmFiACm',
                'expanded_url': 'https://goo.gl/C5GYHl'
            }],
            'symbols': []
        },
        'possibly_sensitive': False,
        'in_reply_to_status_id': None,
        'in_reply_to_screen_name': None,
        'truncated': False,
        'text': "How to get full path of current file's directory in Python? #python #filesystems https://t.co/0qYkmFiACm",
        'favorite_count': 0,
        'user': {
            'id': 747460774998605825,
            'name': 'PythonQnA',
            'profile_image_url_https': 'https://pbs.twimg.com/profile_images/747461193653092352/Mz9NjeE__normal.jpg',
            'notifications': False,
            'profile_sidebar_border_color': 'C0DEED',
            'profile_link_color': '1DA1F2',
            'url': None,
            'profile_text_color': '333333',
            'profile_sidebar_fill_color': 'DDEEF6',
            'profile_use_background_image': True,
            'protected': False,
            'location': 'Bengaluru, India',
            'entities': {
                'description': {
                    'urls': []
                }
            },
            'time_zone': None,
            'profile_background_image_url': None,
            'created_at': 'Mon Jun 27 16:05:10 +0000 2016',
            'favourites_count': 0,
            'profile_background_image_url_https': None,
            'is_translator': False,
            'followers_count': 697,
            'lang': 'en',
            'following': False,
            'profile_background_tile': False,
            'default_profile_image': False,
            'follow_request_sent': False,
            'screen_name': 'PythonQnA',
            'geo_enabled': False,
            'profile_background_color': 'F5F8FA',
            'listed_count': 274,
            'statuses_count': 93653,
            'profile_banner_url': 'https://pbs.twimg.com/profile_banners/747460774998605825/1467044067',
            'default_profile': True,
            'friends_count': 64,
            'is_translation_enabled': False,
            'translator_type': 'none',
            'has_extended_profile': True,
            'profile_image_url': 'http://pbs.twimg.com/profile_images/747461193653092352/Mz9NjeE__normal.jpg',
            'id_str': '747460774998605825',
            'utc_offset': None,
            'verified': False,
            'description': 'I tweet Python questions from stackoverflow.',
            'contributors_enabled': False
        },
        'id_str': '890775573047455745',
        'geo': None,
        'source': '<a href="http://jarvis.ratankumar.org/" rel="nofollow">PythonQnA</a>',
        'lang': 'en',
        'in_reply_to_status_id_str': None
    }]
}
```

*Este dicionário foi “embelezado” pelo [Python Beautifier](http://www.cleancss.com/python-beautify/)*

A chave “search_metadata” tem como valor os metadados da busca, já a chave “statuses” tem como valor uma lista de tweets, que é o que queremos agora. Então podemos trocar a linha que atribuía todo o dicionário a variável tweets, por esta linha:

```python
tweets = json.loads(response.text)['statuses']
```

Agora a variável tweets contém uma lista de tweets!


### PASSO 9
Um fato interessante é que cada tweet nessa lista, é um dicionário. Se necessário, volte ao dicionário embelezado das páginas anteriores. A chave “text” do dicionário, tem o texto do tweet como valor. Então, se quisermos imprimir o conteúdo de todos os tweets recuperados pela nossa busca, podemos rodar um laço pela lista e printar o valor de chave ‘text’.

```python
for tweet in tweets:
    print(tweet['text'])
```


### PASSO 10
E se para cada tweet retornado pela consulta, quisermos imprimir o nome do usuário que tweetou? Podemos ver no dicionário embelezado que o dicionário do tweet tem uma chave “user”. O valor que a chave “user” referencia, por sua vez, também é um dicionário, que tem uma chave “name” e outra chave “screen_name”. Essas chaves referenciam respectivamente os valores de nome do usuário, e [login](twitter.com/login) do usuário que criou aquele tweet. Confuso? Volte alguns passos. Ver o dicionário embelezado ajuda bastante a entender.

```python
for tweet in tweets:
    print("%s (%s)" % (tweet['user']['name'], tweet['user']['screen_name']))
```


### PASSO 11
E se quisermos postar no nosso Twitter, como fazer? Na [documentação da atualização de status](https://dev.twitter.com/rest/reference/post/statuses/update), podemos ver que a URL base muda para 

**https://api.twitter.com/1.1/statuses/update.json**

e o parâmetro para passar o texto é o ‘status’. Se vamos postar a palavra “projeto”, teremos a url

**https://api.twitter.com/1.1/statuses/update.json?status=projeto**

Vamos armazenar a url base em uma variável e o texto do tweet em outra

```python
url = "https://api.twitter.com/1.1/statuses/update.json?status=%s"
texto = "projeto"
url = url % projeto
```

Anteriormente, nossa session foi feita usando o método .get(), mas se agora queremos enviar informações do nosso programa para o Twitter, teremos que usar o método .post().

```python
response = session.post(url)
```

Verifique se a atualização de status apareceu no seu Twitter!


### PASSO 12
Uma url não pode conter espaços e alguns caracteres especiais, certo? Para conseguir usar uma hashtag ou um @, é necessário formatar o texto antes. O método quote pode fazer isso por você!

```python
import requests

#some code

texto = 'Olá, mundo!'
texto = requests.utils.quote(texto)
```


### PASSO 13
Com o twitter hoje, é possível usar emojis em algumas partes do site. 😱 Aqui tem uma solução em Python que consegue converter os emojis para que não gere nenhum erro no IDLE. 😊 Assumindo que uma variável texto contenha um emoji:

```python
non_bmp_map = dict.fromkeys(range(0x10000, sys.maxunicode + 1), 0xfffd)
texto.translate(non_bmp_map)
```

Por segurança, faça isso com todos os textos que podem conter emojis, como nomes de usuário e texto dos tweets. 😉


### PASSO 14
Para outras funcionalidades, acesse a [documentação de referência](https://dev.twitter.com/rest/reference). Todos os endpoints que estão na sessão GET, irão usar o método .get(), todos os que estão na sessão POST, irão usar o método POST.
Atenção: Não confunda POST / .post() com o ato “twittar” (atualização de status). Diariamente, utilizamos o verbo “postar” querendo falar da nossa atualização de status, porém, nesse contexto, POST significa qualquer operação que irá ENVIAR informações ao twitter, e nem sempre essa operação é uma atualização de status.
Quando você quiser recuperar informações vindas do Twitter, utilize o método .get(). Quando você quiser enviar informações ao Twitter, utilize o método .post().
Exemplo:
Operação para conseguir o nome de um usuário: GET,
Operação para seguir um usuário: POST 


### AGORA É COM VOCÊ!
Agora que aprendemos a utilizar a API REST do Twitter, o seu trabalho é criar funções para facilitar essa conexão. 

Dica: os parâmetros da função estão em **negrito**


1. Faça uma função que crie e retorne uma sessão através das chaves **API_KEY**, **API_SECRET**, **ACCESS_TOKEN**, **ACCESS_TOKEN_SECRET**.
2. Faça uma função que retorne o url base de busca de tweets (com o parâmetro **q** na url).
3. Faça uma função que faça uma requisição GET ao Twitter, de uma sessão **s** com uma url **url**. (utilize o método .get() e retorne o response)
4. Faça uma função que faça uma requisição POST ao Twitter, de uma sessão **s** com uma url **url**. (utilize o método .post() e retorne o response)
5. Faça uma função que receba um response **r** e retorne a lista de tweets dele.
6. Faça uma função que retorne o nome do usuário criador de um tweet **tw**.
7. Faça uma função que retorne o login do usuário criador de um tweet **tw**.
8. Faça uma função que retorne as informações de data e hora de criação de um tweet **tw**.
9. Faça uma função que imprima um tweet **tw** num formato que apareça o seu texto, data, nome do usuário e login.
10. Faça uma função que receba uma lista de tweets **ltw** e para cada tweet, imprima ele no formato dito anteriormente.
11. Faça uma função que formate um texto **t**. (‘Como vai?’ -> 'Como%20vai%3F')
12. Faça uma função que com uma sessão **s**, retorne uma lista dos últimos **n** tweets contendo um texto **t**.
13. Faça uma função que retorne o url base de timeline de usuário (com os parâmetros **screen_name** e count na **url**).
14. Faça uma função que com uma sessão **s**, retorne uma lista dos últimos **n** tweets de um usuário de login **un**. 
15. Faça uma função que retorne o url base de atualização de status (com o parâmetro **status** na url).
16. Faça uma função que com uma sessão **s**, atualize o status (poste um tweet) com um texto **t**.
17. Faça uma função que com uma sessão **s**, responda um tweet **tw** com um texto **t**.
18. Faça uma função que retorne o url base de criar amizade, ou seja, seguir alguém (com o parâmetro **screen_name** na url).
19. Faça uma função que com uma sessão **s**, siga um usuário de login **un**.
20. Faça uma função que com uma sessão **s**, siga a última pessoa que tweetou um texto **t**.
21. Faça uma função que retorne o url base de favoritar um tweet (com o parâmetro **id** na url).
22. Faça uma função que com uma sessão **s**,  favorite um tweet **tw**.
23. Faça uma função que com uma sessão **s**, favorite o último tweet de um usuário de login **un**.
24. Faça uma função que retorne o url base de retweetar um tweet.
25. Faça uma função que com uma sessão **s**, retweete um tweet **tw**.
26. Faça uma função que com uma sessão **s**, retweete o último tweet contendo um texto **t**.
27. Faça uma função que imprima um usuário **u** num formato que apareça o seu nome e o seu login com um login com @ na frente, 
28. Faça uma função que retorne o url base de lista de seguidores (com os parâmetros **screen_name** e **count** na url).
29. Faça uma função que com uma sessão **s**, retorne uma lista dos últimos **n** seguidores de um usuário com login **un**.
30. Faça uma função que com uma sessão **s**, para uma lista de usuários **lu**, envie um tweet para cada usuário saudando ele. (A saudação deve conter o nome dele)
