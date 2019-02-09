# Upload3-constructeurs

## TOC:
- import and validate xlsx
- pull catalogs directory from CMS
- pull constructeurs (publishers) directory from CMS
- for each constructeur in xlsx check constructeur in CMS, report missing, and commit if option (--commit) is set.
- for each catalog in xlsx report missing-catalog in CMS, and commit if option (--create) is set.
- for each catalog in xlsx, find constructeur and add a reference to this catalog. (cross-index)
- for each constructeur, insert/update cc-relations (catalogs->construteur) when needed.
- report constructeurs without pdf.

## Command line Options supersede other instructions.
- (--zero-constructeurs) remove all constructeurs from CMS instance.
- (--zero-catalogs) remove all catalogs from CMS instance.
- (--zero-cc) remove all cc-relation (catalogue-constructeur) from CMS instance.
- (-h _host-name_) 
- (-p _password_) 

## Environment Variables
```
$ export PGHOST=inhelium.com
$ export PGPORT=5432
$ export PGUSER=postgres
$ export PGDATABASE=cms-oacs
$ export PGPASSWORD=my-secret-password
```

## Configuration file
example:
```
---
  host: inhelium.com
  port: 5432
  database: cms-oacs
  user: postgres
  pg_monitor: false
  app_instance: 'cms-236393'
  xlsx: ./0-Heating-Museum-from-start-to-31-December-2018-FRENCN-20190208.xlsx
```
DO NOT STORE the password in that file, it must be set on command line or environments variables.

## Operations
### Settings
- configuration/settings are specified in file `.env.yaml` or on the command line
- the program look first for `./.env.yaml` in current/working directory.
- use option (-y _<yaml-env>_) to open another config file

# Import and validate xlsx
- xlsx file is most of the time specified in yaml configuration file.
- if given on the command line, it supersed yaml config.
- xlsx-not-found is a fatal-error and stops the program.
- xlsx is scanned, validated and reformatted to give (constructeur, auteurs, indexNames[]) for each xlsx line.

# Validate constructeurs against CMS.
- clear constructeurs from CMS instance using option (--zero-constructeurs)
- pull constructeurs directory from CMS.
- Note: constructeurs referenced by their name (nor_au2), not their title.
- Ideally h1 should be used as title: constructeur legal (official) name, but due to incorrect h1, indexName[0] is used as official name instead.
- for each catalog (xlsx line sec!=3), find constructeur and alert if not in cms-constructeurs-directory.
- if option (--commit-constructeurs), then commit missing constructeur.
- if constructeur exists, add a (dirty) reference to this catalog if needed.
- report constructeurs without catalogs.

# Commit relations (cc) catalog-constructeur
- clear relations cc from CMS instance using option (--zero-cc)
- for each constructeur, check for dirty references (not commited) to catalogs.
- commit if option (--commit-cc) is set.

