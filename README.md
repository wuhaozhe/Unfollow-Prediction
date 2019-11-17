# Unfollow-Prediction Dataset
Repository for paper "Mining Unfollow Behavior in Large-Scale Online Social Networks via Spatial-Temporal Interaction".
We construct a real-world benchmark dataset on Sina Weibo with 1.8 million Chinese users and 400 million social relationships. It records user’s post content and relationship dynamics from September 28, 2012 to October 29, 2012.
This dataset contains two parts, the link dynamics file and post content file. The link dynamics file record users' follow and unfollow dynamics from September 28, 2012 to October 29, 2012. The post content file record each user's post content before October 29, 2012. Details are illustrated as follows.

------
### Link Dynamics File

------
### Post Content File
Since that post content of 1.78 million users is much too large, we construct a subset and only publish post content of those users. Further training and testing protocol is conducted on this subset.
Download link:
```
Baidu Disk: https://pan.baidu.com/s/1Y6UQisRz4uBGUyBNISvf5Q
Dropbox: https://www.dropbox.com/sh/3y323wl3mvom959/AADO8RfhtisVR-Kmk_sFFF-Sa?dl=0
```
We have six seperate files: *StrHole_hold.txt*, *StrHole_unfollow.txt*, *OpnLdr_hold.txt*, *OpnLdr_unfollow.txt*, *OrdUsr_hold.txt*, *OrdUsr_unfollow.txt*. Each file contains thousands of follow/unfollow records, each record has the following formats:
```
---
[followee id]
[follower id]
[time of follow/unfollow](if relationship keeps follow, the time is 2012/10/29)
src(src means followee) 0 [time of post]
[post content]
src 1 [time of post]
[post content]
......
src n(post number of followee) [time of post]
[post content]
dst(dst means follower) 0 [time of post]
[post content]
dst 1 [time of post]
[post content]
......
dst n(post number of follower) [time of post]
[post content]
```
StrHole/OpnLdr/OpnLdr means the followee has corresponding social role, hold/unfollow means the link has corresponding status.

------
### Training and Testing Protocal
For training and testing, we extract network embedding and collaborative features from link dynamics file, extract text features from post content file. We concat all records in six post content files and conduct 5-fold cross validations.

### Citing
------
If you used our data, please kindly cite our paper:

If you have questions about any part of the paper, please e-mail wuhz19@mails.tsinghua.edu.cn.
