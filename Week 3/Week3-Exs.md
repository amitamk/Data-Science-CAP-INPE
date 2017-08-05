Exs Week3
================
Amita Muralikrishna

-   [Working with Tidy Data](#working-with-tidy-data)
    -   [Fazendo download dos dados de vendedores de comida das ruas de Baltimore:](#fazendo-download-dos-dados-de-vendedores-de-comida-das-ruas-de-baltimore)
    -   [Lendo e armazenando o conteúdo do arquivo baixado:](#lendo-e-armazenando-o-conteúdo-do-arquivo-baixado)
    -   [Removendo "variables" com dados iguais para todas as observações:](#removendo-variables-com-dados-iguais-para-todas-as-observações)
    -   [Convertendo o vetor ItemsSold de cada observação para caixa baixa.](#convertendo-o-vetor-itemssold-de-cada-observação-para-caixa-baixa.)
    -   [Buscando quais locais vendem "hot dog" OU "sausage" ou "franks", criando mais uma variável chamada hotdog, do tipo lógica, que armazena TRUE para locais que vendem um ou mais dos três produtos:](#buscando-quais-locais-vendem-hot-dog-ou-sausage-ou-franks-criando-mais-uma-variável-chamada-hotdog-do-tipo-lógica-que-armazena-true-para-locais-que-vendem-um-ou-mais-dos-três-produtos)
    -   [Procurando por estabelecimentos que vendem pizza:](#procurando-por-estabelecimentos-que-vendem-pizza)
    -   [Extraindo o nome da cidade da variável location:](#extraindo-o-nome-da-cidade-da-variável-location)

Working with Tidy Data
----------------------

### Fazendo download dos dados de vendedores de comida das ruas de Baltimore:

``` r
vendors <- "https://data.baltimorecity.gov/api/views/bqw3-z52q/rows.csv?accessType=DOWNLOAD"
download.file(vendors,destfile = "../BFood.csv")
if (file.exists("../BFood.csv"))
  {
  tam <- file.info("../BFood.csv")$size
  paste("File downloaded, ",tam," bytes")
  } else
  {
  "Error downloading file!"
  }
```

    ## [1] "File downloaded,  15816  bytes"

### Lendo e armazenando o conteúdo do arquivo baixado:

``` r
bVendors <- read.csv(file="../BFood.csv", header=TRUE, sep=",", stringsAsFactors=FALSE)
str(bVendors)
```

    ## 'data.frame':    77 obs. of  8 variables:
    ##  $ Id        : int  0 0 0 0 0 0 0 0 0 0 ...
    ##  $ LicenseNum: chr  "DF000166" "DF000075" "DF000133" "DF000136" ...
    ##  $ VendorName: chr  "Abdul-Ghani, Christina, \"The Bullpen Bar\"" "Ali, Fathi" "Ali, Fathi" "Ali, Fathi" ...
    ##  $ VendorAddr: chr  "508 Washington Blvd, confined within 10 x 10 space" "SEC Calvert & Madison on Calvert" "NEC Baltimore & Pine Sts" "NEC Light & Redwood Sts" ...
    ##  $ ItemsSold : chr  "Grilled food, pizza slices, gyro sandwiches" "Hot Dogs, Sausage, Snacks, Gum, Candies, Drinks" "Hot dogs, Sausage, drinks, snacks, gum, & candy" "Hot dogs, sausages, chips, snacks, drinks, gum" ...
    ##  $ Cart_Descr: chr  "Two add'l tables to be added to current 6' table in U shape, with grill & warming pans, Tent" "Pushcart" "Pushcart" "Pushcart" ...
    ##  $ St        : chr  "MD" "MD" "MD" "MD" ...
    ##  $ Location.1: chr  "Towson 21204\n(39.28540000000, -76.62260000000)" "Owings Mill 21117\n(39.29860000000, -76.61280000000)" "Owings Mill 21117\n(39.28920000000, -76.62670000000)" "Owings Mill 21117\n(39.28870000000, -76.61360000000)" ...

### Removendo "variables" com dados iguais para todas as observações:

``` r
bVendors$Id <- NULL
bVendors$LicenseNum <- as.factor(bVendors$LicenseNum)

bVendors$St <- as.factor(bVendors$St)
str(bVendors$St)
```

    ##  Factor w/ 1 level "MD": 1 1 1 1 1 1 1 1 1 1 ...

``` r
bVendors$St <- NULL

names(bVendors)[names(bVendors) == "Location.1"] <- "location"
str(bVendors)
```

    ## 'data.frame':    77 obs. of  6 variables:
    ##  $ LicenseNum: Factor w/ 76 levels "DF000001","DF000002",..: 50 35 46 48 1 38 33 2 3 4 ...
    ##  $ VendorName: chr  "Abdul-Ghani, Christina, \"The Bullpen Bar\"" "Ali, Fathi" "Ali, Fathi" "Ali, Fathi" ...
    ##  $ VendorAddr: chr  "508 Washington Blvd, confined within 10 x 10 space" "SEC Calvert & Madison on Calvert" "NEC Baltimore & Pine Sts" "NEC Light & Redwood Sts" ...
    ##  $ ItemsSold : chr  "Grilled food, pizza slices, gyro sandwiches" "Hot Dogs, Sausage, Snacks, Gum, Candies, Drinks" "Hot dogs, Sausage, drinks, snacks, gum, & candy" "Hot dogs, sausages, chips, snacks, drinks, gum" ...
    ##  $ Cart_Descr: chr  "Two add'l tables to be added to current 6' table in U shape, with grill & warming pans, Tent" "Pushcart" "Pushcart" "Pushcart" ...
    ##  $ location  : chr  "Towson 21204\n(39.28540000000, -76.62260000000)" "Owings Mill 21117\n(39.29860000000, -76.61280000000)" "Owings Mill 21117\n(39.28920000000, -76.62670000000)" "Owings Mill 21117\n(39.28870000000, -76.61360000000)" ...

### Convertendo o vetor ItemsSold de cada observação para caixa baixa.

``` r
for(i in 1:NROW(bVendors$ItemsSold))
{
  bVendors$ItemsSold[i]<-tolower(bVendors$ItemsSold[i])
}
bVendors$ItemsSold
```

    ##  [1] "grilled food, pizza slices, gyro sandwiches"                                                                                                                                             
    ##  [2] "hot dogs, sausage, snacks, gum, candies, drinks"                                                                                                                                         
    ##  [3] "hot dogs, sausage, drinks, snacks, gum, & candy"                                                                                                                                         
    ##  [4] "hot dogs, sausages, chips, snacks, drinks, gum"                                                                                                                                          
    ##  [5] "large & small beef franks, soft drinks, water, all types of nuts & chips"                                                                                                                
    ##  [6] "hot dogs, sodas, chips"                                                                                                                                                                  
    ##  [7] "hot dogs, sausages, prepackaged snacks, sodas, water, juice, coffee"                                                                                                                     
    ##  [8] "hot dogs, snacks, coffee and soda"                                                                                                                                                       
    ##  [9] "hot dogs, burgers, sausage, chips, soda, water, pretzels"                                                                                                                                
    ## [10] "hot dogs, burgers, sausage, chips, soda, water, pretzels"                                                                                                                                
    ## [11] "hot dogs, hamburgers, soda, water"                                                                                                                                                       
    ## [12] "peanuts, pistachios, water & soda"                                                                                                                                                       
    ## [13] "hot dogs, sodas, peanuts & chips"                                                                                                                                                        
    ## [14] "hot dogs, chips, sodas"                                                                                                                                                                  
    ## [15] "hot dogs, chips & sodas"                                                                                                                                                                 
    ## [16] "hot dogs, sodas, chips"                                                                                                                                                                  
    ## [17] "bottled water, soda & gatorade"                                                                                                                                                          
    ## [18] "hot dogs, chips, sodas & candy"                                                                                                                                                          
    ## [19] "hot dogs, sodas & peanuts"                                                                                                                                                               
    ## [20] "hot dogs, sodas & peanuts"                                                                                                                                                               
    ## [21] "nuts & confections, hot dogs,burgers,& tenders, chili,hot & cold sandwiches, chips,nachos & fries, crab/fish cakes, tacos, breakfast sandwiches & pastries, snowballs, hot & cold drinks"
    ## [22] "soda, water, peanuts & hot dogs"                                                                                                                                                         
    ## [23] "soda, water, peanuts & hot dogs"                                                                                                                                                         
    ## [24] "produce, fruit, vegetables"                                                                                                                                                              
    ## [25] "peanuts, sodas"                                                                                                                                                                          
    ## [26] "hot dogs, italian sausages, hamburgers, peanuts, pistachios, water & soda"                                                                                                               
    ## [27] "hot dogs, peanuts & sodas"                                                                                                                                                               
    ## [28] "hot dogs, sodas, peanuts, chips, water"                                                                                                                                                  
    ## [29] "beef hot dogs, chicken kabobs, soda, chips"                                                                                                                                              
    ## [30] "soda, water,peanuts, sunflower seeds"                                                                                                                                                    
    ## [31] "kebab (lamb & chicken) over rice or sandwich,"                                                                                                                                           
    ## [32] "hot dogs, pre-packaged snacks, pre-packaged soft drinks"                                                                                                                                 
    ## [33] "gyros, hot dogs, souvlaki & soda"                                                                                                                                                        
    ## [34] "hot dogs, burgers"                                                                                                                                                                       
    ## [35] "snowballs, \"pickles pub\" game day menu"                                                                                                                                                
    ## [36] "hot dogs, sandwiches, pretzels, chips, soda, desserts, water, jucie"                                                                                                                     
    ## [37] "hot dogs, jumbo hot dogs, polish sausage, bottled beverages"                                                                                                                             
    ## [38] "hot dogs, jumbo hot dogs, polish hot dogs, chips, drinks"                                                                                                                                
    ## [39] "hot dogs, soda, nachos, pretzels, italian sausage"                                                                                                                                       
    ## [40] "gyro over rice, chicken over rice,gyro on pita,"                                                                                                                                         
    ## [41] "snacks, drinks, cotton candy, hot dogs"                                                                                                                                                  
    ## [42] "snacks, drinks, cotton candy, hot dogs"                                                                                                                                                  
    ## [43] "hot dogs, peanuts, sodas, water, chips, snacks"                                                                                                                                          
    ## [44] "peanuts, soda, water, pistachios"                                                                                                                                                        
    ## [45] "mediterranean rice w/meat, mediterranean wrap sandwiches, chips, soda"                                                                                                                   
    ## [46] "burgers, hot dogs, chawrma, falafel, deli sandwiches, beverage, coffee"                                                                                                                  
    ## [47] "hot dogs, sausage & meatballs, soda, juice,water, chips, candy, cookies, fresh fruit"                                                                                                    
    ## [48] "chicken or lamb over rice, wraps & sandwiches, hot dogs, breakfast, chips, soda"                                                                                                         
    ## [49] "assorted nuts, soda, water, gatorade, cooked food"                                                                                                                                       
    ## [50] "grilled hot dogs, burgers, chicken sandwiches, chicken & pork souvlaki, grilled cheese, nachos & cheese, lamb, chops & sandwiches, soda, juice, snacks"                                  
    ## [51] "hot dogs, chips, candy, bottled water, canned sodas"                                                                                                                                     
    ## [52] "hot dogs, sodas, waters, chips"                                                                                                                                                          
    ## [53] "water, drinks and peanuts"                                                                                                                                                               
    ## [54] "water, peanuts, hot dogs"                                                                                                                                                                
    ## [55] "water & peanuts"                                                                                                                                                                         
    ## [56] "hot dogs, burgers, subs, soda, chips, water, candy, chicken"                                                                                                                             
    ## [57] "mexican food, sodas, water, coffee,chips"                                                                                                                                                
    ## [58] "water, soda"                                                                                                                                                                             
    ## [59] "hot dogs, chips, soda"                                                                                                                                                                   
    ## [60] "gyro sandwiches, hot dogs, chips, sodas"                                                                                                                                                 
    ## [61] "hot dogs, chips, sodas,cookies, snowballs, juice"                                                                                                                                        
    ## [62] "hot dogs, chips, sodas, cookies, snowballs, juice"                                                                                                                                       
    ## [63] "hot dogs, chips, sodas, cookies, snowballs, juice"                                                                                                                                       
    ## [64] "hot dogs, soda, chips"                                                                                                                                                                   
    ## [65] "hot dogs, polish, potato chips, soda, juice, cookies and candy"                                                                                                                          
    ## [66] "hot dogs, hamburgers, chips, soda"                                                                                                                                                       
    ## [67] "hot & cold beverages, packaged snacks, &"                                                                                                                                                
    ## [68] "hot dogs, chicken, lamb, rice, gyros, chips, soda, water"                                                                                                                                
    ## [69] "hot dogs, chili & cheese, popcorn, coffee, tea, hot chocolate, sodas, chips, candy, sodas,"                                                                                              
    ## [70] "hot dogs, drinks, snacks, sausages"                                                                                                                                                      
    ## [71] "mango, pineapple, strawberry, melon, cane, avocado, watermelon, coconut"                                                                                                                 
    ## [72] "hot dogs, chips, cookies, candy, packaged goods, sodas, bottled water"                                                                                                                   
    ## [73] "hot dogs, polish sausage, sodas, chips, juices, candy"                                                                                                                                   
    ## [74] "mediterranean wraps & sandwiches, chicken or beef, over rice, beverages (water, soda, coffee), chips"                                                                                    
    ## [75] "hot dogs, chips, cookies, soda, water, juice, donuts, bagels, candy"                                                                                                                     
    ## [76] "hot dogs, snacks, soda & water, candy"                                                                                                                                                   
    ## [77] "hot dogs, hamburgers, chips, candy, gum, soda, juice"

### Buscando quais locais vendem "hot dog" OU "sausage" ou "franks", criando mais uma variável chamada hotdog, do tipo lógica, que armazena TRUE para locais que vendem um ou mais dos três produtos:

``` r
bVendors$hotdog <- grepl("hot dog|sausage|franks",bVendors$ItemsSold)
head(subset(bVendors, select = c(ItemsSold,hotdog)))
```

    ##                                                                  ItemsSold
    ## 1                              grilled food, pizza slices, gyro sandwiches
    ## 2                          hot dogs, sausage, snacks, gum, candies, drinks
    ## 3                          hot dogs, sausage, drinks, snacks, gum, & candy
    ## 4                           hot dogs, sausages, chips, snacks, drinks, gum
    ## 5 large & small beef franks, soft drinks, water, all types of nuts & chips
    ## 6                                                   hot dogs, sodas, chips
    ##   hotdog
    ## 1  FALSE
    ## 2   TRUE
    ## 3   TRUE
    ## 4   TRUE
    ## 5   TRUE
    ## 6   TRUE

### Procurando por estabelecimentos que vendem pizza:

``` r
bVendors$pizza <- grepl("pizza",bVendors$ItemsSold)
head(subset(bVendors, select = c(ItemsSold,pizza)))
```

    ##                                                                  ItemsSold
    ## 1                              grilled food, pizza slices, gyro sandwiches
    ## 2                          hot dogs, sausage, snacks, gum, candies, drinks
    ## 3                          hot dogs, sausage, drinks, snacks, gum, & candy
    ## 4                           hot dogs, sausages, chips, snacks, drinks, gum
    ## 5 large & small beef franks, soft drinks, water, all types of nuts & chips
    ## 6                                                   hot dogs, sodas, chips
    ##   pizza
    ## 1  TRUE
    ## 2 FALSE
    ## 3 FALSE
    ## 4 FALSE
    ## 5 FALSE
    ## 6 FALSE

### Extraindo o nome da cidade da variável location:

``` r
cidades <- regmatches(bVendors$location,gregexpr("[A-Za-z]+",bVendors$location))
cidadesNovo = vector(mode = "character", length = length(cidades))
for(i in 1:NROW(cidades))
{
  if(!is.na(cidades[[i]][2])){
    cidadesNovo [i] <- paste(cidades[[i]][1],cidades[[i]][2])
  }
  else cidadesNovo[i] <- cidades[[i]][1]
}

cidadesNovo
```

    ##  [1] "Towson"       "Owings Mill"  "Owings Mill"  "Owings Mill" 
    ##  [5] "Baltimore"    "Baltimore"    "Baltimore"    "Baltimore"   
    ##  [9] "Baltimore"    "Baltimore"    "Baltimore"    "Laurel"      
    ## [13] "Randallstown" "Baltimore"    "Baltimore"    "Baltimore"   
    ## [17] "Baltimore"    "Baltimore"    "Baltimore"    "Baltimore"   
    ## [21] "Baltimore"    "Baltimore"    "Baltimore"    "Baltimore"   
    ## [25] "Baltimore"    "Laurel"       "Owings Mill"  "Baltimore"   
    ## [29] "Baltimore"    "Glen Burnie"  "Baltimore"    "Middle River"
    ## [33] "Baltimore"    "Baltimore"    "Baltimore"    "Baltimore"   
    ## [37] "Reisterstown" "Reisterstown" "Baltimore"    "Baltimore"   
    ## [41] "Baltimore"    "Baltimore"    "Baltimore"    "Baltimore"   
    ## [45] "Windsor Mill" "Baltimore"    "Baltimore"    "Windsor Mill"
    ## [49] "Baltimore"    "Baltimore"    "Baltimore"    "Pikesville"  
    ## [53] "Baltimore"    "Baltimore"    "Baltimore"    "Edgewood"    
    ## [57] "Baltimore"    "Baltimore"    "Baltimore"    "Baltimore"   
    ## [61] "Baltimore"    "Baltimore"    "Baltimore"    "Baltimore"   
    ## [65] "Baltimore"    "Pasadena"     "Towson"       "Baltimore"   
    ## [69] "Baltimore"    "Laurel"       "Rosedale"     "Baltimore"   
    ## [73] "Baltimore"    "Windsor Mill" "Baltimore"    "Baltimore"   
    ## [77] "Pikesville"
