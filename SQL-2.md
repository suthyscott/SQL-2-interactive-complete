## Updating
 
1. update the artworks table so that the piece of art with the id of 7 has a name of 'Saint George and the Dragon'
```
UPDATE artworks 
SET name = 'Saint George and the Dragon'
WHERE artwork_id = 7; 
```

2. update the artworks table so that the piece of art with the id of 1 has a painter id (foreign key) of of 1.
```
UPDATE artworks 
SET painter_id = 1 
WHERE artwork_id = 1; 
```

3. update the artworks table so that the piece of art with the id of 31 has a year of 1962. 
```
UPDATE artworks 
SET year = 1962 
WHERE artwork_id = 31;
```

## Joins, subqueries, and views

1. 
  - show all the painter names from the painters table along with all the era names from the eras table.

```
SELECT p.name, e.name from painters p 
JOIN eras e ON p.era_id = e.era_id;
```

  -  Do the same as the previous problem, but this time alias the column names so that we can see which is the painter and which is the era. 
```
SELECT a.name artwork, a.year, p.name painter from artworks a 
JOIN painters p ON a.painter_id = p.painter_id;
```

2. 
  - Show the artworks names, artworks years, and painters names from the artworks and painters tables. 
  ```
  SELECT a.name, a.year, p.name from artworks a 
  JOIN painters p ON a.painter_id = p.painter_id;
  ```
  - Do the same as the previous problem, but alias the painting name and the painter's name so that we can see which is which. 
  ```
  SELECT a.name artwork, a.year, p.name painter from artworks a 
  JOIN painters p ON a.painter_id = p.painter_id;
```

3.  select the artwork name (aliased as artwork), the artwork year, and the painter's name (aliased as painter) from the artworks and painters table. Order the results by year, starting with the earliest. 
  ```
  SELECT a.name as artwork, a.year, p.name as painter from artworks a 
  JOIN painters p ON a.painter_id = p.painter_id
  WHERE a.year > 1600
  ORDER BY a.year;
```

4. Select the era name (aliased as era), the painter's name (aliased as painter), the artwork name (as artwork) and the artwork year from the artworks, eras and painters tables.
  ```
  SELECT e.name era, p.name painter, a.name artwork, a.year from artworks a
JOIN painters p on a.painter_id = p.painter_id
JOIN eras e ON p.era_id = e.era_id;
``` 

5. Save the above query as a view called all_artwork_info. 
```
CREATE VIEW all_artwork_info AS
SELECT e.name era, p.name painter, a.name artwork, a.year FROM artworks a
JOIN painters p ON a.painter_id = p.painter_id
JOIN eras e ON p.era_id = e.era_id;
```

6. Run the view you just created
```
SELECT * FROM all_artwork_info;
```

7. Run the all_artwork_info view, but only show the results where the year is earlier than 1800. 
```
SELECT * FROM all_artwork_info
WHERE year < 1800;
```

8. Using a subquery, select everything from the painters table where the corresponding name in the eras table equals "Cubism"
```
SELECT * FROM painters 
WHERE era_id IN (select era_id FROM eras WHERE name = 'Cubism');
```

9. Using a subquery, select the names and years of the rows in the artworks table where the corresponding painter's name in the painter's table starts with "M". 
```
SELECT name, year FROM artworks 
WHERE painter_id IN (SELECT painter_id FROM painters WHERE name LIKE 'M%');
```

10. Do the same as previous problem, but filter the results to show only those where the year is later than 1700. Then save that query to a view called new_m_painters. 
```
SELECT name, year FROM artworks 
WHERE painter_id IN (select painter_id FROM painters WHERE name LIKE 'M%')
AND year > 1700;

CREATE VIEW new_m_painters AS
SELECT name, year FROM artworks 
WHERE painter_id IN (SELECT painter_id FROM painters WHERE name LIKE 'M%')
AND year > 1700;
```


## Deleting

1. 
 Why does the below query not work?
```
DELETE FROM painters 
WHERE painter_id = 9;
```

There is a row(s) in the artworks table that are depending on the painter_id of 9 as a foreign key. Resolve the error from the above query so that you can run it successfully. 
```
DELETE FROM artworks 
WHERE painter_id = 9; 
```

2. Delete everything from the artworks table that is later than 1960 
```
DELETE FROM artworks
WHERE year > 1960;
```

3. Using nested subqueries, delete everything from artworks where the associated era name starts with "I"
```
DELETE FROM artworks 
WHERE painter_id IN (
    SELECT painter_id FROM painters 
    WHERE era_id IN (
        SELECT era_id FROM eras 
        WHERE name LIKE 'I%'
    )
);
```