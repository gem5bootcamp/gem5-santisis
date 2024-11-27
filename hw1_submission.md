# Homework cache coherence

Sebastián Santisi (<ssantisi@fi.uba.ar>)

## Question 1

The performance sightly decrease increasing the number of threads.
## Question 2


### a

The performance increases up to 8 cores, then starts to hurt. At 32 cores the speedup is less tan 1 comparing with serial implementation.

### b

The speedups are:
 *  2 cores: 1.68
 *  4 cores: 2.49
 *  8 cores: 2.54
 *  16 cores: 1.80
 *  32 cores: 0.89
 *  64 cores: 0.44

 ## Question 3

### a

 The most important optimization is putting padding between the result address. The other optimizations only hurt the performance comparing with serial implementation.

 ### b

 I think that as every CPU has it's own L1 cache, having all the CPUs writing at the same memory block invalidates the L1 cache value for all the other CPUs on each write.


## Question 4

### a

Algorithm 1 in 1 core takes 0.000838 seconds, and 0.000917 on 16 cores. The speedup is 0.91

For algorithm 6 we have 0.000111 for 16 cores. The speedup is 7.55.

### b

The speedup of algorithm 6 is far beyond the performance in the real system, even when the real system has 64 cores.

The speedup of algorithm 1 is better than in the real system, that was a consistent ~0.2 for every core number major than 2.

## Question 5

```
root@codespaces-36f82b:/workspaces/latin-america-2024/homework/cache-coherence# grep "board.cache_hierarchy.ruby_system.l1_controllers2.L1Dcache.m_demand_\(hits\|accesses\)" out_[1-6]_16/stats.txt
out_1_16/stats.txt:board.cache_hierarchy.ruby_system.l1_controllers2.L1Dcache.m_demand_hits        25443                       # Number of cache demand hits (Unspecified)
out_1_16/stats.txt:board.cache_hierarchy.ruby_system.l1_controllers2.L1Dcache.m_demand_accesses        29625                       # Number of cache demand accesses (Unspecified)
out_2_16/stats.txt:board.cache_hierarchy.ruby_system.l1_controllers2.L1Dcache.m_demand_hits        31529                       # Number of cache demand hits (Unspecified)
out_2_16/stats.txt:board.cache_hierarchy.ruby_system.l1_controllers2.L1Dcache.m_demand_accesses        33773                       # Number of cache demand accesses (Unspecified)
out_3_16/stats.txt:board.cache_hierarchy.ruby_system.l1_controllers2.L1Dcache.m_demand_hits        27493                       # Number of cache demand hits (Unspecified)
out_3_16/stats.txt:board.cache_hierarchy.ruby_system.l1_controllers2.L1Dcache.m_demand_accesses        31675                       # Number of cache demand accesses (Unspecified)
out_4_16/stats.txt:board.cache_hierarchy.ruby_system.l1_controllers2.L1Dcache.m_demand_hits        35627                       # Number of cache demand hits (Unspecified)
out_4_16/stats.txt:board.cache_hierarchy.ruby_system.l1_controllers2.L1Dcache.m_demand_accesses        37873                       # Number of cache demand accesses (Unspecified)
out_5_16/stats.txt:board.cache_hierarchy.ruby_system.l1_controllers2.L1Dcache.m_demand_hits        29831                       # Number of cache demand hits (Unspecified)
out_5_16/stats.txt:board.cache_hierarchy.ruby_system.l1_controllers2.L1Dcache.m_demand_accesses        32005                       # Number of cache demand accesses (Unspecified)
out_6_16/stats.txt:board.cache_hierarchy.ruby_system.l1_controllers2.L1Dcache.m_demand_hits        35913                       # Number of cache demand hits (Unspecified)
out_6_16/stats.txt:board.cache_hierarchy.ruby_system.l1_controllers2.L1Dcache.m_demand_accesses        36146                       # Number of cache demand accesses (Unspecified)
root@codespaces-36f82b:/workspaces/latin-america-2024/homework/cache-coherence#
```

The ratios are: 85%, 93%, 86%, 94%, 93% and 99% for each algorithm.

The best performances are chunking and padding. The chunking ones raises the number to 93% as the padding one. Combining both gives 99% hit ratio.

## Question 6

```
root@codespaces-36f82b:/workspaces/latin-america-2024/homework/cache-coherence# grep "board.cache_hierarchy.ruby_system.L1Cache_Controller.Fwd_GETS " out_[1-6]_16/stats.txt | cut -b 70-160
er.Fwd_GETS |         410     17.23%     17.23% |        1809     76.01%     93.24% |
er.Fwd_GETS |         405     67.61%     67.61% |          20      3.34%     70.95% |
er.Fwd_GETS |         411     17.23%     17.23% |         376     15.77%     33.00% |
er.Fwd_GETS |         411     67.38%     67.38% |          14      2.30%     69.67% |
er.Fwd_GETS |         411     17.10%     17.10% |        1769     73.59%     90.68% |
er.Fwd_GETS |         403     64.38%     64.38% |          22      3.51%     67.89% |
root@codespaces-36f82b:/workspaces/latin-america-2024/homework/cache-coherence#
```

The numbers are 1809, 20, 376, 14, 1769, 22.

Chunking seems to be the operation that minimizes the read sharing.

I can't understand why res-race-opt has less read sharing than block-race-opt. In both implementations the access is unchunked and with different counters. The only difference between them is the padding between the counters.

## Question 7

```
root@codespaces-36f82b:/workspaces/latin-america-2024/homework/cache-coherence# grep "board.cache_hierarchy.ruby_system.L1Cache_Controller.Fwd_GETX " out_[1-6]_16/stats.txt | cut -b 70-160
er.Fwd_GETX |          42      0.13%      0.13% |        2008      6.10%      6.23% |
er.Fwd_GETX |          43      0.13%      0.13% |        1978      6.02%      6.15% |
er.Fwd_GETX |          40      0.12%      0.12% |        2010      6.13%      6.25% |
er.Fwd_GETX |          41      0.13%      0.13% |        1985      6.06%      6.18% |
er.Fwd_GETX |          40     20.83%     20.83% |          13      6.77%     27.60% |
er.Fwd_GETX |          46     20.00%     20.00% |          15      6.52%     26.52% |
root@codespaces-36f82b:/workspaces/latin-america-2024/homework/cache-coherence#
```

The numbers are 2008, 1978, 2010, 1985, 13, 15.

The operation that minimizes the write sharing is the padding between the counters.

## Question 8

### a

As predicted the write sharing is causing the most significant optimization.

Looking the average latency:

```
root@codespaces-36f82b:/workspaces/latin-america-2024/homework/cache-coherence# grep board.cache_hierarchy.ruby_system.m_latencyHistSeqr::mean out_[1-6]_16/stats.txt
out_1_16/stats.txt:board.cache_hierarchy.ruby_system.m_latencyHistSeqr::mean    33.514760                       (Unspecified)
out_2_16/stats.txt:board.cache_hierarchy.ruby_system.m_latencyHistSeqr::mean    36.181364                       (Unspecified)
out_3_16/stats.txt:board.cache_hierarchy.ruby_system.m_latencyHistSeqr::mean    20.633511                       (Unspecified)
out_4_16/stats.txt:board.cache_hierarchy.ruby_system.m_latencyHistSeqr::mean    19.909316                       (Unspecified)
out_5_16/stats.txt:board.cache_hierarchy.ruby_system.m_latencyHistSeqr::mean     3.109813                       (Unspecified)
out_6_16/stats.txt:board.cache_hierarchy.ruby_system.m_latencyHistSeqr::mean     1.480445                       (Unspecified)
root@codespaces-36f82b:/workspaces/latin-america-2024/homework/cache-coherence#
```

It's evident its correlation with write sharing for this algorithm.

### b

It's the overhead of accessing main memory when the cache is not available. Decreasing the memory time gap will increase the performance on the unoptimized versions.

### Question 9

```
root@codespaces-36f82b:/workspaces/latin-america-2024/homework/cache-coherence# grep board.cache_hierarchy.ruby_system.m_latencyHistSeqr::mean out_[1,6]_16*/stats.txt
out_1_16_1/stats.txt:board.cache_hierarchy.ruby_system.m_latencyHistSeqr::mean    11.103182                       (Unspecified)
out_1_16_25/stats.txt:board.cache_hierarchy.ruby_system.m_latencyHistSeqr::mean    67.327086                       (Unspecified)
out_1_16/stats.txt:board.cache_hierarchy.ruby_system.m_latencyHistSeqr::mean    33.514760                       (Unspecified)
out_6_16_1/stats.txt:board.cache_hierarchy.ruby_system.m_latencyHistSeqr::mean     1.265153                       (Unspecified)
out_6_16_25/stats.txt:board.cache_hierarchy.ruby_system.m_latencyHistSeqr::mean     1.858592                       (Unspecified)
out_6_16/stats.txt:board.cache_hierarchy.ruby_system.m_latencyHistSeqr::mean     1.480445                       (Unspecified)
root@codespaces-36f82b:/workspaces/latin-america-2024/homework/cache-coherence#
```

(Directories ending with `_1` are 1 cycle latency, with `_25` 25 cycles, and without sufix 10 cycles latency.)

On unoptimized algorithm 1 as the caches are missused the latency has a big impact.

On the other hand, on algorithm 6 there's no huge impact on the numbers. Yes, it's an improve of 50% from 1 to 25, but it's unrealistic having such low latency.

