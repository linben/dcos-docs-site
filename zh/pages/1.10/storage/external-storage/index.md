---
layout: layout.pug
navigationTitle: 外部持久化卷
title: 外部持久化卷
menuWeight: 20
excerpt: ""
experimental: true
enterprise: false
---
<!-- This source repo for this topic is https://github.com/dcos/dcos-docs -->

**Warning:** Volume size is specified in GiB.

当容错对您的应用程序至关重要时, 请使用外部卷。如果主机出现故障, 本机马拉松实例将您的应用程序重新在另一台主机上, 以及与其相关的数据, 而无需用户干预。 外部卷通常还提供更大的存储空间。

马拉松应用程序在终止和重新运行时通常会失去状态。 在某些情况下, 例如, 如果您的应用程序使用 MySQL, 您将希望应用程序保留其状态。 You can use an external storage service, such as Amazon's Elastic Block Store (EBS), to create a persistent volume that follows your application instance.

# Create an application with an external persistent volume

## Create an application with a Marathon app definition

You can specify an external volume in your [Marathon app definition](/1.10/deploying-services/creating-services/).

### Using the Universal Container Runtime

The `cmd` in this app definition appends the output of the `date` command to `test.txt`. You can verify that the external volume is being used correctly if you see that the logs of successive runs of the application show more and more lines of `date` output.

```json
{
  "id": "hello",
  "instances": 1,
  "cpus": 0.1,
  "mem": 32,
  "cmd": "date >> test-rexray-volume/test.txt; cat test-rexray-volume/test.txt",
  "container": {
    "type": "MESOS",
    "volumes": [
      {
        "containerPath": "test-rexray-volume",
        "external": {
          "size": 100,
          "name": "my-test-vol",
          "provider": "dvdi",
          "options": { "dvdi/driver": "rexray" }
          },
        "mode": "RW"
      }
    ]
  },
  "upgradeStrategy": {
    "minimumHealthCapacity": 0,
    "maximumOverCapacity": 0
  }
}
```

#### Volume configuration options

* `containerPath`: The path where the volume is mounted inside the container. For Mesos external volumes, this must be a single-level path relative to the container; it cannot contain a forward slash (`/`). For more information, see [the REX-Ray documentation on data directories](https://rexray.readthedocs.io/en/v0.9.0/user-guide/config/#data-directories).
* `mode`: The access mode of the volume. Currently, `"RW"` is the only possible value and will let your application read from and write to the volume.
* `external.size`: The size of the volume in **GiB**.
* `external.name`: The name that your volume driver uses to look up your volume. When your task is staged on an agent, the volume driver queries the storage service for a volume with this name. If one does not exist, it is [created implicitly](#implicit-vol). Otherwise, the existing volume is reused.
* `external.options["dvdi/driver"]`: which Docker volume driver to use for storage. The only Docker volume driver provided with DC/OS is `rexray`. Learn more about [REX-Ray](https://rexray.readthedocs.io/en/v0.9.0/user-guide/schedulers/).
* You can specify additional options with `container.volumes[x].external.options[optionName]`. The dvdi provider for Mesos containers uses `dvdcli`, which offers the following [options](https://github.com/emccode/dvdcli#extra-options). The availability of any option depends on your volume driver.
* Create multiple volumes by adding additional items in the `container.volumes` array.
* You cannot change volume parameters after you create the application.
* Marathon will not launch apps with external volumes if `upgradeStrategy.minimumHealthCapacity` is greater than 0.5, or if `upgradeStrategy.maximumOverCapacity` does not equal 0.

### Using a Docker Engine

Below is a sample app definition that uses a Docker Engine and specifies an external volume. The `cmd` in this app definition appends the output of the `date` command to `test.txt`. You can verify that the external volume is being used correctly if you see that the logs of successive runs of the application show more and more lines of `date` output.

```json
{
  "id": "/test-docker",
  "instances": 1,
  "cpus": 0.1,
  "mem": 32,
  "cmd": "date >> /data/test-rexray-volume/test.txt; cat /data/test-rexray-volume/test.txt",
  "container": {
    "type": "DOCKER",
    "docker": {
      "image": "alpine:3.1",
      "network": "HOST",
      "forcePullImage": true
    },
    "volumes": [
      {
        "containerPath": "/data/test-rexray-volume",
        "external": {
          "name": "my-test-vol",
          "provider": "dvdi",
          "options": { "dvdi/driver": "rexray" }
        },
        "mode": "RW"
      }
    ]
  },
  "upgradeStrategy": {
    "minimumHealthCapacity": 0,
    "maximumOverCapacity": 0
  }
}
```

#### Volume configuration options

* `containerPath` must be absolute. 
* Only certain versions of Docker are compatible with the REX-Ray volume driver. Refer to the [REX-Ray documentation](https://rexray.readthedocs.io/en/v0.9.0/user-guide/schedulers/#docker-containerizer-with-marathon).

## Create an application from the DC/OS web interface

1. Click the **Services** tab, then **RUN A SERVICE**.
2. If you are using a Docker container, click **Container Settings** and configure your container runtime.
3. Click **Volumes** and enter your Volume Name and Container Path.
4. Click **Deploy**.

<a name="implicit-vol"></a>

# Implicit volumes

The default implicit volume size is 16 GiB. If you are using the Universal Container Runtime, you can modify this default for a particular volume by setting `volumes[x].external.size`. You cannot modify this default for a particular volume if you are using the Docker Engine. For both runtimes, however, you can modify the default size for all implicit volumes by modifying the [REX-Ray configuration](https://rexray.readthedocs.io/en/v0.9.0/user-guide/config/).

# Scaling your app

Apps that use external volumes can only be scaled to a single instance because a volume can only attach to a single task at a time.

If you scale your app down to 0 instances, the volume is detached from the agent where it was mounted, but it is not deleted. If you scale your app up again, the data that was associated with it is still be available.

# Potential pitfalls

* You can assign only one task per volume. Your storage provider might have other limitations.
* The volumes you create are not automatically cleaned up. If you delete your cluster, you must go to your storage provider and delete the volumes you no longer need. If you're using EBS, find them by searching by the `container.volumes.external.name` that you set in your Marathon app definition. This name corresponds to an EBS volume `Name` tag.
* Volumes are namespaced by their storage provider. If you're using EBS, volumes created on the same AWS account share a namespace. Choose unique volume names to avoid conflicts.
* If you are using Docker, you must use a compatible Docker version. Refer to the [REX-Ray documentation](https://rexray.readthedocs.io/en/v0.9.0/user-guide/schedulers/#docker-containerizer-with-marathon) to learn which versions of Docker are compatible with the REX-Ray volume driver.
* If you are using Amazon's EBS, it is possible to create clusters in different availability zones (AZs). If you create a cluster with an external volume in one AZ and destroy it, a new cluster may not have access to that external volume because it could be in a different AZ.
* Launch time might increase for applications that create volumes implicitly. The amount of the increase depends on several factors which include the size and type of the volume. Your storage provider's method of handling volumes can also influence launch time for implicitly created volumes.
* For troubleshooting external volumes, consult the agent or system logs. If you are using REX-Ray on DC/OS, you can also consult the systemd journal.