
### Define shared memory
A shared memory model is one in which processors communicate by reading and writing locations in a shared memory that is equally accessible by all processors. Each processor may have registers , buffers , caches , and local memory banks as additional memory resources.

---
### Blocking network
if two pairs attempt to communicate at the same time along a shared path , one pair must wait for the other , this is called blocking , and such MINs are called blocking networks , A network that can handle all possible connections without blocking is called non-blocking network.

----
### what is a multiprocessor system ? Explain SIMD , MIMD
a multiprocessor system consists of multiple processing units connected via some interconnection network plus the software needed to make the processing units work together

SIMD : single instruction multiple data streams.
MIMD : multiple instruction multiple data streams.

parallel computers are either SIMD or MIMD when there is only one control unit and all processors execute the same instruction in a synchronized fashion , the parallel machine is classified as SIMD , in MIMD machine each processor has its own control unit and can execute different instructions on different data.

---
### notes on Omega , Butterfly and rearrangeable networks
**The omega network** (shuffle shift down L to M and rotate) , the omega network represents another well-known type of MINs. A size N omega networks consists of log2(N) shuffle Exchange networks , each stage consists of a column of N = 2 , two input switching elements whose input is a shuffle connection.

**Butterfly permutation**
do truth table where each number maps to same number with reverse of MSB , LSB
B(110) --> 011 , B(100) --> 001

**Rearrangeable Networks**
Rearrangeable networks are characterized by the property that it is always possible to rearrange already established connections in order to make allowance for other connections to be established simultaneously 

---------------
### define miss rate of cache
The miss rate of a cache is the fraction of the references that cannot be satisfied by the cache , and so must be copied from the global memory , across the bus , into the cache , and then passed on to the local processor.

-------------
### cache coherence and switching techniques
**cache coherence**
In multiprocessing system , when a task is running on the processor p it requests the data in the global memory location X , for example the contents of X are copied to processor P's local cache where it is passed on to P , Now suppose processor Q also accesses X and what happens if Q wants to write new value over old value of X , processor Q invalidates all other copies of X when it writes a new value into its cache , this sets the dirty bit for X and Q continue to change X without further notifications to other caches because Q has the only valid copy of X , however when processor P wants to read X it must wait until X is updated and the dirty bit is cleared , write update maintains consistency by immediately updating all copies in all caches , All dirty bits are set during each write operation , After all copies have been update , all dirty bits are cleared.

**switching techniques**
we have 2 main switching techniques 

circuit switching : a complete path has to be established prior to the start communication between a source and a destination.

Packet switching : communication between a source and a destination takes place via message divided into smaller entities called packets

---
![[Pasted image 20250407030049.png]]

![[Pasted image 20250407030118.png]]

-------
omega , shuffle --> cyclic shift left
banyan --> cyclic shift right 

0 ---> up
1 ---> down

![[Pasted image 20250407045850.png]]