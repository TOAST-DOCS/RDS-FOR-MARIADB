## Database > RDS for MariaDB > Release Notes

### November 15, 2022

#### Bug Fixes

* Fixed an issue where, when `sha256_password` is set under the `default_authentication_plugin` parameter, high availability configuration is turned off.

### October 11, 2022

#### Feature Updates

* Changed the domain change tooltip displayed on the instance details screen

#### Bug Fixes

* Removed no longer used event codes
* Fixed an issue where a read replica cannot be deleted intermittently under certain conditions

### September 14, 2022

#### Bug Fixes

* Fixed an issue where an error message is left on the browser’s developer console
* Fixed an issue where backup fails when a single instance in a **Not Use** status for **Use table locking** is changed to a high availability instance

### August 9, 2022

#### Added Features

* Added a feature to export event lists to Excel

#### Feature Updates

* Made modifications so that the DB Configuration of an instance where high availability is paused can be changed
* Changed the maximum backup retention period from 30 days to 2 years
* Made improvements so that, when backup fails due to DDL execution, the cause is left in the event message

#### Bug Fixes

* Fixed an issue where backup fails intermittently due to communication issues with internal agents

### July 12, 2022

#### Added Features

* Added a feature to view charts by grouping them per server on Server Dashboard

#### Feature Updates

* Made modifications so that the DB Configuration of a read replica in a **replication stopped** status can be changed

#### Bug Fixes

* Fixed an issue where DB instances in a **connection failed** status are displayed as **normal** intermittently
* Fixed an issue where, when creating a read replica, backup execution is left in event logs even if the execution is not performed
* Fixed an issue where volume scaling fails intermittently

### June 14, 2022

#### Feature Updates

* Made improvements so that an event is logged when restart fails due to replication delay
* Changed the access information domain from cloud.toast.com to nhncloudservice.com

#### Bug Fixes

* Fixed an issue where high availability configuration is not possible when the validate password plugin is used
* Fixed an issue where, even though the type change of the high availability instance has failed, the type of the candidate master is displayed as the type after the change

### May 10, 2022

#### Feature Updates

* Changed the error log storage location to the data volume
* Made changes so that error logs are rotated up to 10 logs with a size of 100 MB
* Made modifications so that, when a forced restart is executed, the web console cannot be operated until it can be used again
* Made modifications so that, after failover starts, the target instance cannot be manipulated in the web console
* Improved usability so that you can view the innodb status in the processlist
* Made improvements so that you can move to other pages by numbers in the processlist
* Made improvements so that you can zoom in the chart in the processlist to view only the corresponding section
* Made improvements so that you can search by keywords in the processlist
* Made improvements so that you can download the results searched in the processlist in CSV format

#### Bug Fixes

* Fixed an issue where, when performing point-in-time restoration with a backup of a read replica, a wrong restoration available time could be selected
* Fixed an issue where the monitoring graph is not visible in Safari
* Fixed an issue where, after changing the parameters of the master instance, changing the parameters of a read replica failed

### April 12, 2022

#### Feature Updates

* Made improvements so that, when changing a read replica or normal instance to a high availability instance, replication is configured without additional backup if there is an existing backup available

#### Bug Fixes

* Fixed an issue where, if instance stop and instance volume scaling are performed at the same time, the instance volume scaling operation does not end indefinitely
* Fixed an issue where an error occurs while restarting when the remaining space of the data volume is less than 1%
* Fixed an issue where an event of backup failure is logged intermittently even after successful backup

### March 15, 2022

#### Added Features

* Added a feature to use variables in **DB configuration**

#### Bug Fixes

* Fixed an issue where monitoring data is not collected under certain conditions
* Fixed an issue where an automatic backup of failed-over instance is not deleted
* Fixed an issue where an automatic backup that has failed to be created is not deleted when it reaches its expiration date
* Fixed an issue where restoration fails when there are too many users registered in MySQL
* Fixed an issue where a backup settings modification event is logged even when the access rule is modified

### January 11, 2022

#### Added Features

* Added a feature to check the running process list and InnoDB status in MariaDB

#### Feature Updates

* Changed the minimum length of the name that can be entered when creating a DB schema in the web console to 1 character, which is the same as that of MySQL

### December 14, 2021 

#### New Releases 

- TOAST Relational Database Service (RDS) provides Relational Database in the cloud environment. 
- No complicated configuration is required to enable relational database. 
- Supports MariaDB 10.3.30.  
