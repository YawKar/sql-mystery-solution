# Получаем нужный отчёт об убийстве
Запрос:
```sql
SELECT * 
  FROM crime_scene_report
 WHERE date = '20180115'
       AND city = 'SQL City'
       AND type = 'murder'
```
Вывод:
| date     | type   | description                                                                                                                                                                                  | city     |
|----------|--------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|----------|
| 20180115 | murder | Security  footage shows that there were 2 witnesses. The first witness lives at  the last house on "Northwestern Dr". The second witness, named Annabel,  lives somewhere on "Franklin Ave". | SQL City |

# Найдём обоих подозреваемых в таблице `person`
Запрос:
```sql
SELECT *
  FROM person
 WHERE name LIKE 'Annabel %'
       OR (address_street_name = 'Northwestern Dr'
           AND address_number = (SELECT MAX(address_number)
                                   FROM person
                                  WHERE address_street_name = 'Northwestern Dr'))
```
Вывод:
| id    | name           | license_id | address_number | address_street_name | ssn       |
|-------|----------------|------------|----------------|---------------------|-----------|
| 14887 | Morty Schapiro | 118009     | 4919           | Northwestern Dr     | 111564949 |
| 16371 | Annabel Miller | 490173     | 103            | Franklin Ave        | 318771143 |
