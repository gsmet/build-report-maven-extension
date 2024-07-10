{#if report.cancelled}
> [!NOTE]
> :no_entry_sign: This build has been cancelled.

{/if}
{#if report.failure && !report.jobsFailing}
> [!CAUTION]
> This build has failed but no jobs reported an error. Something weird happened, please check [the workflow run page]({report.workflowRunUrl}) carefully.

{/if}
{#if report.jobsFailing}
## <a id="build-summary-top"></a>Failing Jobs - Building {report.sha} - [Back to {workflowContext.type}]({workflowContext.htmlUrl})

{#if !artifactsAvailable && !report.cancelled}
> [!WARNING]
> Artifacts of the workflow run were not available thus the report misses some details.

{/if}

| Status | Name | Step | Failures | Logs | Raw logs | Build scan |
| :-:  | --  | --  | :-:  | :-:  | :-:  | :-:  |
{#for job in report.jobs}
{#if workflowReportJobIncludeStrategy.include(report, job)}
| {job.conclusionEmoji} | {job.name} | {#if job.failingStep}`{job.failingStep}`{/if} | {#if job.reportedFailures}[Failures](#user-content-{job.failuresAnchor}){#else if job.failing}:warning: Check →{/if} | {#if job.url}[Logs]({job.url}){/if} | {#if job.rawLogsUrl}[Raw logs]({job.rawLogsUrl}){/if} | {#if job.gradleBuildScanUrl}[:mag:]({job.gradleBuildScanUrl}){#else}:construction:{/if}
{/if}
{/for}
{/if}

{#if report.flakyTests}
> [!WARNING]
> This workflow run had flaky tests. See details for more information.

{/if}