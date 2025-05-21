الاسم : خالد محمد محمد لبيب
الرقم الاكاديمي : 15210113
#### Write‑Once Protocol

**Motivation & States.**  
The Write‑Once design sits between pure write‑through and write‑back schemes to save bus bandwidth without sacrificing coherence. Each cache block maintains two bits of metadata: a _shared_ flag (has anyone else read it?) and a _dirty_ flag (has this block been modified since it came from memory?). Blocks start in the “Clean Exclusive” state on a read miss, then can transition to “Dirty Exclusive” after the first write.

**Operation Details.**

1. **Read Miss:**
    
    - Issue a BusRd, fetch data from memory (or another cache), set _shared_=1 if any other cache had it.
        
    - Cache line enters Clean state if shared, Exclusive if no other sharers.
        
2. **First Write (Write‑Through):**
    
    - On a write hit when _dirty_=0, the processor updates the cache and simultaneously issues a BusWr to memory.
        
    - The bus transaction invalidates any copies in other caches (using BusRdX or explicit invalidate).
        
    - Mark the line _dirty_=1.
        
3. **Subsequent Writes (Write‑Back):**
    
    - Hits when _dirty_=1 update only the local cache; no further bus traffic.
        
4. **Eviction:**
    
    - If a dirty line is evicted, issue a write‑back to memory; clean lines simply disappear.
        

**Example Sequence.**

- CPU1 reads A → BusRd → Exclusive clean in Cache1.
    
- CPU1 writes A → BusWr to memory + invalidate in others → A is dirty in Cache1.
    
- CPU1 writes A again → local update, no bus.
    
- CPU1 evicts A → write‑back dirty word to memory.
    

**Pros & Cons.**

- **Pros:** After the first costly write‑through, multiple in‑cache updates are free; good balance of consistency and reduced bus traffic.
    
- **Cons:** The first write still floods memory; invalidations can cause extra misses if other cores promptly access the same line.
    

---

#### Write‑Update & Partial Write‑Through Protocol

**Conceptual Blend.**  
This hybrid fuses the “update on write” idea—where every store broadcasts the new value to all other caches—with a “write‑through to memory” that happens only periodically or under thresholds, not on every write.

**Core Mechanisms.**

- **Cache Writes → Broadcast Updates:**  
    Each store issues a BusUpd carrying the modified word and its address. Other caches that hold the line in a shared or exclusive state simply overwrite their copy rather than invalidating it.
    
- **Partial Write‑Through to Memory:**  
    The cache controller tracks how many BusUpd events have occurred on a given block (or elapsed time). After _N_ updates or _T_ milliseconds, it bursts all accumulated modifications back to main memory in a single write‑through sequence, then resets its counter.
    

**State & Sharers Management.**

- Blocks are either _Shared_ or _Exclusive_. Since updates never invalidate, multiple sharers can remain active indefinitely. A directory of sharers (or simple bus snoop filters) ensures updates only go to caches that actually hold the block.
    

**Illustration.**

- CPU2 and CPU3 both have A in shared state.
    
- CPU2 writes A → BusUpd, both caches replace old A with new value, no invalidation.
    
- After 10 updates, CPU2 writes A to memory (one combined burst), resetting its counter.
    

**Strengths & Weaknesses.**

- **Strengths:** Keeps all caches closely synchronized with minimal invalidation-induced misses; memory traffic is amortized in bursts.
    
- **Weaknesses:** Frequent BusUpd broadcasts can clog the bus in write‑heavy workloads; sharer lists can grow large in read‑dominated scenarios.
    

---

#### Write‑Update & Write‑Back Protocol

**Pure Update, Lazy Memory Sync.**  
Here, every write still generates an update packet sent to all other caches that have the line, but main memory remains untouched until the cache block is evicted—fully combining write‑update coherence with write‑back performance.

**Protocol Flow.**

1. **Read Miss:** BusRd brings the block into cache, marked Shared or Exclusive.
    
2. **Write Hit:**
    
    - Issue a BusUpd that includes the new data and address.
        
    - All snooping caches holding that block simply overwrite their copy, remaining coherent without invalidation.
        
    - The local cache marks the block _dirty_.
        
3. **Eviction:** If the block is dirty at eviction, send the updated block to memory; clean blocks vanish quietly.
    

**Sharer Tracking & Updates.**

- A sharers directory or explicit “cache presents” messages track who has which block.
    
- Each update multicast consults that directory to reduce unnecessary traffic.
    

**Example Timeline.**

- CPU1 reads B → BusRd → Exclusive clean.
    
- CPU1 writes B → BusUpd “B=5” → mark dirty.
    
- CPU2 had B in shared state → snoops BusUpd, updates B to 5.
    
- CPU1 evicts B → write‑back “5” to memory.
    

**Advantages & Drawbacks.**

- **Advantages:**
    
    - Memory sees writes only on eviction, drastically cutting memory bus usage.
        
    - All caches stay in sync immediately after each write.
        
- **Drawbacks:**
    
    - BusUpd storms can saturate the bus if many writes occur, especially with large sharer sets.
        
    - Directory overhead to track sharers can be substantial.
        

---

#### Comparative Summary

|Protocol|Bus Traffic (Writes)|Memory Traffic|Cache‑to‑Cache Sync|Directory Needs|
|---|---|---|---|---|
|**Write‑Once**|1st write → invalidate, then none  <br>Eviction → write‑back|Moderate|Through invalidation misses|Low (just snooping)|
|**Write‑Update + Partial WT**|Each write → BusUpd;  <br>Every _N_ bursts to memory|Low per write, batched overall|Immediate via updates|Medium (sharer list)|
|**Write‑Update + Write‑Back**|Each write → BusUpd;  <br>Eviction → write‑back|Very low until eviction|Immediate via updates|Medium (sharer list)|

Each strategy sits at a different point in the design space between bus utilization, memory bandwidth, and coherence latency. Choose the one whose trade‑offs best match your workload’s read/write mix, sharing patterns, and hardware cost constraints.