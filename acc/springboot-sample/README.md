## Deploy SpringBoot demo application to Application Cloud Container Services using Developer Cloud Services ##

This demo conatins a simple SpringBoot application which will deployed on Application Cloud Container Services.

The SpringBoot sample application is a web application serving one simple JSP page.

Main steps:

- Get Oracle Cloud Services account (contains DevCS and ACCS)
- Create new project in DevCS
- Configure build job for sample application
- Configure Application Cloud Container service deployment in DevCS
- Build and deploy sample application

----------

#### 1. Open Developer Cloud Service ####

One easy way to open the Developer Cloud Service Console to sign in to [Oracle Public Cloud](https://cloud.oracle.com/en_US/sign-in). First select your datacenter then provide the identity domain and credentials. After a successful login you will see your Dashboard. Find the Developer services tile and click on the hamburger. In the dropdown menu click on Open Service Console.
![alt text](https://github.com/oracle-weblogic/cloud/blob/master/acc/springboot-sample/md.resources/dashboard.png "Open Developer Cloud Service")

#### 2. Create new Developer Cloud Service Project ####

Log in to Oracle Developer Cloud Services and create a new project.

![alt text](https://github.com/oracle-weblogic/cloud/blob/master/acc/springboot-sample/md.resources/new.project.png "Create new Developer Cloud Service project")

Enter the name of the project and set the desired properties. Click on **Next** and select *Initial Repository* as template.

![alt text](https://github.com/oracle-weblogic/cloud/blob/master/acc/springboot-sample/md.resources/select.template.png "Template selection")

Click on **Next** and on the Properties page select *Import existing repository*.
Enter or copy the *https://github.com/oracle-weblogic/cloud.git* repository address.

![alt text](https://github.com/oracle-weblogic/cloud/blob/master/acc/springboot-sample/md.resources/import.repository.png "Import external repository")

Now click on Finish to create the project and to clone the repository.

### 3. Configure build job for sample application ###

Once the project provisioning is ready let's create the build job to compile and package the sample SpringBoot application to the desired format for Application Cloud Container services.

Select **Build** page and click on the **New Job** button.

![alt text](https://github.com/oracle-weblogic/cloud/blob/master/acc/springboot-sample/md.resources/new.job.png "Create new build job")

Enter a name for the new job. Select the *Create a free-style job* option and save.
On the Main configuration page of the newly created job make sure **JDK 8** is the selected JDK.

![alt text](https://github.com/oracle-weblogic/cloud/blob/master/acc/springboot-sample/md.resources/job.main.png "Configure job")

Change to the **Source Control** tab and select **Git**. In the git's properties section select the one URL (Initial repository from the imported sources) which is provided in the list. Leave the advanced settings default.

![alt text](https://github.com/oracle-weblogic/cloud/blob/master/acc/springboot-sample/md.resources/job.source.control.png "Configure job")

Change to **Build Steps** tab and add **Maven 3** build step. Enter **clean install** as Goals and **acc/springboot-sample/pom.xml** to POM File field. (In case if Build Steps tab just shows **Loading...** long time ago, save the Build configuration then re-open and continue.)

![alt text](https://github.com/oracle-weblogic/cloud/blob/master/acc/springboot-sample/md.resources/job.build.steps.png "Configure job")

Finally change to Post Build tab and check in the **Archive the artifacts** option. Enter **acc/springboot-sample/target/\*.zip** into **Files To Archive** field.

![alt text](https://github.com/oracle-weblogic/cloud/blob/master/acc/springboot-sample/md.resources/job.post.build.png "Configure job")

Click on **Save** to update the new job configurations. To check the build job click on **Build Now** on the job's detail page. Once the job is done check the archived artifacts. It should be the following: springbootdemo-0.0.1.zip

![alt text](https://github.com/oracle-weblogic/cloud/blob/master/acc/springboot-sample/md.resources/build.artifacts.png "Build job")

Please note the build job contains an extra build step which packs the default artifact (springbootdemo-0.0.1.jar) and manifest.json (ACCS descriptor from the *src/acc.resources* folder) into a zip archive. This archive is the desired format to deploy a Java application to ACCS.

### 4. Configure Application Cloud Container service deployment ###

Now create deployment configuration which enable direct deployment to Application Cloud Container services after a successful build job.
Change to **Deploy** page in DevCS and create **New Configuration** and set the following properties.

- **Configuration Name**: any name to identify deployment configuration
- **Application Name**: instance name in ACCS
- **Deployment Target**: click on New and select Application Cloud Container... and define connection properties such as **Data center**, **Identity Domain** and **credentials**. 
- **Type**: select **Automatic** which means auto deploy after a successful execution of the build job. Select your previously created job and its artifact to deploy.

![alt text](https://github.com/oracle-weblogic/cloud/blob/master/acc/springboot-sample/md.resources/deploy.config.png "Deployment Configuration")

### 5. Build and deploy the sample application ###

Now go back to **Build** tab and execute again the build job for SpringBoot sample application. Once the job execution is ready (this may take a while) go back to your Dashboard and open Application Cloud Container Console. Note: if you can not see Application Cloud Container Services tile you need to switch back to the original dashboard where you can find ACCS section.

![alt text](https://github.com/oracle-weblogic/cloud/blob/master/acc/springboot-sample/md.resources/switch.dashboard.png "Switch dashboard")

Search the Application Cloud Container frame and click on  Service Console.

![alt text](https://github.com/oracle-weblogic/cloud/blob/master/acc/springboot-sample/md.resources/old.dashboard.png "Old dashboard")

Check the new instance created by DevCS and click on the URL to access your application.

![alt text](https://github.com/oracle-weblogic/cloud/blob/master/acc/springboot-sample/md.resources/acc.console.png "ACC Console")

After this setup build execution triggers a new(re) deployment to ACCS. There are many other option to trigger this deployment. For example build can be triggered by source changes or can be scheduled to specific time of the day.

### + Optional step: Make changes in the application ###

Prerequisites: Git client, Text editor

Clone your newly created Git repository hosted on Developer Cloud Service to your local machine using basic or your favourite Git tool. Make small changes for example in the JSP file. Push changes to DevCS remote repository, execute Build again and check the changes on the redeployed application.


