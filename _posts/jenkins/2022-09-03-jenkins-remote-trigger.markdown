---
layout: post
title:  "Trigger a Jenkins build remotely"
date:   2022-09-02 00:00:00 +0530
categories: jenkins ci
background: '/img/jenkins/nodejs/banner.jpg'
---

{: style="text-align: justify; font-size:25px"}
> If you are here, you already know what Jenkins is and what it does.

### Problem Statement
There are use cases when one wants to trigger a Jenkins build remotely either from a script or by just using a `curl` command rather than opening the dashboard and triggering it from there. In this doc we will understand what all we need and how do we trigger a build remotely.

<p></p><p></p>

### Prerequisites

* Working Jenkins Setup
* Knowledge of HTTP calls

<p></p><p></p>

### Solution
**Steps**

{: style="text-align: justify;"}
1. Create a Build User in Jenkins
    - Login to Jenkins as Admin User
    - Click on <ins>*Manage Jenkins*</ins>
    - Click on <ins>*Manage Users*</ins>
    - Click on <ins>*Create User*</ins>

    {: style="text-align: center;"}
    ![Create User]({{ site.baseurl }}/img/jenkins/remote-trigger/create-user.jpg){:class="img-fluid"}

2. Create an Authentication token for the Build User created in the preceding step
    - Login to Jenkins as Build User
    - Click on <ins>*Build User*</ins>
    - Click on <ins>*Configure*</ins>

    {: style="text-align: center;"}
    ![Configure]({{ site.baseurl }}/img/jenkins/remote-trigger/configure.jpg){:class="img-fluid"}

    - In the configuration page, Go to the <ins>*API Token*</ins> Section and click on <ins>*Add New Token*</ins>
    - Enter a name for the token and click on <ins>*Generate*</ins>

    {: style="text-align: center;"}
    ![Auth Token]({{ site.baseurl }}/img/jenkins/remote-trigger/auth-token.jpg){:class="img-fluid"}

    - Once clicked on the <ins>*Generate*</ins> buttion you would be presented with the API token as shown below.

    {: style="text-align: center;"}
    ![Auth Token 2]({{ site.baseurl }}/img/jenkins/remote-trigger/auth-token-2.jpg){:class="img-fluid"}

3. Create a Job/Project as per your needs
4. Create an API token for the job created
    - Open job configuration
    - On the same Job Configuration page, under the <ins>*Build Triggers section*</ins>; select the **Trigger builds remotely** option and enter some Authentication Token as shown below

    {: style="text-align: center;"}
    ![Job Auth Token]({{ site.baseurl }}/img/jenkins/remote-trigger/job-auth-token.jpg){:class="img-fluid"}

    **Note:** The auth token created above is job auth token and not same user auth token as created in step 2

5. Test the Remote API to trigger the build
    - Trigger build from the browser
    ```
      http://<JENKINS_URL>/job/<JOB_NAME>/build?token=SOMESECURETOKEN
    ```
    - Trigger build using curl
    ```
      curl -I -u <BUILD_USER>:<BUILD_USER_AUTH_TOKEN> 'http://<JENKINS_URL>/job/<JOB_NAME>/build?token=<JOB_TOKEN>
    ```

    **Note:** If you have the job which has some build parameters then replace `/build` with `/buildWithParameters` and pass parameters in the url. See the below example

    ```
    curl -I -u <BUILD_USER>:<BUILD_USER_AUTH_TOKEN> 'http://<JENKINS_URL>/job/<JOB_NAME>/buildWithParameters?token=<JOB_TOKEN>&PARAM1=true&PARAM2=false'
    ```
