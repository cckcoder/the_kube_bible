# Kube Cronjobs

## Usecase:
* Taking database backup regularly every Sunday 1 P.M.
* Clearingb cached data every Monday at 4 P.M.
* Sending a queued email every 5 minute
* Various maintenance operations t be executed regulary

## Cron Expression

* Minutes
* Hour
* Day of month
* Month
* Day of week

### Example
* "10 11 * * ****" => mean "At 11:10 every day of every month."
* "10 11 * 12 *" => mean "At 11:10 every day of December."
* "10 11 * 12 1****" => mean "At 11:10 every Monday of December."
* "10 11 * * 1,2****" => mean "At 11:10 every Monday and Tueday
of every month."
* "10 11 * 2-5 *" => mean "At 11:10 every day from Feb to May."


## Ref
* [kube-cronjob](https://www.baeldung.com/ops/kubernetes-run-cron-jobs)
* [google kube-cronjob](https://cloud.google.com/kubernetes-engine/docs/how-to/cronjobs)
* [kube cronjob trick](https://medium.com/@pranay.shah/how-to-get-logs-from-cron-job-in-kubernetes-last-completed-job-7957327c7e76)
