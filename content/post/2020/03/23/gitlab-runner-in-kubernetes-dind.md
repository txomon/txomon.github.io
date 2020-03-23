---
title: "Gitlab Runner in Kubernetes Dind"
date: 2020-03-23T22:02:08+01:00
---

So today after a lot of fighting against the setup we have of gitlab runners in kubernetes, trying to get it to do dind properly, I was able to achieve a setup that wasn't convincing.

Learnt that:

* Gitlab runner helm chart is extremely hacky. Like crazy hacky. No idea who designed that but they have templates that come from config maps that feed from environment variables that come from secrets that create configuration files and copy them to the worker's workdir.

* Docker in docker has a really weird entrypoint that complicates things if you are trying to run without TLS but only listening to localhost (which is the best less worse option for k8s)

You can find the details at https://gitlab.com/gitlab-org/charts/gitlab-runner/-/issues/151
