# CFC Studio 共学 Epoch1 指引
---
# [ArchSerein]

> 通过共学计划完成 cs162 的剩余实验和学习 cmu18447

## 笔记证明

> 下面的日期请保证至少两个字符占位
>
>   * 正确：12.06
>   * 错误：12.6
>
> 若当天没有学习内容，请直接跳过该日期的记录，不要在日期下记录：“请假” 等内容

### 12.06

举例示范：

今日学习时间：XXXX
学习内容小结：XXXX
Homework 部分（如果有安排需要填写证明完成）
Question and Ideas（有什么疑问/或者想法，可以记在这里，也可以分享到群里讨论交流）

### 01.06

> 今日学习时间：

> 3h

> 今日学习任务：

> reading -> Multiprocessor Benchmarks and Performance Models

> 学习内容小结：

> in this subsection, the main discussion is about evaluating processor performance through benchmarking. But due to such traditional restrictions to benchmark is chiefly limited to the architecture and compiler, better data structuers and algorithms maybe give a misleading result. Then, ucbidentified 13 design patterns that they will be part of applications of the future. Next, the main discussion of The Roofline Model, how to use it to find performance bottlenecks and how to optimize the processor. How to reduce computational bottlenecks: mix floating-point operation, improve inst-level parallelism and apply SIMD; How to reduce memory bottlenecks: software prefetching and memory affinity.

> Question and Ideas（有什么疑问/或者想法，可以记在这里，也可以分享到群里讨论交流）

> a description of The Roofline Models is at the link [the roofline models wiki](https://en.wikipedia.org/wiki/Roofline_model)

### 01.07

> 今日学习时间：

3h

> 今日学习任务：

> Completing the file system buffer cache

> 学习内容小结：

> The original pintos implementation of the file system, when it performs read/write operations, that will directly access the file system's underlying block device. Now, I need to add a buffer cache for the file system.

> the buffer cache structure I designed is:
```
struct buf {
  int cnt;
  bool dirty;
  struct block *dev;
  block_sector_t blockno;
  struct list_elem elem;
  uint8_t data[BLOCK_SECTOR_SIZE];
};
/**
* cnt: number of references to the current buffer 
* dirty: when data is only written to the buffer and not synchronized to disk, dirty is true, otherwise false
* dev: maybe always fs_device, but like fsutil.c use other device, maybe they don't need the buffer cache
* blockno: sector number, the position of read/write
* elem: list element, Data Structures for Bidirectional Chained Tables
* data: the data aera of buffer
**/
```
> when pintos need to read/write, it will use bread/bwrite, not block_read/block_write. block_read/block_write will be used in the bread/bwrite, when caching missing. If there are no free cache blocks, the LRU algorithm will be used to replace the one that meets the requirement. You need to make sure that ditry is set before replacing it, and if it is true you need to write it back to disk and replace it.


