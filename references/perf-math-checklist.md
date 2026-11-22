# Full-Spectrum Performance, Mathematical & Resilience Checklist

Audit systematically across **all categories** below. Cite exact `file:line` evidence.

## 1. Algorithmic & Data Structure Losses
- **Quadratic / Cubic Loops (`O(n^2)` / `O(n^3)`)**: Nested loops, `includes()` / `find()` / `starts_with()` inside loops -> replace with `HashSet` / `HashMap` / Trie (`O(1)` lookup).
- **Data Structure Mismatch**: Using array `shift()` as a queue (`O(n)`), linked lists instead of contiguous arrays (cache misses), passing huge structs by value.
- **Catastrophic Regex Backtracking (ReDoS, `O(2^n)`)**: Nested quantifiers (`(a+)+b`) or overlapping alternation on untrusted input -> rewrite without ambiguity or use linear-time `RE2` / Rust `regex`; hoist regex compilation out of loops.
- **Over-Sorting & Unmemoized Recursion**: Full `O(n log n)` sort when only Top-K is needed -> bounded heap (`O(n log k)`); overlapping recursive subproblems -> memoize or DP; string `+=` in loops -> buffer/join.

## 2. Numerical, Mathematical & Time/Encoding Traps
- **Floating-Point Precision Drift**: Binary floats (`f32`/`float`/`Number`) for currency/totals -> integer minor units or `Decimal`; never compare floats with `==`.
- **Catastrophic Cancellation & Overflow**: Subtracting nearly equal floats (`a - b`), unstabilized `exp()`/`log()` chains -> **log-sum-exp**; widen integers before summing.
- **Long-Sum & Rolling Stats Drift**: Naive sum of thousands of floats -> **Kahan summation**; re-scanning windows for mean/variance -> sliding-window delta (`O(1)`) or **Welford's online algorithm**.
- **Redundant Hot-Loop Math**: Calling `sqrt()` for distance comparisons -> compare `dx*dx + dy*dy`; recomputing `sin`/`cos`/`pow` in inner loops -> precompute LUT or hoist invariants; scalar loops on bulk arrays -> vectorize.
- **Time, Timezone & Encoding Traps**: Mixing local time and UTC, hardcoding `86400` seconds/day (ignoring DST/leap), slicing UTF-8/Emoji strings by byte index.

## 3. Memory, Allocation & Cache Locality Losses
- **Hot-Path Cloning & Resizing**: Unnecessary `.clone()` / deep copies inside loops; growing vectors/arrays without `with_capacity()`.
- **Unbounded Growth & Leaks**: Caches/maps without LRU/TTL eviction; uncleaned event listeners or timers; buffering huge files whole instead of streaming.

## 4. I/O, Database & Caching Losses
- **N+1 Queries & Syscalls**: SQL, HTTP, IPC, or `stat()` inside loops -> batch (`IN (...)`, pipeline, or carry metadata from traversal); avoid `SELECT *`.
- **Database Index & Pagination Traps**: Missing/broken indexes (wrapping indexed column in functions), `OFFSET` deep pagination, unbounded queries missing `LIMIT`.
- **Cache Stampede (Thundering Herd)**: Popular key expires -> 100+ concurrent DB hits -> single-flight coalescing or logical expiry; `JSON.parse(JSON.stringify())` deep-clone abuse.

## 5. Concurrency, Locking & Async Losses
- **Lock Contention Across I/O**: Holding `Mutex` across `await` or slow I/O -> copy minimal state, drop lock, then do I/O.
- **Blocking Event Loop / UI Thread**: Heavy CPU or sync disk I/O on async/UI thread -> worker pool / `spawn_blocking`; busy polling instead of events/backoff; unbounded fan-out without semaphore.

## 6. Frontend Rendering & Asset Losses
- **Layout Thrashing**: Interleaving DOM reads (`offsetHeight`) and writes (`style.*`) in loops -> batch reads, then writes.
- **Unthrottled Listeners & Missing Virtualization**: Heavy work on raw `scroll`/`resize`; rendering thousands of DOM nodes without windowing; unoptimized images/fonts or missing tree-shaking.

## 7. Resilience & Fault-Tolerance Gaps
- **Unsafe Retries**: Retrying non-idempotent actions or retrying without **exponential backoff + jitter + retry budget**.
- **Missing Timeouts, Circuit Breakers & Bulkheads**: Hanging forever instead of `p95` timeouts + fail-fast **circuit breakers**; shared pool exhaustion instead of **bulkheads** and cached floor fallbacks.
