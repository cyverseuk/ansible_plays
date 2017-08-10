# Ansible playbooks


This directory contains ansible playbooks to (re-)deploy a cluster with the same architecture of the current CyVerseUK one.  
When adding a virtual machine to the HTCondor pool and/or to the Cyverse infrastructure edit the hosts file.

`cypool.yaml` will setup workers (assume they all have centOS7 as OS) to run condor and have the docker universe available. The submit machines are supposed to have centOS as well. It also setup crontab and watch service on the condor manager (internal docs).

`cypool_updates.yaml` can upgrade the condor version. It will now work if not using the official repository. Use the limit flag and choose which nodes to apply it to as it would kill any running jobs by default. It needs to have the version of HTCondor specified in the var file.
