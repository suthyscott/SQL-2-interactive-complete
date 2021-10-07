## Updating
 
1. update the artworks table so that the piece of art with the id of 7 has a name of 'Saint George and the Dragon'
```update artworks 
set name = 'Saint George and the Dragon'
where artwork_id = 7; 
```

2. update the artworks table so that the piece of art with the id of 1 has a painter id (foreign key) of of 1.
```update artworks 
set painter_id = 1 
where artwork_id = 1; 
```

3. update the artworks table so that the piece of art with the id of 31 has a year of 1962. 
```update artworks 
set year = 1962 
where artwork_id = 31;
```

## Joins, subqueries, and views

1. 
  - show all the painter names from the painters table along with all the era names from the eras table.

```
select p.name, e.name from painters p 
join eras e on p.era_id = e.era_id;
```

  -  Do the same as the previous problem, but this time alias the column names so that we can see which is the painter and which is the era. 
```
select a.name artwork, a.year, p.name painter from artworks a 
join painters p on a.painter_id = p.painter_id;
```

2. 
  - Show the artworks names, artworks years, and painters names from the artworks and painters tables. 
  ```
  select a.name, a.year, p.name from artworks a 
  join painters p on a.painter_id = p.painter_id;
  ```
  - Do the same as the previous problem, but alias the painting name and the painter's name so that we can see which is which. 
  ```
  select a.name artwork, a.year, p.name painter from artworks a 
join painters p on a.painter_id = p.painter_id;
```

3.  select the artwork name (aliased as artwork), the artwork year, and the painter's name (aliased as painter) from the artworks and painters table. Order the results by year, starting with the earliest. 
  ```
  select a.name as artwork, a.year, p.name as painter from artworks a 
join painters p on a.painter_id = p.painter_id
where a.year > 1600
order by a.year;
```

4. Select the era name (aliased as era), the painter's name (aliased as painter), the artwork name (as artwork) and the artwork year from the artworks, eras and painters tables.
  ```
  select e.name era, p.name painter, a.name artwork, a.year from artworks a
join painters p on a.painter_id = p.painter_id
join eras e on p.era_id = e.era_id;
``` 

5. Save the above query as a view called all_artwork_info. 
```
create view all_artwork_info as
select e.name era, p.name painter, a.name artwork, a.year from artworks a
join painters p on a.painter_id = p.painter_id
join eras e on p.era_id = e.era_id;
```

6. Run the view you just created
```
select * from all_artwork_info;
```

7. Run the all_artwork_info view, but only show the results where the year is earlier than 1800. 
```
select * from all_artwork_info
where year < 1800;
```

8. Using a subquery, select everything from the painters table where the corresponding name in the eras table equals "Cubism"
```
select * from painters 
where era_id in (select era_id from eras where name = 'Cubism');
```

9. Using a subquery, select the names and years of the rows in the artworks table where the corresponding painter's name in the painter's table starts with "M". 
```
select name, year from artworks 
where painter_id in (select painter_id from painters where name like 'M%');
```

10. Do the same as previous problem, but filter the results to show only those where the year is later than 1700. Then save that query to a view called new_m_painters. 
```
select name, year from artworks 
where painter_id in (select painter_id from painters where name like 'M%')
and year > 1700;

create view new_m_painters as 
select name, year from artworks 
where painter_id in (select painter_id from painters where name like 'M%')
and year > 1700;
```


## Deleting

1. 
  - Why does the below query not work?
```
delete from painters 
where painter_id = 9;
```

  - Resolve the error from the above query so that you can run it successfully. 
```
delete from artworks 
where painter_id = 9; 
```

2. Delete everything from the artworks table that is later than 1960 
```
delete from artworks
where year > 1960;
```

3. Using nested subqueries, delete everything from artworks where the associated era name starts with "I"
```
delete from artworks 
where painter_id in (
    select painter_id from painters 
    where era_id in (
        select era_id from eras 
        where name like 'I%'
    )
);
```