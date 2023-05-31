---
id: 451
title: 'Glossary in Distributed TensorFlow'
date: '2016-08-26T19:52:36+00:00'
author: weiwen.web
layout: revision
guid: 'http://www.pittnuts.com/2016/08/444-revision-v1/'
permalink: '/?p=451'
---

In the figure, we take distributed deep learning as an example to explain the glossary of Client, Cluster, Job, Task, TensorFlow server, Master service, Worker Service in TensorFlow (TF).

[![TFramework](http://www.pittnuts.com/wp-content/uploads/2016/08/TFramework.png)](http://www.pittnuts.com/wp-content/uploads/2016/08/TFramework.png)

In the figure, the model parallelism within every model replica and data parallelism among replicas are adopted, for distributed deep learning. A example of mapping physical nodes to TensorFlow glossary is illustrated.

- The whole system is mapped to a TF cluster.
- Parameter servers are mapped to a job
- Each model replica is mapped to a job
- Each physical computing node is mapped to a task within its job
- Each task has a TF server, using “Master service” to communicate and coordinate works and using “Worker service” to compute designated operations in the TF graph by local devices.

## Official Explanation of [Glossary](https://www.tensorflow.org/versions/r0.10/how_tos/distributed/index.html) in TensorFlow:

<dl><dt>Client</dt><dd>A client is typically a program that builds a TensorFlow graph and constructs a `tensorflow::Session` to interact with a cluster. Clients are typically written in Python or C++. A single client process can directly interact with multiple TensorFlow servers (see “Replicated training” above), and a single server can serve multiple clients.</dd><dt>Cluster</dt><dd>A TensorFlow cluster comprises a one or more “jobs”, each divided into lists of one or more “tasks”. A cluster is typically dedicated to a particular high-level objective, such as training a neural network, using many machines in parallel. A cluster is defined by a `tf.train.ClusterSpec` object.</dd><dt>Job</dt><dd>A job comprises a list of “tasks”, which typically serve a common purpose. For example, a job named `ps` (for “parameter server”) typically hosts nodes that store and update variables; while a job named `worker` typically hosts stateless nodes that perform compute-intensive tasks. The tasks in a job typically run on different machines. The set of job roles is flexible: for example, a `worker` may maintain some state.</dd><dt>Master service</dt><dd>An RPC service that provides remote access to a set of distributed devices, and acts as a session target. The master service implements the `tensorflow::Session` interface, and is responsible for coordinating work across one or more “worker services”. All TensorFlow servers implement the master service.</dd><dt>Task</dt><dd>A task corresponds to a specific TensorFlow server, and typically corresponds to a single process. A task belongs to a particular “job” and is identified by its index within that job’s list of tasks.</dd><dt>TensorFlow server</dt><dd>A process running a `tf.train.Server` instance, which is a member of a cluster, and exports a “master service” and “worker service”.</dd><dt>Worker service</dt><dd>An RPC service that executes parts of a TensorFlow graph using its local devices. A worker service implements[`worker_service.proto`](https://www.tensorflow.org/code/tensorflow/core/protobuf/worker_service.proto). All TensorFlow servers implement the worker service.</dd></dl>