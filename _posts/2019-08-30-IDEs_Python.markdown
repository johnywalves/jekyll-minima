---
layout: post
title: "IDEs Python (Jupyter e Spyder)"
date: 2018-08-30 13:32:48 -0300
categories: Ambiente Python
output: html_document      
---



Ambientes de Desenvolvimentos ou IDE (Integrated Development Environment), a definição de um verdadeiro programador é o tema de sua IDE (sempre dark, para evitar cansaços aos olhos), essa ferramenta deve auxliar o desenvolvimento com validação de sintaxe, autopreencimento e facilidade para executar e publicar o objeto devenvolvido.<br>
**Microsoft Visual Code**, **Sublime**

### Jupyter Notebook




{% highlight bash %}
pip install jupyter
{% endhighlight %}



{% highlight bash %}
python jupter notebook
{% endhighlight %}

### Spyder

Na pasta de scripts poder ser achado o executável do spyder


{% highlight bash %}
pip install spyder
{% endhighlight %}


{% highlight bash %}
spyder3
{% endhighlight %}
<div class="post-categories">
            Categorias: 
            {% if post %}
            {% assign categories = post.categories %}
            {% else %}
            {% assign categories = page.categories %}
            {% endif %}
            {% for category in categories %}
            <a href="{{site.baseurl}}/categorias/#{{category|slugize}}">{{category}}</a>
            {% unless forloop.last %}&nbsp;{% endunless %}
            {% endfor %}
            </div>
