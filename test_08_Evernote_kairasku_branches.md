## Test #8

## Objective

The purpose of this test is evaluate the performance of branches developed by Kai Risku running backups of a Evernote repository:

* [hash_window](https://github.com/kairisku/duplicacy/tree/hash_window)
* [file_boundaries](https://github.com/kairisku/duplicacy/tree/file_boundaries)

## Test parameters

* Repository: an Evernote data folder, with ~3.47 Gb, of which the database (*exb* file) represents 3.38 Gb (97%).
* Storages - three local folders:
	* one for the "Duplicacy official" compilation (DO)
	* one for the "hash_window" compilation (HW)
	* one for the "file_boundaries" compilation (FB)
* All storages with variable 1M chunks.
	
## Method

* Perform the full initial backup of the entire folder using the three jobs.
* Proceed with normal daily use for a few days, that is, add/edit some notes, representing a few kb of changes per day, and run a daily incremental backup.
* At the end, run the internal "optimize" command from Evernote to evaluate the impact.
* The repository and storage sizes were measured with Rclone [(www.rclone.org)](http://www.rclone.org).
* No ```prune``` command will be executed.

## Results

In the days with "normal usage" the total size of the storage was consistently **smaller** for the **hash_window** compilation.

![Graph01][1]

With respect to the total number of chunks, the performance of the **hash_windows** compilation was similar to the **"official**" compilation, but the **file boundaries** branch presented a much different performance.

![Graph02][2]

But when we evaluate the new daily chunks this difference is smaller, that is, the difference seems to be due to the initial generation of chunks.

![Graph03][3]

Upload performance is aligned with new daily chunks:

![Graph04][4]

Then, on the last day, when performing the "optimization" of the database, the "disaster" (in terms of storages sizes) happens:

![Graph05][5]

Note that the upload is even bigger than the full initial backup:

![Graph06][6]

And the same applies to new daily chunks:

![Graph07][7]

**But**, the total chunks showed a completely different behavior:

![Graph08][8]

By my understanding, although all the chunks were replaced in the 3 jobs, the **file boundaries** compilation generated less chunks this time.

## Conclusions

* In this test, as in Test 7, the **hash window** compilation showed a slightly better performance.

* The Evernote database optimization command, which basically consists of a general reindexing, strongly impacted the results again. The backup is only feasible if the Duplicacy prune command is synchronized with the Evernote optimization command.

## 

  [1]: https://raw.githubusercontent.com/TowerBR/backup_software_testing/master/images/test08/graph01.png
  [2]: https://raw.githubusercontent.com/TowerBR/backup_software_testing/master/images/test08/graph02.png
  [3]: https://raw.githubusercontent.com/TowerBR/backup_software_testing/master/images/test08/graph03.png  
  [4]: https://raw.githubusercontent.com/TowerBR/backup_software_testing/master/images/test08/graph04.png  
  [5]: https://raw.githubusercontent.com/TowerBR/backup_software_testing/master/images/test08/graph05.png  
  [6]: https://raw.githubusercontent.com/TowerBR/backup_software_testing/master/images/test08/graph06.png    
  [7]: https://raw.githubusercontent.com/TowerBR/backup_software_testing/master/images/test08/graph07.png   
  [8]: https://raw.githubusercontent.com/TowerBR/backup_software_testing/master/images/test08/graph08.png     

  ## Full data

| Day | Repository   size     by Rclone | Repository   increase     (kb) |
|:---:|--------------------------------:|-------------------------------:|
|  01 |                       3.470.924 |                                |
|  02 |                       3.471.599 |                            675 |
|  03 |                       3.471.656 |                             57 |
|  04 |                       3.471.738 |                             82 |
|  05 |                       3.473.386 |                          1.648 |

| DO - storage size by Rclone | DO - storage increase | Revision | DO - all chunks | DO - new chunks | DO - uploaded | backup time - Complete (1M VAR) |
|----------------------------:|----------------------:|:--------:|----------------:|----------------:|--------------:|--------------------------------:|
|                   2.942.939 |                       |     1    |           2.640 |           2.618 |     2.798.000 |              05:17              |
|                   2.964.553 |                21.614 |     2    |           2.642 |              27 |        21.107 |              01:07              |
|                   2.994.070 |                29.517 |     3    |           2.651 |              40 |        28.824 |              01:46              |
|                   3.023.966 |                29.896 |     4    |           2.650 |              39 |        29.194 |              01:32              |
|                   5.967.668 |             2.943.702 |     5    |           2.688 |           2.681 |     2.807.000 |              04:16              |
  
| HW - storage size by Rclone | HW - storage increase | Revision | HW - all chunks | HW - new chunks | HW - uploaded | backup time - DB (1M FIX) |
|----------------------------:|----------------------:|:--------:|----------------:|----------------:|--------------:|--------------------------:|
|                   2.942.324 |                       |     1    |           2.643 |           2.622 |     2.795.000 |           04:41           |
|                   2.959.113 |                16.789 |     2    |           2.647 |              25 |        16.395 |           01:22           |
|                   2.985.043 |                25.930 |     3    |           2.648 |              35 |        25.321 |           01:32           |
|                   3.009.486 |                24.443 |     4    |           2.648 |              33 |        23.869 |           01:30           |
|                   5.952.276 |             2.942.790 |     5    |           2.664 |           2.655 |     2.806.000 |           06:20           |  

| FB - storage size by Rclone | FB - storage increase | Revision | FB - all chunks | FB - new chunks | FB - uploaded | backup time - DB (1M FIX) |
|----------------------------:|----------------------:|:--------:|----------------:|----------------:|--------------:|--------------------------:|
|                   2.942.453 |                       |     1    |           2.691 |           2.643 |     2.791.000 |           03:17           |
|                   2.962.382 |                19.929 |     2    |           2.694 |              29 |        19.461 |           01:07           |
|                   2.989.558 |                27.176 |     3    |           2.700 |              41 |        26.538 |           01:22           |
|                   3.016.707 |                27.149 |     4    |           2.697 |              33 |        26.511 |           01:17           |
|                   5.960.044 |             2.943.337 |     5    |           2.629 |           2.620 |     2.806.000 |           04:38           |  
  
[![Analytics](https://ga-beacon.appspot.com/UA-113708097-1/test_08?pixel)](https://github.com/igrigorik/ga-beacon)
