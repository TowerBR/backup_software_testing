## Test #7

## Objective

Evaluate the performance of branches developed by Kai Risku:

* [hash_window](https://github.com/kairisku/duplicacy/tree/hash_window)
* [file_boundaries](https://github.com/kairisku/duplicacy/tree/file_boundaries)

## Test parameters

* Repository: a Thunderbird profile folder, with ~23 Gb, of which the Mbox files represents 19 Gb (82%), and the largest SQLite file has approximately 450 Mb.
* Storages - three local folders:
	* one for the "Duplicacy official" compilation (DO)
	* one for the "hash_window" compilation (HW)
	* one for the "file_boundaries" compilation (FB)
* All storages with variable 1M chunks.
	
## Method

* Perform the full initial backup of the entire folder using the three jobs.
* Proceed with normal daily use for a few days, that is, sending and receiving emails from multiple profile accounts, and run a daily incremental backup.
* On the last day, perform a "compacting" (Thunderbird command) of the files.
* The repository and storage sizes were measured with Rclone [(www.rclone.org)](http://www.rclone.org).
* No ```prune``` command will be executed.

## Results

By the chart below we can see that the total size of the storage was consistently **smaller** for the **hash_window** compilation.

![Graph01][1]

We can also see that the daily increase in storage size is slightly **smaller** for **hash_window** compilation, *except* for the last day when compaction was performed, , and the performance of the original Duplicacy compilation was better.

![Graph02][2]

The hash window compilation was also better in the total number of chunks and the number of new chunks each day:

![Graph03][3]

![Graph04][4]

The uploads were aligned with the storages increases:

![Graph05][5]

**Then**, in the middle of the test, as the result already seemed consistent, and remembering Test 6, I decided to **add 3 more jobs** (DO, HW and FB), but **without the databases**, and see the results:

(See the difference on the last day)

![Graph06][6]

![Graph07][7]

*Note: on day 4 the "official" compilation job didn't run because there was a typo in the script and I only found it the next day when I checked the logs.*


And finally, an important point to evaluate, especially in the case of cloud storages, is how much storage grows as the repository grows. And in this regard, the big "villain" is again the database backup:
 
(Remembering that the backup was running with variable chunks, which is not ideal for databases)


## Conclusions

* The **hash window** build seems to actually have slightly better performance for this use case.

## 

  [1]: https://raw.githubusercontent.com/TowerBR/backup_software_testing/master/images/test07/graph01.png
  [2]: https://raw.githubusercontent.com/TowerBR/backup_software_testing/master/images/test07/graph02.png
  [3]: https://raw.githubusercontent.com/TowerBR/backup_software_testing/master/images/test07/graph03.png  
  [4]: https://raw.githubusercontent.com/TowerBR/backup_software_testing/master/images/test07/graph04.png  
  [5]: https://raw.githubusercontent.com/TowerBR/backup_software_testing/master/images/test07/graph05.png  
  [6]: https://raw.githubusercontent.com/TowerBR/backup_software_testing/master/images/test07/graph06.png    
  

  ## Full ata

Several other analyzes can be made, then I provide the complete data below:

| Day | Repository   size     by Rclonee | Repository   increase |
|-----|----------------------------------|-----------------------|
| 01  | 17.655.687                       |                       |
| 02  | 17.661.631                       | 5.944                 |
| 03  | 17.676.296                       | 14.665                |
| 04  | 17.691.496                       | 15.200                |
| 05  | 17.696.373                       | 4.877                 |
| 06  | 17.660.070                       | -36.303               |






  
  
  
[![Analytics](https://ga-beacon.appspot.com/UA-113708097-1/test_07?pixel)](https://github.com/igrigorik/ga-beacon)
