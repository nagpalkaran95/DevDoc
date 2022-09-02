---
layout: post
title:  "Using Jenkins + NodeJS"
date:   2022-09-02 00:00:00 +0530
categories: jenkins ci
background: '/img/jenkins/nodejs/banner.jpg'
---

{: style="text-align: justify; font-size:25px"}
> If you are here, you already know what Jenkins is and what it does.

{: style="text-align: justify;"}
Recently I started exploring Jenkins while I was working on something. I encountered an issue while using NodeJS with Jenkins and found it to be a head scratcher. After referring to multiple docs and videos I was able to sort it out and thought of putting it in one place.


### Problem Statement
Jenkins unable to locate package (node/npm)

> Error: npm command not found

(I was using `npm install` in my script)

<p></p><p></p>

### Prerequisites

* Working Jenkins Setup
* Working knowledge of NodeJS

<p></p><p></p>

### Solutions
1. [Use system installed NodeJS](#system-node)
2. [New NodeJS installation for the Jenkins setup](#new-node)

<p></p><p></p>

### 1. Use system installed NodeJS {#system-node}

{: style="text-align: justify;"}


**GUI**
{: style="text-align: justify;"}
When using GUI to create a build job, you can add an export statement at the start of script under `Execute Shell` option. You need to basically add system installed NodeJS version to the PATH variable. When Jenkins is running your build, the PATH variable is reset everytime. So this will ensure NodeJS is added to the PATH before running required statements.

```sh
export PATH=$PATH:<PATH_OF_NODE>
# other commands to follow
```

Example:
```sh
export PATH=$PATH:/Users/jenkinsdemo/.nvm/versions/node/v12.22.10/bin/
```

**JenkinsFile**
{: style="text-align: justify;"}
Another way of of creating a jenkins job is using a Jenkinsfile. The same steps would be required here just under `sh` block.

```
pipeline {
  ### other declarations
  stages {
    stage('setup') {
      steps {

        sh 'export PATH=$PATH:<PATH_OF_NODE>'
        
        ### other commands
      }
    }
  }
}
```

<p></p><p></p>

### 2. New NodeJS installation for the Jenkins setup {#new-node}
{: style="text-align: justify;"}
1. Go to Jenkins Dashboard
2. Click on <ins>*Manage Jenkins*<ins>


    {: style="text-align: center;"}
    <img alt="Manage Jenkins" class="img-fluid" src="{{ site.baseurl }}/img/jenkins/nodejs/manage-jenkins.jpg" width="250" height="300">

    {: style="text-align: center;"}
    *Figure 1:* Jenkins Main Dashboard

3. Click on <ins>*Global Tool Configuration*<ins>

    {: style="text-align: center;"}
    ![Global Tool Configuration]({{ site.baseurl }}/img/jenkins/nodejs/global-tool-conf.jpg){:class="img-fluid"}

    {: style="text-align: center;"}
    *Figure 2:* Manage Jenkins View

4. Scroll down to the <ins>*NodeJS*<ins> section and click on <ins>*Add NodeJS*<ins>

    {: style="text-align: center;"}
    ![Node Install]({{ site.baseurl }}/img/jenkins/nodejs/node-install.jpg){:class="img-fluid"}

    {: style="text-align: center;"}
    *Figure 3:* Add NodeJS block

5. Enter the name of you choice and select the NodeJS version you want and then click on <ins>*Save*<ins>
6. Build the job again and now Jenkins will be able to find and use the installed Nodejs version
