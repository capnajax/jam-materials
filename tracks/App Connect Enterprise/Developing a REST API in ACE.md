# Developing, Deploying, and Testing a REST API in ACE using Swagger 2.0 on CP4i (OpenShift)

This tutorial provides a step-by-step walkthrough for creating, deploying, and testing a REST API using Swagger 2.0 within IBM App Connect Enterprise (ACE) as part of Cloud Pak for Integration (CP4I) on Red Hat OpenShift.

You will learn how to design the API in App Connect Toolkit, package and deploy it to an ACE Dashboard instance running on OpenShift, and validate its functionality using the built-in testing tools.

By the end of this guide, you will have a fully operational REST API running in a cloud-native integration environment.

 ${toc}

## Scenario Overview

In this scenario, you will act as an integration developer responsible for exposing a RESTful interface for a backend service. The API will be designed using a Swagger 2.0 definition and implemented in the App Connect Toolkit.

Once the interface is created, you will upload the BAR file to an ACE integration server managed through the App Connect Dashboard on OpenShift.

Finally, you will verify the API behavior by invoking its endpoints directly from the dashboard.

This end-to-end process demonstrates how ACE and CP4I simplify API development, deployment, and testing in a modern, containerized environment.

### Demo products

- App Connect Enterprise
- App Connect Dashboard
- CP4i

### Demo capabilities

IBM App Connect Enterprise toolkit, Integration Runtime, Dashboards, Rest API

### Demo script

The goal of this demo is to demonstrate how to design, deploy, and test a REST API using a Swagger 2.0 definition within IBM App Connect Enterprise (ACE) running on Cloud Pak for Integration (CP4I) on OpenShift.The demo will walk through creating the REST API in the App Connect Toolkit, packaging it into a BAR file, uploading and deploying it to an ACE integration server through the App Connect Dashboard, and finally testing the API endpoints directly in the dashboard.By the end of the demo, you will see how ACE provides a streamlined, developer-friendly workflow for building modern RESTful interfaces in a cloud-native environment.

## Creating the REST API

 ${issue @tdw We need to explain "the toolkit" and how to obtain it}

1. In the toolkit, click **New** → **Rest API**

    ![Screenshot](images/DR1.png)

2. Add a name and select Specification Swagger 2.0

    ![Screenshot](images/DR2.png)

    The header will be created with the information:

    - Rest API base URL
    - Title
    - Description
    - Version

    ![Screenshot](images/DR3.png)

3. Add Resources

    ![Screenshot](images/DR4.png)

4. Select **GET** and **POST**

    ![Screenshot](images/DR5.png)

    Two resources will be created

    ![Screenshot](images/DR5-1.png)

5. Add the Model Definitions, type `petsApi`

    ![Screenshot](images/DR6.png)

6. Click on the **petsApi** object and then on the add button to add

    ![Screenshot](images/DR7.png)

7. Add `ID` as **integer**, `Name` as **string** and `Specie` as **String**

    ![Screenshot](images/DR8.png)

8. Select the Schema Type for the resources created

    ![Screenshot](images/DR9.png)
    ![Screenshot](images/DR10.png)

9. Add the Subflow for the GET Operation

    ![Screenshot](images/DR11.png)

10. Drag and drop the compute node

    ![Screenshot](images/DR12.png)

11. Double click the compute node

    ![Screenshot](images/DR12-1.png)

12. Add these lines to the compute node:

    ```sql
    CALL CopyMessageHeaders();
    SET OutputRoot.JSON.Data.petsApi.ID = 123;
    SET OutputRoot.JSON.Data.petsApi.Name = 'Fido';
    SET OutputRoot.JSON.Data.petsApi.Specie = 'Dog';
    ```

    ![Screenshot](images/DR13.png)

    Close and save the file.

13. Go back to the Pets API tab

    ![Screenshot](images/DR14.png)

14. Add the Subflow

    ![Screenshot](images/DR15.png)

15. Add the Compute node for the Post Operation

    ![Screenshot](images/DR16.png)

16. Double Click the compute node

    ![Screenshot](images/DR17.png)

17. Uncomment the `CALL CopyMessageHeaders`

    ![Screenshot](images/DR18.png)

## Create Bar File

1.  Right click on the REST API application > New > BAR file

    ![Screenshot](images/DR19.png)

2. Give a name

    ![Screenshot](images/DR20.png)

3. Select the deployable resources and click on the Build and Save Button

    ![Screenshot](images/DR21.png)

    Location of the Bar file is:

    **Windows**: `C:\Users\<your user>\IBM\ACET12\workspace\BARfiles`
    **MAC**: `/Users/<your user>/IBM/ACET12/workspace/BARfiles`

## Upload the Bar file to App Connect on OCP

1. From the Cloud Pak for Integration UI go into the Integration dashboard:

    ![Screenshot](images/DR22.png)

2. On the Menu go to Bar Files

    ![Screenshot](images/DR23.png)

3. Import the Barfile

    ![Screenshot](images/DR24.png)

4. Click on the drag and drop

    ![Screenshot](images/DR25.png)


5. Click on the continue button

    ![Screenshot](images/DR26.png)

6. Go to the configuration Menu

    ![Screenshot](images/DR27.png)

7. Create configuration

    ![Screenshot](images/DR28.png)

8.  Select Type server.config.yaml and give a name:

    ![Screenshot](images/DR29.png)

    ```yaml
    Cors:
    ResourceManagers:
      HTTPConnector:
        CORSEnabled: true
        CORSAllowOrigins: '*'
        CORSAllowMethods: 'GET,HEAD,POST,PUT,PATCH,DELETE,OPTIONS'
        CORSAllowHeaders: 'Accept,Accept-Language,Content-Language,Content-Type'
    ```

Create the Integration Runtime

1. Go to Home

    ![Screenshot](images/DR30.png)

2. Click on Deploy integrations

    ![Screenshot](images/DR31.png)

3.  Select Quick start integration and next

    ![Screenshot](images/DR32.png)

4. Select the bar file and click next:

    ![Screenshot](images/DR33.png)

5. Select the configuration and click next

    ![Screenshot](images/DR34.png)

6.  Select next:

    **Name**: Any name
    **Version**: choose the latest version
    **License use**: CloudPakForIntegrationNonProductionFree
    **Create**.

    ![Screenshot](images/DR35.png)

## Test the REST API

1. Go into the Runtime created

    ![Screenshot](images/DR36.png)

2. Select the Rest API

    ![Screenshot](images/DR37.png)

    You will see the REST API operations listed

    ![Screenshot](images/DR38.png)

3. Select the GET /petsAPI
You also can copy the URL to test from third party tools like Postman

    ![Screenshot](images/DR39.png)

4. Click on **Try it** and then click **send**

    ![Screenshot](images/DR40.png)

    You will receive the response as set in the compute node

    ![Screenshot](images/DR41.png)

5. Test the POST, click the `POST /petsAPI` and **Try It**

    ![Screenshot](images/DR42.png)

6. Enter the body and click send

    ```json
    {
      "ID":133
      "Name":"Daisy"
      "Specie":"cat"
    }
    ```

    ![Screenshot](images/DR43.png)

    You will get the headers, as indicated from the compute node

    ![Screenshot](images/DR44.png)

 ${comment @tdw needs a conclusion}
