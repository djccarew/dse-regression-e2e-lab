
# Lab - Create and deploy a scoring model to predict house prices w/Bluemix and IBM Data Science Experience

© Copyright IBM Corporation 2017

IBM, the IBM logo and ibm.com are trademarks of International Business Machines Corp., registered in many jurisdictions worldwide. Other product and service names might be trademarks of IBM or other companies. A current list of IBM trademarks is available on the Web at &quot;Copyright and trademark information&quot; at www.ibm.com/legal/copytrade.shtml.

This document is current as of the initial date of publication and may be changed by IBM at any time.

The information contained in these materials is provided for informational purposes only, and is provided AS IS without warranty of any kind, express or implied. IBM shall not be responsible for any damages arising out of the use of, or otherwise related to, these materials. Nothing contained in these materials is intended to, nor shall have the effect of, creating any warranties or representations from IBM or its suppliers or licensors, or altering the terms and conditions of the applicable license agreement governing the use of IBM software. References in these materials to IBM products, programs, or services do not imply that they will be available in all countries in which IBM operates. This information is based on current IBM product plans and strategy, which are subject to change by IBM without notice. Product release dates and/or capabilities referenced in these materials may change at any time at IBM&#39;s sole discretion based on market opportunities or other factors, and are not intended to be a commitment to future product or feature availability in any way.

# Overview

This lab is designed to demonstrate how to use IBM Data Science Experience to build a predictive model within a Jupyter Notebook. The predictive model is then deployed to the Watson Machine Learning Service in Bluemix where it is consumed by users accessing a Node.js application.

![Flow](images/Picture35.png)

1. The developer creates an IBM Data Science Experience Workspace.
2. IBM Data Science Experience depends on an Apache Spark service.
3. IBM Data Science Experience uses Cloud Object storage to manage your data.
4. This lab is built around a Jupyter Notebook, this is where the developer will import data, train, and evaluate their model.
5. Import data on property values in Rotterdam.
6. Trained models are deployed into production using IBM's Watson Machine Learning Service.
7. A Node.js web app is deployed on Bluemix calling the predictive model hosted in the Watson Machine Learning Service.
8. A user visits the web app, enters  information about a property  in Rotterdam , and the model returns a prediction of  the value of the property.


## Prerequisites

* Bluemix supported [web browser](https://console.ng.bluemix.net/docs/overview/prereqs.html#prereqs)
* An [IBM Bluemix Account](https://console.bluemix.net)

## Before you begin
To be able do this lab a Bluemix account is necessary. If you don't have one yet -- or you did not complete the initial set up of your Bluemix account -- follow the steps below.

Your account must have enough resources available for at least 1 application (128MB) and 4 services.

### Already registered and completed set-up

When you already registered and completed the initial set-up of your Bluemix account, you directly jump to [Create a space in Bluemix US region](#create-a-space-in-bluemix-us-region).

### Not registered

Use Ctrl-click (or the equivalent action for your system) to open the [Sign Up for Bluemix](https://console.bluemix.net/registration/trial) page in a separate tab. Fill in the form and click **Start your FREE Bluemix trial** to complete the registration. You will receive an activation mail in your inbox.

### First time login

Use Ctrl-click (or the equivalent action for your system) to open the [Login to Bluemix](https://console.bluemix.net/login) in a separate tab. First time users need to complete a 4-step wizard. This starts by accepting the terms & conditions.

  ![Terms & conditions][1]

Define a name for your organization.

  ![Organization name][2]

Choose a name for your space. Typically `dev` would be a good name for your first space.

  ![Space name][3]

On the last page, click **I'm Ready** to complete the set up process.

### Create a space in Bluemix US region

For the remainder of this lab we switch to the **US region** of Bluemix. For this, use Ctrl-click (or the equivalent for your system) to open the Bluemix dashboard. Click your account and choose **US South** as your active region.

![Select US region][5]

If you are all OK, you get the dashboard. Otherwise, you will be asked to create your first space in this region -- as depicted in the screenshot below. Typically `dev` would be a good name for your space.

![Create space in US][4]

### Download the Rotterdam property value data

Use Ctrl-click (or the equivalent action for your system) to open the <a href="https://ibm.ent.box.com/v/propertyvalues" target=download>propertvalues.csv</a> CSV file in a separate tab. Click **Download** to download this file to your own device.

  ![Download CSV][14]

Congrats, you're now ready to start your data science experience :smiley:!!

# Step 1: Sign up for IBM Data Science Experience

IBM Data Science Experience is an interactive, collaborative, cloud-based environment where data scientists can use multiple tools to activate their insights. In this part of the lab you will sign up for a 30-day trial of IBM Data Science Experience.

  1.  In a web browser navigate to [https://datascience.ibm.com](https://datascience.ibm.com).

  2.  Click on **Sign Up** at the top right.

  ![Sign Up][36]

  3. Click on **Sign in with your IBM id** and enter your Bluemix credentials.

  ![Sign In][37]

  4. Follow the instructions to complete the sign up for IBM Data Science Experience. Note that two Bluemix services will be created for you -- a Cloud Object Storage service and an Apache Spark service. As soon as the 'Get Started' button is clickable, click it and you should be directed to the Data Science Experience dashboard as shown below.

  ![DSX dashboard][6]

# Step 2: Deploy the testing application
In this part of the lab you'll deploy the application that you will use later to test the predictive model that you create.

  1. Use Ctrl-click on the **Deploy to Bluemix** button below to open the deployment process in a separate tab.

  [![Deploy to Bluemix](https://bluemix.net/deploy/button.png)](https://bluemix.net/deploy?repository=https://github.com/djccarew/dse-regression-lab.git)

  2. Log in into Bluemix with your credentials by clicking on the **Log in** link at the top right.

  3. Make sure to deploy the application to the same region and space as where the *Apache Spark* and *Cloud Object Storage* services were created when you signed up for IBM Data Science Experience. Please take note of this space as later in this lab the Watson Machine Learning service needs to be deployed into the same space.

  4. Click on **Deploy** to deploy the application.

  ![Deploy][7]

  5. A Toolchain and Delivery Pipeline will be created for you to pull the app out of Github and deploy it in to Bluemix. Click on the Delivery Pipeline tile to see the status of the deployment.

  ![Toolchain][21]

  6. Wait for the **Deploy Stage** to complete successfully.

  ![Deploy Stage][22]

# Step 3: Create an instance of the Watson Machine Learning Service

In this part of the lab, you'll create an instance of the Watson Machine Learning service and bind it to the application that you created in Step 2.

  1. In your browser go to the Bluemix Dashboard and click **Catalog**.

  2. In the navigation menu at the left, select **Data  & Analytics** (under **Platform**) and then select **Machine Learning**.

  ![Watson ML Service][8]

  3. In the **Connect to** drop-down, select the application that you deployed earlier in Step 2 of this lab.

  ![Connect to Service][9]

  4. Verify this service is being created in the same space as the app in Step 2.

  5. Click **Create**, followed by **Restage** when you’re prompted to restage your application.

  ![Connect to Service][23]

  6. Go back to the Bluemix dashboard and wait until the app shows that it is running again.

  ![Overview DSX services and test app][10]

## Step 4: Create a project in IBM Data Science Experience and bind it to your Watson Machine Learning service instance

In this part of the lab you will create a new project in IBM Data Science Experience and bind it to your instance of the Watson Machine Learning service.

  1. In a new browser tab go to [https://datascience.ibm.com](https://datascience.ibm.com).

  2. Click on **Sign In** at the top of the page.

  3. From the dashboard, click on **Create new** from the top-right. From the drop-down menu select **Project**.

  ![Create New Project][11]

  4. Enter _Watson ML Integration_ as the project name and click **Create**. Leave the other settings on their default value.

  5. On the right, in the Files section, click on Browse to upload the data file you’ll use to create a predictive model.

  ![Import Data][12]

  6. On your laptop, browse to the location where you downloaded the file **propertyvalues.csv** in the section [Download the Rotterdam property value data](#download-the-rotterdam-property-value-data) of this lab. Select the file and click on Open (or the equivalent action for your operating system).

  7. Once successfully uploaded, the file should appear in the **Data Assets** section.

  ![Data Assets][13]

  8. Click on **Settings** for the project.

  ![Settings][24]

  9. Click on add associated service and select **Machine Learning**.

  ![Associate Service][25]

  10. Choose your existing Machine Learning instance and click on **Select**.

  ![Add Watson ML Service][26]

  11. Click on your browser’s Back button and verify that the Watson Machine Learning service is now listed as one of your **Associated Services**.

  ![Associated Services][27]

  12. Leave the browser tab open for later.

## Step 5: Save the credentials for your Watson Machine Learning Service

In this part of the lab you’ll save the credentials for your Watson Machine Learning instance so you can use it later in your code.

  1. In a different browser tab go to [http://console.bluemix.net](http://console.bluemix.net) and log in to the Dashboard.

  2. Click on your Watson Machine Learning instance under **Services**.

  ![Save Credentials][28]

  3. Click on **Service credentials** and then on **View credentials** to see the credentials.

  ![View Credentials][29]

  4. Save the username and password to a text file on your machine. You’ll need this information later in your Jupyter notebook.

## Step 6: Create a notebook in IBM Data Science Experience

In this part of the lab you’ll create a Jupyter notebook and import the code to create a predictive model.

  1. If you don't have your newly created Project open, first click **Projects -> View All Projects**, and then select your newly created project from Step 4. Next, in the Data Science Experience browser tab click on **Overview** and then click **add notebooks**.

  ![Add Notebook][15]

  2. Click on **From URL** and name the notebook _Apache Spark Regression with Watson ML_.

  3. Under **Notebook URL** provide the following url: https://raw.githubusercontent.com/djccarew/dse-regression-lab/master/regression.ipynb

  ![Create Notebook][16]

  4. Click **Create Notebook** to create the new notebook.

  5. Leave your browser tab open for the next part.


## Step 7: Run the notebook in IBM Data Science Experience

In this part of the lab you will run the Jupyter Notebook code creating a predictive model, and save it in the Watson Machine Learning Service.

  1. Place your cursor in the first code block in the notebook.

  ![Insert Credentials Step 1](images/Picture23v2.png)

  2. Click on the **Find and Add** data icon -- see step 1 in diagram below -- and then select **Insert to code** under the file **propertyvalues.csv**. This is step 2 in diagram below. Finally select **Insert Credentials** -- which is step 3 in diagram below.

  ![Insert Credentials Step 2][30]

  3. Your Object Storage credentials should now be in the cell. Make sure to rename the variable to `credentials_1`.

  ![Insert Credentials Step 3](images/Picture25v2.png)

  4. Click on the **Run** icon to run the code in the cell.

  ![Run Notebook][31]

  5. Move your cursor to each code cell and run the code in it. Read the comments for each cell to understand what the code is doing. **Important** when the code in a cell is still running, the label to the left changes to **In [\*]**:.
  Do **not** continue to the next cell until the code is finished running.

  6. When you get to the cell that says **Stop here !!!!** insert the username and password that you saved from your Watson Machine Learning instance into the code before running it.

  ![Run Notebook](images/Picture27.png)

  7. Continue running each cell until you finish the entire notebook.

## Step 8: Deploy the saved predictive model as a scoring service

In this part of the lab you’ll deploy the model you saved by running the Python notebook as a scoring service in Watson Machine Learning.

1. In a different browser tab go to [http://console.bluemix.net](http://console.bluemix.net) and log in to your dashboard.

2. Click on the entry for your Watson Machine Learning service under **Services**.

3. Click **Manage** and then click on the **Launch Dashboard** button in the Watson Machine Learning tile.

![Launch Dashboard][17]

4. Your saved model should appear. Under **Actions** select **Create Deployment**.

![Create Deployment Step 1][18]

5. Name the deployment _Property Value Prediction Model Deployment_ and click **Save**. Keep the type of deployment set to the default value 'Online'.

![Create Deployment Step 2][19]

6. Your model should now be deployed and visible as a Deployment with status 'ACTIVE'.

![Create Deployment Step 3][20]

7. Restart the Node.js Web App. For this, return to your Bluemix Dashboard and select the restart icon to restart the web application.

![Restart WebApp][32]


## Step 9: Test the deployed Model

In this part of the lab you’ll test the deployed model with the Node.js application that you deployed earlier.

1. Click on the application URL to open the application in a separate tab.

![Test Deployed Model][33]

3. When the application appears click on **Score now** to test the scoring model with the pre-selected  values.  

4. Verify that the model predicts a value for  that property  that is close to  390,000 (the actual value of the property)

![Score][34]

5. **Click Close**. Run the app again choosing one or more  sets of input values from the  table below and comparing the prediction to the actual value. 

Neigborhood | Building Type | Year Built | Interior Volume | Other Volume | Lot Size | Actual Value
--- | --- | --- | --- | --- | --- | ---
Rijkzicht | Appartement | 1979 | 166 | 11 | 100 | 90,500
Bomenbuurt | Tussenwoning | 1926 | 236 | 14 | 59 | 130,000
Bloemenbuurt | Vrijstaand | 1978 | 712 | 90 | 855 | 410000

Congratulations, you successfully created a predictive model in Apache Spark and deployed and tested it using the Watson Machine Learning Service in Bluemix :smiley:!!

## Additional links

* More data science journeys on IBM Code: [https://developer.ibm.com/code/journey/category/data-science/](https://developer.ibm.com/code/journey/category/data-science/)
* IBM Data Science Experience: [https://www.ibm.com/analytics/us/en/watson-data-platform/data-science-experience/](https://www.ibm.com/analytics/us/en/watson-data-platform/data-science-experience/)
* Watson Data Platform: [https://www.ibm.com/analytics/us/en/watson-data-platform/](https://www.ibm.com/analytics/us/en/watson-data-platform/)


[1]: images/terms.png?raw=true
[2]: images/create-org.png?raw=true
[3]: images/create-space.png?raw=true
[4]: images/create-us-space.png?raw=true
[5]: images/select-region.png?raw=true
[6]: images/dsx-dashboard.png?raw=true
[7]: images/pipelinev2.png?raw=true
[8]: images/create-ml-instance.png?raw=true
[9]: images/connect-tov2.png?raw=true
[10]: images/overview-services-and-appv2.png?raw=true
[11]: images/create-project.png?raw=true
[12]: images/browse-file.png?raw=true
[13]: images/data-assetsv2.png?raw=true
[14]: images/download-csvV2.png?raw=true
[15]: images/add-notebook.png?raw=true
[16]: images/create-notebookv2.png?raw=true
[17]: images/launch-dashboard.png?raw=true
[18]: images/create-deployment1v2.png?raw=true
[19]: images/create-deployment2v2.png?raw=true
[20]: images/create-deployment3v2.png?raw=true
[21]: images/toolchainv2.png?raw=true
[22]: images/deploy-stagev2.png?raw=true
[23]: images/restage-appv2.png?raw=true
[24]: images/settings.png?raw=true
[25]: images/add-associated-service.png?raw=true
[26]: images/choose-ml-service.png?raw=true
[27]: images/associated-services.png?raw=true
[28]: images/services-overview.png?raw=true
[29]: images/ml-credentials.png?raw=true
[30]: images/insert-credentials2v2.png?raw=true
[31]: images/run-notebookv2.png?raw=true
[32]: images/restart-app.png?raw=true
[33]: images/open-appv2.png?raw=true
[34]: images/app-prediction.png?raw=true
[36]: images/dsx-signup.png?raw=true
[37]: images/use-existing-id.png?raw=true
