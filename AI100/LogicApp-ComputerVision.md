# Create Logic App to automate Computer Vision

## Prerequisites:
- Azure Pass Subscription
- Azure Storage Account
- Azure Cosmos DB
- Cognitive Services
See [Lab 1 - Technical Requirements](https://github.com/MicrosoftLearning/AI-100-Design-Implement-Azure-AISol/blob/master/Lab1-Technical_Requirements/02-Technical_Requirements.md) for instructions on how to create these necessary resources. 

## Create the Logic App
1. Open the Azure Portal.
2. Select **+ Create a Resource** and then enter **logic app** in the search box. 
3. Choose **Logic App** from the available options, then select **Create**. 
4. Select your subcription and resource group. 
6. Choose a name for your Logic App.
7. For **Select the location**, keep it at the default **Region**.
8. For **Location**, choose one near you. 
9. Keep **Log Analytics** on **Off**. 
10. Click on **Review + Create**. 
![alt text](https://github.com/madiepev/Tutorials/blob/main/images/logicappcreation.PNG?raw=true)
11. Once validation passes, select **Create**.

## Upload images to trigger the Logic App
1. In the Azure Portal, navigate to the Azure Storage Account you created before.
2. Select **Overview**, then select **Containers**. 
![alt text](https://github.com/madiepev/Tutorials/blob/main/images/lab01-storageaccountcontainers.png?raw=true)
3. Select the **images** container you created earlier. 
4. Select **Upload**. 
5. On the right you'll see **Upload blob**, click on the icon of a folder next to **Select a file** to upload the images.
6. Select the images from the folder C:\Lab_Files\Lab2-Implement_Computer_Vision\sample_images. **Note!** Don't upload the .py and .md files but only those files that end with .jpg. 
