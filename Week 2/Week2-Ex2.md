Week2-Ex2
================
Amita Muralikrishna

-   [Exercício 2 da Week 2](#exercício-2-da-week-2)
    -   [Criando vetor com as médias mensais de precipitação de Baltimore (em inches):](#criando-vetor-com-as-médias-mensais-de-precipitação-de-baltimore-em-inches)
    -   [Criando vetor com os nomes abreviados dos meses do ano:](#criando-vetor-com-os-nomes-abreviados-dos-meses-do-ano)
    -   [Convertendo as temperaturas de inch para mm (1 inch = 25.4 mm):](#convertendo-as-temperaturas-de-inch-para-mm-1-inch-25.4-mm)
    -   [Calculando e exibindo, respectivamente, as medidas de média, mínima e máxima precipitação:](#calculando-e-exibindo-respectivamente-as-medidas-de-média-mínima-e-máxima-precipitação)

Exercício 2 da Week 2
---------------------

### Criando vetor com as médias mensais de precipitação de Baltimore (em inches):

``` r
baltimore <- c(3.47,    3.02, 3.93, 3.00,   3.89,   3.43,   3.85,   3.74,   3.98,   3.16,   3.12,   3.35)
```

### Criando vetor com os nomes abreviados dos meses do ano:

``` r
names(baltimore) <- c("Jan","Feb","Mar","Apr","May","Jun","Jul","Aug","Sep","Oct","Nov","Dec")
```

### Convertendo as temperaturas de inch para mm (1 inch = 25.4 mm):

``` r
baltimoreInMM <- baltimore * 25.4
baltimoreInMM
```

    ##     Jan     Feb     Mar     Apr     May     Jun     Jul     Aug     Sep 
    ##  88.138  76.708  99.822  76.200  98.806  87.122  97.790  94.996 101.092 
    ##     Oct     Nov     Dec 
    ##  80.264  79.248  85.090

### Calculando e exibindo, respectivamente, as medidas de média, mínima e máxima precipitação:

``` r
media <- mean(baltimoreInMM)
media
```

    ## [1] 88.773

``` r
minima <- min(baltimoreInMM)
minima
```

    ## [1] 76.2

``` r
maxima <- max(baltimoreInMM)
maxima
```

    ## [1] 101.092
