---
title: TX1 查看cpu gpu占用
url: 573.html
id: 573
categories:
  - 技术
date: 2017-02-22 20:41:07
tags:
---

running ~/tegrastats and look for GR3D
```
sudo ~/tegrastats
[sudo] password for ubuntu:
RAM 277/3854MB (lfb 700x4MB) SWAP 0/0MB (cached 0MB) cpu [0%,0%,0%,0%]@1912 EMC 0%@1600 AVP 5%@115 VDE 0 GR3D 0%@76 EDP limit 1912
RAM 277/3854MB (lfb 700x4MB) SWAP 0/0MB (cached 0MB) cpu [1%,0%,0%,1%]@102 EMC 1%@408 AVP 33%@12 VDE 0 GR3D 0%@76 EDP limit 1912
```

EMC – memory controller  
AVP – audio/video processor  
VDE – video decoder engine  
GR3D – GPU  

```
sudo ~/jetson_clocks.sh --show
```