# Glossary

## AppStack

A portable hard disk volume (usually a .vmdk or .vhd) that contains one or more applications. These at attached/mounted to virtual machines to instantly deliver applications.

## Writable Volumes

A portable hard disk volume (usually a .vmdk or .vhd) that can persist data. The type of data and eligible directory locations are determined by the template used when a writable is created.

## Storage Group

A logical grouping of hypervisor (usually vCenter/ESX datastores) storage locations/devices. This concept is mostly used when creating writable volumes. Rather than choosing a single storage location as the target destination for a writable, the storage group can be used which can contain any number of storage locations (datastores). Multiple strategies can be used to determine how items are distributed amoungst the group.
This concept also works for other hypervisor types too.

## VM

Virtual Machine

## AD

ActiveDirectory

## Administrator Group

...

## Power Watcher

This is a application server process that is not receive http requests. Instead this process maintains a connection to all configured vCenters. This connection performs a [long-poll](https://en.wikipedia.org/wiki/Push_technology#Long_polling) request using vCenter vSphere API to monitor power changes on all virtual machines.

## Background Job

Background jobs are processed by one or more background workers. These background workers are additional application server (Ruby on Rails) process that do not receive http requests. Instead they only process jobs from a queue stored in the database.