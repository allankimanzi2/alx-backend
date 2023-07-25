n computing, cache replacement policies (also frequently called cache replacement algorithms or cache algorithms) are optimizing instructions, or algorithms, that a computer program or a hardware-maintained structure can utilize in order to manage a cache of information stored on the computer. Caching improves performance by keeping recent or often-used data items in memory locations that are faster or computationally cheaper to access than normal memory stores. When the cache is full, the algorithm must choose which items to discard to make room for the new ones.
Overview

The average memory reference time is[1]

    T = m × T m + T h + E {\displaystyle T=m\times T_{m}+T_{h}+E}

where

    m m = miss ratio = 1 - (hit ratio)
    T m T_{m} = time to make a main memory access when there is a miss (or, with multi-level cache, average memory reference time for the next-lower cache)
    T h T_{h}= the latency: the time to reference the cache (should be the same for hits and misses)
    E E = various secondary effects, such as queuing effects in multiprocessor systems

There are two primary figures of merit of a cache: the latency and the hit ratio. There are also a number of secondary factors affecting cache performance.[1]

The "hit ratio" of a cache describes how often a searched-for item is actually found in the cache. More efficient replacement policies keep track of more usage information in order to improve the hit rate (for a given cache size).

The "latency" of a cache describes how long after requesting a desired item the cache can return that item (when there is a hit). Faster replacement strategies typically keep track of less usage information—or, in the case of direct-mapped cache, no information—to reduce the amount of time required to update that information.

Each replacement strategy is a compromise between hit rate and latency.

Hit rate measurements are typically performed on benchmark applications. The actual hit ratio varies widely from one application to another. In particular, video and audio streaming applications often have a hit ratio close to zero, because each bit of data in the stream is read once for the first time (a compulsory miss), used, and then never read or written again. Even worse, many cache algorithms (in particular, LRU) allow this streaming data to fill the cache, pushing out of the cache information that will be used again soon (cache pollution).[2]

Other things to consider:

    Items with different cost: keep items that are expensive to obtain, e.g. those that take a long time to get.
    Items taking up more cache: If items have different sizes, the cache may want to discard a large item to store several smaller ones.
    Items that expire with time: Some caches keep information that expires (e.g. a news cache, a DNS cache, or a web browser cache). The computer may discard items because they are expired. Depending on the size of the cache no further caching algorithm to discard items may be necessary.

Various algorithms also exist to maintain cache coherency. This applies only to situation where multiple independent caches are used for the same data (for example multiple database servers updating the single shared data file).
Policies
Bélády's algorithm

The most efficient caching algorithm would be to always discard the information that will not be needed for the longest time in the future. This optimal result is referred to as Bélády's optimal algorithm/simply optimal replacement policy or the clairvoyant algorithm. Since it is generally impossible to predict how far in the future information will be needed, this is generally not implementable in practice. The practical minimum can be calculated only after experimentation, and one can compare the effectiveness of the actually chosen cache algorithm.
Optimal Working

At the moment when a page fault occurs, some set of pages is in memory. In the example, the sequence of '5', '0', '1' is accessed by Frame 1, Frame 2, Frame 3 respectively. Then when '2' is accessed, it replaces value '5', which is in frame 1 since it predicts that value '5' is not going to be accessed in the near future. Because a real-life general purpose operating system cannot actually predict when '5' will be accessed, Bélády's Algorithm cannot be implemented on such a system.
Random replacement (RR)

Randomly selects a candidate item and discards it to make space when necessary. This algorithm does not require keeping any information about the access history. For its simplicity, it has been used in ARM processors.[3] It admits efficient stochastic simulation.[4]
Simple queue-based policies
First in first out (FIFO)

Using this algorithm the cache behaves in the same way as a FIFO queue. The cache evicts the blocks in the order they were added, without any regard to how often or how many times they were accessed before.
Last in first out (LIFO) or First in last out (FILO)

Using this algorithm the cache behaves in the same way as a stack and opposite way as a FIFO queue. The cache evicts the block added most recently first without any regard to how often or how many times it was accessed before.
Simple recency-based policies
Least recently used (LRU)

Discards the least recently used items first. This algorithm requires keeping track of what was used when, which is expensive if one wants to make sure the algorithm always discards the least recently used item. General implementations of this technique require keeping "age bits" for cache-lines and track the "Least Recently Used" cache-line based on age-bits. In such an implementation, every time a cache-line is used, the age of all other cache-lines change. LRU is actually a family of caching algorithms with members including 2Q by Theodore Johnson and Dennis Shasha,[5] and LRU/K by Pat O'Neil, Betty O'Neil and Gerhard Weikum.[6]

The access sequence for the below example is A B C D E D F.
LRU working

In the example once A B C D gets installed in the blocks with sequence numbers (Increment 1 for each new Access) and when E is accessed, it is a miss and it needs to be installed in one of the blocks. According to the LRU Algorithm, since A has the lowest Rank(A(0)), E will replace A.

In the second to last step, D is accessed and therefore the sequence number is updated.

Finally, F is accessed taking the place of B which had the lowest Rank(B(1)) at the moment.
Time aware least recently used (TLRU)

The Time aware Least Recently Used (TLRU)[7] is a variant of LRU designed for the situation where the stored contents in cache have a valid life time. The algorithm is suitable in network cache applications, such as Information-centric networking (ICN), Content Delivery Networks (CDNs) and distributed networks in general. TLRU introduces a new term: TTU (Time to Use). TTU is a time stamp of a content/page which stipulates the usability time for the content based on the locality of the content and the content publisher announcement. Owing to this locality based time stamp, TTU provides more control to the local administrator to regulate in network storage. In the TLRU algorithm, when a piece of content arrives, a cache node calculates the local TTU value based on the TTU value assigned by the content publisher. The local TTU value is calculated by using a locally defined function. Once the local TTU value is calculated the replacement of content is performed on a subset of the total content stored in cache node. The TLRU ensures that less popular and small life content should be replaced with the incoming content.
Most recently used (MRU)

In contrast to Least Recently Used (LRU), MRU discards the most recently used items first. In findings presented at the 11th VLDB conference, Chou and DeWitt noted that "When a file is being repeatedly scanned in a [Looping Sequential] reference pattern, MRU is the best replacement algorithm."[8] Subsequently, other researchers presenting at the 22nd VLDB conference noted that for random access patterns and repeated scans over large datasets (sometimes known as cyclic access patterns) MRU cache algorithms have more hits than LRU due to their tendency to retain older data.[9] MRU algorithms are most useful in situations where the older an item is, the more likely it is to be accessed.

The access sequence for the below example is A B C D E C D B.
MRU working

Here, A B C D are placed in the cache as there is still space available. At the 5th access E, we see that the block which held D is now replaced with E as this block was used most recently. Another access to C and at the next access to D, C is replaced as it was the block accessed just before D and so on.
Segmented LRU (SLRU)

SLRU cache is divided into two segments, a probationary segment and a protected segment. Lines in each segment are ordered from the most to the least recently accessed. Data from misses is added to the cache at the most recently accessed end of the probationary segment. Hits are removed from wherever they currently reside and added to the most recently accessed end of the protected segment. Lines in the protected segment have thus been accessed at least twice. The protected segment is finite, so migration of a line from the probationary segment to the protected segment may force the migration of the LRU line in the protected segment to the most recently used (MRU) end of the probationary segment, giving this line another chance to be accessed before being replaced. The size limit on the protected segment is an SLRU parameter that varies according to the I/O workload patterns. Whenever data must be discarded from the cache, lines are obtained from the LRU end of the probationary segment.[10]
LRU Approximations

LRU can be prohibitively expensive in caches with higher associativity. Practical hardware usually employs an approximation to achieve similar performance at a much lower hardware cost.
Pseudo-LRU (PLRU)
Further information: Pseudo-LRU

For CPU caches with large associativity (generally >4 ways), the implementation cost of LRU becomes prohibitive. In many CPU caches, a scheme that almost always discards one of the least recently used items is sufficient, so many CPU designers choose a PLRU algorithm which only needs one bit per cache item to work. PLRU typically has a slightly worse miss ratio, has a slightly better latency, uses slightly less power than LRU and lower overheads compared to LRU.

The following example shows how Bits work as a binary tree of 1-bit pointers that point to the less recently used subtree. Following the pointer chain to the leaf node identifies the replacement candidate. Upon an access all pointers in the chain from the accessed way's leaf node to the root node are set to point to subtree that does not contain the accessed way.

The access sequence is A B C D E.
Pseudo LRU working

The principle here is simple to understand if we only look at the arrow pointers. When there is an access to a value, say 'A', and we cannot find it in the cache, then we load it from memory and place it at the block where the arrows are currently pointing, going from top to bottom. After we have placed that block we flip those same arrows so they point the opposite way. In the above example we see how 'A' was placed, followed by 'B', 'C and 'D'. Then as the cache became full 'E' replaced 'A' because that was where the arrows were pointing at that time, and the arrows that led to 'A' were flipped to point in the opposite direction. The arrows then led to 'B', which will be the block replaced on the next cache miss.
CLOCK-Pro

LRU algorithm cannot be directly implemented in the critical path of computer systems, such as operating systems, due to its high overhead. An approximation of LRU, called CLOCK is commonly used for the implementation. Similarly, CLOCK-Pro is an approximation of LIRS for a low cost implementation in systems.[11] CLOCK-Pro is under the basic CLOCK framework, but has three major distinct merits. First, CLOCK-Pro has three "clock hands" in contrast to a simple structure of CLOCK where only one "hand" is used. With the three hands, CLOCK-Pro is able to measure the reuse distance of data accesses in an approximate way. Second, all the merits of LIRS are retained, such as quickly evicting one-time accessing and/or low locality data items. Third, the complexity of the CLOCK-Pro is same as that of CLOCK, thus it is easy to implement at a low cost. The buffer cache replacement implementation in the current version of Linux is a combination of LRU and CLOCK-Pro.[12][13]
Simple frequency-based policies
Least-frequently used (LFU)
Further information: Least-frequently used

Counts how often an item is needed. Those that are used least often are discarded first. This works very similar to LRU except that instead of storing the value of how recently a block was accessed, we store the value of how many times it was accessed. So of course while running an access sequence we will replace a block which was used fewest times from our cache. E.g., if A was used (accessed) 5 times and B was used 3 times and others C and D were used 10 times each, we will replace B.
Least frequent recently used (LFRU)

The Least Frequent Recently Used (LFRU)[14] cache replacement scheme combines the benefits of LFU and LRU schemes. LFRU is suitable for 'in network' cache applications, such as Information-centric networking (ICN), Content Delivery Networks (CDNs) and distributed networks in general. In LFRU, the cache is divided into two partitions called privileged and unprivileged partitions. The privileged partition can be defined as a protected partition. If content is highly popular, it is pushed into the privileged partition. Replacement of the privileged partition is done as follows: LFRU evicts content from the unprivileged partition, pushes content from privileged partition to unprivileged partition, and finally inserts new content into the privileged partition. In the above procedure the LRU is used for the privileged partition and an approximated LFU (ALFU) scheme is used for the unprivileged partition, hence the abbreviation LFRU.

The basic idea is to filter out the locally popular contents with ALFU scheme and push the popular contents to one of the privileged partition.
LFU with dynamic aging (LFUDA)

A variant called LFU with Dynamic Aging (LFUDA) that uses dynamic aging to accommodate shifts in the set of popular objects. It adds a cache age factor to the reference count when a new object is added to the cache or when an existing object is re-referenced. LFUDA increments the cache ages when evicting blocks by setting it to the evicted object's key value. Thus, the cache age is always less than or equal to the minimum key value in the cache.[15] Suppose when an object was frequently accessed in the past and now it becomes unpopular, it will remain in the cache for a long time thereby preventing the newly or less popular objects from replacing it. So this Dynamic aging is introduced to bring down the count of such objects thereby making them eligible for replacement. The advantage of LFUDA is it reduces the cache pollution caused by LFU when cache sizes are very small. When Cache sizes are large few replacement decisions are sufficient and cache pollution will not be a problem.
RRIP-style policies

RRIP-style policies form the basis for many other cache replacement policies including Hawkeye[16] which won the CRC2 championship and was considered the most advanced cache replacement policy of its time.
Re-Reference Interval Prediction (RRIP)

RRIP[17] is a very flexible policy proposed by Intel that attempts to provide good scan resistance while also allowing older cache lines that haven't been reused to be evicted. All cache lines have a prediction value called the RRPV (Re-Reference Prediction Value) that should correlate with when the line is expected to be reused. On insertion, this RRPV is usually high, so that if the line isn't reused soon, it will be evicted, this is done to prevent scans (large amounts of data that is used only once) from filling up the cache. When a cache line is reused, this RRPV is set to zero, indicating that this line has been reused once, and is likely to be reused again.

On a cache miss, the line with the RRPV equal to the maximum possible RRPV is evicted (for example, with 3-bit values, the line with the RRPV of 23 - 1 = 7 is evicted), if no lines have this value, all RRPVs in the set are incremented by 1 until one reaches it. A tie breaker is needed, and usually it is the first line on the left. This increment is needed to make sure that older lines are aged properly and will be evicted if they aren't reused.
Static RRIP (SRRIP)

SRRIP inserts lines with an RRPV value of maxRRIP. This means that a line that has just been inserted will be the most likely to be evicted on a cache miss.
Bimodal RRIP (BRRIP)

SRRIP performs well in the normal case, but suffers when the working set is much larger than the cache size and causes cache thrashing, this is remedied by inserting lines with an RRPV value of maxRRPV most of the time, and inserting lines with an RRPV value of maxRRPV - 1 randomly with a low probability. This causes some lines to "stick" in the cache and help against thrashing.

However, BRRIP degrades performance on non thrashing accesses.
Dynamic RRIP (DRRIP)

SRRIP performs best when the working set is smaller than the cache size, while BRRIP performs best when the working set is larger than the cache size.

DRRIP[17] aims for the best of both worlds. It uses set dueling[18] to select whether to use SRRIP or BRRIP. It dedicates a few sets (typically 32) to only use SRRIP and another few to only use BRRIP, and it uses a policy counter that monitors which of these sets performs better to determine which policy will be used by the rest of the cache.
Cache replacement policies approximating Bélády's algorithm

Bélády's algorithm is the optimal cache replacement policy, but it requires knowledge of the future to evict lines that will be reused farthest in the future. Multiple replacement policies have been proposed that attempt to predict future reuse distances from past access patterns,[19] thus allowing them to approximate the optimal replacement policy. Some of the best performing cache replacement policies are ones that attempt to imitate Bélády's algorithm.
Hawkeye

Hawkeye[16] attempts to emulate Bélády's algorithm by using past accesses by a PC to predict whether the accesses it produces generate cache friendly accesses (accesses that get used later on) or cache averse accesses (don't get used later on).

It does this by sampling a number of the cache sets (that aren't aligned), it uses a history of length 8 × the cache size {\displaystyle 8\times {\text{the cache size}}} and emulates Bélády's algorithm on these accesses. This allows the policy to figure out which lines should have been cached and which should not have been cached.

This data allows it to predict whether an instruction is cache friendly or cache averse. This data is then fed into an RRIP, and means accesses from instructions that are cache friendly have a lower RRPV value (likely to be evicted later), and accesses from instructions that are cache averse have a higher RRPV value (likely to be evicted sooner).

The RRIP backend is the part that does the actual eviction decisions. The sampled cache and OPT generator are only used to set the initial RRPV value of the inserted cache lines.

Hawkeye won the CRC2 cache championship in 2017,[20] beating all other cache replacement policies at the time.

Harmony[21] is an extension of Hawkeye that improves prefetching performance.
Block diagram of the Mockingjay cache replacement policy.
Mockingjay

Mockingjay[22] tries to improve upon Hawkeye in multiple ways. First, it drops the binary prediction, allowing it to make more fine grained decisions about which cache lines to evict. Second, it leaves the decision about which cache line to evict later on, after more information is available.

It achieves this by keeping a sampled cache of unique accesses, the PCs that produced them and their timestamps. When a line in the sampled cache gets accessed again, the difference in time will be sent to the Reuse Distance Predictor, which uses Temporal Difference Learning,[23] where the new RDP value will be incremented or decremented by a small number to compensate for outliers. The number is calculated as w = m i n ( 1 , timestamp difference 16 ) {\displaystyle w=min(1,{\frac {\text{timestamp difference}}{16}})}. Except when the value has not been initialized, in which case the observed reuse distance is inserted directly. If the sampled cache is full, and we need to throw away a line, we train the RDP that the PC that last accessed it produces streaming accesses.

On an access or insertion, the Estimated Time of Reuse (ETR) for this line is updated to reflect the predicted reuse distance. Every few accesses to the set, decrement all the ETR counters of the set (which can fall into the negative, if not accesses past their estimated time of reuse).

On a cache miss, the line with the highest absolute ETR value is evicted (The line which is estimated to be reused farthest in the future, or has been estimated to be reused farthest in the past and hasn't been reused).

Mockingjay achieves results that are very close to the optimal Bélády's algorithm, typically within only a few percent performance difference.
Cache replacement policies using machine learning

Multiple cache replacement policies have attempted to use perceptrons, markov chains or other types of machine learning to predict while line to evict.[24][25] There are also learning augmented algorithms for the cache replacement problem. [26][27]
Other cache replacement policies
Low inter-reference recency set (LIRS)
Further information: LIRS caching algorithm

LIRS is a page replacement algorithm with an improved performance over LRU and many other newer replacement algorithms. This is achieved by using reuse distance as a metric for dynamically ranking accessed pages to make a replacement decision.[28] LIRS effectively address the limits of LRU by using recency to evaluate Inter-Reference Recency (IRR) for making a replacement decision.
LIRS algorithm working

In the above figure, "x" represents that a block is accessed at time t. Suppose if block A1 is accessed at time 1 then Recency will become 0 since this is the first accessed block and IRR will be 1 since it predicts that A1 will be accessed again in time 3. In the time 2 since A4 is accessed, the recency will become 0 for A4 and 1 for A1 because A4 is the most recently accessed Object and IRR will become 4 and it will go on. At time 10, the LIRS algorithm will have two sets LIR set = {A1, A2} and HIR set = {A3, A4, A5}. Now at time 10 if there is access to A4, miss occurs. LIRS algorithm will now evict A5 instead of A2 because of its largest recency.
Adaptive replacement cache (ARC)
Further information: Adaptive replacement cache

Constantly balances between LRU and LFU, to improve the combined result.[29] ARC improves on SLRU by using information about recently evicted cache items to dynamically adjust the size of the protected segment and the probationary segment to make the best use of the available cache space. Adaptive replacement algorithm is explained with the example.[30]
AdaptiveClimb (AC)

Uses recent hit/miss to adjust the jump where in climb any hit switches the position one slot to the top, and in LRU hit switches the position of the hit to the top. Thus, benefiting from the optimality of climb when the program is in a fixed scope, and the rapid adaptation to a new scope, as LRU does.[31] [32] Also support cache sharing among cores by releasing extras when the references are to the top part of the cache.
Clock with adaptive replacement (CAR)
Further information: Clock with adaptive replacement

Combines the advantages of Adaptive Replacement Cache (ARC) and CLOCK. CAR has performance comparable to ARC, and substantially outperforms both LRU and CLOCK. Like ARC, CAR is self-tuning and requires no user-specified magic parameters. It uses 4 doubly linked lists: two clocks T1 and T2 and two simple LRU lists B1 and B2. T1 clock stores pages based on "recency" or "short term utility" whereas T2 stores pages with "frequency" or "long term utility". T1 and T2 contain those pages that are in the cache, while B1 and B2 contain pages that have recently been evicted from T1 and T2 respectively. The algorithm tries to maintain the size of these lists B1≈T2 and B2≈T1. New pages are inserted in T1 or T2. If there is a hit in B1 size of T1 is increased and similarly if there is a hit in B2 size of T1 is decreased. The adaptation rule used has the same principle as that in ARC, invest more in lists that will give more hits when more pages are added to it.
Multi queue (MQ)

The multi queue algorithm or MQ was developed to improve the performance of second level buffer cache for e.g. a server buffer cache. It is introduced in a paper by Zhou, Philbin, and Li.[33] The MQ cache contains an m number of LRU queues: Q0, Q1, ..., Qm-1. Here, the value of m represents a hierarchy based on the lifetime of all blocks in that particular queue. For example, if j>i, blocks in Qj will have a longer lifetime than those in Qi. In addition to these there is another history buffer Qout, a queue which maintains a list of all the Block Identifiers along with their access frequencies. When Qout is full the oldest identifier is evicted. Blocks stay in the LRU queues for a given lifetime, which is defined dynamically by the MQ algorithm to be the maximum temporal distance between two accesses to the same file or the number of cache blocks, whichever is larger. If a block has not been referenced within its lifetime, it is demoted from Qi to Qi−1 or evicted from the cache if it is in Q0. Each queue also has a maximum access count; if a block in queue Qi is accessed more than 2i times, this block is promoted to Qi+1 until it is accessed more than 2i+1 times or its lifetime expires. Within a given queue, blocks are ranked by the recency of access, according to LRU.[34]
Multi Queue Replacement

We can see from Fig. how the m LRU queues are placed in the cache. Also see from Fig. how the Qout stores the block identifiers and their corresponding access frequencies. a was placed in Q0 as it was accessed only once recently and we can check in Qout how b and c were placed in Q1 and Q2 respectively as their access frequencies are 2 and 4. The queue in which a block is placed is dependent on access frequency(f) as log2(f). When the cache is full, the first block to be evicted will be the head of Q0 in this case a. If a is accessed one more time it will move to Q1 below b.
Pannier: Container-based caching algorithm for compound objects

Pannier[35] is a container-based flash caching mechanism that identifies divergent (heterogeneous) containers where blocks held therein have highly varying access patterns. Pannier uses a priority-queue based survival queue structure to rank the containers based on their survival time, which is proportional to the live data in the container. Pannier is built based on Segmented LRU (S2LRU), which segregates hot and cold data. Pannier also uses a multi-step feedback controller to throttle flash writes to ensure flash lifespan.
Static analysis

One may want to establish, through static analysis, which accesses are cache hits or misses, for instance to rigorously bound the worst-case execution time of a program.[36] The output of static analysis is thus, for every access in the program, an indication if it always a cache hit, always a miss, or in indeterminate status. Many refinements are possible: for instance one may establish that an access is a hit if the procedure where it is located is used in certain calling contexts, that an access in a loop a cache miss in the first iteration of that loop but a hit in the other iterations, etc.

A classical approach to analyzing properties of LRU caches is to associate to each block in the cache an "age" (0 for the most recently used, and so on up to cache associativity) and compute intervals for possible ages.[37] This analysis can be refined to distinguish automatically cases where the same program point is accessible by paths that result, for some, in misses, and for some, in hits.[38] One may even obtain an exact analysis (with respect to an execution model that considers only the syntactic control flow of the program, without semantics for conditions) that is at the same time efficient by abstracting sets of cache states by antichains, which are themselves represented by compact binary decision diagrams.[39]

This good property of LRU from the point of view of static analysis does not generalize to pseudo-LRU policies. It indeed can be shown that, from the point of view of computational complexity theory, static analysis problems posed by pseudo-LRU and FIFO belong to higher complexity classes than those for LRU[40] · .[41]
See also

    Cache-oblivious algorithm
    Locality of reference
    Distributed cache
