---
layout: post

type: "article"

title: "Advanced scheduling of experiments and jobs on Polyaxon"
subtitle: "Advanced scheduling of experiments and jobs on Polyaxon."
cover_image: posts/polyaxon_logo.png
cover_image_caption: ""

excerpt: "Advanced scheduling of experiments and jobs on Polyaxon."

author:
  name: Mourad Mourafiq.
  twitter: mmourafiq
  bio: Maths, Technology, Philosophy, Startups, ...
  image: logo.png
---
For some advanced use cases, users might need to have more control over where specific [Polyaxon](https://github.com/polyaxon/polyaxon) jobs should be deployed on their clusters.

Polyaxon provides a list of options to select which nodes should be used for the core platform, for the dependencies, and for the experiments. These options are [Node selectors](https://kubernetes.io/docs/concepts/configuration/assign-pod-node/#nodeselector), [Tolerations](https://kubernetes.io/docs/concepts/configuration/taint-and-toleration/), and [Affinity](https://kubernetes.io/docs/concepts/configuration/assign-pod-node/#affinity-and-anti-affinity).


[Read more](https://medium.com/polyaxon/advanced-scheduling-of-experiments-and-jobs-on-polyaxon-c0fa071e2ae2)
