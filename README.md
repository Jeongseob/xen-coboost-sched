# xen-coboost-sched
This is a modified Xen credit scheduler to reflect the urgency of guest operating systems on scheduling virtual CPUs. When PAUSE-loop occurs in systems, the scheduler boosts the virtual CPU siblingsfor the target VM. 
