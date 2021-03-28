---
title: "Prefect architecture overview"
date: 2021-03-28T12:02:08+01:00
draft: true
---

# Prefect overview

I have been looking for an Airflow replacement for a while. The main reasons are:

* Airflow is constrained on per-date execution

* The core code quality is not very high, and the operators are very low quality, usually
  buggy, making me recreate them every time.

Although the project has been improving a lot in the recent years, I'm still not very
satisfied, hence when I found [Prefect](https://prefect.io) I was pretty happy to try it
out.

I don't have extensive knowledge of the architecture, so bear in mind the information
might be from unnaccurate to just wrong.

## Definitions

There are a few terms that need to be defined before jumping into explanations:

* Task (Airflow's Operator): A step within a Flow.

* Flow (Airflow's Dag): A combination of tasks that acts as a single scheduling unit

* [Flow Run](https://docs.prefect.io/orchestration/concepts/flow_runs.html#creating-a-flow-run)
  (Airflow's DagRun): A specific flow instance with a specific set of parameters.

* Backend (part of Airflow's Scheduler): A service that receives prefect pickled flows
  and is in charge of making them available to the Agents. It's a combination of
  different services AKA Server, currently Prefect Cloud (as a service) and Prefect
  Server (open source) exist.

    * UI (Airflow's UI): Interface to monitor and do some limited actions on the Backend.
      It's main focus is to give an overview of the current state.

    * Apollo (Airflow doesn't have analogous): API endpoint to interact with the server,
      works as a proxy to the rest of the Backend services.

    * Hasura (Airflow doesn't have analogous): GraphQL API on top of postgres.

    * GraphQL (Airflow doesn't have analogous): (???) Server business logic that exposes
      GraphQL mutations

    * Towel: Set of auxiliary services for server maintenance

        * Scheduler (Airflow's scheduler): In charge of creating new flow runs according
          to schedule.

        * Zombie Killer: (Airflow doesn't have analogous): In charge of removing any task
          that is still running when they failed heartbeat

        * Lazarus: (part of Airflow scheduler): Reschedules flow runs that maintain a
          unusual state for longer than they should

* Agent (part of Airflow's worker): A running service that is in charge of getting tasks
  from the Backend and making sure they run, using an Executor. AKA Runner in some docs,
  currently Local, Docker, Kubernetes, ECS and Fargate exist.

* Run Config (part of Airflow's worker): Python API that specifies how a whole flow
  should be executed. Currently UniversalRun, LocalRun, DockerRun, KubernetesRun and
  ECSRun exist

* Executor (part of Arflow's worker): Python API that specifies how each of the tasks of
  the flow should be executed. Currently LocalExecutor, LocalDaskExecutor and
  DaskExecutor exist.

### Relations between terms

Trying to clear up my understanding around the different concepts there are, I wrote down
a few phrases trying to get concise connections:

* A Flow is composed of Tasks.

* A Flow can be submitted pickled to the Backend through Apollo.

* Apollo connects to Hasura's GraphQL interface.

* Hasura exposes a GraphQL interface on top of PostgreSQL

* A user can check the UI to see the Flows.

* The UI connects to the Backend through Apollo

* A Flow can have a schedule attached which is stored in Postgres

* Scheduler checks for the Schedules in Postgres and creates new FlowRuns

* FlowRuns can be also created manually by the user

* FlowRuns are stored in Postgres

* A Flow can have a RunConfig attached which is stored in Postgres

* GraphQL makes sure to only make available compatible FlowRuns to specific Agents by
  making sure RunConfigs are compatible.

* Agents pull FlowRuns from Apollo

* Agents read FlowRuns and create or connect to Executors to run each of the tasks

* Agents using LocalExecutor will run themselves the workload

* Agents using LocalDaskExecutor will spin up a Dask instance with workers/threads to run
  the different tasks in parallel

* Agents using DaskExecutor will spin up a Dask cluster wherever specified (Kubernetes,
  Fargate), etc. And then send the tasks to happen in that cluster

* Agents using DaskExecutor which given an existing cluster address will connect to it
  and send the different tasks to run

* When using DaskExecutor, the supplied image should contain the same version of prefect
  as in the Agent, and any additional libraries that the code may use.

* Native python functions decorated with `@task` will always be coupled to prefect
  dependencies (example, when using kubernetes python library, prefect pinned kubernetes
  version needs to be used)

## Architecture
