---
title: Offer Decisioning - Test your Decision
description: Offer Decisioning - Test your Decision
kt: 5342
audience: Data Engineer, Data Architect, Orchestration Engineer, Marketer
doc-type: tutorial
activity: develop
exl-id: c14c50fc-e76d-4dcf-9cf5-ac38ae74b48d
---
# 9.3 Prepare your Adobe Experience Platform Data Collection Client property and Web SDK setup for Offer Decisioning

>[!NOTE]
>
>The use of Offer Decisioning in Adobe Experience Platform Web SDK is currently available in early access to select users. This functionality is not available to all IMS organizations.

## 9.3.1 Update your Datastream

In [Exercise 0.2](./../../modules/module0/ex2.md), you created your own **[!UICONTROL Datastream]**. You then used the name `--demoProfileLdap-- - Demo System Datastream`.

In this exercise, you need to configure that **[!UICONTROL Datastream]** to work with **[!DNL Offer Decisioning]**.

To do that, go to [https://experience.adobe.com/#/data-collection/](https://experience.adobe.com/#/data-collection/). You'll then see this. Click **[!UICONTROL Datastreams]** or **[!UICONTROL Datastreams (Beta)]**. 

In the top right corner of your screen, select your sandbox name, which should be `--aepSandboxId--`.

![Click Edge Configuration icon in the left navigation](./images/edgeconfig1b.png)

Search for your **[!UICONTROL Datastream]**, which is named `--demoProfileLdap-- - Demo System Datastream`. Click your **[!UICONTROL Datastream]** to open it. 

![WebSDK](./images/websdk1.png)

You'll then see this. Click **...** next to **Adobe Experience Platform** and then click **Edit**.

![WebSDK](./images/websdk3.png)

To enable **[!DNL Offer Decisioning]**, check the box for **[!DNL Offer Decisioning]**. Click **Save**.

![WebSDK](./images/websdk5.png)

Your **[!UICONTROL Datastream]** is now ready to work with **[!DNL Offer Decisioning]**.

![WebSDK](./images/websdk4.png)

## 9.3.2 Configure your Adobe Experience Platform Data Collection Client property to request Personalized Offers

Go to [https://experience.adobe.com/#/data-collection/](https://experience.adobe.com/#/data-collection/), to **[!UICONTROL Client]**. Search for your Data Collection properties, which are named `--demoProfileLdap-- - Demo System (DD/MM/YYYY)`. Open your Data Collection client property for web.

![WebSDK](./images/launch1.png)

In you property, go to **[!UICONTROL Rules]** and open the rule **[!UICONTROL Page View]**. 

![WebSDK](./images/launch2.png)

Click to open the [!UICONTROL Action] **[!UICONTROL AEP Web SDK - Send Event]**.

![WebSDK](./images/launch3.png)

You'll then see this. You'll notice the menu option for **[!UICONTROL Decision Scopes]**.

![WebSDK](./images/launch4.png)

For every request sent to the edge and to Adobe Experience Platform, it's possible to provide one or more **[!UICONTROL Decision Scopes]**. A **[!UICONTROL Decision Scope]** is a combination of two elements:

- [!UICONTROL Decision ID]
- [!UICONTROL Placement ID]

Let's first have a look where you can find those two elements.

### 9.3.2.1 Retrieve your [!UICONTROL Placement ID]

The [!UICONTROL Placement ID] identifies the location and type of asset that is required. For instance, the hero image on the Luma website homepage corresponds with the [!UICONTROL Placement ID] for [!UICONTROL Web - Image].

>[!NOTE]
>
>As part of module 6, you already configured an Adobe Target Experience Targeting activity that will change the image of the hero location on the homepage, as you can see in the screenshot. For the sake of the exercise, you'll now make your offers appear on the image below the hero image as indicated in the screenshot. 

![WebSDK](./images/launch5.png)

To find the [!UICONTROL Placement ID] for [!UICONTROL Web - Image] go to Adobe Journey Optimizer by going to [Adobe Experience Cloud](https://experience.adobe.com). Click **Journey Optimizer**.

![ACOP](../module7/images/acophome.png)

You'll be redirected to the **Home**  view in Journey Optimizer. First, make sure you're using the correct sandbox. The sandbox to use is called `--aepSandboxId--`. To change from one sandbox to another, click on **PRODUCTION Prod (VA7)** and select the sandbox from the list. In this example, the sandbox is named **AEP Enablement FY22**. You'll then be in the **Home** view of your sandbox `--aepSandboxId--`.

![ACOP](../module7/images/acoptriglp.png)

Next, go to [!UICONTROL Components] and then to [!UICONTROL Placements]. Click the [!UICONTROL Web - Image] placement to see its details.

![WebSDK](./images/launch6.png)

As you can see in the above image, in this example the [!UICONTROL Placement ID] is `xcore:offer-placement:14bf09dc4190ebba`. Write down the [!UICONTROL Placement ID] for your placement for [!UICONTROL Web - Image] as you'll need it in the next exercise.

### 9.3.2.2 Retrieve your [!UICONTROL Decision ID]

The [!UICONTROL Decision ID] identifies which combination of Personalized Offers and Fallback Offer you'd like to use. In the previous exercise, you created your own [!UICONTROL Decision] and named it `--demoProfileLdap-- - Luma Decision`.

To find the [!UICONTROL Decision ID] for your `--demoProfileLdap-- - Luma Decision`, go to [https://platform.adobe.com](https://platform.adobe.com). 

Next, go to [!UICONTROL Offers] and then go to [!UICONTROL Decisions]. Click to select your [!UICONTROL Decision], which is named `--demoProfileLdap-- - Luma Decision`.

![WebSDK](./images/launch7.png)

As you can see in the above image, in this example the [!UICONTROL Decision ID] is `xcore:offer-activity:14c052382e1b6505`. Write down the [!UICONTROL Decision ID] for your decision `--demoProfileLdap-- - Luma Decision` as you'll need it in the next exercise.

Now that you've retrieved the two elements you need to create a **[!UICONTROL Decision Scopes]**, you can continue with the next step, which involves encoding the decision scope.

### 9.3.2.3 BASE64 Encoding

The **[!UICONTROL Decision Scope]** you need to enter is a BASE64-encoded string. This BASE64-encoded string is a combination of the [!UICONTROL Placement ID] and the [!UICONTROL Decision ID], as you can see below. 

```json
{
  "activityId":"xcore:offer-activity:14c052382e1b6505",
  "placementId":"xcore:offer-placement:14bf09dc4190ebba"
}
```

The **[!UICONTROL Decision Scope]** can be generated in two ways:

- Use a public service like [https://www.base64encode.org/](https://www.base64encode.org/). Enter the JSON code as mentioned above, click **[!UICONTROL Encode]** and you'll get your BASE64 encoded string below.

  ![WebSDK](./images/launch8.png)

- Retrieve the BASE64 encoded string from Adobe Experience Platform. Go to [!UICONTROL Decisions] and click to open your [!UICONTROL Decision], which is named `--demoProfileLdap-- - Luma Decision`.

  ![WebSDK](./images/launch9.png)
  
  After opening `--demoProfileLdap-- - Luma Decision`, you'll see this. Locate the placement [!UICONTROL Web - Image] and click the **[!UICONTROL Copy]** button. Next, click **[!UICONTROL Encoded decision scope]**. The **[!UICONTROL Decision Scope]** is now copied to your clipboard.
    
  ![WebSDK](./images/launch10.png)

Next, go back to Launch, to your action **[!UICONTROL AEP Web SDK - Send Event]**.

![WebSDK](./images/launch4.png)

Paste your encoded decision scope in the input field.

![WebSDK](./images/launch11.png)

Save your changes in the action **[!UICONTROL AEP Web SDK - Send Event]** by clicking **[!UICONTROL Keep Changes]**. 

![WebSDK](./images/keepchanges.png)

Next, click **[!UICONTROL Save]** or **[!UICONTROL Save to Library]**

![WebSDK](./images/launch12.png)

In Adobe Experience Platform Data Collection, go to **[!UICONTROL Publishing Flow]** and open your **[!UICONTROL Development Library]** which is named **[!UICONTROL Main]**. Click **[!UICONTROL + Add All Changed Resources]** and then click **[!UICONTROL Save & Build for Development]**. Your changes will now be published to your demo website.

![WebSDK](./images/launch13.png)

Every time you're loading a **General Page** now, like for instance the homepage of the demo website, Offer Decisioning will evaluate what the applicable offer is and will return a response back to the website with the details of the the offer to show. Showing the offer on the website requires additional configuration, which you'll do in the next step.

## 9.3.3 Configure your Adobe Experience Platform Data Collection Client property to receive and apply Personalized Offers

Go to [https://experience.adobe.com/#/data-collection/](https://experience.adobe.com/#/data-collection/), to **[!UICONTROL Properties]**. Search for your Data Collection properties, which are named `--demoProfileLdap-- - Demo System (DD/MM/YYYY)`. Open your Data Collection property for web.

![WebSDK](./images/launch1.png)

In you property, go to **[!UICONTROL Rules]**.

![WebSDK](./images/decrec1.png)

Search and open the rule **Decision Received**.

![WebSDK](./images/decrec2.png)

You'll then see this. Open the action **Place the offer on the page**.

![WebSDK](./images/decrec6a.png)

Click **[!UICONTROL Open Editor]**

![WebSDK](./images/decrec6.png)

Overwrite the code by pasting the below code in the editor.

```javascript
if(!Array.isArray(event.decisions)) {
  console.log('No decisions returned')
  return;
}
console.log("decision",event.decisions)

event.decisions.forEach(function(payload) {
  payload.items.forEach(function(item){
    console.log("Response from Offer Decisioning ", item.data.content);
   
    var element = document.querySelector("#root > div > div > div.app-content > div > section.feature_part.padding_top > div > div.row.align-items-center.justify-content-between > div.col-lg-7.col-sm-6.\\30  > div");
    if(!element){
      console.log("Offer Placement Area Selector not found")
      return;
    }
    if(!item.data){
      return
    }
    //check if offer already exists
    var offer = document.querySelector("#root > div > div > div.app-content > div > section.feature_part.padding_top > div > div.row.align-items-center.justify-content-between > div.col-lg-7.col-sm-6.\\30  > div");
    if(!offer){ 
      element.insertAdjacentHTML('afterbegin', item.data.content) 
    }
    else { 
      console.log("item.data.deliveryURL: " + item.data.deliveryURL)
      document.querySelector("#root > div > div > div.app-content > div > section.feature_part.padding_top > div > div.row.align-items-center.justify-content-between > div.col-lg-7.col-sm-6.\\30  > div").style.background="url('"+item.data.deliveryURL+"')";
      document.querySelector("#root > div > div > div.app-content > div > section.feature_part.padding_top > div > div.row.align-items-center.justify-content-between > div.col-lg-7.col-sm-6.\\30  > div").style.backgroundRepeat="no-repeat";
      document.querySelector("#root > div > div > div.app-content > div > section.feature_part.padding_top > div > div.row.align-items-center.justify-content-between > div.col-lg-7.col-sm-6.\\30  > div").style.backgroundPosition="center center";
      document.querySelector("#root > div > div > div.app-content > div > section.feature_part.padding_top > div > div.row.align-items-center.justify-content-between > div.col-lg-7.col-sm-6.\\30  > div").style.backgroundSize = "contain";
    }  
  })
});
```

Lines 26-27-28-29 will apply the image that's being returned by Offer Decisioning to the website. Click **[!UICONTROL Save]**.

![WebSDK](./images/decrec7.png)

Click **[!UICONTROL Keep Changes]**.

![WebSDK](./images/keepchanges1dd.png)

Next, click **[!UICONTROL Save]** or **[!UICONTROL Save to Library]**

![WebSDK](./images/decrec8.png)

In Adobe Experience Platform Data Collection, go to **[!UICONTROL Publishing Flow]** and open your **[!UICONTROL Development Library]** which is named **[!UICONTROL Main]**. Click **[!UICONTROL + Add All Changed Resources]** and then click **[!UICONTROL Save & Build for Development]**. Your changes will now be published to your demo website.

![WebSDK](./images/decrec9.png)

With this change, this rule in Adobe Experience Platform Data Collection will now be listening to the response from Offer Decisioning which is part of the Web SDK response, and when the response is received, the image of the offer will be displayed on the homepage.

Looking at the demo website, you'll see that this image will be replaced now:

>[!NOTE]
>
>As part of module 6, you already configured an Adobe Target Experience Targeting activity that will change the image of the hero location on the homepage, as you can see in the screenshot. For the sake of the exercise, you'll now make your offers appear on the image below the hero image as indicated in the screenshot. 

![WebSDK](./images/launch5.png)

And instead of the default Luma website images, you will now see an offer like this one. In this case, the fallback offer is displayed.

![WebSDK](./images/decrec10.png)

You've now configured 2 types of personalization:

- 1 Experience Targeting activity using Adobe Target in Module 6
- 1 Offer decisioning implementation using your Data Collection property

In the next exercise you'll see how you can combine your offers and decisions that were created in Adobe Journey Optimizer with an Adobe Target Experience Targeting activity.

Next Step: [9.4 Combine Adobe Target and Offer Decisioning](./ex4.md)

[Go Back to Module 9](./offer-decisioning.md)

[Go Back to All Modules](./../../overview.md)
