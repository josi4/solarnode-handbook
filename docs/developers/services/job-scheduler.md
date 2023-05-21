# Job Scheduler

SolarNode provides a [ManagedJobScheduler][ManagedJobScheduler] service that can automatically execute
jobs exported by plugins that have user-defined schedules.

!!! tip "The Job Scheduler uses the Task Scheduler"

	The Job Scheduler service uses the [Task Scheduler](task-scheduler.md) internally, which means
	the number of jobs that can execute simultaneously will be limited by its thread pool
	configuration.

## Managed Jobs

Any plugin simply needs to register a [ManagedJob][ManagedJob] service for the Job Scheduler to
automatically schedule and execute the job. The schedule is provided by the [`getSchedle()`][getSchedule]
method, which can return a [cron expression][cron] or a plain number representing a millisecond period.

## Job

The `ManagedJob` API delegates the actual task work to a [`JobService`][JobService] API. The `executeJobService()`
method will be invoked when the job executes.

## Example Managed Job

Let's imagine you have a `com.example.Job` class that you would like to allow users to schedule. Your
class would implement the `JobService` interface, and then you would provide a localized messages
properties file and configure the service using [OSGi Blueprint](../osgi/blueprint.md).

=== "com.example.Job.java"

	```java
	package com.example;

	import java.util.Collections;
	import java.util.List;
	import net.solarnetwork.node.job.JobService;
	import net.solarnetwork.node.service.support.BaseIdentifiable;
	import net.solarnetwork.settings.SettingSpecifier;

	/**
	 * My super-duper job.
	 */
	public class Job exetnds BaseIdentifiable implements JobService {
		@Override
		public String getSettingUid() {
			return "com.example.job"; // (1)!
		}

		@Override
		public List<SettingSpecifier> getSettingSpecifiers() {
			return Collections.emptyList(); // (2)!
		}

		@Override
		public void executeJobService() throws Exception {
			// do great stuff here!
		}
	}
	```

	1. The setting UID will be configured in the Blueprint XML as well.
	2. The `SimpleManagedJob` class we'll configure in Blueprint XML will automatically
	   add a `schedule` setting to configure the job schedule.

=== "com.example.Job.properties"

	```properties
	title = Super-duper Job
	desc = This job does it all.

	schedule.key = Schedule
	schedule.desc = The schedule to execute the job at. \
		Can be either a number representing a frequency in <b>milliseconds</b> \
		or a <a href="{0}">cron expression</a>, for example <code>0 * * * * *</code>.
	```

=== "OSGi Blueprint"

	```xml
	<service interface="net.solarnetwork.node.job.ManagedJob"><!-- (1)! -->
		<service-properties>
			<entry key="service.pid" value="com.example.job"/>
		</service-properties>
		<bean class="net.solarnetwork.node.job.SimpleManagedJob"><!-- (2)! -->
			<argument>
				<bean class="com.example.Job">
					<property name="uid" value="com.example.job"/><!-- (3)! -->
					<property name="messageSource">
	                    <bean class="org.springframework.context.support.ResourceBundleMessageSource">
	                        <property name="basenames" value="com.example.Job"/>
	                    </bean>
	                </property>
				</bean>
			</argument>
			<property name="schedule" value="0 * * * * *"/>
		</bean>
	</service>
	```

	1. This registers a `ManagedJob` service with the SolarNode runtime.
	2. The `SimpleManagedJob` class is a handy `ManagedJob` implementation. It adds a `schedule`
	   setting to any settings returned by the `JobService`.
	3. The `uid` value should match the `service.pid` used earlier, which matches the value
	   returned by the `getSettingUid()` method in the `Job` class.

When this plugin is deployed in SolarNode, the component will appear on the main [Settings
page](../../users/setup-app/settings/index.md) and offer a configurable **Schedule** setting,
like this:

![Example job in Settings UI](../../images/developers/services/example-managed-job-settings-ui.png){width=658}

[cron]: https://github.com/SolarNetwork/solarnetwork/wiki/SolarNode-Cron-Job-Syntax
[getSchedule]: https://javadoc.io/doc/net.solarnetwork.node/net.solarnetwork.node/latest/net/solarnetwork/node/job/ManagedJob.html#getSchedule()
[JobService]: https://javadoc.io/doc/net.solarnetwork.node/net.solarnetwork.node/latest/net/solarnetwork/node/job/JobService.html
[ManagedJob]: https://javadoc.io/doc/net.solarnetwork.node/net.solarnetwork.node/latest/net/solarnetwork/node/job/ManagedJob.html
[ManagedJobScheduler]: https://javadoc.io/doc/net.solarnetwork.node/net.solarnetwork.node/latest/net/solarnetwork/node/runtime/ManagedJobScheduler.html
