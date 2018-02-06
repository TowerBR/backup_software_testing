## Objective

This is a test to compare Duplicacy performance backing up a Evernote repository with variable and fixed chunks. The objective is to find out which of the two chunk settings (variable or fixed) best applies to back up a large file that has small daily modifications.

## Test parameters

* Repository: An Evernote data folder, with ~3.47 Gb, of which the database (*exb* file) represents 3.38 Gb (97%)
* Storage: Two local folders
* Chunks: 1M variable and 1M fixed (two backup jobs)

## Method

Perform the full initial backup of the entire folder
Proceed with normal daily use for a few days, that is, add/edit some notes, representing a few kb of changes per day, and run a daily incremental backup.
At the end, run the internal "optimize" command from Evernote to evaluate the impact.

## Results

