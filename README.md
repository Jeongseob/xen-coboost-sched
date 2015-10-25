# Opportunistic co-boosting virtual CPU siblings of a VM

This is a modified Xen credit scheduler to reflect the urgency of guest operating systems on scheduling virtual CPUs. In virtualized systems, a virtual CPU holding a spinlock, which is used in guest operating systems, can be preempted by hypervisors. Then, other virtual CPUs waiting the spinlock cannot make a forward progress because the lock is already held. This is well known to lock holder preemption(**LHP**) problem. In addition, current Linux implements ticket lock to prevent starvation of lock acquisition by providing the order of lock requesters. It introduces a new problem called lock waiter preemption (**LWP**) problem in virtualized systems. A virtual CPU waiting the lock to be released can be preempted. Then, the next lock requester cannot acquire the lock even if the lock is not held. These two problems can significantly degrade performance of virtual machines.

To minimize the effect of lock holder and waiter preemption problem, we introduced a very simple co-boosting technique. Currently, the spinlock is implemented by PAUSE instructions in x86 architectures. So, when a physical CPU excessively executing the PAUSE instructions in a given time, x86 systems raise an exception called [Pause-Loop Exiting](http://www.intel.com/Assets/en_US/PDF/manual/253669.pdf) on Intel processors. When the exception occurs, current Xen hypervisor just yields the virtual CPU excessively executing PAUSE instructions and schedule another virtual CPU waiting the physical CPU. Although it improves the system throughput, it cannot solve the fundamental problem. 

Our approach is to co-schedule the virtual CPU siblings of the VM incurring the PAUSE exception. Then, potentially the virtual CPUs holding a spinlock can complete its critical section and release the lock. So, other virtual CPUs can enter the critical section. Also, virtual CPUs waiting the lock can take physical CPUs. We could minimize the lock waiter preemption(LWP) problem as well.  

This work done while I am a Ph.D. student at KAIST.  

# Authors
* Jeongseob Ahn
* Chang Hyun Park
