# ``` BigQuery: Qwik Start - Command Line ```
```bash
bq show bigquery-public-data:samples.shakespeare
bq help query
bq query --use_legacy_sql=false \
'SELECT
   word,
   SUM(word_count) AS count
 FROM
   `bigquery-public-data`.samples.shakespeare
 WHERE
   word LIKE "%raisin%"
 GROUP BY
   word'
bq query --use_legacy_sql=false \
'SELECT
   word
 FROM
   `bigquery-public-data`.samples.shakespeare
 WHERE
   word = "huzzah"'
bq ls
bq ls bigquery-public-data:
bq mk babynames
bq ls
curl -LO http://www.ssa.gov/OACT/babynames/names.zip
ls
unzip names.zip
ls
bq load babynames.names2010 yob2010.txt name:string,gender:string,count:integer
bq ls babynames
bq show babynames.names2010
bq query "SELECT name,count FROM babynames.names2010 WHERE gender = 'F' ORDER BY count DESC LIMIT 5"
bq query "SELECT name,count FROM babynames.names2010 WHERE gender = 'M' ORDER BY count ASC LIMIT 5"
bq rm -r babynames
```
Confirm the delete command by typing Y.
