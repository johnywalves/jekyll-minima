---
layout: post
title: "PipEnv e Virtual Enviroment"
date: "2018-08-26 11:50:38"
categories: Ambiente Python
output: html_document      
---



Com um [Ambiente Python](../Ambiente_Python) preparado 

## Gerenciando dependências

Um aplicativo usa sua própria coleção de bibliotecas, programadores tem o hábito de colecionar pacotes e usar somente alguns.


{% highlight bash %}
pip install pipenv
{% endhighlight %}

No exemplo a seguir vamos usar o pacote flask, que mesmo instalado no ambiente da máquina, precisa estar no *Virtual*, ou somente no virtual 


{% highlight bash %}
pipenv install flask
{% endhighlight %}

Com o comando anterior o sistema cria no arquvio **Pipfile** na pasta atual, o arquivo contém todas as configurações do ambiente como versão do Pyhton e pacotes instalados nele e o **Pipfile.lock** com as versões dos pacotes<br><br>
Para instalar um ambiente apartir de um já configurado basta executar o comando a seguir em uma pasta com os **Pipfile** e o **Pipfile.lock**


{% highlight bash %}
pipenv install
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

Para executar a aplicação em ambiente virtual, podemos executar sua chamada pelo shell 


{% highlight bash %}
pipenv shell
{% endhighlight %}

Ou executar direto para execução da aplicação 


{% highlight bash %}
pipenv run python web.py
{% endhighlight %}

## Referências
<http://flask.pocoo.org/docs/1.0/quickstart/>
<https://robots.thoughtbot.com/how-to-manage-your-python-projects-with-pipenv>

