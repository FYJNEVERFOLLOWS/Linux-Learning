# Kaldi 从0搭建语音识别引擎流程

# Kaldi 环境搭建

需要在Linux上完成Kaldi的安装

下载链接

**cd to /kaldi and follow INSTALL instructions**

验证安装成功：

运行yesno例子，将kaldi/egs/yesno/s5/path.sh中的export KALDI_ROOT=`pwd`/../../..改为export KALDI_ROOT=/home/xx/kaldi（=后面为自己kaldi的路径，可以cd kaldi然后pwd查看并复制过来）。

终端输入```cd /kaldi/egs/yesno/s5``` (进入yesno例子的s5目录) 运行run.sh文件 (要保证yesno的脚本文件都没有被打开不然会报错，就像word文件被别人打开了就不能更改)

运行命令：```sudo ./run.sh```

运行结果：%WER 0.00 [ 0 / 232, 0 sudo apt-get update，sudo apt-get upgradeins, 0 del, 0 sub ] exp/mono0a/decode_test_yesno/wer_10_0.0 即安装成功。

![](https://raw.githubusercontent.com/FYJNEVERFOLLOWS/Picture-Bed/main/202303/20230308145603.png)

Kaldi数据准备：4个文件

```
1. wav.scp

   [utt-id] [wav-path]

   例子：sen_1 /home/train/data/sen_1.wav2

2. text

   [utt-id] [text-content]

   例子：sen_1 今天天气很好

3. utt2spk

   [utt-id] [spk-id]

   例子：sen_1 speaker_1

4. spk2utt

   [spk-id] [utt-id]

   例子：speaker_1 sen_1
```



# 以 AISHELL-1 为例进行数据准备

下载 AISHELL-1_sample.zip，解压，将所有 .wav / .txt 放到 /data/AISHELL-sample/wav (/data/AISHELL-sample/text) 文件夹

统计一共有多少条音频：
```bash
find ./wav/ -iname "*.wav" | wc -l
```
## **wav.scp**

```
find /data/AISHELL-sample/wav -iname '*.wav' > wav.scp.temp
```
使用上述命令得到所有 .wav 文件的绝对路径，并写入 wav.scp.temp 文件

TODO https://blog.csdn.net/weixin_41126303/article/details/120837111

使用 awk 指定 '/' 为分隔符，过滤得到最后一个 field ($NF)，使用 sed 将 .wav 替换为空生成 wav_id 文件

TODO https://blog.csdn.net/weixin_41126303/article/details/120837111

将 wav_id 和 wav.scp.temp (path 文件) 按列拼接

## text

TODO

## utt2spk & spk2utt

spk_id 的获取：

TODO

至此，4 个文件全部准备完毕。需要对 text 中的说话内容进行分词

https://www.bilibili.com/video/BV19a4y1h7cB?p=2



先写一个去除标点符号的 python 脚本 `rm_puncs.py` (remove punctuations)

```python
import re
import sys

from zhon.hanzi import punctuation

for line in sys.stdin:
        line = line.strip('\n')
        print(re.sub(ur'[%s]+' %punctuation, '', line.decode('utf-8')))
```

再写一个中文分词的 python 脚本 `segment.py`

```python
#coding:utf-8

import sys
import re
import jieba

for line in sys.stdin:
        sentence=line.strip().split('\t')[1]
        # remove chars which are not Chinese, English chars or numbers
        # filtered_sen = re.sub(u"[^\u4e00-\u9fa5\u0030-\u0039\u0041-\u005a\u0061-\u007a]","",sentence)
        filtered_sen = re.sub(u"[^\u4e00-\u9fa5A-Za-z0-9]","",sentence)
        res = " ".join(jieba.cut(filtered_sen))
        print(res)
```

sort -u 排序去重生成词典

