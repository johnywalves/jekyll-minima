---
layout: post
title: "Mortes em Kill Bill (Linguagem R)"
date: 2018-08-19 15:00:00 -0300
categories: Visualização R
output: html_document
---
Quando estava aprendendo o básico da **Linguagem R** e o **R Studio** em uma workshop do Igor Alcantara, ele disponibilizou algumas bases de dados dentre eles com ofecenças e mortes em filmes do Quentin Tarantino. Brincando um pouco a base encontrei uma lista de mortes em ambos volumes de Kill Bill



### Ambiente

Para preparar o ambiente informei o diretório de trabalho indicado pelo **"."** e limpei todas as variáveis de ambiente após pesquisar todas pelo *list = ls()*


{% highlight r %}
setwd(".")
rm(list = ls())
{% endhighlight %}

Carreguei a bilbioteca do *tidyverse* pois contém a *ggplot2*, geração de gráfico, e a *dplyr*, filtros de dados


{% highlight r %}
library('tidyverse')
{% endhighlight %}

Carregando os dados através do *csv* na pasta e a visualização proporcionou uma análise exploratória


{% highlight r %}
tarantino <- read.csv('tarantino.csv')
head(tarantino)
{% endhighlight %}



{% highlight text %}
##            movie type     word minutes_in
## 1 Reservoir Dogs word     dick       0.40
## 2 Reservoir Dogs word    dicks       0.43
## 3 Reservoir Dogs word   fucked       0.55
## 4 Reservoir Dogs word  fucking       0.61
## 5 Reservoir Dogs word bullshit       0.61
## 6 Reservoir Dogs word     fuck       0.66
{% endhighlight %}

Os dados estão organizados com os nomes dos filmes, acontecimento de tempo em minutos

### Organização

Somente me interessa os dados de mortes dos volumes 1 e 2 de Kill Bill, com *dplyr* apliquei um filtro


{% highlight r %}
mortes <- 
  tarantino %>% 
  filter(type == 'death' & (movie == 'Kill Bill: Vol. 1' | movie == 'Kill Bill: Vol. 2'))
{% endhighlight %}

Para ter uma coluna com quantidade crescente de mortes, para facilitar na geração no gráfico


{% highlight r %}
qtdMortes <- nrow(as.data.frame(mortes$minutes_in))
mortes$qtd <- seq(1, qtdMortes)
{% endhighlight %}

Adicionei de 112 minutos antes do segundo para duração do primeiro volume acrescido de um minuto.<br/>
Fonte da duração <https://www.imdb.com/title/tt0266697/>


{% highlight r %}
mortes[(mortes$movie == 'Kill Bill: Vol. 2'),]$minutes_in <- 
    mortes[(mortes$movie == 'Kill Bill: Vol. 2'),]$minutes_in + 112
{% endhighlight %}

### Visualização

Com os dados organaizados, o uso do *ggplot2* com os comandos:<br/>
*labs* Legendas do gráfico<br/>
*geom_point* Visualização dos dados por pontos<br/>
*geom_segment* Linha vermelha para separar o momento entre os dois filmes<br/>
*theme_minimal* Interface com mínimo de recursos


{% highlight r %}
graph <- ggplot(mortes, aes(x=qtd, y=minutes_in)) + 
  labs(title = "Mortes por tempo em Kill Bill (Vol. 1 & 2)",
       x = "Número de mortes",
       y = "Tempo de filme (minutos)") +
  geom_point(color='#db0d0d', size = 1.5, alpha = 0.5) +
  geom_segment(color='#0d74db', size = 1, 
               aes(x = 0, xend = qtdMortes, y = 112, yend = 112), alpha = 0.5) +
  theme_minimal()

graph
{% endhighlight %}

![plot of chunk KillBillDeath](/./assets/Rfig/KillBillDeath-1.svg)

Por final, gerei o arquivo para visualização futura e divulgação no site


{% highlight r %}
ggsave(filename = 'KillBillDeath.jpg', graph,
       width = 9.38, height = 5.47, dpi = 95, units = 'in', device = 'jpg')
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
