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

#
