# Лабораторна робота No 4. Зчитування даних з реляційних баз даних.
Дані для цієї лабораторної роботи взяті з «https://www.kaggle.com/benhamner/nips-2015-papers» Для виконання лабораторної необхідно завантажити файл бази даних SQLite за посиланням: «https://www.dropbox.com/s/pf2htfcrdoqh3ii/database.sqlite?dl=0».
В цьому файлі містяться дані по доповідям на Neural Information Processing Systems (NIPS) яка є однією з ведучих конференцій по машинному навчанню в світі. База даних складається з наступних таблиць:
Papers
This file contains one row for each of the 403 NIPS papers from this year's conference.
It includes the following fields
 Id - unique identifier for the paper (equivalent to the one in NIPS's system)
 Title - title of the paper
 EventType - whether it's a poster, oral, or spotlight presentation
 PdfName - filename for the PDF document
 Abstract - text for the abstract (scraped from the NIPS website)
 PaperText - raw text from the PDF document (created using the tool pdftotext)
Authors
This file contains id's and names for each of the authors on this year's NIPS papers.
 Id - unique identifier for the author (equivalent to the one in NIPS's system)
 Name - author's name
PaperAuthors
This file links papers to their corresponding authors.
 Id - unique identifier
 PaperId - id for the paper
 AuthorId - id for the author
Для роботи з базою даних SQLite в R можна використовувати бібліотекі DBI та
RSQLite.
Спочатку необхідно створити з’єдання з базою даних
conn <- dbConnect(RSQLite::SQLite(), "Шлях до БД")
Функція dbSendQuery(conn, "Запит») формує запит і виконує його в БД. Для
отримання результатів у вигляді фрейму даних необхідно використати функцію
dbFetch(res, n), де n – кількість строк, які будуть отримані. Для обмеження виводу
даних, в подальших завданнях встановіть n = 10.
В результаті виконання лабораторної роботи необхідно створити фрейми даних:
1. Назва статті (Title), тип виступу (EventType). Необхідно вибрати тільки статті
с типом виступу Spotlight. Сортування по назві статті.
2. Ім’я автора (Name), Назва статті (Title). Необхідно вивести всі назви статей
для автора «Josh Tenenbaum». Сортування по назві статті.
3. Вибрати всі назви статей (Title), в яких є слово «statistical». Сортування по
назві статті.
4. Ім’я автору (Name), кількість статей по кожному автору (NumPapers).
Сортування по кількості статей від більшої кількості до меньшої.

## 0.
```{r}
library(RSQLite)
dbPath <- "./database.sqlite"
conn <- dbConnect(RSQLite::SQLite(), dbPath)
n <- 10
fetchQuery <- function(query) {
  res <- dbSendQuery(conn, query)
  dataFetched <- dbFetch(res, n=n)
  dbClearResult(res)
  return(dataFetched)
}
```

## 1.
```{r}
sqlQuery1 <- "SELECT Title, EventType FROM Papers WHERE EventType='Spotlight' ORDER BY Title"
dataFrame1 <- fetchQuery(sqlQuery1)
dataFrame1
```
```
                                                                                          Title EventType
1  A Tractable Approximation to Optimal Point Process Filtering: Application to Neural Encoding Spotlight
2                                    Accelerated Mirror Descent in Continuous and Discrete Time Spotlight
3                        Action-Conditional Video Prediction using Deep Networks in Atari Games Spotlight
4                                                                      Adaptive Online Learning Spotlight
5                          Asynchronous Parallel Stochastic Gradient for Nonconvex Optimization Spotlight
6                                                 Attention-Based Models for Speech Recognition Spotlight
7                                                       Automatic Variational Inference in Stan Spotlight
8                                   Backpropagation for Energy-Efficient Neuromorphic Computing Spotlight
9                       Bandit Smooth Convex Optimization: Improving the Bias-Variance Tradeoff Spotlight
10                         Biologically Inspired Dynamic Textures for Probing Motion Perception Spotlight
```

## 2.
```{r}
sqlQuery2 <- "SELECT Authors.Name, Papers.Title FROM Papers
JOIN PaperAuthors ON Papers.Id=PaperAuthors.PaperId
JOIN Authors ON Authors.Id=PaperAuthors.AuthorId
WHERE Name='Josh Tenenbaum' ORDER BY Papers.Title"
dataFrame2 <- fetchQuery(sqlQuery2)
dataFrame2
```
```
                                                                                                      Name                                                                                             Title
1 Josh Tenenbaum                                                       Deep Convolutional Inverse Graphics Network
2 Josh Tenenbaum Galileo: Perceiving Physical Object Properties by Integrating a Physics Engine with Deep Learning
3 Josh Tenenbaum                                                Softstar: Heuristic-Guided Probabilistic Inference
4 Josh Tenenbaum                                                        Unsupervised Learning by Program Synthesis

```
## 3.
```{r}
sqlQuery3 <- "SELECT Title FROM Papers WHERE Title LIKE '%statistical%' ORDER BY Title"
dataFrame3 <- fetchQuery(sqlQuery3)
dataFrame3
```
```                                                                                                                                                                                    Title
1 Adaptive Primal-Dual Splitting Methods for Statistical Learning and Image Processing
2                                Evaluating the statistical significance of biclusters
3                  Fast Randomized Kernel Ridge Regression with Statistical Guarantees
4     High Dimensional EM Algorithm: Statistical Optimization and Asymptotic Normality
5                Non-convex Statistical Optimization for Sparse Tensor Graphical Model
6            Regularized EM Algorithms: A Unified Framework and Statistical Guarantees
7                            Statistical Model Criticism using Kernel Two Sample Tests
8                         Statistical Topological Data Analysis - A Kernel Perspective
```

## 4.
```{r}
sqlQuery4 <- "SELECT Authors.Name, count(Authors.Id) as NumPapers FROM Papers
JOIN PaperAuthors ON Papers.Id=PaperAuthors.PaperId
JOIN Authors ON Authors.Id=PaperAuthors.AuthorId
GROUP BY Authors.Id ORDER BY count(Authors.Id) DESC"
dataFrame4 <- fetchQuery(sqlQuery4)
dataFrame4
```
```                                                                                                                                                                                                       Name NumPapers
1  Pradeep K. Ravikumar         7
2               Han Liu         6
3        Lawrence Carin         6
4   Inderjit S. Dhillon         5
5               Le Song         5
6     Zoubin Ghahramani         5
7        Christopher Ré         4
8  Simon Lacoste-Julien         4
9          Zhaoran Wang         4
10     Csaba Szepesvari         4
```
