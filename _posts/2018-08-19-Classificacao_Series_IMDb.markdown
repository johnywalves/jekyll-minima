---
layout: post
title: "Classificação de Séries IMDb (Linguagem R)"
date: 2018-08-19 15:00:00 -0300
categories: Visualização R
output: html_document
---

[Explicação da base de dados](https://www.imdb.com/interfaces/) 

[Base de dados](https://datasets.imdbws.com/) 



### Ambiente

Para preparar o ambiente informei o diretório de trabalho indicado pelo **"."** e limpei todas as variáveis de ambiente após pesquisar todas pelo *list = ls()*.


{% highlight r %}
setwd('.')
rm(list=ls())
{% endhighlight %}

Carregar a fonte de dados de classificações e epísodios


{% highlight r %}
ratings = read.table(gzfile("title.ratings.tsv.gz"), header = TRUE)
episode = read.table(gzfile("title.episode.tsv.gz"), header = TRUE)
{% endhighlight %}


{% highlight r %}
#  
episodeRating <- cbind(episode[match(ratings$tconst, episode$tconst),], ratings)
#
episodeRating <- episodeRating[!is.na(episodeRating$parentTconst),]
#
episodeRating <- episodeRating[(episodeRating$seasonNumber != '\\N'),]
# Remover episódios
episodeRating <- episodeRating[(episodeRating$numVotes > 50),]
# Ordenar por episódios
episodeRating <- episodeRating[order(episodeRating$seasonNumber, episodeRating$episodeNumber),] 
# 
episodeRating$seasonNumber <- as.integer(as.character(episodeRating$seasonNumber))
episodeRating$episodeNumber <- as.integer(as.character(episodeRating$episodeNumber))
{% endhighlight %}




{% highlight r %}
meanRating <- episodeRating[,c('parentTconst', 'averageRating')]
meanRating <- aggregate(averageRating ~ parentTconst, meanRating, mean)
meanRating <- meanRating[order(-meanRating$averageRating),]
{% endhighlight %}




[Curva ABC](https://pt.wikipedia.org/wiki/Curva_ABC) 


{% highlight r %}
mediaCurvaA <- mean(meanRating[1:nrow(meanRating)*0.2,]$averageRating)
mediaCurvaB <- mean(meanRating[nrow(meanRating)*0.2:nrow(meanRating)*0.5,]$averageRating)
mediaCurvaC <- mean(meanRating[nrow(meanRating)*0.5:nrow(meanRating),]$averageRating)
{% endhighlight %}

[My Little Poney: Friendship is Magic](https://www.imdb.com/title/tt1751105/) 

**tt1751105**



{% highlight r %}
codSerie <- 'tt1751105' # My Little Poney: Friendship is Magic

# Cálculo da média da série
mediaSerie <- meanRating[meanRating$parentTconst == codSerie,]$averageRating

# Filtar os episódios da série e remover atributos duplicados
episodeRatingSerie = episodeRating[episodeRating$parentTconst == codSerie,-5]

# Organizar e ordenar episodios por sequência
library(stringr)
episodeRatingSerie$episodeNumber <- str_pad(episodeRatingSerie$episodeNumber, 2, pad = "0")
episodeRatingSerie$seasonNumber <- str_pad(episodeRatingSerie$seasonNumber, 2, pad = "0")
episodeRatingSerie$episode <- paste(episodeRatingSerie$seasonNumber, 
                                    episodeRatingSerie$episodeNumber, sep = "")
# Ordenar
episodeRatingSerie <- episodeRatingSerie[order(episodeRatingSerie$episode),]
episodeRatingSerie$ordemEpisode <- seq(1,nrow(episodeRatingSerie))
{% endhighlight %}




{% highlight r %}
# Definir cores as linhas
rainbow <- rainbow(3)
# Geração de gráfico por episódios
library(ggplot2)
ggplot(episodeRatingSerie, aes(x = ordemEpisode, y = averageRating)) +
  labs(x = "Episódio", y = "Média de pontuação") +
  geom_line(color=rainbow[1]) +
  geom_segment(color=rainbow[2], size=0.8, 
               aes(x=1, xend=nrow(episodeRatingSerie), y=mediaSerie, yend=mediaSerie), alpha = 0.5) +  
  theme_minimal()
{% endhighlight %}

![plot of chunk serie](/./assets/Rfig/serie-1.svg)



{% highlight r %}
mediaTemporada = aggregate(episodeRatingSerie$averageRating,
                           by=list(Category=episodeRatingSerie$seasonNumber), mean)
mediaTemporada$Category = as.integer(mediaTemporada$Category)
colnames(mediaTemporada) <- c('seasonNumber', 'averageRating')
{% endhighlight %}




{% highlight r %}
rainbow <- rainbow(5)

ggplot(mediaTemporada, aes(x=seasonNumber, y=averageRating)) +
    labs(x = "Temporada", y = "Média de pontuação") +
    geom_line(color=rainbow[1]) +
    
    geom_segment(color=rainbow[2], size=1, 
                 aes(x=1, xend=nrow(mediaTemporada), y=mediaSerie, yend=mediaSerie), alpha = 0.5) +
    geom_segment(color=rainbow[3], size=1, 
                 aes(x=1, xend=nrow(mediaTemporada), y=mediaCurvaA, yend=mediaCurvaA), alpha=0.5) +
    geom_segment(color=rainbow[4], size=1, 
                 aes(x=1, xend=nrow(mediaTemporada), y=mediaCurvaB, yend=mediaCurvaB), alpha=0.5) +  
    geom_segment(color=rainbow[5], size=1, 
                 aes(x=1, xend=nrow(mediaTemporada), y=mediaCurvaC, yend=mediaCurvaC), alpha=0.5) +    
    
    geom_text(aes(x=1.1, y=mediaSerie, label='Série'), 
              hjust=0, vjust=0.55, size=4, colour='red') +
    geom_text(aes(x=1.1, y=mediaCurvaA, label='A'), 
              hjust=0, vjust=0.55, size=4, colour='red') +
    geom_text(aes(x=1.1, y=mediaCurvaB, label='B'), 
              hjust=0, vjust=0.55, size=4, colour='red') +
    geom_text(aes(x=1.1, y=mediaCurvaC, label='C'), 
              hjust=0, vjust=0.55, size=4, colour='red') + 
    
    theme_minimal()
{% endhighlight %}

![plot of chunk temporadas](/./assets/Rfig/temporadas-1.svg)




{% highlight r %}
# Importar títulos das séries 
# quote padrão é o "'" e comment.char padrão é o "#", mas alguns nomes possuem eles
titles = read.table(gzfile("title.basics.tsv.gz"), sep="\t", quote="", comment.char="", header = TRUE)

#codSerie <- 'tt0407362' # Battlestar Galactica 
#codSerie <- 'tt0411008' # Lost
#codSerie <- 'tt0944947' # Game of Thrones 
#codSerie <- 'tt0903747' # Breaking Bad
#codSerie <- 'tt0141842' # The Sopranos

# Listagem das séries
codSeries <- c('tt0407362', 'tt0411008', 'tt0944947', 'tt0903747', 'tt0141842')

# Carregar pontuação da série
grupoSeries <- episodeRating[episodeRating$parentTconst %in% codSeries,]
grupoSeries <- aggregate(grupoSeries$averageRating, 
                         by=list(seasonNumber=grupoSeries$seasonNumber,
                                 parentTconst=grupoSeries$parentTconst), mean)
colnames(grupoSeries)[3] <- 'averageRating'

# Capturar somente as colunas  títulos das séries 
titlesSeries <- titles[titles$tconst %in% as.factor(codSeries), c('tconst', 'primaryTitle')]

grupoSeries <- cbind(titlesSeries[match(grupoSeries$parentTconst, titlesSeries$tconst),], grupoSeries)
{% endhighlight %}



{% highlight r %}
ggplot(grupoSeries, aes(x=seasonNumber, y=averageRating)) +
    labs(x = "Temporada", y = "Média de pontuação", colour = "Séries") +  
    geom_line(aes(group = primaryTitle, colour = primaryTitle)) +
    theme_minimal()
{% endhighlight %}

![plot of chunk comparacao](/./assets/Rfig/comparacao-1.svg)


{% highlight r %}
# Quais as melhores de todos os tempos
melhoresSeries <- head(meanRating)
melhoresTitles <- titles[titles$tconst %in% as.factor(melhoresSeries$parentTconst), 
                         c('tconst', 'primaryTitle')]
melhoresSeries <- cbind(melhoresTitles[match(melhoresSeries$parentTconst, melhoresTitles$tconst),],
                        melhoresSeries)

print(melhoresSeries)
{% endhighlight %}



{% highlight text %}
##            tconst             primaryTitle parentTconst averageRating
## 381214  tt0396991                 LazyTown    tt0396991     10.000000
## 2535133 tt2973110         BK Comedy Series    tt2973110     10.000000
## 386084  tt0401969      Playing It Straight    tt0401969      9.975000
## 3213875 tt4517806            Furusato-Time    tt4517806      9.950000
## 2332879 tt2487370 Whose Line Is It Anyway?    tt2487370      9.947059
## 183451  tt0190196  Romper Room and Friends    tt0190196      9.900000
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
