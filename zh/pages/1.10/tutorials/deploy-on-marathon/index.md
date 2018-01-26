---
layout: layout.pug
navigationTitle: 与Jenkins部署Marathon应用程序
title: 与Jenkins部署Marathon应用程序
menuWeight: 5
excerpt: ""
enterprise: false
---
本教程演示如何使用Jenkins for DC / OS在[ Marathon ](https://mesosphere.github.io/marathon/)上部署应用程序。 We'll walk you through creating a new Jenkins job, publishing a Docker container on source code changes, and deploying those changes to Marathon based on the [application definition](https://mesosphere.github.io/marathon/docs/application-basics.html) contained in the project’s `marathon.json` file.

<table class="table" bgcolor="#FAFAFA"> <tr> <td style="border-left: thin solid; border-top: thin solid; border-bottom: thin solid;border-right: thin solid;"><b>重要提示：</b> Mesosphere不支持本教程，相关脚本或命令，它们不提供任何形式的担保。 本教程的目的是演示功能，可能不适合在生产环境中使用。 在您的环境中使用类似的解决方案之前，您必须进行调整，验证和测试。</td> </tr> </table>

的**先决条件：** 本教程假定您有一个正在工作的Jenkins安装和在Marathon上启动应用程序的权限。 针对DC / OS的Jenkins必须按照[ Jenkins快速入门](/services/jenkins/quickstart/)页面中的说明进行安装。

# 示例项目

本教程中使用的项目取自[ cd-demo ](https://github.com/mesosphere/cd-demo)存储库，并在Docker容器内运行Jekyll网站。

本教程所需的文件是` Dockerfile `，` conf/cd-demo-appn.json `和` site `目录。 Copy those items to a new project and push to a new Git repository on the host of your choice.

This tutorial uses [Docker Hub](https://hub.docker.com/) to store the created image and requires account information to perform this task.

# Accessing Jenkins for DC/OS

Jenkins for DC/OS can be accessed through the Dashboard or Services navigation menu’s within the [DC/OS web interface](/1.10/gui/).

Click the “Jenkins” service and then "Open Service" to access the Jenkins web interface.

![dcos-velocity-jenkins-ui.png](/1.10/img/dcos-velocity-jenkins-ui.png)

# Adding Docker Hub Credentials

Jenkins stores account credentials within its Credential Store, which allows jobs to utilize credentials in a secure manner. From the main Jenkins page, click "Credentials" from the left-hand menu. From there, select "System" (also from the left-hand menu) and finally the "Global credentials (unrestricted)" link presented in the main viewing area. The left-hand menu should now have an "Add Credentials" option.

Click "Add Credentials" to create a new credential for Docker Hub. The "Kind" drop-down should have the "Username with password" option selected. Fill out the rest of the information to match your Docker Hub account.

![dcos-velocity-jenkins-creds-new.png](/1.10/img/dcos-velocity-jenkins-creds-new.png)

# The Job

We’ll create a new Jenkins job that performs several operations with Docker Hub and then either update or create a Marathon application.

Create a new **Freestyle** job with a name that includes only lowercase letters and hyphens. This name will be used later in the Docker image name and possibly as the Marathon application ID.

![dcos-jenkins-new-freestyle.png](/1.10/img/dcos-jenkins-new-freestyle.png)

# SCM / Git

From the **Example Project** section above, fill in the Git repository URL with the newly created Git repository. This must be accessible to Jenkins and may require adding credentials to the Jenkins instance.

![dcos-jenkins-repourl.png](/1.10/img/dcos-jenkins-repourl.png)

# Build Triggers

Select the **Poll SCM** build trigger with a schedule of: `*/5 * * * *`. This will check the Git repository every five minutes for changes.

# Build Steps

The Jenkins job performs these actions:

1. Build a new Docker image.
2. Push the new image to Docker Hub.

These steps can be performed by a single build step using the **Docker Build and Publish** plugin, which is already included and ready for use. From the **Add build step** drop-down list, select the **Docker Build and Publish** option.

![dcos-velocity-jenkins-build-docker.png](/1.10/img/dcos-velocity-jenkins-build-docker.png)

Fill in the following fields:

* **Repository Name** with your Docker Hub username and `/${JOB_NAME}` as the suffix ("myusername/${JOB_NAME}")
* **Tag** with `${GIT_COMMIT}`
* **Registry credentials** to the credentials for Docker Hub created above

![dcos-velocity-jenkins-build-docker-config.png](/1.10/img/dcos-velocity-jenkins-build-docker-config.png)

# Marathon Deployment

Add a Marathon Deployment post-build action by selecting the **Marathon Deployment** option from the **Add post-build action** drop-down.

![dcos-jenkins-plugin-popup.png](/1.10/img/dcos-jenkins-plugin-popup.png)

Fill in the following fields:

* **Marathon URL** can be accessed within DC/OS using the URL `http://leader.mesos/service/marathon`
* **Application Definition** with the relative path to the marathon application file (`conf/cd-demo-app.json`)
* **Docker Image** with the image created above (`myusername/${JOB_NAME}:${GIT_COMMIT}`)

![dcos-velocity-marathon-config.png](/1.10/img/dcos-velocity-marathon-config.png)

## How It Works

The Marathon Deployment post-build action reads the application definition file, by default `marathon.json`, contained within the project’s Git repository. This is a JSON file and must contain a valid [Marathon application definition](https://mesosphere.github.io/marathon/docs/application-basics.html).

The configurable fields in the post-build action will overwrite the content of matching fields from the file. For example, setting the "Application Id" will replace the `id` field in the file. In the configuration above, "Docker Image" is configured and will overwrite the `image` field contained within the [docker field](https://mesosphere.github.io/marathon/docs/native-docker.html).

The final JSON payload is sent to the configured Marathon instance and the application is updated or created.

# 保存

Save the job configuration.

# Build It

Click **Build Now** and let the job build.

![dcos-jenkins-build-now.png](/1.10/img/dcos-jenkins-build-now.png)

# Deployment

Upon a successful run in Jenkins, the application will begin deployment on DC/OS. You can visit the DC/OS web interface to monitor progress.

When the **Status** has changed to **Running**, the deployment is complete and you can visit the website.

## Visit Your Site

Visit port `80` on the public DC/OS agent to display a jekyll website.

![dcos-jekyll-site1.png](/1.10/img/dcos-jekyll-site1.png)

# 添加新帖子

The content in the `_posts` directory generates a Jekyll website. For this example project, that directory is `site/_posts`. Copy an existing post and create a new one with a more recent date in the filename. I added a post entitled "An Update".

Commit the new post to Git. Shortly after the new commit lands on the master branch, Jenkins will see the change and redeploy to Marathon.

![dcos-jekyll-updated.png](/1.10/img/dcos-jekyll-updated.png)