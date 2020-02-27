---
title: you_get 自定义
date: 2019-12-30 21:31:41
categories:
- other
toc: true
tags:
- python
- you-get
---
## you get简介
you-get 用来下载哔哩哔哩多p视频
**缺点:**
* 不能指定开始p
* 下载的视频没有放在子目录里面
* 不支持失败重试
* 有时候，一集下完了还在疯狂下载，下十几个G都不停的

针对前2个缺点，做出自定义

项目地址：
[soimort/you-get: Dumb downloader that scrapes the web](https://github.com/soimort/you-get)

<!--more-->
## 操作过程记录
### git clone
```shell
git clone https://github.com/soimort/you-get.git
cd you-get
```
### 修改代码
#### common.py
添加start参数
```python
# script_main 方法中， parser = 下一句
parser.add_argument(
        '--start', help='start from index [start]'
    )
# 修改 download_main
if args.start:
    start = args.start
else:
    start = 1
ic(start)
download_main(
    download, download_playlist,
    URLs, args.playlist,
    output_dir=args.output_dir, merge=not args.no_merge,
    info_only=info_only, json_output=json_output, caption=caption,
    password=args.password,
    start=start,
    **extra
)
```
#### extractors/bilibili.py
download_playlist_by_url 中
```python
# 添加
ic(kwargs['start'])
start = int(kwargs['start'])
```

```python
if sort == 'video':
    initial_state_text = match1(html_content, r'__INITIAL_STATE__=(.*?);\(function\(\)')  # FIXME
    initial_state = json.loads(initial_state_text)
    # 添加下面几行
    title = initial_state['videoData']['title']
    kwargs['output_dir'] = os.path.join(kwargs['output_dir'], title)
    if os.path.exists(kwargs['output_dir']):
        os.system(f"mkdir -p '{kwargs['output_dir']}'")
```
elif sort == 'bangumi': 上面

```python
for pi in range(pn):
    if pi + 1 < start:
        continue
    self.prepare_by_cid(aid, initial_state['videoData']['pages'][pi]['cid'], 'P%s. %s' % (
        pi + 1, initial_state['videoData']['pages'][pi]['part']),
                        html_content, playinfo, playinfo_, url)
    self.extract(**kwargs)
    self.download(**kwargs)
```

## 再次修改
### 修改内容：

### 1. common.py
```python
# if args.skip_existing_file_size_check:
#     skip_existing_file_size_check = True
skip_existing_file_size_check = True
if args.output_dir == '.':
    args.output_dir = '/Volumes/My Passport/data/ut下载/0  未分类/ytdl videos/bilibili'
ic(args.output_dir)
```

```python
download_main(
    download, download_playlist,
    URLs, True,
    # URLs, args.playlist,
    output_dir=args.output_dir, merge=not args.no_merge,
    info_only=info_only, json_output=json_output, caption=caption,
    password=args.password,
    start=start,
    **extra
)
```
### 2. extractors/bilibili.py
```python
title = initial_state['videoData']['title']
kwargs['output_dir'] = os.path.join(kwargs['output_dir'], title)
if not os.path.exists(kwargs['output_dir']):
    os.system(f"mkdir -p '{kwargs['output_dir']}'")
try:
    videos = sorted(os.listdir(kwargs['output_dir']), key=lambda i: int(i.split('.')[0][1:]))
    if videos[-1].endswith('download'):
        os.system(f"rm '{os.path.join(kwargs['output_dir'], videos[-1])}'")
    start = int(videos[-1].split('.')[0][1:])
except:
    start = 1
ic(start)
```

### pip install .
```shell
pip install .
```
## 使用
### shell
然后就可以用 --start 参数
例如：

```shell
you-get --playlist --skip-existing-file-size-check --start 27 -d -o '/Volumes/My Passport/data/ut下载/0  未分类/ytdl videos/bilibili/test' https://www.bilibili.com/video/av81274709
```
### python

```python
import sys
from you_get import common as you_get
directory = r'/Volumes/My Passport/data/ut下载/0  未分类/ytdl videos/bilibili/test'
url = r'https://www.bilibili.com/video/av81274709'
sys.argv = ['you-get', '-d', '-o', directory, '--playlist', '--start', '27', url]
you_get.main()
```
