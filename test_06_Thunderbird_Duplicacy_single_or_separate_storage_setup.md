## Test #6

## Objective

We have seen from test # 4 that the best configuration for database backup is to use 1M **fixed** chunk.

On the other hand, it seems that the best setting for the other file types is 1M **variable** chunks.

In this test we will use a repository with *Mbox* and SQLite files (a Thunderbird profile), that is, files of both types.

So the purpose of this test is to evaluate whether it is worth setting up 2 jobs, one with fixed chunks (for SQL files) and other with variables (for the rest), comparing them with a single job for all files, with variable chunk. 

That is:

![Scheme][0]

## Test parameters

* Repository: a Thunderbird profile folder, with ~23 Gb, of which the Mbox files represents 19 Gb (82%), and the largest SQLite file has approximately 450 Mb.
* Storages - three local folders:
	* one for the "**complete**" job with 1M variable chunks
	* the other two for "**remainder**" (1M var) and "**DB**" (1M fix) jobs. 
* The jobs were configured, via ```filters``` file, to exclude the databases ("**remainder**") and include *only* the databases ("**DB**").
	* "**remainder**": ```e:(?i).*.sqlite*$``` and ```i:.*``` (include everything else)
	* "**DB**": ```i:(?i).*.sqlite*$```
	
## Method

* Perform the full initial backup of the entire folder using the three jobs.
* Proceed with normal daily use for a few days, that is, sending and receiving emails from multiple profile accounts, and run a daily incremental backup.
* The repository and storage sizes were measured with Rclone [(www.rclone.org)](http://www.rclone.org).
* No ```prune``` command will be executed.

## Results

In the first days, comparing the **size** of the "complete" backup **storage** (gray) with the sum of the storage of the "remainder" (blue) and "DB" (orange) backups, you do not see much difference:

![Graph01][1]

However, when evaluating **uploads**, the performance of "separate" jobs is consistently better than the "single" job:

![Graph02][2]

**But** there is another important aspect: the databases have a total size of ~ 200 Mb and the remaining files ~ 7 **Gb**, so the DB uploads represent on average about **16%** of their size and remainder about **0.7%**. That is, although they have similar absolute sizes, the uploads of the remaining files are relatively much smaller.


On the other hand, the **running time** showed variable results. But I understand that this time can be affected by more variables (in the computer itself). Even though I checked the Windows logs, I didn't find anything different, so I couldn't identify the reasons.

![Graph03][3]

On the 5th day, many messages were deleted, and jobs performance [completed x (remainder + DB)] was similar.  
On the 6th day, back to normal use, and performance came back to look like the early days, with the performance of "separate" jobs being better than the "single" job.

**UPLOADS:**

![Graph04][4]


**RUNNING TIME:**

![Graph05][5]

*It's worth noting that when messages are deleted, they are deleted "inside" the mbox files, but they remain there, only the index changes.*

Another aspect to note is the relationship between size increasing of the repository and size increasing of the storage. We can see in the graph below that storage increases from **5 to 26** times the increase of the repository.


![Graph06][6]




## Conclusions

* It is clear that for normal daily use it is better to have separate jobs / settings for database files (with fixed chunls) and for other files (with variable chunks). When "exceptional" operations are performed, the performance of the two solutions is approximately equivalent.

* Neither of the configurations is fully suitable for backing up a mixed repository (text and SQL files), as the increase in storage is much bigger than the repository increase. Even with an aggressive prune policy, the increase in storage will be exponential. 

## 

  [0]: https://raw.githubusercontent.com/TowerBR/backup_software_testing/master/images/test06/scheme.png
  [1]: https://raw.githubusercontent.com/TowerBR/backup_software_testing/master/images/test06/graph01.png
  [2]: https://raw.githubusercontent.com/TowerBR/backup_software_testing/master/images/test06/graph02.png
  [3]: https://raw.githubusercontent.com/TowerBR/backup_software_testing/master/images/test06/graph03.png  
  [4]: https://raw.githubusercontent.com/TowerBR/backup_software_testing/master/images/test06/graph04.png  
  [5]: https://raw.githubusercontent.com/TowerBR/backup_software_testing/master/images/test06/graph05.png  
  [6]: https://raw.githubusercontent.com/TowerBR/backup_software_testing/master/images/test06/graph06.png    
  

[![Analytics](https://ga-beacon.appspot.com/UA-113708097-1/test_06?pixel)](https://github.com/igrigorik/ga-beacon)
