---
layout: post
title: Import CSV file into SQLite
date: 2026-03-11
categories: sqlite bash csv
---

1. Download SQLite precompiled binaries and add them to your $PATH  
  https://sqlite.org/download.html
2. Open SQLite and import the CSV file:
```bash
# Note: First row is used for column names, if you don't have header - create table before import
sqlite3 <db-name>
 .mode csv # or use flag --csv
 .import <path-your-csv-file> <db-table-name> --skip <n>

sqlite3 <db-name> 
 .separator , # optional as comma is default separator
 .import <path-your-csv-file> <db-table-name> --skip 1 --csv
```

3. That's it, now you can execute any SQL query on the imported data.
```bash
sqlite3 <db-name>
 .tables #show list of tables
 .schema <table-name> #i.e. you have imported your csv file into test table
 .mode column #by default there is list mode - no column names
 select * from <table-name> limit 3; 
```

4. Alternatively you can use `DB Browser for SQLite`, it has GUI:  
   <https://sqlitebrowser.org/>
