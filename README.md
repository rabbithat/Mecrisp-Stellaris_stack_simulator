# Mecrisp-Stellaris_stack_simulator
A stack simulator used for debugging code that causes stack crashes 

The way it works is this: you take the problematic line of forth code, 
such as:

: rv 0 5 6 7 8 0 >R begin dup 0<> while >R repeat begin R> dup 0= until drop ;

and convert it into the stack simulator, which won't actually crash your
CPU (like perhaps the above code might) but will instead allow you to 
track the events and state of the stacks before a crash *would* happen.

: rev0 ." ===rev0=== " CR status CR 0 pushd 5 pushd 6 pushd 7 pushd 8 pushd ;

: rev1 ." ===rev1=== " CR status CR 0 pushd pushr begin dupd popd 0<> while pushR repeat ;

: rev2 ." ===rev2=== " CR status CR begin popR dupd popD 0= until dropD ;

: rev CR rev0 rev1 rev2 ;

The output looks like this:
rev
===rev0===
-------------------------------------------
dStack: empty.
Stack: [0 ]  TOS: 42  *>

rStack: empty.
Stack: [0 ]  TOS: 42  *>

-------------------------------------------

pushD pushing 0 onto dStack.
-------------------------------------------
dStack: 0
Stack: [0 ]  TOS: 42  *>

rStack: empty.
Stack: [0 ]  TOS: 42  *>

-------------------------------------------
pushD pushing 5 onto dStack.
-------------------------------------------
dStack: 0 5
Stack: [0 ]  TOS: 42  *>

rStack: empty.
Stack: [0 ]  TOS: 42  *>

-------------------------------------------
pushD pushing 6 onto dStack.
-------------------------------------------
dStack: 0 5 6
Stack: [0 ]  TOS: 42  *>

rStack: empty.
Stack: [0 ]  TOS: 42  *>

-------------------------------------------
pushD pushing 7 onto dStack.
-------------------------------------------
dStack: 0 5 6 7
Stack: [0 ]  TOS: 42  *>

rStack: empty.
Stack: [0 ]  TOS: 42  *>

-------------------------------------------
pushD pushing 8 onto dStack.
-------------------------------------------
dStack: 0 5 6 7 8
Stack: [0 ]  TOS: 42  *>

rStack: empty.
Stack: [0 ]  TOS: 42  *>

-------------------------------------------
===rev1===
-------------------------------------------
dStack: 0 5 6 7 8
Stack: [0 ]  TOS: 42  *>

rStack: empty.
Stack: [0 ]  TOS: 42  *>

-------------------------------------------

pushD pushing 0 onto dStack.
-------------------------------------------
dStack: 0 5 6 7 8 0
Stack: [0 ]  TOS: 42  *>

rStack: empty.
Stack: [0 ]  TOS: 42  *>

-------------------------------------------
popD popping 0 from dStack.
-------------------------------------------
dStack: 0 5 6 7 8
Stack: [1 ] 42  TOS: 0  *>

rStack: empty.
Stack: [1 ] 42  TOS: 0  *>

-------------------------------------------
pushR pushing 0 onto rStack.
-------------------------------------------
dStack: 0 5 6 7 8
Stack: [0 ]  TOS: 42  *>

rStack: 0
Stack: [0 ]  TOS: 42  *>

-------------------------------------------
dupD
pushD pushing 8 onto dStack.
-------------------------------------------
dStack: 0 5 6 7 8 8
Stack: [0 ]  TOS: 42  *>

rStack: 0
Stack: [0 ]  TOS: 42  *>

-------------------------------------------
popD popping 8 from dStack.
-------------------------------------------
dStack: 0 5 6 7 8
Stack: [1 ] 42  TOS: 8  *>

rStack: 0
Stack: [1 ] 42  TOS: 8  *>

-------------------------------------------
popD popping 8 from dStack.
-------------------------------------------
dStack: 0 5 6 7
Stack: [1 ] 42  TOS: 8  *>

rStack: 0
Stack: [1 ] 42  TOS: 8  *>

-------------------------------------------
pushR pushing 8 onto rStack.
-------------------------------------------
dStack: 0 5 6 7
Stack: [0 ]  TOS: 42  *>

rStack: 0 8
Stack: [0 ]  TOS: 42  *>

-------------------------------------------
dupD
pushD pushing 7 onto dStack.
-------------------------------------------
dStack: 0 5 6 7 7
Stack: [0 ]  TOS: 42  *>

rStack: 0 8
Stack: [0 ]  TOS: 42  *>

-------------------------------------------
popD popping 7 from dStack.
-------------------------------------------
dStack: 0 5 6 7
Stack: [1 ] 42  TOS: 7  *>

rStack: 0 8
Stack: [1 ] 42  TOS: 7  *>

-------------------------------------------
popD popping 7 from dStack.
-------------------------------------------
dStack: 0 5 6
Stack: [1 ] 42  TOS: 7  *>

rStack: 0 8
Stack: [1 ] 42  TOS: 7  *>

-------------------------------------------
pushR pushing 7 onto rStack.
-------------------------------------------
dStack: 0 5 6
Stack: [0 ]  TOS: 42  *>

rStack: 0 8 7
Stack: [0 ]  TOS: 42  *>

-------------------------------------------
dupD
pushD pushing 6 onto dStack.
-------------------------------------------
dStack: 0 5 6 6
Stack: [0 ]  TOS: 42  *>

rStack: 0 8 7
Stack: [0 ]  TOS: 42  *>

-------------------------------------------
popD popping 6 from dStack.
-------------------------------------------
dStack: 0 5 6
Stack: [1 ] 42  TOS: 6  *>

rStack: 0 8 7
Stack: [1 ] 42  TOS: 6  *>

-------------------------------------------
popD popping 6 from dStack.
-------------------------------------------
dStack: 0 5
Stack: [1 ] 42  TOS: 6  *>

rStack: 0 8 7
Stack: [1 ] 42  TOS: 6  *>

-------------------------------------------
pushR pushing 6 onto rStack.
-------------------------------------------
dStack: 0 5
Stack: [0 ]  TOS: 42  *>

rStack: 0 8 7 6
Stack: [0 ]  TOS: 42  *>

-------------------------------------------
dupD
pushD pushing 5 onto dStack.
-------------------------------------------
dStack: 0 5 5
Stack: [0 ]  TOS: 42  *>

rStack: 0 8 7 6
Stack: [0 ]  TOS: 42  *>

-------------------------------------------
popD popping 5 from dStack.
-------------------------------------------
dStack: 0 5
Stack: [1 ] 42  TOS: 5  *>

rStack: 0 8 7 6
Stack: [1 ] 42  TOS: 5  *>

-------------------------------------------
popD popping 5 from dStack.
-------------------------------------------
dStack: 0
Stack: [1 ] 42  TOS: 5  *>

rStack: 0 8 7 6
Stack: [1 ] 42  TOS: 5  *>

-------------------------------------------
pushR pushing 5 onto rStack.
-------------------------------------------
dStack: 0
Stack: [0 ]  TOS: 42  *>

rStack: 0 8 7 6 5
Stack: [0 ]  TOS: 42  *>

-------------------------------------------
dupD
pushD pushing 0 onto dStack.
-------------------------------------------
dStack: 0 0
Stack: [0 ]  TOS: 42  *>

rStack: 0 8 7 6 5
Stack: [0 ]  TOS: 42  *>

-------------------------------------------
popD popping 0 from dStack.
-------------------------------------------
dStack: 0
Stack: [1 ] 42  TOS: 0  *>

rStack: 0 8 7 6 5
Stack: [1 ] 42  TOS: 0  *>

-------------------------------------------
===rev2===
-------------------------------------------
dStack: 0
Stack: [0 ]  TOS: 42  *>

rStack: 0 8 7 6 5
Stack: [0 ]  TOS: 42  *>

-------------------------------------------

popR popping 5 from rStack.
pushD pushing 5 onto dStack.
-------------------------------------------
dStack: 0 5
Stack: [0 ]  TOS: 42  *>

rStack: 0 8 7 6
Stack: [0 ]  TOS: 42  *>

-------------------------------------------
dupD
pushD pushing 5 onto dStack.
-------------------------------------------
dStack: 0 5 5
Stack: [0 ]  TOS: 42  *>

rStack: 0 8 7 6
Stack: [0 ]  TOS: 42  *>

-------------------------------------------
popD popping 5 from dStack.
-------------------------------------------
dStack: 0 5
Stack: [1 ] 42  TOS: 5  *>

rStack: 0 8 7 6
Stack: [1 ] 42  TOS: 5  *>

-------------------------------------------
popR popping 6 from rStack.
pushD pushing 6 onto dStack.
-------------------------------------------
dStack: 0 5 6
Stack: [0 ]  TOS: 42  *>

rStack: 0 8 7
Stack: [0 ]  TOS: 42  *>

-------------------------------------------
dupD
pushD pushing 6 onto dStack.
-------------------------------------------
dStack: 0 5 6 6
Stack: [0 ]  TOS: 42  *>

rStack: 0 8 7
Stack: [0 ]  TOS: 42  *>

-------------------------------------------
popD popping 6 from dStack.
-------------------------------------------
dStack: 0 5 6
Stack: [1 ] 42  TOS: 6  *>

rStack: 0 8 7
Stack: [1 ] 42  TOS: 6  *>

-------------------------------------------
popR popping 7 from rStack.
pushD pushing 7 onto dStack.
-------------------------------------------
dStack: 0 5 6 7
Stack: [0 ]  TOS: 42  *>

rStack: 0 8
Stack: [0 ]  TOS: 42  *>

-------------------------------------------
dupD
pushD pushing 7 onto dStack.
-------------------------------------------
dStack: 0 5 6 7 7
Stack: [0 ]  TOS: 42  *>

rStack: 0 8
Stack: [0 ]  TOS: 42  *>

-------------------------------------------
popD popping 7 from dStack.
-------------------------------------------
dStack: 0 5 6 7
Stack: [1 ] 42  TOS: 7  *>

rStack: 0 8
Stack: [1 ] 42  TOS: 7  *>

-------------------------------------------
popR popping 8 from rStack.
pushD pushing 8 onto dStack.
-------------------------------------------
dStack: 0 5 6 7 8
Stack: [0 ]  TOS: 42  *>

rStack: 0
Stack: [0 ]  TOS: 42  *>

-------------------------------------------
dupD
pushD pushing 8 onto dStack.
-------------------------------------------
dStack: 0 5 6 7 8 8
Stack: [0 ]  TOS: 42  *>

rStack: 0
Stack: [0 ]  TOS: 42  *>

-------------------------------------------
popD popping 8 from dStack.
-------------------------------------------
dStack: 0 5 6 7 8
Stack: [1 ] 42  TOS: 8  *>

rStack: 0
Stack: [1 ] 42  TOS: 8  *>

-------------------------------------------
popR popping 0 from rStack.
pushD pushing 0 onto dStack.
-------------------------------------------
dStack: 0 5 6 7 8 0
Stack: [0 ]  TOS: 42  *>

rStack: empty.
Stack: [0 ]  TOS: 42  *>

-------------------------------------------
dupD
pushD pushing 0 onto dStack.
-------------------------------------------
dStack: 0 5 6 7 8 0 0
Stack: [0 ]  TOS: 42  *>

rStack: empty.
Stack: [0 ]  TOS: 42  *>

-------------------------------------------
popD popping 0 from dStack.
-------------------------------------------
dStack: 0 5 6 7 8 0
Stack: [1 ] 42  TOS: 0  *>

rStack: empty.
Stack: [1 ] 42  TOS: 0  *>

-------------------------------------------
dropD
popD popping 0 from dStack.
-------------------------------------------
dStack: 0 5 6 7 8
Stack: [1 ] 42  TOS: 0  *>

rStack: empty.
Stack: [1 ] 42  TOS: 0  *>

-------------------------------------------
 ok.


The outcome is the same as if I run the original, non-simulated code:

: rv 0 5 6 7 8 0 >R begin dup 0<> while >R repeat begin R> dup 0= until drop ;  

ok.

rv  ok.

.s Stack: [5 ] 42 0 5 6 7  TOS: 8  *>

ok.

In this particular case, the code doesn't crash anymore because I used the simulator 
to debug it.  It turns out the rv algorithm (the line of forth that the simulator
helped me debug) didn't do what I thought it would, but I didn't realize that
until I could keep it from crashing, which the simulator helped me do.

In any case, the results of the simulator are functionally equivalent to the
non-simulated code, so if it works on the simulator, it will work on the 
non-simulated code too.

The next step, which i haven't yet taken, would be to re-write the original Forth words so that they automatically use the simulator, and in that way any debug code could be run without having to explicitly convert the original Forth code into simulator words first.  This should be fairly easy to do, so maybe in a future version....
