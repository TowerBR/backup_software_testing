# Introduction

I've been using Rclone [(www.rclone.org)](http://www.rclone.org) for some time to back up 80% of my files (documents, spreadsheets, images, etc).  
I use the ```rclone sync``` command with the ```--backup-dir``` parameter, which allows me to do incremental backups without problems. Even though it does not have a *full snapshot* functionality, it's a great tool, very stable, predictable, and reliable.

The remaining 20% are large files that have minor daily changes. Examples: Veracrypt volumes, Evernote databases, *mbox* files, and some other SQL databases.

The ```rclone sync``` command does not apply well to backing up this type of file, because as it is a *sync* command, it would create "full" copies of the files every day, exponentially increasing the use of remote backup space. If you are using a cloud backup there is still a problem with the associated upload time. To back up this files you need a software that has the ability to do deduplication and/or delta copying.

So, after looking a little, I ended up with two options: 
* Duplicati [(www.duplicati.com)](http://www.duplicati.com) [(github.com/duplicati/duplicati)](https://github.com/duplicati/duplicati)

and 

* Duplicacy (CLI version) [(www.duplicacy.com)](http://www.duplicacy.com) [(github.com/gilbertchen/duplicacy)](https://github.com/gilbertchen/duplicacy)

I started testing both with some test jobs, and this repository here contains the results of these tests.

The first test I registered here was # 4, only with Duplicacy. The previous ones are in the respective product forums and I will bring them here. And I still want to do other tests.

# Conclusions so far

* Duplicati has a serious point of failure represented by the use of local databases. The time expended (by the software and by the users) dealing with repairs, reconstructing, etc is excessive. But the worst thing is that these database crashes reduce one of the most essential features of backup software: reliability. So I suspended the use of the software. But I believe that in the future it might be interesting to evaluate it again.

* For backup of Evernote repositories with Duplicacy, the best configuration seems to be the use of 1M fixed chunks (for a ~4Gb database). Although there is still a problem with the backup after the Evernote "optimize" command is performed, this impact can be minimized by synchronizing the execution of this command and the Duplicacy prune commands.
