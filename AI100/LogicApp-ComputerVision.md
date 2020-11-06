# Create Logic App to automate Computer Vision

## Prerequisites:
- Azure Pass Subscription
- Azure Storage Account
- Azure Cosmos DB
- Cognitive Services
See [Lab 1 - Technical Requirements](https://github.com/MicrosoftLearning/AI-100-Design-Implement-Azure-AISol/blob/master/Lab1-Technical_Requirements/02-Technical_Requirements.md) for instructions on how to create these necessary resources. 

## Objective
We are going to create a Logic App which will analyze images using the Computer Vision API which are stored in a Blob Storage and save the descriptions and tags in our Cosmos DB. This lab is an alternative to Lab 2 of the AI-100 course. 

## Create a container in Cosmos DB to store metadata
1. Go to your Azure Cosmos DB in the Azure Portal.
2. In **Overview**, click on **+ Add Container** in the top bar. 

![alt text](https://github.com/madiepev/Tutorials/blob/main/images/addcontainercosmosdb.png?raw=true)

3. Create a new Database called **images**. 
4. Keep the **Throughput** at manual and 400. 
5. Fill in **metadata** for **Container id**. 
6. Put in **/id** for the **Partition key**. 

![alt text](https://github.com/madiepev/Tutorials/blob/main/images/createcontainercosmosdb.PNG?raw=true)

## Create the Logic App
1. Open the Azure Portal.
2. Select **+ Create a Resource** and then enter **logic app** in the search box. 
3. Choose **Logic App** from the available options, then select **Create**. 
4. Select your subcription and resource group. 
6. Choose a name for your Logic App.
7. For **Select the location**, keep it at the default **Region**.
8. For **Location**, choose one near you. 
9. Keep **Log Analytics** on **Off**. 
![alt text](https://github.com/madiepev/Tutorials/blob/main/images/logicappcreation.PNG?raw=true)
10. Click on **Review + Create**. 
11. Once validation passes, select **Create**.

## Customize the Logic App
1. Once deployment is complete. Navigate to your new Logic App. 
2. Scroll down and under **Templates**, click on **Blank Logic App**. 

![alt text](https://github.com/madiepev/Tutorials/blob/main/images/blanklogicapp.png?raw=true)

First thing we have to to do in our Logic Apps Designer is to create a trigger that will kick off our workflow. 

3. Search for *blob* and select **When a blob is added or modified**. 

![alt text](https://github.com/madiepev/Tutorials/blob/main/images/whenblobisadded.png?raw=true)

The trigger is now added to your workflow. 

4. Create a connection to your existing Storage Account.
5. Select the container **images** that you created during Lab 1: Technical Requirements. 

![alt text](https://github.com/madiepev/Tutorials/blob/main/images/nextstep.png?raw=true)

6. Click on **+ New step** to add a next step and action. 
7. Search for *get blob content* and select the action **Get blob content** to add this to your workflow. 

![alt text](https://github.com/madiepev/Tutorials/blob/main/images/getblobcontent.png?raw=true)

8. Click on the field next to **Blob**, a new pop-up window will appear on the right with **Dynamic content**. 
9. In this pop-up window, select **List of Files Path** to use this to specify the blob. 

![alt text](https://github.com/madiepev/Tutorials/blob/main/images/listofilespath.png?raw=true)

10. Again, click on **+ New step**. 

![alt text](https://github.com/madiepev/Tutorials/blob/main/images/newstepafterblobcontent.png?raw=true)

11. Search for **describe image** and select the **Describe Image** action to be added to your workflow. 

![alt text](https://github.com/madiepev/Tutorials/blob/main/images/describeimage.png?raw=true)

12. Connect to your existing Cognitive Service by copying the endpoint and key from your Cognitive Service resource. 
13. Click on the field next to **Image Source** and select **Image Content**. 
14. Click on the field next to **Image Content** and use the **Dynamic content** pop-up window on the right to fill in **File Content**. 

![alt text](https://github.com/madiepev/Tutorials/blob/main/images/filecontent.png?raw=true)

15. Add a **+ New step**. 
16. Search for *initialize variable* and choose **Initialize variable** as an action to add to your workflow. 
17. After **Name** put **ID**. 
18. For **Type** select **Integer**.
19. For **Value** type in **999**.

![alt text](https://github.com/madiepev/Tutorials/blob/main/images/variableid.png?raw=true)

20. Add a **+ New step**. 
21. Search for *compose* and add the **Compose** action to your workflow.
22. Click on the field of **Inputs**. 

![alt text](https://github.com/madiepev/Tutorials/blob/main/images/composejson.png?raw=true)

We are going to create a JSON object to store in our Cosmos DB. It should end up looking like the screenshot below. Create this by typing into the **Inputs** and using the **Dynamic Content** to add the necessary variables. Make sure all keys and values are between quotation marks ("key":"value"), except for the *Tag Names*. 
When you add *Captions Caption Text*, it will add a **for each** loop for you. Accept this change and don't remove it.

![alt text](https://github.com/madiepev/Tutorials/blob/main/images/foreachcaption.png?raw=true)

23. Within your **For each loop**, add a **+ New step**. 
24. Search for *cosmos db* and select **Create or update document** action to add to your workflow. 
25. Connect to your previously created **Cosmos DB account**. 
26. For **Database ID** select **images**.
27. For **Collection ID** select **metadata**.
28. For **Document** use the **Dynamic Content** to fill in **Outputs**. 
29. Add new parameter and add the **Partition key value**.
30. For **Partition key value**, fill in “List of Files Name”. Don’t forget the quotation marks! 

![alt text](https://github.com/madiepev/Tutorials/blob/main/images/createorupdatedocument.png?raw=true)

## Upload images to trigger the Logic App
1. In the Azure Portal, navigate to the Azure Storage Account you created before.
2. Select **Overview**, then select **Containers**.

![alt text](https://github.com/madiepev/Tutorials/blob/main/images/lab01-storageaccountcontainers.png?raw=true)

3. Select the **images** container you created earlier. 
4. Select **Upload**. 
5. On the right you'll see **Upload blob**, click on the icon of a folder next to **Select a file** to upload the images.
6. Select the images from the folder C:\Lab_Files\Lab2-Implement_Computer_Vision\sample_images. **Note!** Don't upload the .py and .md files but only those files that end with .jpg. 
