## Create cron job - 1
  * create a `cronjob` named `ppi` and run with single pod
    * name: `pi`
    * image: `perl:5`
    * command: ["perl", "-Mbignum=bpi", "-wle", "print bpi(2000)"]
  * and the settings is
    * run every `5` minites
    * save `2 completed` jobs
    * save `4 failed` jobs
    * never restart
    * stop pod after `8` seconds
 
## 

