# Ansible playbooks


This directory contains ansible playbooks to (re-)deploy a cluster with the same architecture of the current CyVerseUK one.  
When adding a virtual machine to the HTCondor pool and/or to the Cyverse infrastructure edit the hosts file.

`cypool.yaml` will setup workers (assume they all have centOS7 as OS) to run condor and have the docker universe available. The submit machines are supposed to have centOS as well. It also setup crontab and watch service on the condor manager (internal docs).

`cypool_updates.yaml` can upgrade the condor version. It will now work if not using the official repository. Use the limit flag and choose which nodes to apply it to as it would kill any running jobs by default. It needs to have the version of HTCondor specified in the var file.

NOTES for future updates:

* don't write playbooks assuming condor workers host names. They may change. See internal EI documentation about this. If needed a solution is there as well.
  _expection now in docker.yaml as that is a single test machine ATM_
* manager is not up to date as it has to be re-registered. Consider this before running the playbook on the whole pool.
* for // universe add the following to the workers that should run parallel applications:

  ```DedicatedScheduler = "DedicatedScheduler@<exact host name as in condor_status --schedd>"
  STARTD_ATTRS = $(STARTD_ATTRS), DedicatedScheduler
  START          = True
  SUSPEND        = False
  CONTINUE       = True
  PREEMPT        = False
  KILL           = False
  WANT_SUSPEND   = False
  WANT_VACATE    = False
  RANK           = Scheduler =?= $(DedicatedScheduler)
  
  MPI_CONDOR_RSH_PATH = $(LIBEXEC)
  CONDOR_SSHD = /usr/sbin/sshd
  CONDOR_SSH_KEYGEN = /usr/bin/ssh-keygen
  ```  
  
  note this relies on the submit machine name. It's not in the playbook as it's now not needed.
