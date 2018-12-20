#

Launching the Virtual Machine:
Launch the Vagrant VM inside Vagrant sub-directory using command:
  ```
$ vagrant up
  ```
Log into this using command:
  ```
$vagrant ssh
  ```
Change directory to /vagrant and look for the LogsAnalysis folder.
newsdata.sql places in this folder

Load the data in local database using the command:
  ```
psql -d news -f newsdata.sql
  ```
Connect to database using this command:
  ```
psql -d news
  ```

Create view article_view using:
  ```
create view article_view as select title,author,count(*) as views from articles,log where
  log.path like concat('%',articles.slug) group by articles.title,articles.author
  order by views desc;
  ```

  Check the View:
  ```
\d article_view
 title  | text    |
 author | integer |
 views  | bigint  |
  ```
Create vier error_log_view using:
   ```
 create view error_log_view as select date(time),round(100.0*sum(case log.status when '200 OK'
  then 0 else 1 end)/count(log.status),2) as "Percent Error" from log group by date(time)
  order by "Percent Error" desc;
  ```
Check the error_log_view using:
  ```
news=> \d error_log_view
 date          | date    |
 Percent Error | numeric |
  ```

Running the application :
  ```
$ python3 LogsAnaysis.py
  ```
