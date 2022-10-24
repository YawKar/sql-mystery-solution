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

# Найдём обоих свидетелей в таблице `person`
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

# Разузнаем информацию про <b>Morty Schapiro</b> и <b>Annabel Miller</b>
Запрос:
```sql
SELECT name, age, height, eye_color, hair_color, gender,
       plate_number, car_make, car_model,
       annual_income
  FROM person p
       LEFT JOIN drivers_license dl ON p.license_id = dl.id
       LEFT JOIN income i ON i.ssn = p.ssn
 WHERE p.id = 14887
       OR p.id = 16371
```
Вывод:
| name           | age | height | eye_color | hair_color | gender | plate_number | car_make      | car_model | annual_income |
|----------------|-----|--------|-----------|------------|--------|--------------|---------------|-----------|---------------|
| Morty Schapiro | 64  | 84     | blue      | white      | male   | 00NU00       | Mercedes-Benz | E-Class   | null          |
| Annabel Miller | 35  | 65     | green     | brown      | female | 23AM98       | Toyota        | Yaris     | null          |

# Достанем интервью с обоими свидетелями
Запрос:
```sql
SELECT name, transcript
  FROM person p
       LEFT JOIN interview i ON i.person_id = p.id
 WHERE p.id = 14887
       OR p.id = 16371
```
Вывод:
| name           | transcript                                                                                                                                                                                                                          |
|----------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Morty Schapiro | I  heard a gunshot and then saw a man run out. He had a "Get Fit Now Gym"  bag. The membership number on the bag started with "48Z". Only gold  members have those bags. The man got into a car with a plate that  included "H42W". |
| Annabel Miller | I saw the murder happen, and I recognized the killer from my gym when I was working out last week on January the 9th.                                                                                                               |

