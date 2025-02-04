---
type: docs
title: "Dapr Scheduler control plane service overview"
linkTitle: "Scheduler"
description: "Overview of the Dapr scheduler service"
---

The Dapr Scheduler service is used to schedule different types of jobs, running in [self-hosted mode]({{< ref self-hosted >}}) or on [Kubernetes]({{< ref kubernetes >}}). 
- Jobs created through the Jobs API
- Actor reminder jobs (used by the Actor Reminders feature)
- Actor reminder jobs created by the Workflow API (which uses Actor Reminders under the hood)

As of Dapr v1.15, the Scheduler service is used by default to schedule actor reminders as well as actor reminders under the hood for the Workflow API. All of these jobs are tracked by the Scheduler service and stored in an embedded etcd database. 

There is no concept of a leader Scheduler instance. All Scheduler service replicas are considered peers. All receive jobs to be scheduled for execution and the jobs are divvied up between the available Scheduler service replicas for trigger load balancing.

The diagram below shows how the Scheduler service is used via the jobs API when called from your application. All the jobs that are tracked by the Scheduler service are stored in an embedded etcd database. 

<img src="/images/scheduler/scheduler-architecture.png" alt="Diagram showing the Scheduler control plane service and the jobs API">

## Actor reminders

Prior to Dapr v1.15, [actor reminders]({{< ref "actors-timers-reminders.md#actor-reminders" >}}) were run using the Placement service. Now, by default, the [`SchedulerReminders` feature flag]({{< ref "support-preview-features.md#current-preview-features" >}}) is set to `true`, and all new actor reminders you create are run using the Scheduler service to make them more scalable.

When you deploy Dapr v1.15, any _existing_ actor reminders are automatically migrated from the Placement service to the Scheduler service as a one time operation for each actor type. There will be _no_ loss of reminder triggers during the migration. However, you can prevent this migration and keep the existing actor reminders running using the Placement service by setting the `SchedulerReminders` flag to `false` in application configuration file for the actor type.

## Job Triggering

### Job Ordering

When the Scheduler service triggers a job there is no guarantee of job trigger ordering, meaning we do not guarantee FIFO or LIFO trigger ordering. 

### Job Failure Policy and Staging Queue

When the Scheduler service triggers a job and it has a client side error, with the failure policy, the job is retried by default with a 1s interval and 3 maximum retries. Failure policy can be configured for a consistent retry or to drop a job. Actor reminder type jobs will retry forever until successful completion. Workflow reminders are oneshot, meaning they will only trigger once successfully with a 1 second retry interval.

For non-client side errors, for example, when a job cannot be sent to an available Dapr sidecar at trigger time, it is placed in a staging queue within the Scheduler service. Jobs remain in this queue until a suitable sidecar instance becomes available, at which point they are automatically sent to the appropriate Dapr sidecar instance. Jobs are sent back to a single replica for the same app ID that scheduled the job in a round robin manner.

## Self-hosted mode

The Scheduler service Docker container is started automatically as part of `dapr init`. It can also be run manually as a process if you are running in [slim-init mode]({{< ref self-hosted-no-docker.md >}}).

## Kubernetes mode

The Scheduler service is deployed as part of `dapr init -k`, or via the Dapr Helm charts. You can run Scheduler in high availability (HA) mode. [Learn more about setting HA mode in your Kubernetes service.]({{< ref "kubernetes-production.md#individual-service-ha-helm-configuration" >}})

When a Kubernetes namespace is cleaned up, all the jobs corresponding to that namespace are also cleaned up preventing unnecessary resource and memory usage in the embedded etcd.

## Disabling the Scheduler service

If you are not using any features that require the Scheduler service (Jobs API, Actor Reminders, or Workflows), you can disable it by setting `global.scheduler.enabled=false`.

For more information on running Dapr on Kubernetes, visit the [Kubernetes hosting page]({{< ref kubernetes >}}).

## Related links

[Learn more about the Jobs API.]({{< ref jobs_api.md >}})