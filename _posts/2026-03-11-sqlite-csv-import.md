---
layout: post
title: Import CSV file into SQLite
date: 2026-03-11
categories: sqlite bash csv
---

1. Download SQLite precompiled binaries and add them to your $PATH  
  https://sqlite.org/download.html
2. Open SQLite and import the CSV file:  
    > note : first row is used for table names, if you want to skip it
```bash
sqlite3
 .mode csv
 .import <path-your-csv-file> <db-table-name>
```
3. That's it, now you can execute any SQL query on the imported data.
```bash
sqlite3
 .tables #show list of tables
test
 .schema test #i.e. you have imported your csv file into test table
 .mode column #by default there is list mode - no column names
 select * from test limit 3; 
```
4. Alternatively you can use `DB Browser for SQLite`, it has GUI:  
   <https://sqlitebrowser.org/>
