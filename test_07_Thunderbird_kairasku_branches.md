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

We can also see that the daily increase in storage size is slightly **smaller** for **hash_window** compilation, *except* for the last day when compaction was performed, and the performance of the original Duplicacy compilation was better.

![Graph02][2]

The **hash window** compilation was also better in the total number of chunks and the number of new chunks each day:

![Graph03][3]

![Graph04][4]

The uploads were aligned with the storages increases:

![Graph05][5]

**Then**, in the middle of the test, as the result already seemed consistent, and remembering Test 6, I decided to **add 3 more jobs** (DO, HW and FB), but **without the databases**, and see the results:

![Graph06][6]

![Graph07][7]

*Note: on day 4 the "official" compilation job didn't run because there was a typo in the script and I only found it the next day when I checked the logs.*


And finally, an important point to evaluate, especially in the case of cloud storages, is how much storage grows as the repository grows. And in this regard, the big "villain" is again the database backup:
 
(Remembering that the backup was running with variable chunks, which is not ideal for databases)

![Graph08][8]

## Conclusions

* The **hash window** build seems to actually have slightly better performance for this use case.

* The storage of the backup with all files (databases and txt files) grows from 3 to 21 (!) times the repository increase, but when we exclude the databases from backup, the increase in storage is only 1.2 to 2.5 times. We conclude then that the use of variable chunks is not really the best option when databases are involved, making backup unfeasible. On the other hand this configuration applies well to the other types of files.

## 

  [1]: https://raw.githubusercontent.com/TowerBR/backup_software_testing/master/images/test07/graph01.png
  [2]: https://raw.githubusercontent.com/TowerBR/backup_software_testing/master/images/test07/graph02.png
  [3]: https://raw.githubusercontent.com/TowerBR/backup_software_testing/master/images/test07/graph03.png  
  [4]: https://raw.githubusercontent.com/TowerBR/backup_software_testing/master/images/test07/graph04.png  
  [5]: https://raw.githubusercontent.com/TowerBR/backup_software_testing/master/images/test07/graph05.png  
  [6]: https://raw.githubusercontent.com/TowerBR/backup_software_testing/master/images/test07/graph06.png    
  [7]: https://raw.githubusercontent.com/TowerBR/backup_software_testing/master/images/test07/graph07.png   
  [8]: https://raw.githubusercontent.com/TowerBR/backup_software_testing/master/images/test07/graph08.png     

  ## Full data

Several other analyzes can be made, then I provide the complete data below:

| Day | Repository   size     by Rclone | Repository   increase |
|-----|---------------------------------|-----------------------|
| 01  | 17.655.687                      |                       |
| 02  | 17.661.631                      | 5.944                 |
| 03  | 17.676.296                      | 14.665                |
| 04  | 17.691.496                      | 15.200                |
| 05  | 17.696.373                      | 4.877                 |
| 06  | 17.660.070                      | -36.303               |

| DO - storage size by Rclone | DO - storage increase | Revision | DO - all chunks | DO - new chunks | DO - uploaded | backup time |
|-----------------------------|-----------------------|----------|-----------------|-----------------|---------------|-------------|
| 7.095.713                   |                       | 1        | 7.338           | 6.810           | 6.766.000     | 10:52       |
| 7.183.204                   | 87.491                | 2        | 7.355           | 150             | 85.439        | 00:39       |
| 7.291.001                   | 107.797               | 3        | 7.373           | 185             | 105.269       | 01:05       |
| 7.413.852                   | 122.851               | 4        | 7.372           | 213             | 119.970       | 00:42       |
| 7.519.854                   | 106.002               | 5        | 7.392           | 192             | 103.517       | 00:39       |
| 7.625.669                   | 105.815               | 6        | 7.379           | 136             | 103.334       | 00:53       |

| HW - storage size by Rclone | HW - storage increase | Revision | HW - all chunks | HW - new chunks | HW - uploaded | backup time |
|-----------------------------|-----------------------|----------|-----------------|-----------------|---------------|-------------|
| 7.022.541                   |                       | 1        | 6.913           |                 |               | 08:02       |
| 7.103.604                   | 81.063                | 2        | 6.930           | 120             | 79.162        | 00:13       |
| 7.204.768                   | 101.164               | 3        | 6.946           | 145             | 98.792        | 00:34       |
| 7.324.531                   | 119.763               | 4        | 6.962           | 179             | 116.955       | 00:26       |
| 7.427.801                   | 103.270               | 5        | 6.980           | 157             | 100.848       | 00:16       |
| 7.539.491                   | 111.690               | 6        | 6.957           | 119             | 109.072       | 00:12       |

| FB - storage size by Rclone | FB - storage increase | Revision | FB - all chunks | FB - new chunks | FB - uploaded | backup time |
|-----------------------------|-----------------------|----------|-----------------|-----------------|---------------|-------------|
| 7.160.252                   |                       | 1        | 7.025           |                 |               | 33:49       |
| 7.251.295                   | 91.043                | 2        | 7.043           | 159             | 88.908        | 00:08       |
| 7.368.427                   | 117.132               | 3        | 7.062           | 202             | 114.385       | 00:33       |
| 7.496.918                   | 128.491               | 4        | 7.069           | 238             | 125.478       | 00:45       |
| 7.605.065                   | 108.147               | 5        | 7.065           | 193             | 105.611       | 00:25       |
| 7.723.064                   | 117.999               | 6        | 7.056           | 143             | 115.232       | 00:23       |


**WITHOUT DB'S:**

| DOnoDB - storage size by   Rclone     (kb) | DOnoDB - storage increase | Revision | DOnoDB - all chunks | DOnoDB - new chunks | DOnoDB - uploaded | backup time |
|--------------------------------------------|---------------------------|----------|---------------------|---------------------|-------------------|-------------|
|                                            |                           |          |                     |                     |                   |             |
|                                            |                           |          |                     |                     |                   |             |
|                                            |                           |          |                     |                     |                   |             |
| 6.967.402                                  |                           | 1        | 7.249               |                     |                   | 09:56       |
| 6.984.916                                  | 17.514                    | 2        | 7.270               | 33                  | 17.102            | 00:07       |
| 7.045.525                                  | 60.609                    | 3        | 7.256               | 61                  | 59.187            | 00:06       |

| HWnoDB - storage size by   Rclone     (kb) | HWnoDB - storage increase | Revision | HWnoDB - all chunks | HWnoDB - new chunks | HWnoDB - uploaded | backup time |
|--------------------------------------------|---------------------------|----------|---------------------|---------------------|-------------------|-------------|
|                                            |                           |          |                     |                     |                   |             |
|                                            |                           |          |                     |                     |                   |             |
| 6.892.434                                  |                           | 1        | 7.094               |                     |                   | 25:55       |
| 6.911.328                                  | 18.894                    | 2        | 7.114               | 31                  | 18.451            | 00:13       |
| 6.923.950                                  | 12.622                    | 3        | 7.124               | 27                  | 12.325            | 00:06       |
| 6.988.390                                  | 64.440                    | 4        | 7.099               | 63                  | 62.928            | 00:05       |

| FBnoDB - storage size by   Rclone     (kb) | FBnoDB - storage increase | Revision | FBnoDB - all chunks | FBnoDB - new chunks | FBnoDB - uploaded | backup time |
|--------------------------------------------|---------------------------|----------|---------------------|---------------------|-------------------|-------------|
|                                            |                           |          |                     |                     |                   |             |
|                                            |                           |          |                     |                     |                   |             |
| 7.064.503                                  |                           | 1        | 6.789               |                     |                   |             |
| 7.097.462                                  | 32.959                    | 2        | 6.814               | 51                  | 32.185            | 00:07       |
| 7.119.468                                  | 22.006                    | 3        | 6.827               | 46                  | 21.489            | 00:06       |
| 7.185.353                                  | 65.885                    | 4        | 6.813               | 66                  | 64.339            | 00:05       |





  
  
  
[![Analytics](https://ga-beacon.appspot.com/UA-113708097-1/test_07?pixel)](https://github.com/igrigorik/ga-beacon)
