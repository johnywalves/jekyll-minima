---
layout: post
title: "PipEnv e Virtual Enviroment"
date: 2018-08-25 15:00:00 -0300
categories: Ambiente Python
output: html_document      
---



Com um [Ambiente Python](../Ambiente_Python) preparado 

## Gerenciando dependências

Programadores tem o hábito de colecionar pacotes, mas projetos usam suas próprias bibliotecas, para facilitar a


{% highlight bash %}
pip install pipenv
{% endhighlight %}

## Gerenciar ambiente virtual

Para criar um ambiente virtual navegue para a pasta de trabalho e execute o comando 'pipenv install' ou 'pipenv shell' usando a versão instalada e configurada como padrão do Python<br>


{% highlight bash %}
pipenv install 
{% endhighlight %}

Com o comando anterior o sistema cria no arquvio **Pipfile** na pasta atual, o arquivo contém todas as configurações do ambiente como versão do Pyhton e pacotes instalados nele e o **Pipfile.lock** com as versões dos pacotes<br><br>
Para instalar um ambiente apartir de um já configurado basta executar novamente o comando em uma pasta com os **Pipfile** e o **Pipfile.lock**

No exemplo a seguir vamos usar o pacote flask, que mesmo instalado no ambiente da máquina, precisa estar no *Virtual*, ou somente no virtual


{% highlight bash %}
pipenv install flask
{% endhighlight %}

## Programa

Com biblioteca do *Flask* instalada, basta criar um arquivo web.py desta maneira


{% highlight python %}
import os
from flask import Flask
app = Flask(__name__)

@app.route('/')
def hello_world():
    return 'Aeeee!!! Funciona'
	
if __name__ == '__main__':
    port = int(os.environ.get("PORT", 5000))
    app.run(host='0.0.0.0', port=port)
{% endhighlight %}

## Execução

Para executar a aplicação em ambiente virtual, podemos executar a chamada 'python web.py' pelo shell 


{% highlight bash %}
pipenv shell
``

Ou executar direto para execução da aplicação 

{% endhighlight %}

{% highlight bash %}
pipenv run python web.py
{% endhighlight %}
## Referências
<http://flask.pocoo.org/docs/1.0/quickstart/><br>
<https://robots.thoughtbot.com/how-to-manage-your-python-projects-with-pipenv>

