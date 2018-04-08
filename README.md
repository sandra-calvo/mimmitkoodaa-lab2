# IBM Cloud :cloud: & Watson
## Mimmitkoodaaa Workshop

# Create a Node-RED app that uses Watson Visual Recognition

<!--- GIF & images
![Alt Text](https://media.giphy.com/media/vFKqnCdLPNOKc/giphy.gif)
--->
 
## Introduction 
In this guide, we will create a web application that uses our custom Watson Visual Recognition classifier. 

*Check Lab 1 to create your custom classifier and visual recognition service. (https://github.com/sandra-calvo/mimmitkoodaa-lab1)*

#### Prerequisites
- Register for free on IBM Cloud at https://bluemix.net


## Step 1. Create Your Node-RED Application on IBM Cloud

#### About Node-RED
Node-RED is a visual tool for wiring the internet of things - connecting hardware devices, APIs and online services in a new and interesting way. Node-RED provides a browser-based flow editor that makes it easy to wire together flows using the wide range nodes in the palette. Flows can be then deployed to the runtime in a single-click.
- JavaScript functions can be created within the editor using a rich text editor.
- A built-in library allows you to save useful functions, templates or flows for re-use.
- See https://nodered.org for more information. 


**NOTE:** If you already have a Node-RED application and a Visual Recognition service in use in Bluemix, you can use those - In that case jump to Step 2.

1.	In a browser navigate to https://bluemix.net
2.	Select 'LOG IN' then enter your log in information and press 'SIGN IN'.  
3.	Select the 'CATALOG' view.
4.	Locate the NODE-RED Starter in the boilerplate section of the catalog and click on it.  
 
 ![](/screenshots/Picture11.png?raw=true)

5.	Enter a name for your application, as shown below (host will automatically be completed).  The host name must be unique on IBM Cloud, so please choose a name for example using your initials. Press 'CREATE'. 

 ![](/screenshots/Picture12.png?raw=true)
 
6.	Your application is now staging and will be up and running/awake in a short while. Press 'OVERVIEW' to see information about your application. 

7.	When fully staged, click on the View app link, next to the green circle, this launches the Node-RED main page in a new tab.
If your are using the free account your application will not say running, instead it will be **"awake"**. That means that if in 10 days the app has no traffic or changes IBM will stop it. You can always start it again from the dashboard. 

 ![](/screenshots/Picture13.png?raw=true)
 
8.	Configure your Node-RED editor. In this section, you will set up a username and password to protect your flow. 

 ![](/screenshots/Picture14.png?raw=true)

9.	Write an username and a password of your choice and click 'Next'. Remember that it does not have to be related to your IBM Cloud access. 

 ![](/screenshots/Picture15.png?raw=true)
 
Node-RED is an open source project so you can add new nodes to the palette by modifying the package.json file. 

  ![](/screenshots/Picture16.png?raw=true)
  
  ![](/screenshots/Picture17.png?raw=true)
 
*Your Node-RED flow is all set!

Now click Go to your Node-RED flow editor to open the flow editor and enter your credentials to access the editor.

  ![](/screenshots/Picture18.png?raw=true)
  
  ![](/screenshots/Picture19.png?raw=true)

10.	When using Node-RED we build our apps using this graphical editor interface to wire together the blocks we need. We can simply drag and drop the blocks from the left menu into the workspace in the center of the screen and connect them to create a new flow. 

Note: If you get an "Authorization denied" message when deploying your applications make your sure you are logged in. Click on the icon on the top right side of the Node-RED canvas and login with the credentials you created in the previous steps. 

 ![](/screenshots/Picture20.png?raw=true)

## Step 2. Build your Node-RED flow to classify images
This is the flow we are going to build in this step to test the visual recognition capabilities:

![](/screenshots/Picture21b.png?raw=true)
 
Locate the inject node under the input section in the palette window.  
The inject node is simply a node to send a message (string, timestamp etc) through your flow.

1.	Drag the node onto the workspace in the middle of the screen. Double click on the inject node to configure the node. Change the payload to be a string, and add a direct link to an image you would like to analyze. 
For example: https://www.thesun.co.uk/wp-content/uploads/2017/06/nintchdbpict0002407806071.jpg?strip=all&w=960 

![](/screenshots/Picture22.png?raw=true)
 
2.	Next, we add the Visual recognition node, under the Watson section in the palette.   

3.	Drag the node onto the workspace. Connect the Visual recognition node with the inject node by dragging the output and input dots together and double click in the visual recognition node to configure the setting.
This node will analyze the image sent by the inject node.  

![](/screenshots/Picture23.png?raw=true)
 
Configure the node like the above image with your own API key.

4.	Add a function node for parse the output of the visual recognition.   
Double click on the function node and add the following code: (Be aware, that sometimes there can be format problems when copying the code.)

``` javascript
msg.payload="Watson says that is a/an: \n ";
    for (var i=0; i < msg.result.images[0].classifiers[0].classes.length; i++) {
    msg.payload += msg.result.images[0].classifiers[0].classes[i].class + " with a score of " + msg.result.images[0].classifiers[0].classes[i].score + "\n";
    }
    return msg;
```

5.	To see the results of the image analysis in the debug tab add a debug node. This node doesn't need any configuring. 

6.	The flow is now complete. Make sure that all nodes are connected then click 'Deploy' in the top right corner on the screen.  
 
Click the button to the right of the inject node. To see the results of the analysis, take a look at the debug tab. 

![](/screenshots/Picture25.png?raw=true)
 
In order to make the lab easier we are going to import the rest of the code. 

You can get the complete Node-RED flow from the **mimmitkoodaa_visualRecognition_flow.txt**.

Import the flow by simply clickcing on the 3 white lines on the top right corner of the Node-RED window.  Import - Clipboard - and paste the text you copied from above. 

![](/screenshots/Picture26.png?raw=true)

![](/screenshots/Picture27.png?raw=true)
 

Now your flow should look like this:

![](/screenshots/Picture21c.png?raw=true)

You will need to do some editing on few nodes, because credentials are not transferred with the rest of the code. 
- Edit the purple Visual Recognition nodes with your own credentials (API Key). 

Feel free to find you own images and add them to the different inject nodes to classify images.
 
## Step 3. Test your customized classifier
Same way you imported the code in the previous step, copy the following text and import it in Node-RED. In this case use **mimmitkoodaa_customClassifier_flow.txt**

We should have the next flow:
 
 ![](/screenshots/Picture28.png?raw=true)
 
This flow takes a picture and runs it through the custom classifier created in Step 2. If it finds a match it will tell us what Watson sees in the picture, if it can't find a match it will print a message saying that. 

Remember you will need to edit the flow: 
- Add your credentials (API Key) to the Visual Recognition node 
- Write your own classifier ID in the function "Define the classifier ID"

Your classifier ID can be found in the Visual Recognition Tool: (Do not use the ID shown in the picture)

![](/screenshots/Picture29.png?raw=true)
 
Edit the blue timestamp/inject node with the image URL you want to run through the classifier. 


## Step 4. Create your webapp (UI)
First we will add some new nodes to our palette. 

In the Node-RED window click on the three lines on the top right corner and in the menu, click on the "Manage palette". 
This will open the node menu where you can add new nodes to your application. 

 ![](/screenshots/Picture21g.png?raw=true)
 
You will see the nodes that are installed by default and if you go to the 'install' tab you can search for any node package and add it directly to your app.
                  
  <img src="/screenshots/Picture21d.png" width="70%" height="70%">
 
Search for the dashboard nodes by writing 'dashboard'. This will return multiple node packages, you need to install the package 'node-red-dashboard'. Find it in the search results and click on install. 

This will prompt a window to confirm the installation. Click on install and wait few minutes, the application may require a restart. Click "Done" to close the left side menu. 

 <img src="/screenshots/Picture21e.png" width="70%" height="70%">

After few minutes you will see the new nodes in your Node-RED palette. 
 
  <img src="/screenshots/Picture21f.png" width="30%" height="30%">

Same way you imported the code in the previous steps, copy the following text and import it in Node-RED. In this case use **mimmitkoodaa_UI_flow.txt**

We added the UI flow:

 ![](/screenshots/Picture30.png?raw=true)
 
 You will need to add to the visual recognition nodes your own API Key. 
 
 And also your own classifier ID to the node named custom. 
 
 ## Step 5. Check your webapp! 
The dashboard nodes added an UI to our Node-RED application. To access the UI go to:

http://yourAppName.mybluemix.net/ui - US South

Remember that if you are in UK, Germany Sydney the addredd will look slightly different:
http://yourAppName.eu-gb.mybluemix.net/ui - UK

http://yourAppName.eu-de.mybluemix.net/ui - Germany

http://yourAppName.au-syd.mybluemix.net/ui - Sydney

Awesome, you web app is ready! :+1:

Play with the visual recognition classifier using the UI.
Enter the image URL and choose default classifier or custom and check the results!

 ![](/screenshots/Picture31.png?raw=true)

 



