# Introduction

I've been using Rclone [(www.rclone.org)](http://www.rclone.org) for some time to back up 80% of my files (documents, worksheets, images, etc).  
I use the ```rclone sync``` command with the ```--backup-dir``` parameter, which allows me to do incremental backups without problems. Even though it does not have a *full snapshot* functionality, it's a great tool, very stable, predictable, and reliable.

The remaining 20% are large files that have minor daily changes. Examples: Veracrypt volumes, Evernote databases, *mbox* files, and some other SQL databases.

The ```rclone sync``` command does not apply well to backing up this type of file, because as it is a *sync* command, it would create "full" copies of the files every day, exponentially increasing the use of remote backup space. If you are using a cloud backup there is an additional problem with the associated upload size/time. To back up this files you need a software that has the ability to do deduplication and/or delta copying.

So, after looking a little, I ended up with two options: 
* Duplicati [(www.duplicati.com)](http://www.duplicati.com) [(github.com/duplicati/duplicati)](https://github.com/duplicati/duplicati)

and 

* Duplicacy (CLI version) [(www.duplicacy.com)](http://www.duplicacy.com) [(github.com/gilbertchen/duplicacy)](https://github.com/gilbertchen/duplicacy)

I started testing both with some test jobs, and this repository here contains the results of these tests.

The first test I registered here was # 4, only with Duplicacy. The previous ones are in the respective product forums and I will bring them here. And I still want to do other tests.

# Tests

1. Test #1:
Evernote repository  
Duplicati default configuration vs.  
Duplicacy default configuration  
([Duplicati forum)](http://bit.ly/2sdVLrX)

2. Test #2:
Evernote repository  
Duplicacy with 1M kb fixed chunks and 128 kb fixed chunks   
([Duplicacy forum, starting at Jan 22 6:53AM 2018)](http://bit.ly/2E8B9H9)  

3. Test #3:
Evernote repository  
Duplicati default configuration vs.  
Duplicacy with 128 kb fixed chunks  
([Duplicati forum)](http://bit.ly/2nOCwQh)

4. Test #4:
Evernote repository  
Duplicacy with 1 Mb fixed chunks vs. 1 Mb variable chunks  
([test_04_Evernote_Duplicacy.md](http://bit.ly/2E5Wf9a) above)

5. Test #5:
Thunderbird profile (Mbox and SQLite files)  
Duplicacy with 1 Mb variable chunks   
([test_05_Thunderbird_Duplicacy.md](http://bit.ly/2EbdciE) above)  

6. Test #6:
Thunderbird profile (Mbox and SQLite files)  
Duplicacy with 3 jobs:
    - whole profile with 1 Mb variable chunks  vs.  
    - database (1Mb fixed chunks) + the rest (1Mb variable chunks)  
	
    ([test_06_Thunderbird_Duplicacy_single_or_separate_storage_setup.md](http://bit.ly/2BpdU95) above)  
  
7. Test #7:
Thunderbird profile (Mbox and SQLite files) with Kai Rasku branches 
Duplicacy with 3 jobs:
	- one for the "Duplicacy official" compilation (DO)
	- one for the "hash_window" compilation (HW)
	- one for the "file_boundaries" compilation (FB)
	
    ([test_07_Thunderbird_kairasku_branches.md](http://bit.ly/2GiIYH1) above)  
  
8. Test #8:
Evernote repository (~SQLite file) with Kai Rasku branches 
Duplicacy with 3 jobs:
	- one for the "Duplicacy official" compilation (DO)
	- one for the "hash_window" compilation (HW)
	- one for the "file_boundaries" compilation (FB)
	
    ([test_08_Evernote_kairasku_branches.md](http://bit.ly/2EyBxLv) above)  
  
9. Test #9:
Test of wide range chunk setup
Duplicacy with 2 jobs:
	- one with default chunk setting: ```-c 1M -min 250kb -max 4M```
	- another with "wide" chunk setting: ```-c 1M -min 32kb -max 10M```
	
    ([test_09_txt_files_diferent_chunk_sizes.md](http://bit.ly/2ocMsE7) above)  
  
 
    
  
**To be executed:**
 
  
10. Test #10: 
Multimedia files  
Duplicacy with 1 Mb variable chunks 

11. Test #11:
Veracrypt volume  
Duplicacy with ????

12. Test #12:  
VirtualBox VM's  
Duplicacy with ????



# Conclusions so far

* Duplicati has a serious point of failure represented by the use of local databases. The time expended (by the software and by the users) dealing with repairing, rebuilding, etc is excessive. But the worst thing is that these database crashes reduce one of the most essential features of backup software: reliability. So I suspended the use of the software. But I believe that in the future it might be interesting to evaluate it again.

* For backup of Evernote repositories with Duplicacy, the best configuration seems to be the use of 1 Mb fixed chunks (for a ~4Gb database) (see Test #4 above). Although there is still a problem with the backup after the Evernote "optimize" command is performed, this impact can be minimized by synchronizing the execution of this command and the Duplicacy prune commands.

[![Analytics](https://ga-beacon.appspot.com/UA-113708097-1/readme?pixel)](https://github.com/igrigorik/ga-beacon)
