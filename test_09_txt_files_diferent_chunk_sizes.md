## Test #9

## Objective

The purpose of this test is to evaluate how chunks are generated with different range settings. More specifically, how a wide range (128 kb - 10 Mb) affects chunks generation.

## Test parameters

* Repository: a folder with 4,055 txt files.
* Storages - two local folders:
	* one with default chunk setting: ```-c 1M -min 250kb -max 4M```
	* another with "wide" chunk setting: ```-c 1M -min 32kb -max 10M```
	
## Method

* Perform both backups and evaluate the generated chunks by grouping them by size.

## Results

The chart below shows the distribution of file sizes. 

Note that the scale of the X axis is not linear:
- 0 to 1,000 kb more detailed (few kb increments)
- then 1,000 to 4,000 (the maximum size of the default setting)
- then 4,000 to 10,0000 (the maximum size of the "wide" setting)

We can see that most files are really small (less than 32 kb).

![chart01][1]

This is the graph of the chunks sizes for the default storage configuration. 

![chart02][2]

And this is the graph for the "wide" setting:

![chart03][3]

We can see that the Duplicacy tries to group the files as much as possible (large chunks):

![chart04][4]

And I thought it would try to group the files as close as possible to the maximum chunk size, but it seems that it also takes into account the sizes of the source files, because in the configuration with 10M maximum size, not many chunks were generated above 4M:

![chart05][5]


## Conclusions

* Waiting for contributions  :-)

## 

  [1]: https://raw.githubusercontent.com/TowerBR/backup_software_testing/master/images/test09/chart01.png
  [2]: https://raw.githubusercontent.com/TowerBR/backup_software_testing/master/images/test09/chart02.png
  [3]: https://raw.githubusercontent.com/TowerBR/backup_software_testing/master/images/test09/chart03.png
  [4]: https://raw.githubusercontent.com/TowerBR/backup_software_testing/master/images/test09/chart04.png
  [5]: https://raw.githubusercontent.com/TowerBR/backup_software_testing/master/images/test09/chart05.png
  
  [![Analytics](https://ga-beacon.appspot.com/UA-113708097-1/test_09?pixel)](https://github.com/igrigorik/ga-beacon)
