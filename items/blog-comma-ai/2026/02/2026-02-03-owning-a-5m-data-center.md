---
title: Owning a $5M data center
link: https://blog.comma.ai/datacenter/
source: blog-comma-ai
published: 2026-02-03T18:00:00Z
updated: 2026-02-03T18:00:00Z
first_seen: 2026-09-19T21:30:23.395188495Z
authors:
- Harald Schäfer
summary: These days it seems you need a trillion fake dollars, or lunch with politicians to get your own data center. They may help, but they’re not required. At comma we’ve been running our own data center for years. All of our model training, metrics, and data live in our …
content: extracted
html: 2026-02-03-owning-a-5m-data-center.html
preview:
  file: 2026-02-03-owning-a-5m-data-center.preview-6cce706ca99b.webp
  width: 256
  height: 80
  alt: data center power usage
  color: '#344151'
images:
- source: https://blog.comma.ai/img/datacenter/power.png
  original:
    file: 2026-02-03-owning-a-5m-data-center.image-f5ee754e19ee.png
    width: 2462
    height: 768
  color: '#222227'
- source: https://blog.comma.ai/img/datacenter/side_view2.png
  original:
    file: 2026-02-03-owning-a-5m-data-center.image-43fa3de35de4.png
    width: 1198
    height: 613
  variants:
  - file: 2026-02-03-owning-a-5m-data-center.image-3e2d3d077964.webp
    width: 320
    height: 164
  - file: 2026-02-03-owning-a-5m-data-center.image-6414dca83808.webp
    width: 640
    height: 327
  - file: 2026-02-03-owning-a-5m-data-center.image-4a8652aaaf9f.webp
    width: 960
    height: 491
  - file: 2026-02-03-owning-a-5m-data-center.image-5d26ddf38cee.webp
    width: 1198
    height: 613
  color: '#373736'
- source: https://blog.comma.ai/img/datacenter/air_cooling.jpg
  original:
    file: 2026-02-03-owning-a-5m-data-center.image-0fa6193a95f5.jpg
    width: 4032
    height: 3024
  color: '#878a88'
- source: https://blog.comma.ai/img/datacenter/breakers.jpg
  original:
    file: 2026-02-03-owning-a-5m-data-center.image-996f4e1bc893.jpg
    width: 4032
    height: 3024
  color: '#666966'
- source: https://blog.comma.ai/img/datacenter/mkv_machines.png
  original:
    file: 2026-02-03-owning-a-5m-data-center.image-1ffcd6261090.png
    width: 895
    height: 703
  variants:
  - file: 2026-02-03-owning-a-5m-data-center.image-2226631d461a.webp
    width: 320
    height: 251
  - file: 2026-02-03-owning-a-5m-data-center.image-84f4c8baf1e7.webp
    width: 640
    height: 503
  - file: 2026-02-03-owning-a-5m-data-center.image-8ff311489d24.webp
    width: 895
    height: 703
  color: '#585855'
- source: https://blog.comma.ai/img/datacenter/reporter.png
  original:
    file: 2026-02-03-owning-a-5m-data-center.image-b1e81f05fc74.png
    width: 2478
    height: 644
  color: '#fbfbfc'
- source: https://blog.comma.ai/img/datacenter/tbox2.jpg
  original:
    file: 2026-02-03-owning-a-5m-data-center.image-296bb6500e46.jpg
    width: 4032
    height: 3024
  color: '#494945'
- source: https://blog.comma.ai/img/datacenter/onpol.png
  original:
    file: 2026-02-03-owning-a-5m-data-center.image-d2ce8be52363.png
    width: 1654
    height: 905
  variants:
  - file: 2026-02-03-owning-a-5m-data-center.image-d38671436c21.webp
    width: 320
    height: 175
  - file: 2026-02-03-owning-a-5m-data-center.image-541ac3df382a.webp
    width: 640
    height: 350
  - file: 2026-02-03-owning-a-5m-data-center.image-8f25e22b9cb2.webp
    width: 960
    height: 525
  - file: 2026-02-03-owning-a-5m-data-center.image-6bb1eb143426.webp
    width: 1280
    height: 700
  - file: 2026-02-03-owning-a-5m-data-center.image-ec230f5d1ee8.webp
    width: 1654
    height: 905
  color: '#010101'
---

These days it seems you need a trillion fake dollars, or lunch with politicians to get your own data center. They may help, but they’re not required. At comma we’ve been running our own data center for years. All of our model training, metrics, and data live in our own data center in our own office. Having your own data center is cool, and in this blog post I will describe how ours works, so you can be inspired to have your own data center too.

![Our data center](https://blog.comma.ai/img/datacenter/side_view2.png)

Our data center

## Why no cloud?[Permalink](https://blog.comma.ai/datacenter/#why-no-cloud "Permalink")

If your business relies on compute, and you run that compute in the cloud, you are putting a lot of trust in your cloud provider. Cloud companies generally make onboarding very easy, and offboarding very difficult. If you are not vigilant you will sleepwalk into a situation of high cloud costs and no way out. If you want to control your own destiny, you must run your own compute.

Self-reliance is great, but there are other benefits to running your own compute. It inspires good engineering. Maintaining a data center is much more about solving real-world challenges. The cloud requires expertise in company-specific APIs and billing systems. A data center requires knowledge of Watts, bits, and FLOPs. I know which one I rather think about.

Avoiding the cloud for ML also creates better incentives for engineers. Engineers generally want to improve things. In ML many problems go away by just using more compute. In the cloud that means improvements are just a budget increase away. This locks you into inefficient and expensive solutions. Instead, when all you have available is your current compute, the quickest improvements are usually speeding up your code, or fixing fundamental issues.

Finally there’s cost, owning a data center can be far cheaper than renting in the cloud. Especially if your compute or storage needs are fairly consistent, which tends to be true if you are in the business of training or running models. In comma’s case I estimate we’ve spent ~5M on our data center, and we would have spent 25M+ had we done the same things in the cloud.

## What’s all needed?[Permalink](https://blog.comma.ai/datacenter/#whats-all-needed "Permalink")

Our data center is pretty simple. It’s maintained and built by only a couple engineers and technicians. Your needs may be slightly different, our implementation should provide useful context.

### Power [Permalink](https://blog.comma.ai/datacenter/#power "Permalink")

To run servers you need power. We currently use about 450kW at max. Operating a data center exposes you to many fun engineering challenges, but procuring power is not one of them. San Diego power cost is over 40c/kWh, ~3x the global average. It’s a ripoff, and overpriced simply due to political dysfunction. We spent $540,112 on power in 2025, a big part of the data center cost. In a future blog post I hope I can tell you about how we produce our own power and you should too.

![data center power usage](https://blog.comma.ai/img/datacenter/power.png)

data center power usage

### Cooling [Permalink](https://blog.comma.ai/datacenter/#cooling "Permalink")

Data centers need cool dry air. Typically this is achieved with a CRAC system, but they are power-hungry. San Diego has a mild climate and we opted for pure outside air cooling. This gives us less control of the temperature and humidity, but uses only a couple dozen kW. We have dual 48" intake fans and dual 48" exhaust fans to keep the air cool. To ensure low humidity (<45%) we use recirculating fans to mix hot exhaust air with the intake air. One server is connected to several sensors and runs a PID loop to control the fans to optimize the temperature and humidity.

![Filtered intake fan on right, 2 recirculating fans at the top](https://blog.comma.ai/img/datacenter/air_cooling.jpg)

Filtered intake fan on right, 2 recirculating fans at the top

### Servers [Permalink](https://blog.comma.ai/datacenter/#servers "Permalink")

The majority of our current compute is 600 GPUs in 75 [TinyBox Pro machines](https://tinycorp.myshopify.com/products/tinybox-pro-v2). They were built in-house, which saves us money and ensures they suit our needs. Our self-built machines fail at a similar rate to pre-built machines we’ve bought, but we’re capable of fixing them ourselves quickly. They have 2 CPUs and 8 GPUs each, and work as both training machines and general compute workers.

![Breaker panels for all the computers, that’s a lot of breakers!](https://blog.comma.ai/img/datacenter/breakers.jpg)

Breaker panels for all the computers, that’s a lot of breakers!

For data storage we have a few racks of Dell machines (R630 and R730). They are filled with SSDs for a total of ~4PB of storage. We use SSDs for reliability and speed. Our main storage arrays have no redundancy and each node needs to be able to saturate the network bandwidth with random access reads. For the storage machines this means reading up to 20Gbps of each 80TB chunk.

Other than storage and compute machines we have several one-off machines to run services. This includes a router, climate controller, data ingestion machine, storage master servers, metric servers, redis servers, and a few more.

Running the network requires switches, but at this scale we don’t need to bother with complicated switch topologies. We have 3 100Gbps interconnected Z9264F switches, which serve as the main ethernet network. We have two more infiniband switches to interconnect the 2 tinybox pro groups for training all-reduce.

### The software [Permalink](https://blog.comma.ai/datacenter/#the-software "Permalink")

To effectively use all these compute and storage machines you need some infra. At this scale, services don’t need redundancy to achieve 99% uptime. We use a single master for all services, which makes things pretty simple.

##### Setup [Permalink](https://blog.comma.ai/datacenter/#setup "Permalink")

All servers get ubuntu installed with pxeboot and are managed by [salt](https://github.com/saltstack/salt).

##### Distributed storage: minikeyvalue [Permalink](https://blog.comma.ai/datacenter/#distributed-storage-minikeyvalue "Permalink")

All of our storage arrays use [mkv](https://github.com/commaai/minikeyvalue/tree/prod). The main array is 3PB of non-redundant storage hosting our driving data we train on. We can read from this array at ~1TB/s, which means we can train directly on the raw data without caching. Redundancy is not needed since no specific data is critical.

![Storage nodes](https://blog.comma.ai/img/datacenter/mkv_machines.png)

Storage nodes

We have an additional ~300TB non-redundant array to cache intermediate processed results. And lastly, we have a redundant mkv storage array to store all of our trained models and training metrics. Each of these 3 arrays have a separate single master server.

##### Workload management: slurm [Permalink](https://blog.comma.ai/datacenter/#workload-management-slurm "Permalink")

We use slurm to manage the compute nodes, and compute jobs. We schedule two types of distributed compute. Pytorch training jobs, and miniray workers.

##### Distributed training: pytorch [Permalink](https://blog.comma.ai/datacenter/#distributed-training-pytorch "Permalink")

To train models across multiple GPU nodes we use `torch.distributed` FSDP. We have 2 separate training partitions, each intra-connected with Infiniband for training across machines. We wrote our own training framework which handles the training loop boilerplate, but it’s mostly just pytorch.

![reporter; comma’s experiment tracking service](https://blog.comma.ai/img/datacenter/reporter.png)

reporter; comma’s experiment tracking service

We have a custom model experiment tracking service (similar to wandb or tensorboard). It provides a dashboard for tracking experiments, and shows custom metrics and reports. It is also the interface for the mkv storage array that hosts the model weights. The training runs store the model weights there with a uuid, and they are available to download for whoever needs to run them. The metrics and reports for our latest models [are also open](https://commaai.github.io/model_reports/).

##### Distributed compute: miniray [Permalink](https://blog.comma.ai/datacenter/#distributed-compute-miniray "Permalink")

Besides training we have many other compute tasks. This can be anything from running tests, running models, pre-processing data, or even running agent rollouts for on-policy training. We wrote a lightweight [open-source task scheduler called miniray](https://github.com/commaai/miniray) that allows you to run arbitrary python code on idle machines. This is a simpler version of dask, with a focus on extreme simplicity. Slurm will schedule any idle machine to be an active miniray worker, and accept pending tasks. All the task information is hosted in a central redis server.

![Our main training/compute machines. Notice the 400Gbps switch in the center.](https://blog.comma.ai/img/datacenter/tbox2.jpg)

Our main training/compute machines. Notice the 400Gbps switch in the center.

Miniray workers with GPUs will spin up a [triton inference server](https://github.com/triton-inference-server/server) to run model inference with dynamic batching. A miniray worker can thus easily and efficiently run any of the models hosted in the model mkv storage array.

Miniray makes it extremely easy to scale parallel tasks to hundreds of machines. For example, the [controls challenge record](https://comma.ai/leaderboard) was set by just having ~1hr of access to our data center with miniray.

##### Code NFS monorepo [Permalink](https://blog.comma.ai/datacenter/#code-nfs-monorepo "Permalink")

All our code is in a monorepo that we have cloned on our workstations. This monorepo is kept small (<3GB), so it can easily be copied around. When a training job or miniray distributed job is started on any workstation, the local monorepo is cached on a shared NFS drive including all the local changes. Training jobs and miniray tasks are pointed towards this cache, such that all distributed work uses the exact codebase you have locally. Even all the python packages are identical, UV on the worker/trainer syncs the packages specified in the monorepo before starting any work. This entire process of copying your entire local codebase and syncing all the packages takes only ~2s, and is well worth it to prevent the issues mismatches can cause.

## All together now [Permalink](https://blog.comma.ai/datacenter/#all-together-now "Permalink")

The most complex thing we do at comma is train driving models on-policy, these training runs require training data to be generated during training by running simulated driving rollouts with the most recent model weights. Here’s a real-world command we just used to train such a model. This training run uses all of the infrastructure described above. While only this small command is needed to kick everything off, it orchestrates a lot of moving parts.

```
./training/train.sh N=4 partition=tbox2 trainer=mlsimdriving dataset=/home/batman/xx/datasets/lists/train_500k_20250717.txt vision_model=8d4e28c7-7078-4caf-ac7d-d0e41255c3d4/500 data.shuffle_size=125k optim.scheduler=COSINE bs=4
```

![Diagram of all infrastructure involved in training an on-policy driving model.](https://blog.comma.ai/img/datacenter/onpol.png)

Diagram of all infrastructure involved in training an on-policy driving model.

## Like this stuff?[Permalink](https://blog.comma.ai/datacenter/#like-this-stuff "Permalink")

Does all this stuff sound exciting? Then build your own datacenter for yourself or your company! [You can also come work here.](https://comma.ai/jobs)

*Harald Schäfer*\
 *CTO @ [comma.ai](https://comma.ai)*
