# waitForUpdates()

Does AV Manager do any active state monitoring of VC? E.g. do you call waitForUpdates() to track VM or task state changes?

##Yes

Yeah, we have a separate instance of our app process that is not under nginx that monitors state changes (mostly power events).
 
It does a long poll to each vCenter with a waitForUpdates

We also do a waitForUpdates() for lists of ReconfigVM_Task IDs. Let's say we have a bunch of mounts happening around the same time, we take the task IDs we get back from vm.ReconfigVM_Task(...) and put them into a queue. A separate worker takes chunks from that queue and does a waitForUpdates([123,234,345,456,567,678,...])

--

Each app process (4 per VM) has up to 3 threads per host that can do the waitForUpdates() for mounts. The original idea being that if we need to wait on a bunch of task ids that we'd split it up and do them on a separate requests/connections.

The max would be: (app_processes*wait_worker_threads*vsphere_hosts) + (vsphere_hosts*power_watcher_processes)

Realistically, with 1 vCenter and 8 hosts:
 
ESX Mounting: (4*1*8) + (8*1) = ~45
vCenter Mounting: (4*1*1) + (1*1) = ~5