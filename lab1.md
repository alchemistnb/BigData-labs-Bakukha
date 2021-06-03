## Лабораторна робота № 1. Завантаження та зчитування даних.

# 1. За допомогою download.file() завантажте любий excel файл з порталу
http://data.gov.ua та зчитайте його (xls, xlsx – бінарні формати, тому
встановить mode = “wb”. Виведіть перші 6 строк отриманого фрейму
даних.

```{r}
library(xlsx)
xlsxUrl <- "https://data.gov.ua/dataset/841644c2-4eca-40b0-8e58-6b70db3fc481/resource/162f60ce-2048-4bb5-8546-c28f98aa6f4f/download/fuel_monthly_202002_full.xlsx"
download.file(xlsxUrl, "data1.xlsx", "auto", TRUE, "wb")
data1 <- read.xlsx("data1.xlsx", 1)
head(data1, 6);
```
# A tibble: 6 x 9
   year month company   company_code plant_name fuel_type
  <dbl> <dbl> <chr>     <chr>        <chr>      <chr>    
1  2018     1 "АТ \"Дн~ 00130820     Дніпровсь~ АШ+П     
2  2018     1 "АТ \"Дн~ 00130820     Дніпровсь~ ГД       
3  2018     1 "АТ \"Дн~ 00130820     Дніпровсь~ мазут    
4  2018     1 "АТ \"Дн~ 00130820     Дніпровсь~ газ      
5  2018     1 "АТ \"Хе~ 00131771     Херсонськ~ АШ+П     
6  2018     1 "АТ \"Хе~ 00131771     Херсонськ~ ГД       
# ... with 3 more variables: income <dbl>, spend <dbl>,
#   reserve_fact <dbl>

