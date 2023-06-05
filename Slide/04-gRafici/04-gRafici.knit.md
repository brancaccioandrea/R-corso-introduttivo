---
title: "04-gRafici"
date: '8-9 giugno 2023'
author: |
  | Ottavia M. Epifania
  | Università di Padova
institute: "Lezione di Dottorato @Università Cattolica del Sacro Cuore (MI)"
output: 
  beamer_presentation: 
#    theme: Singapore
    colortheme: spruce
    highlight: kate
header-includes:
    - \AtBeginDocument{\title[g\texttt{R}afici]{04-g\texttt{R}afici}}
    - \AtBeginDocument{\author[OME, UniPD]{Ottavia M. Epifania, Ph.D}}
    - \AtBeginDocument{\date[8-9 Giugno]{8-9 Giugno 2023}}
    - \AtBeginDocument{\institute[Lezioni UniCatt]{Lezione di Dottorato @Università Cattolica del Sacro Cuore (MI)}}
    - \useinnertheme{circles}
    - \usetheme[compressed]{Singapore}
    - \usepackage{graphicx} 
    - \usepackage{setspace}
    - \usepackage{tikzsymbols}
    - \usepackage{tabularx}
    - \usepackage[english]{babel}
    - \usepackage{tikzsymbols}
    - \usepackage{subcaption}
    - \usepackage{tikz}
    - \usepackage{spot}
    - \usepackage{tabularx}
    - \usepackage[absolute,overlay]{textpos}
    - \usepackage{booktabs}
    - \newcommand\Factor{1.2}
    - \setbeamerfont{subtitle}{size=\large, series=\bfseries}
    - \definecolor{template}{RGB}{155, 0, 20}
    - \definecolor{background}{RGB}{250, 250, 250}
    - \setbeamercolor{frametitle}{bg=background}
    - \setbeamertemplate{frametitle}{\color{template}\bfseries\insertframetitle\par\vskip-6pt\hrulefill}
    - \AtBeginSection{\frame{\sectionpage}}
    - \setbeamercolor{section name}{fg=white}
    - \setbeamercolor{title}{fg=template, bg=background}
    - \setbeamercolor{section title}{fg=template}
    - \setbeamercolor{frame subtitle}{fg=template}
    - \setbeamercolor{title}{fg=template}
    - \setbeamercolor{background canvas}{bg=background}
    - \setbeamersize{text margin left=5mm,text margin right=5mm}
    - \setbeamercolor{section in head/foot}{bg=background, fg=template}
    - \setbeamercolor{subsection in head/foot}{fg=template, bg=template!20}
    - \AtBeginSection[]
          {
             \begin{frame}[plain]
             \frametitle{Table of Contents}
             \tableofcontents[currentsection]
              \end{frame}
          }
editor_options: 
  chunk_output_type: console
---





## Table of contents {.plain}

\tableofcontents


##





- Grafici base
- Grid graphics \& `ggplot2`

Entrambi: 

- High level functions $\rightarrow$ le funzioni che producono effettivamente il grafico
- Low level functions $\rightarrow$ Le funzioni che lo rendono più "bello"

# Grafici tradizionali

## High level functions




```r
plot()      # scatter plot, specialized plot methods
boxplot()
hist()      # histogram
qqnorm()    # quantile-quantile plot
barplot()
pie()       # pie chart
pairs()     # scatter plot matrix
persp()     # 3d plot
contour()   # contour plot
coplot()    # conditional plot
interaction.plot()
```


`demo(graphics)` vi fonrisce un tour guidato dei grafici

## Low level functions




```r
points()  # Aggiunge punti al grafico
lines()   # Aggiunge linee  al grafico 
rect()
polygon()
abline() # aggiunge una riga con intercetta a e pendenza b
arrows() # aggiunge barre d'errore     
text()   # aggiunge testo nel plot
mtext()  # aggiunge testo nei margini
axis()   # personalizza gli assi
box()    # box attorno al grafico 
legend() # cambia parametri della legenda
```

## Plot layout

Ogni plot è composto da due regioni:

- Plotting region (dove effettivamente sta il plot)
- La regione dei margini (contiene i margini e le varie etichette degli assi)

\pause

\small

Uno scatter plot: 


```r
x <- runif(50, 0, 2) # 50 numeri random 
y <- runif(50, 0, 2) # da una distr. uniforme
plot(x, y, main="Titolo", 
     sub="Sottotitolo", xlab="x-label",
     ylab="y-label") # ecco il plot
```

Aggiunge del testo al plot



```r
text(0.6, 0.6, "Testo @ (0.6, 0.6)")
abline(h=.6, v=.6, lty=2) # horizont. and vertic. 
                          # lines
```

## Margins region 

\small


\begin{center}\includegraphics[width=1\linewidth]{04-gRafici_files/figure-beamer/unnamed-chunk-6-1} \end{center}


## Modificare il layout dei plot 

Vanno creati dei pannelli

\small


```r
par(mfrow=c(nrighe, ncolonne)) # i pannelli vengono riempiti in riga
par(mfcol=c(nrighe, ncolonne)) # i pannelli vengono riempiti in colonna 
```

## `plot()`


```r
plot(x) # solo una variabile
plot(x, y) # due variabile (scatter plot)
plot(y ~ x) # due variabile, y in funzione di x
```


## Esempi: `plot(x)`


```r
with(benessere, 
     plot(score_au, 
          col = ifelse(genere == 1, "blue", "pink"), 
          pch = ifelse(genere == 1, 3, 16)))
legend(x = 115, y = 48, 
       c("Maschi", "Femmine"), pch = c(3, 16), col =c("blue", "pink"), cex = 2)
```



\begin{center}\includegraphics[width=0.7\linewidth]{04-gRafici_files/figure-beamer/unnamed-chunk-9-1} \end{center}


## Esempi: `plot(x, y)`


```r
with(benessere, 
     plot(score_au, score_ben))
```



\begin{center}\includegraphics[width=0.7\linewidth]{04-gRafici_files/figure-beamer/unnamed-chunk-10-1} \end{center}

## Esempi: `plot(y ~ x)`


```r
with(benessere, 
     plot(score_ben ~ score_au))
```



\begin{center}\includegraphics[width=0.7\linewidth]{04-gRafici_files/figure-beamer/unnamed-chunk-11-1} \end{center}



## Esempi: `plot(y ~ x)` con retta di regressione


```r
with(benessere, 
     plot(score_ben ~ score_au))
abline(lm(score_ben ~ score_au, data = benessere), 
          col = "blue", lty = 3, lwd = 3)
```



\begin{center}\includegraphics[width=0.7\linewidth]{04-gRafici_files/figure-beamer/unnamed-chunk-12-1} \end{center}


## Esempi: `plot(y ~ x)` con x categoriale


```r
benessere$genere <- factor(ifelse(benessere$genere == 1, 
                           "maschio", "femmina"))
plot(score_au ~ genere, data = benessere)
```



\begin{center}\includegraphics[width=0.7\linewidth]{04-gRafici_files/figure-beamer/unnamed-chunk-13-1} \end{center}

## Attenzione!

`plot(y ~ x)` con x categoriale è uguale a `boxplot(y ~ x)`



```r
boxplot(score_au ~ genere, data = benessere)
```



\begin{center}\includegraphics[width=0.7\linewidth]{04-gRafici_files/figure-beamer/unnamed-chunk-14-1} \end{center}




## `hist()`: Frequenze

\scriptsize

```r
hist(benessere$score_au)
```



\begin{center}\includegraphics[width=0.7\linewidth]{04-gRafici_files/figure-beamer/unnamed-chunk-15-1} \end{center}


## `hist()`: Densità

Densità
\scriptsize

```r
hist(benessere$score_au,
     density=50, breaks=20, 
     prob=TRUE, col = "darkblue")
```



\begin{center}\includegraphics[width=0.7\linewidth]{04-gRafici_files/figure-beamer/unnamed-chunk-16-1} \end{center}



## Multi plot (in riga)

\footnotesize


```r
par(mfrow=c(1, 2))

hist(benessere$score_au,density=50, breaks=20, prob=TRUE, 
     main = "Score au")
curve(dnorm(x, mean=mean(benessere$score_au), 
            sd=sd(benessere$score_au)), 
      col="springgreen4", lwd=2, add=TRUE, yaxt="n")

[...]
```




\begin{center}\includegraphics[width=0.7\linewidth]{04-gRafici_files/figure-beamer/unnamed-chunk-18-1} \end{center}





## Multiplot (in colonna)



\begin{center}\includegraphics[width=0.7\linewidth]{04-gRafici_files/figure-beamer/unnamed-chunk-19-1} \end{center}


## Multiplot (in colonna), codice



```r
par(mfcol= c(2,2))
hist(benessere$score_au,density=50, breaks=20, prob=TRUE, 
     main = "Score au")
curve(dnorm(x, mean=mean(benessere$score_au), 
            sd=sd(benessere$score_au)), 
      col="springgreen4", lwd=2, add=TRUE, yaxt="n")

hist(benessere$score_ben,density=50, breaks=20, prob=TRUE, 
     main = "Score benessere")
curve(dnorm(x, mean=mean(benessere$score_ben), 
            sd=sd(benessere$score_ben)), 
      col="royalblue", lwd=2, add=TRUE, yaxt="n")

with(benessere, 
     plot(score_au, score_ben, frame = FALSE))


with(benessere, 
     plot(score_au, score_ben, frame = FALSE))
abline(lm(score_ben ~ score_au, data = benessere), col = "blue", lwd = 2)
```

## `barplot()`

Per creare i grafici a barre quando si hanno variabili discrete o categoriali

Richiede uno step in più $\rightarrow$ la creazione della tabella delle frequenze 

::: columns

:::: column
Frequenze assolute
\small

```r
freq_item1 = table(benessere$item1)
barplot(freq_item1)
```



\begin{center}\includegraphics[width=0.7\linewidth]{04-gRafici_files/figure-beamer/unnamed-chunk-21-1} \end{center}
::::


:::: column
Frequenze relative
\small

```r
perc_item1 = freq_item1/sum(freq_item1)
barplot(perc_item1, ylim = c(0, 1))
```



\begin{center}\includegraphics[width=0.7\linewidth]{04-gRafici_files/figure-beamer/unnamed-chunk-22-1} \end{center}

::::
:::

## `barplot()` con più variabili 


```r
perc_item1_gender = table(benessere$genere, benessere$item1)
perc_item1_gender[1,] = perc_item1_gender[1,]/table(benessere$item1)
perc_item1_gender[2,] = perc_item1_gender[2,]/table(benessere$item1)

barplot(perc_item1_gender, ylim=c(0,1), legend = rownames(perc_item1_gender))
abline(h = .5, lty = 2, col = "red")
```



\begin{center}\includegraphics[width=0.7\linewidth]{04-gRafici_files/figure-beamer/unnamed-chunk-23-1} \end{center}



## Un esempio di multiplot 


\begin{center}\includegraphics[width=0.7\linewidth]{04-gRafici_files/figure-beamer/unnamed-chunk-24-1} \end{center}


## Un esempio di multiplot (codice) 
\footnotesize


```r
item_ben = benessere[, grep("item", colnames(benessere))]

par(mfrow = c(2, round(ncol(item_ben)/2  + 0.2)))
temp = NULL
for (i in 1:ncol(item_ben)) {
  temp = table(benessere$genere, item_ben[,i])
  for (j in 1:nrow(temp)) {
    temp[j,] = temp[j,]/table(item_ben[,i])
  }
barplot(temp, ylim=c(0,1), legend = rownames(temp), main = colnames(item_ben)[i])
abline(h = .5, lty = 2, col = "red")
}
```



## `interaction.plot()`

Permette di vedere l'interazione tra due variabili a seconda di una variabile categoriale: 

`interaction.plot(x, v. categoriale, y)`

\pause

La relazione tra peso e altezza cambia a seconda del genere?




\begin{center}\includegraphics[width=0.7\linewidth]{04-gRafici_files/figure-beamer/unnamed-chunk-26-1} \end{center}


## 


```r
babies$cat_weight = with(babies, 
                         ifelse(peso <= 
quantile(babies$peso)[2], "light", 
ifelse(peso > quantile(babies$peso)[2] & peso > quantile(babies$peso)[4], 
       "medium", "heavy")))
babies$cat_weight = factor(babies$cat_weight, 
                           levels = c("light", "medium", "heavy"))


babies
```

```
       id genere      peso  altezza cat_weight
1   baby1      f  7.424646 62.07722      light
2   baby2      m  7.442727 58.18877      light
3   baby3      f  9.512598 84.52737      heavy
4   baby4      f 11.306349 85.13573     medium
5   baby5      m  9.345165 75.23783      heavy
6   baby6      m  5.411290 46.80163      light
7   baby7      m 17.342840 99.21825     medium
8   baby8      f 11.151913 90.01151      heavy
9   baby9      f 11.854991 82.21790     medium
10 baby10      f  8.906013 65.35360      heavy
```

## 


```r
with(babies, 
     interaction.plot(cat_weight, genere, altezza))
```



\begin{center}\includegraphics[width=0.7\linewidth]{04-gRafici_files/figure-beamer/unnamed-chunk-28-1} \end{center}


# `ggplot2`

## 

è più difficile ma è anche più facile: 


```r
ggplot(dati, 
       ars(x = variabile.x, 
           y = variabile.y, 
           col = variabile.colore.contorno, 
           fill =  variabile.colore.filling, 
           shape =  variabile.shape, 
           size = variabile.size, 
           ...)) + geom_tipo.grafico() + ...
```

Solitamente vuole i dati in formato long

## Scatter plot 


```r
ggplot(benessere, 
       aes(x = score_au, y = score_ben, 
           col = genere, 
           shape = genere)) + 
  geom_point(size = 3)
```



\begin{center}\includegraphics[width=0.7\linewidth]{04-gRafici_files/figure-beamer/unnamed-chunk-30-1} \end{center}

## Scatter plot con retta di regressione 


```r
ggplot(benessere, 
       aes(x = score_au, y = score_ben)) + 
  geom_point(size = 3) + 
  geom_smooth(method = "lm", formula = y ~ x) + theme_light()
```



\begin{center}\includegraphics[width=0.7\linewidth]{04-gRafici_files/figure-beamer/unnamed-chunk-31-1} \end{center}


## `boxplot()` e `violin_plot()`

Sono i grafici prediletti per far vedere le distribuzioni dei dati (specie il violin)

Richiedono che i dati siano in formato long: 




```
    id condition mean_time
1 sbj1         A  4.136174
2 sbj1         B  2.639523
3 sbj2         A  2.547628
4 sbj2         B  4.319068
5 sbj3         A  4.265100
6 sbj3         B  4.113846
```


## 


```r
small = benessere[, c("ID", "score_au", "score_ben")]
score_long  = reshape(small, 
        idvar = "ID", 
        times =names(small)[-1], 
        timevar = "score", v.names = "value",
        varying = list(names(small)[-1]), 
        direction = "long")
head(score_long)
```

```
           ID    score value
1.score_au  1 score_au    33
2.score_au  2 score_au    40
3.score_au  3 score_au    38
4.score_au  4 score_au    38
5.score_au  5 score_au    26
6.score_au  6 score_au    33
```

## boxplot vs violinplot

::: columns

:::: column

Boxplot
\scriptsize


```r
ggplot(score_long, 
       aes(x = score, y = value,
           fill = score)) + geom_boxplot()
```



\begin{center}\includegraphics[width=0.7\linewidth]{04-gRafici_files/figure-beamer/unnamed-chunk-34-1} \end{center}


::::

:::: column

Violinplot

\scriptsize


```r
ggplot(score_long, 
       aes(x = score, y = value,
           fill = score)) + geom_violin(trim = FALSE) 
```



\begin{center}\includegraphics[width=0.7\linewidth]{04-gRafici_files/figure-beamer/unnamed-chunk-35-1} \end{center}


::::

:::

##




```r
score_long = merge(score_long, benessere[, c("ID", "genere")])
ggplot(score_long, 
       aes(x = score, y = value,
           fill = genere)) + geom_violin(trim = FALSE) 
```



\begin{center}\includegraphics[width=0.7\linewidth]{04-gRafici_files/figure-beamer/unnamed-chunk-36-1} \end{center}



# Esportare i grafici

## Esportare i grafici



```r
postscript()  # vector graphics
pdf()

png()          # bitmap graphics
tiff()
jpeg()
bmp()
```


Remember to run off the graphic device once you've saved the graph:


```r
dev.off()
```

(You can do it also manually)




