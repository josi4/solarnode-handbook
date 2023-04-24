# Job Scheduler

SolarNode provides a [Job Scheduler service][ManagedJobScheduler] that can automatically schedule and execute
jobs exported by plugins.

## Configuration

The Job Scheduler supports the following [configuration properties](../../users/configuration.md) in the `net.solarnetwork.node.core` namespace:

<div markdown="1" class="props-explicit-col-widths">

| Property | Default | Description |
|:---------|:--------|:------------|
| `jobScheduler.poolSize` | 10 | The number of threads to maintain in the job scheduler, and thus the maximum number of jobs that can run simultaneously. Must be set to 1 or higher. |
| `scheduler.startupDelay` | 180 | A delay in seconds after creating the job scheduler to start triggering jobs. This can be useful to give the application time to completely initialize before starting to run jobs. |

</div>

[ManagedJobScheduler]: https://javadoc.io/doc/net.solarnetwork.node/net.solarnetwork.node/latest/net/solarnetwork/node/runtime/ManagedJobScheduler.html
