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
 
## Create cronjob - 2
  * you need to create a pod in specified time
    * in `/ckad/CKAD000016/periodc.yaml` will define this pod
    * send command `date` in container `busybox:stable`, and the command should run run every minutes and it should completed in `10` seconds. (Note: cronjob's name and container's name should be `hello`)
    * please use the under resources and confirm that the job should run at least once.
    
    
