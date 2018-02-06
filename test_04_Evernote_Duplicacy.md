## Objective

This is a test to compare Duplicacy performance backing up a Evernote repository with variable and fixed chunks. The objective is to find out which of the two chunk settings (variable or fixed) best applies to back up a large file that has small daily modifications.

## Test parameters

* Repository: An Evernote data folder, with ~3.47 Gb, of which the database (*exb* file) represents 3.38 Gb (97%).
* Storage: Two local folders.
* Chunks: 1M variable and 1M fixed (two backup jobs).

## Method

* Perform the full initial backup of the entire folder.
* Proceed with normal daily use for a few days, that is, add/edit some notes, representing a few kb of changes per day, and run a daily incremental backup.
* At the end, run the internal "optimize" command from Evernote to evaluate the impact.
* The repository and storage sizes were measured with Rclone [(www.rclone.org)](http://www.rclone.org)

## Results

In the first days, the execution of the two jobs showed that fixed chunks seems to make better use of storage space than the variable chunks:

![Graph01][1]


Uploads were also smaller with fixed chunks

![Graph02][2]

*Note 1: The above chart does not show day 1, as there is no "increase" on this day.*

*Note 2: At day 2, even Evernote not being executed, there was some upload in the backup.*

And the upload time was also consistently smaller:

![Graph03][3]

## 

Then on the 5th day the command was run to "optimize" the Evernote database, which basically consists of a reindexing.

The result was that 2,664 chunks (out of a total of 2,667) were reuploaded:

![Graph04][4]

![Graph05][5]

![Graph06][6]

## Conclusions

* The use of fixed-size chunks provides better performance for incremental backups of small daily changes with this type of file (Evernote database, that is basically an SQLite file).
* Neither setups (fixed and variable chunks) fits well when the database is reindexed. But in this case it is not a limitation of Duplicacy because the entire database must have been modified. Using the optimization command in sync with the Duplicacy prune commands may improve the use of storage space.

## 

  [1]: images/teste04/evernote1.png
  [2]: images/teste04/evernote2.png
  [3]: images/teste04/evernote3.png  
  [4]: images/teste04/evernote4.png  
  [5]: images/teste04/evernote5.png  
  [6]: images/teste04/evernote6.png  

## Full data


| Day | Repository size     by Rclone     (kb) | Repository increase     (kb) | Storage size by Duplicati log     (kb) | 1M var - storage size by   Rclone     (kb) | Storage increase     (kb)  | 1M var - uploaded     (All chunks      log line) (kb) | 1M var - backup time | Storage size by Duplicati log     (kb) | 1M fix - storage size by   Rclone     (kb) | Storage increase     (kb)  | 1M fix - uploaded     (All chunks      log line) (kb) | 1M fix - backup time |
|-----|----------------------------------------|------------------------------|----------------------------------------|--------------------------------------------|----------------------------|-------------------------------------------------------|----------------------|----------------------------------------|--------------------------------------------|----------------------------|-------------------------------------------------------|----------------------|
| 01  | 3.474.331                              |                              | 3.313.000                              | 2.945.953                                  |                            |                                                       | 00:03:38             | 3.313.000                              | 2.946.272                                  |                            |                                                       | 00:03:23             |
| 02  | 3.474.331                              | 0                            | 3.315.000                              | 2.945.953                                  | 0                          | 102.846                                               | 00:02:16             | 3.313.000                              | 2.946.272                                  | 0                          | 42.151                                                | 00:00:11             |
| 03  | 3.476.075                              | 1.744                        | 3.317.000                              | 3.328.283                                  | 382.330                    | 270.521                                               | 00:02:12             | 3.315.000                              | 3.109.595                                  | 163.323                    | 117.343                                               | 00:01:37             |
| 04  | 3.476.433                              | 358                          | 3.318.000                              | 3.479.856                                  | 151.573                    | 148.020                                               | 00:02:23             | 3.315.000                              | 3.169.552                                  | 59.957                     | 58.550                                                | 00:01:40             |
| 05  | 3.474.758                              | -1.675                       | 3.317.000                              | 6.417.636                                  | 2.937.780                  | 2.801.000                                             | 00:04:30             | 3.314.000                              | 6.107.636                                  | 2.938.084                  | 2.801.000                                             | 00:03:20             |
