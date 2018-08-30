---
layout: post
title: "Instalação do Python e Pacotes"
date: 2018-08-25 15:00:00 -0300
categories: Ambiente Python
output: html_document
---


### Instalação do Python

Com os intepretadores instalados através do [Python Downloads](https://www.python.org/downloads/) e os caminhos mapeados no sistema operacional, a interface de comando deve responder, apresentando a versão instalada do Python


{% highlight bash %}
python --version
{% endhighlight %}

Na instalação vem junto com o **pip** um gerenciador de pacotes, para verificar a versão use o comando abaixo


{% highlight bash %}
pip -V
{% endhighlight %}

### Instalação de Pacotes 

O pip oferece uma maneira simples de instalar um pacote


{% highlight bash %}
pip install <nome_do_pacote>
{% endhighlight %}

Ou desinstalar 


{% highlight bash %}
pip uninstall <nome_do_pacote>
{% endhighlight %}
Para atualizar um pacote atualizado no ambiente do Python, no instalar deve ser usado a diretiva --update, objetos como **pip** e o **setuptools**


{% highlight bash %}
pip install --upgrade setuptools pip
{% endhighlight %}

Para facilitar a portalidade e replicação de ambientes podemos exportar as listas de todos os pacotes instalados e suas respectivas versões


{% highlight bash %}
pip freeze > requirements.txt
{% endhighlight %}

E posteriormente instalar a lista completa somente com um comando


{% highlight bash %}
pip install -r requirements.txt
{% endhighlight %}
