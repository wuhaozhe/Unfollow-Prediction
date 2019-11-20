# Unfollow-Prediction Dataset
Repository for paper "Mining Unfollow Behavior in Large-Scale Online Social Networks via Spatial-Temporal Interaction".
We construct a real-world benchmark dataset on Sina Weibo with 1.78 million Chinese users and 400 million social relationships. It records user’s post content and relationship dynamics from September 28, 2012 to October 29, 2012.
This dataset contains two parts, the link dynamics file and post content file. The link dynamics file record users' follow and unfollow dynamics from September 28, 2012 to October 29, 2012. The post content file record each user's post content before October 29, 2012. Details are illustrated as follows.

------
### 1. Link Dynamics File

Download link:

```
Baidu Disk: https://pan.baidu.com/s/1ZDpgYqVN1uJyw0QhV0Xt3Q
DropBox: https://www.dropbox.com/s/3sp7jt2740ut8qj/link_dynamic.bin?dl=0 
```

#### 1.1 Online Social Network

The users and their relationships are modeled as a dynamic directed graph $G(V, E, T)$. Each node $n\in V$ represents an online user, the node id range from 0 to |V| - 1 is different from user’s original Weibo id. At a specific time point $T$, users’ relationships can be represented as directed edge set $E$,  $e=(n_i, n_j)\in E$ means user $i$ keeps following relationship with user $j$ at time $T$. The edge status is updated every day.

#### 1.2 Dataset Format

The link dynamic file is organized as a binary file.

##### 1.2.1 Header

```
[node_num(INT)][edge_num(INT)] -------- 8 bytes
[NODE SECTION]                 \
[NODE SECTION]                  |
[NODE SECTION]                   > [node_num] NODE SECTIONS
......                          |
[NODE SECTION]                 / 
```

The first 8 bytes are two integers representing the total node num and recorded edge num in this network. We will record an edge if it appears at least once in the dynamic network during September 28, 2012 to October 29, 2012.

The following bytes are organized as node\_num NODE SECTIONS, see its descriptions in the following part.

##### 1.2.2 NODE SECTION

```
[node_id(INT)][follow_num(INT)] -------- 8 bytes
[EDGE SECTION]                 \
[EDGE SECTION]                  |
[EDGE SECTION]                   > [follow_num] EDGE SECTIONS
......                          |
[EDGE SECTION]                 / 
```

The first 8 bytes are the node id in [0, node\_num) and the follow num of this user. Each EDGE SECTION denotes a edge that appears at least once with current node as the tail.

##### 1.2.3 EDGE SECTION

```
[other_node_id(INT)] -------- 4 bytes
[k]                  -------- 1 byte
[t1][t2]...[tk]      -------- [k] bytes  
```

Each EDGE SECTION begins  with a 4 bytes integer for the head node id of this edge. Then a 1 byte unsigned integer k represents the edge status change num, note that k>=1. The following k bytes are unsigned integers represent k time stamps. 

The unsigned integer ti represent how many days have passed from September 28, 2012 to this time stamp. For example, for time stamp (Sept, 28), t = 0, for time stamp (Oct, 1), t = 3. Note that ti>=0

These time stamps indicate the appear and disappear time of this edge alternatively. Specifically, t1 denotes the time when this edge first appear in the network, t2 represents the time when this edge first disappear in this network, t3 denotes the time when this edge appear again right after its last disappearance, etc. Note that t1 < t2 < t3 < ... < tk.

For  example, if an edge first existed on September 29, and vanished on October 2, then appeared on October 5. We will have k=3, t1=1, t2=4, t3=7.

------
### 2. Post Content File
Since that post content of 1.78 million users is much too large, we construct a subset and only publish post content of those users. Further training and testing protocol is conducted on this subset.
Download link:

```
Baidu Disk: https://pan.baidu.com/s/1Y6UQisRz4uBGUyBNISvf5Q
Dropbox: https://www.dropbox.com/sh/3y323wl3mvom959/AADO8RfhtisVR-Kmk_sFFF-Sa?dl=0
```
We have six separate files: *StrHole_hold.txt*, *StrHole_unfollow.txt*, *OpnLdr_hold.txt*, *OpnLdr_unfollow.txt*, *OrdUsr_hold.txt*, *OrdUsr_unfollow.txt*. Each file contains thousands of follow/unfollow records, each record has the following formats:
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
### 3. Training and Testing Protocal
For training and testing, we extract network embedding and collaborative features from link dynamics file, extract text features from post content file. We concat all records in six post content files and conduct 5-fold cross validations.

### 4. Citing
------
If you used our data, please kindly cite our paper:

If you have questions about any part of the paper, please e-mail wuhz19@mails.tsinghua.edu.cn.
