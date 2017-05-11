# Manager UI

# Using the AppVolumes Manager UI

## Overview

## Dashboard

The dashboard contains three major sections: License utilization, usage graphs, and recently changed system objects.

## Volumes

Management of AppStacks and Writable volumes. Also contains views of all assignments, attachments, and applications.

## Directory

Management of system entities imported from ActiveDirectory such as Users, Computer, Groups, and Organization Units.

## Infrastructure

Management of infrastructure components. Storage locations and Storage groups.

## Activity

Audit and error logs.

## Configuration

Management of configuration, also used as the first-time configuration mode.

## Additional Capabilities

### Version

    GET http://your-manager/cv_api/version

Shows the current version - does not require authentication.

### Health Check

    GET http://your-manager/health-check

Shows some basic server information, can be used to ensure the service is up and running. Does not require authentication!

### Raw Log

    GET http://your-manager/log

Shows the Ruby on Rails server log. This log is a combination of logs from all internal application server processes including background workers and the power-watcher. Requires administrator to be logged in.

### Cleanup

    GET http://your-manager/cleanup

**WARNING** This is the nuclear option. Calling this url will remove all attachments from all known VMs. It also removes any orphaned assignments. Requires administrator to be logged in.

### Manager Threads

    GET http://your-manager/threads

This will show a debug screen showing the thread usage in the Manager. Remember that the Manager is internally load-balanced across many server processes, so you will only see thread information for one of the many of those processes. Calling this on each port, such as :3001, :3002, :3003, :3004, is needed to get the whole picture.

### System Environment

    GET http://your-manager/env

This will show system ENV information. Requires administrator to be logged in.