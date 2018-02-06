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

In the first days, running the two backup jobs showed that the job with fixed chunks seems to make better use of storage space than the variable chunks job:

![Graph01][1]


Uploads were also smaller with fixe chunks

![Graph02][2]

And the upload time was also consistently smaller:

![Graph03][3]


Then on the 5th day the command was run to "optimize" the Evernote database, which basically consists of a reindexing.

The result was that 2,664 chunks (out of a total of 2,667) were reuploaded:

![Graph04][4]

![Graph05][5]

![Graph06][6]




  [1]: images/teste04/evernote1.png
  [2]: images/teste04/evernote2.png
  [3]: images/teste04/evernote3.png  

