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
   .mode csv
   .import <path-your-csv-file> <db-table-name> --skip <n>
   
   # Alternative with explicit separator
   sqlite3 <db-name> 
   .separator ,
   .import <path-your-csv-file> <db-table-name> --skip 1 --csv
   ```
3. That's it, now you can execute any SQL query on the imported data.   
   ```bash
   sqlite3 <db-name>
   .tables
   .schema <table-name>
   .mode column
   SELECT * FROM <table-name> LIMIT 3;
   ```
4. Alternatively you can use `DB Browser for SQLite`, it has GUI:  
   https://sqlitebrowser.org/
