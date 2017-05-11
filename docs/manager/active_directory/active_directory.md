# Active Directory

## Questions

### Domain Controller Auto Discovery

In 2.12 GA release does the DC Auto Discovery work as designed? 
Has anyone received a case of the Auto Discovery not working as expected?
 
It is DNS-based, not sites and services aware. So it won't do anything fancy. It is mostly just keeping track of which DC's work and which ones don't.