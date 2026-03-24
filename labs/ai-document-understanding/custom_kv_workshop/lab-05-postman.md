# Lab 5: Call your model through REST API using Postman
## Introduction

In this lab, we will use Document Understanding Services in Postman.

Estimated Time: 15 minutes


### Objectives

In this workshop, you will:

* Learn how to call the model using REST API calls through postman.

### Prerequisites:

* Basic knowledge of REST API calls.
* Postman GUI in your local setup. If you don't have POSTMAN, please download it from [POSTMAN](https://www.postman.com/downloads/).

## TASK 1: Setting Up Postman for OCI Document Service REST APIs

* Visit the [OCI Postman workspace](https://www.postman.com/oracledevs/workspace/oracle-cloud-infrastructure-rest-apis/api/79bbefd7-4bba-4370-b741-3724bf3c9325) and login with your credentials.
* Fork the Document Understanding API collection in your workspace by navigating to Document Understanding API collection and clicking the **"Fork"** option.

![](./images/postman1.png)

* Enter a name to identify the forked Document Understanding API collection, select the workspace you want to fork the collection to, and click **"Fork Collection"**.

![](./images/postman2.png)

* Fork the OCI Credentials Environment in your workspace by navigating to Environments and clicking the **"Fork"** option.

![](./images/postman3.png)

* Enter a name to identify the forked OCI credentials environment, select the workspace you want to fork the collection to, and click **"Fork Collection"**.

![](./images/postman4.png)

* Navigate to your workspace and open the newly forked environment (OCI Credentials), and set the variables `tenancyId`, `authUserId`, and `keyFingerprint` with values from the .oci file you created in Lab 4 (Task 2).

* **Security Best Practice for Private Key**: Do NOT paste your private key directly into Postman's current or initial values. Instead:
  - Store the private key in a `.env` file in your local project (excluded from version control)
  - Use Postman's **Secret** type for the `privateKey` variable to mask sensitive data
  - Or use environment variables in your system and reference them in Postman

* Set both Initial Value and Current Value of non-sensitive variables to the same value.
* Click the Save button to commit your changes to the environment.
![](./images/postman5.png)


## TASK 2: Invoke Document Understanding OCI REST APIs

* Navigate to **"Create a processor job for document analysis"** section under _"processor Jobs"_.

![](./images/request1.png)

* Update the base url with "https://document.aiservice.{region}.oci.oraclecloud.com/20221109" and region being the one in which model is created.

  (sample url: https://document.aiservice.us-ashburn-1.oci.oraclecloud.com/20221109/processorJobs)

![](./images/request2.png)

* Navigate to **"raw"** under _"body"_ section to enter the payload for request. Change the format to _"JSON"_.

![](./images/request3.png)

* Edit the payload below according to your configuration. Replace placeholder values:
  - `your-model-id`: From Lab 1 (Task 2) model details
  - `your-base64-content`: Base64-encoded document (use an [online encoder](https://www.base64encode.org/) or command line: `base64 your-file.pdf`)
  - `your-bucket-name`: Object Storage bucket name
  - `your-namespace`: OCI Object Storage namespace
  - `your-compartment-id`: Compartment where model exists

  ```json
  {
    "processorConfig": {
      "processorType": "GENERAL",
      "features": [
        {
          "featureType": "KEY_VALUE_EXTRACTION",
          "modelId": "your-model-id"
        }
      ]
    },
    "inputLocation": {
      "sourceType": "INLINE_DOCUMENT_CONTENT",
      "data": "your-base64-content"
    },
    "outputLocation": {
      "bucketName": "your-bucket-name",
      "namespaceName": "your-namespace",
      "prefix": "results"
    },
    "compartmentId": "your-compartment-id"
  }
  ```

* Enter the payload in the postman and click on "send". 
* We will get the response with the request details and Processor job details. In the response **"lifecycleState"** should be in <code>SUCCEEDED</code> state.

![](./images/request4.png)

* The output JSON file can be found in the "outputLocation" specified by the user. 
* Navigate to the bucket mentioned in the ouputLocation and click on Objects under the Resources section. 
* You will find the output file similar to the one shown below in the following path: "prefix/{Processorjob_ID}/_/results/defaultObject.json". 
  (Note: Processorjob_ID can be found denoted as "id" in the response obtained in the previous step)

![](./images/request5.png)

* Sample Output JSON file: [defaultObject.json](./defaultObject.json).

## **Summary**

Congratulations! </br>
In this lab, you have learnt how to invoke the model through postman.

Hurray!! You have successfully completed this workshop. In this workshop you had learnt:
* To create and label a dataset.
* To create and train a model in OCI console.
* Call model through different platforms like OCI console, Datascience Notebook(through preview SDK), and Postman.

## Stay in Touch!

* Read our blog posts at [https://blogs.oracle.com/ai-and-datascience/](https://blogs.oracle.com/ai-and-datascience/).

* [Watch our tutorials on YouTube](https://www.youtube.com/playlist?list=PLKCk3OyNwIzv6CWMhvqSB_8MLJIZdO80L).

* Our service documentation is also available at [https://docs.oracle.com/en-us/iaas/data-science/using/data-science.htm](https://docs.oracle.com/en-us/iaas/data-science/using/data-science.htm).
