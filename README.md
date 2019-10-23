# Codewind Grocery Store Lab Session

This repository contains the frontend component of the grocery store sorting app. You'll be importing the two components of this application:
- groceryfrontend
- grocerybackend
You'll be exploring the Visual Studio Code plugin.
You'll be integrating the components and adding functionality to the application.

## Prereqs (These have already been set up on the lab machine)
* Docker installed (version 17.06 minimum)
* Visual Studio Code, version 1.28 or later installed (https://code.visualstudio.com/)
* The Codewind for VS Code/Eclipse plugin installed [VS Code Getting Started](https://www.eclipse.org/codewind/mdt-vsc-getting-started.html) & [Eclipse Getting Started](https://www.eclipse.org/codewind/mdteclipsegettingstarted.html)
* (optional) Add your ssh key (public) to Github (https://help.github.com/en/github/authenticating-to-github/connecting-to-github-with-ssh)

## About Codewind
Codewind is an end to end development environment that lets you rapidly create, edit, and deploy applications. Applications are run in containers from day one and can be delivered into production on Kubernetes through an automated DevOps pipeline using Tekton. Codewind can be installed locally or on a cloud platform like IBM Cloud Private or OpenShift/OKD, and currently supports Java, Node.js, Swift, and several other languages. 

For this lab, we will be installing and running the Codewind plugin in Visual Studio Code. This means that integration with local tools like Visual Studio Code will be included, but we cannot show the Codewind pipeline (which supports deployment of application to Kubernetes on IBM Cloud Private or IBM Container Service) or other tools included in the hosted version of Codewind on IBM Cloud Private. 

## About the Codewind Grocery Store Inventory app
This lab uses a grocery store inventory system to demonstrate Codewind’s capabilities. The inventory system is comprised of a frontend and a backend project. The frontend is a Node.js application, and the backend is a Java MicroProfile microservice. The frontend displays the user interface of the grocery store inventory system and lets the user manage the inventory via adding, editing, deleting, searching and sorting. The backend microservice is a database to store the item’s status, name, unit price and quantity. In order to use the grocery store inventory system you need to start both projects so that the frontend can call the backend. 

We’ll be using this application to show the various capabilities of Codewind, then we’ll add an action to the backend project (already written for you) to show how changes are automatically synced into the running container. 

## Grocery Store Inventory System Setup
The grocery store inventory front end and back end projects need to be created before you start the lab exercises. Codewind has the capability to create a new project or import existing projects. In this setup phase, we’ll import an initial frontend application and create the backend application, and then connect them together. 

### Important note
These instructions have been tailored to the environment for this specific lab's virtual machine. If you want to complete the lab on your own device you should follow the instructions on the **master** branch.

### Step 1: Import the frontend project 
This step demonstrates Codewind's import feature that allows you to start from an existing project.

1. Copy the groceryfrontend project to the codewind workspace and start Visual Studio Code
    a. `sudo cp -r ~/codewind-lab/groceryfrontend ~/codewind-workspace/groceryfrontend`
    b. `sudo lab-start`
2. Import the frontend project from the Codewind Explorer menu by right clicking on Projects and selecting `Add Existing Project`. (**Note**: For Eclipse, the Codewind Explorer can be viewed by navigating to Window > Show View > Other… > Codewind > Codewind Explorer. Furthermore, the project first needs to be imported to the Eclipse workspace by using the Eclipse Import... > General > Projects from Folder or Archive, and importing the frontend project from the codewind-workspace directory, you cloned above. You can now view the project from `Add Existing Project...` menu option from the Codewind plugin in the Codewind Explorer)
  ![image](https://user-images.githubusercontent.com/20015929/67326859-15169380-f4e5-11e9-88d7-c92d2c879689.png)
3. As the project imports, Codewind will detect the project type. Verify that it detects the project type correctly (nodejs). Click **Yes** to finish importing the project.
The project will appear in the Codewind view and will start building automatically. The first build may take some time to complete because images will need to be pulled and build dependencies downloaded.

### Step 2: Create the backend project  
This step demonstrates Codewind's project creation feature that allows you to quickly bootstrap a microservice project.

1. In the Codewind Explorer menu, right click **Projects** and select **Create New Project**
2. Select the **WebSphere Liberty MicroProfile** type and give it the name 'grocerybackend' and press **Enter**
3. Wait for the project to build and start. The project build status will change to *Build successful* and the project app status will change to *Running*. 
4. Copy some files for initial setup of the backend. Open a terminal window and run the following command:
    `sudo cp -r ~/codewind-lab/resources/* ~/codewind-workspace/grocerybackend/`
5. In VS Code, you should see the grocerybackend automatically start building once it detects the file changes. If not, simply right click the project and select `Build`.
6. Check the Codewind Explorer/Projects view and make sure both the groceryfrontend and grocerybackend projects have built successfully and are running.

## Exploring Codewind (Optional)
The two projects will take some time to build and then start in Docker. In the meantime, we'll explore more of Codewind.

1. You can explore Codewind projects from the Explorer menu. Expand Codewind > Projects, and you should see your `grocerybackend` and `groceryfrontend` projects. 
2. Right-click on the **grocerybackend** project and select **Open Project Overview**
3. To edit the code, look to the Explorer menu for the `Codewind-workspace` section, and you can click through the source code.
4. From the Codewind Explorer (`Codewind > Projects (Local)`), you can right click on `grocerystorebackend` and select `Show All Logs` (**Note**: On Eclipse, this can done by selecting `Show Log Files`)
  You can further explore other logs from the console.
5. Wait for both projects to build and start running. You can check the status of your project in the Codewind Explorer. When both projects show they are in *[Running] [Build Succeeded]* state, proceed to the next section.

## Testing the Application
Let's test the application to see how it behaves.

1. Right-click the *groceryfrontend* project in the Codewind Explorer, then select *Open App*. This will bring the application up in the browser.
![image](https://user-images.githubusercontent.com/20015929/67326861-15169380-f4e5-11e9-8c91-e32697499123.png)
![image](https://user-images.githubusercontent.com/20015929/67326862-15169380-f4e5-11e9-8e23-90f45faf7468.png)
2. Once the application appears, you'll see that the inventory system is blank, and no actions can be done yet. That is because the frontend application does not implement the backend yet. 
![image](https://user-images.githubusercontent.com/20015929/67326863-15169380-f4e5-11e9-8de6-b21f5927293e.png)
3. In the Codewind-Workspace in VS Code, navigate to `groceryfrontend > server > apis > storeAPIs.js`. We need to change the value of backendHost on line 6. 
4. In your terminal, run `docker ps -a`. Your output should look something like:  
```
USER$ docker ps -a
CONTAINER ID        IMAGE                                                     COMMAND                  CREATED             STATUS              PORTS                                                                         NAMES
7bbf6a4cfb4b        cw-groceryfrontend-40c37b90-c8f0-11e9-a816-37bb7467867f   "docker-entrypoint.s…"   12 minutes ago      Up 12 minutes       0.0.0.0:32770->3000/tcp, 127.0.0.1:32769->9229/tcp                            cw-groceryfrontend-40c37b90-c8f0-11e9-a816-37bb7467867f
dc5fb1ccb85c        cw-grocerybackend-1f978240-c8f0-11e9-a816-37bb7467867f    "/home/default/artif…"   14 minutes ago      Up 14 minutes       127.0.0.1:32768->7777/tcp, 0.0.0.0:32769->9080/tcp, 0.0.0.0:32768->9443/tcp   cw-grocerybackend-1f978240-c8f0-11e9-a816-37bb7467867f
213b1c688c6b        codewind-pfe-amd64:latest                                 "sh -c ' /file-watch…"   16 minutes ago      Up 16 minutes       127.0.0.1:10000->9090/tcp                                                     codewind-pfe
1338a1b9d1ee        codewind-performance-amd64:latest                         "docker-entrypoint.s…"   16 minutes ago      Up 16 minutes       127.0.0.1:9095->9095/tcp                                                      codewind-performance
```
5. Copy the backend image name (in the above example, `cw-grocerybackend-1f978240-c8f0-11e9-a816-37bb7467867f`) and replace the value in line 6. 
![image](https://user-images.githubusercontent.com/20015929/67326864-15af2a00-f4e5-11e9-8f83-c3baf5d87610.png)
6. Save your changes and rebuild the groceryfrontend application. 
7. When the groceryfrontend project has built successfully and is back in Running state, open the application again to see that there is now an inventory list.
![screen shot 2019-08-27 at 3 53 39 pm](https://user-images.githubusercontent.com/20015929/67326865-15af2a00-f4e5-11e9-946f-a5916c99a874.png)
8. Test the add, edit, search, and sort features to make sure the front end and back end applications are functional.
- Add an item and double check it was added. 
![screen shot 2019-08-27 at 3 56 44 pm](https://user-images.githubusercontent.com/20015929/67326866-15af2a00-f4e5-11e9-811f-87b380df3a29.png)
- You can also use the search feature to double check.  
![screen shot 2019-08-27 at 2 52 26 pm](https://user-images.githubusercontent.com/20015929/67326867-15af2a00-f4e5-11e9-8cc6-e116053b3c54.png)
![screen shot 2019-08-27 at 4 03 53 pm](https://user-images.githubusercontent.com/20015929/67326869-15af2a00-f4e5-11e9-99bf-30f04301d819.png)
- You can also edit the price and quantity of an item:
![image](https://user-images.githubusercontent.com/20015929/67326870-15af2a00-f4e5-11e9-8f25-19b4cf31950c.png)
- Then check to see that the price and quantity have been updated
- You can also sort the items by Name, Unit Price, and Quantity
![screen shot 2019-08-27 at 4 06 52 pm](https://user-images.githubusercontent.com/20015929/67326871-15af2a00-f4e5-11e9-9095-0cc107046f52.png)

## Exercise 1 - Adding the Item Quantity Feature
The item quantity status feature is used to display the quantity status of a specific item. If the quantity is less than 10, then that means the quantity is low and the seller needs to replenish inventory and use the edit item feature to update the quantity. In this exercise, we are going to add the item quantity status feature to the front end application.

1. Implement the item quantity status feature.  
   a. In the Codewind Explorer view in VS Code, select the **groceryfrontend** project and navigate to `groceryfrontend/public/js/store.js`.  
   b. Search for **ADD STATUS FEATURE HERE - JavaScript Logic** and add the code below. This code is used to check the quantity for each item, and then add the status to the item. If the item's quantity is less than 10, then a warning image will be added to the item's status column. Otherwise, a checkmark image will be added to the item's status column.  
   
   ```
   if(data[i].quantity < 10){
     statusNumber = 0;
     status = '<img src="img/warning.svg" class="status-warning">';
   }
   else {
     statusNumber = 1;
    status = '<img src="img/checkmark.svg" class="status-ok">';
   }
   ```
   c. Save the file and codewind will pick up the changes automatically. Wait for the build status to say **Build successful** and the application status to say **Running**.  
   d. You have now implemented the item quantity status feature; it is time to test it.  
   
2. Test the item quantity status feature  
   a. Open the app. Now click the **Add Item** button and add an item, but set the quantity to a number less than 10.  
   b. Verify that the new item's status is red.  
   c. Then **Edit** the new item so that its quantity is greater than 10.  
   d. Check that the status of that item is now green.  
<img width="1364" alt="screen shot 2019-08-28 at 9 10 56 am" src="https://user-images.githubusercontent.com/20015929/67326872-15af2a00-f4e5-11e9-88a3-34d506271d98.png">
<img width="1364" alt="screen shot 2019-08-28 at 9 15 37 am" src="https://user-images.githubusercontent.com/20015929/67326873-15af2a00-f4e5-11e9-9830-7390633c35c0.png">


## Exercise 2 - Adding the Delete Function to the Application
The delete item feature is used to delete an item that is no longer sold by the seller. In this exercise, we are going to add the delete item feature to the front end application and the back end application. 

### Step 1: Adding the delete item feature
1. If the *Explorer* view is not open, use the icon at the top left. Expand *Codewind Explorer*  
2. Select the **groceryfrontend** project in `codewind-workspace`.  
3. Open **groceryfrontend/public/index.html**  
4. Search for **ADD DELETE ITEM FEATURE HERE - MODAL HTML**. Add in the following code. This code is used to define the delete modal html page. After the user clicks the delete button, the delete modal html page will be brought up to let the user confirm if they want to delete the item.  

```
<!-- ADD DELETE ITEM FEATURE HERE - MODAL HTML -->
<!-- DELETE MODAL -->
  <div data-modal id="modal-phry06w79z" class="bx--modal bx--modal--danger delete-modal" role="dialog" aria-modal="true" aria-labelledby="modal-phry06w79z-label" aria-describedby="modal-phry06w79z-heading" tabindex="-1">
    <div class="bx--modal-container">
      <div class="bx--modal-header">
        <p class="bx--modal-header__heading bx--type-beta" id="modal-phry06w79z-heading">Confirm Deletion</p>
        <button class="bx--modal-close" type="button" data-modal-close aria-label="close modal" >
          <svg class="bx--modal-close__icon" width="10" height="10" viewBox="0 0 10 10" xmlns="http://www.w3.org/2000/svg">
            <title>Close Modal</title>
            <path d="M6.32 5L10 8.68 8.68 10 5 6.32 1.32 10 0 8.68 3.68 5 0 1.32 1.32 0 5 3.68 8.68 0 10 1.32 6.32 5z" fill-rule="nonzero"
            />
          </svg>
        </button>
      </div>
  
      <div class="bx--modal-content description">
        <p></p>
      </div>
  
      <div class="bx--modal-footer modal-footer">
        <button class="bx--btn bx--btn--tertiary" type="button" data-modal-close id="delete-modal-yes-button">Yes</button>
        <button class="bx--btn bx--btn--danger--primary" type="button" aria-label="Danger"
           data-modal-primary-focus id="delete-modal-no-button">No</button>
      </div>
    </div>
  </div>
```

5. Open **groceryfrontend/public/js/store.js**. Search **ADD DELETE ITEM FEATURE HERE - CREATE MODAL** and then type in the following code:
```
    /* ADD DELETE ITEM FEATURE HERE - CREATE MODAL */
    var deleteModal = $('.delete-modal');
    CarbonComponents.Modal.create(deleteModal.get(0));
```
6. Search for **ADD DELETE ITEM FEATURE HERE - CALL MODAL** and add the following code:
```
    /* ADD DELETE ITEM FEATURE HERE - CALL MODAL */
    CarbonComponents.Modal.components.get($('.delete-modal').get(0)).show();
    $('.delete-modal .description').text('Are you sure you want to delete the item ' + escapeHtml(selectedID) + '?');
```
7. Search for **ADD DELETE ITEM FEATURE HERE - CALL BACKEND DELETE FUNCTION** and add the following code to enable the delete action.
```
    /* ADD DELETE ITEM FEATURE HERE - CALL BACKEND DELETE FUNCTION */
    $('#delete-modal-yes-button').on('click', function(){
        $.ajax({
            method: 'DELETE',
            url: '/store/api/v1/item/' + selectedID,
            crossDomain: true,
            success:function(response){ 
                renderTable();
            }
        }).fail(function(jqXHR, textStatus, errorThrown) {
            // do nothing for now
        }).done(function(){
            CarbonComponents.Modal.components.get($('.delete-modal').get(0)).hide();
        });
    });

    $('#delete-modal-no-button').on('click', function(){
        CarbonComponents.Modal.components.get($('.delete-modal').get(0)).hide();
    });
```

### Step 2: Implement the Delete Backend Function
1. Now open **grocerybackend/src/main/java/application/rest/v1/Store.java** and search for **ADD DELETE ITEM FEATURE HERE - DELETE BACKEND FUNCTION** and type in the following code. This code is the backend delete API, which is used to get the delete request from the frontend, process the delete request, and then send the delete response to the frontend.

```
    // ADD DELETE ITEM FEATURE HERE - DELETE BACKEND FUNCTION
    @DELETE
    @Path("/item/{id}")
    @Produces(MediaType.TEXT_PLAIN)
    @Consumes({ MediaType.APPLICATION_JSON, MediaType.APPLICATION_XML})
    public Response deleteItems(@PathParam("id") String id) {
            dbutils.deleteItem(id);
            return Response.status(Response.Status.ACCEPTED).entity("Deleted item successfully.").build();
    }
```
  
<img width="1626" alt="screen shot 2019-08-28 at 9 32 46 am" src="https://user-images.githubusercontent.com/20015929/67326874-15af2a00-f4e5-11e9-9332-241fcee7115d.png">  

2. Expand the *Codewind* view on the left if it is not already open. You can see the backend project being rebuilt and restarted.
3. Wait until the project has finished building and restarting. If you want to see progress, right-click the project and select `Show all logs`.  
<img width="1859" alt="screen shot 2019-08-28 at 2 42 32 pm" src="https://user-images.githubusercontent.com/20015929/67326876-1647c080-f4e5-11e9-9275-3d03f0c66bbc.png">
<img width="1859" alt="screen shot 2019-08-28 at 2 42 32 pm" src="https://user-images.githubusercontent.com/20015929/67326875-15af2a00-f4e5-11e9-8261-bd2e0a79d8bb.png">  


### Step 3: Testing the delete capability
1. Once the project is rebuilt, you can use the *Open in Browser* function in the *Codewind* view from VSCode, or you can use the Application Endpoint provided on the project overview page.
![image](https://user-images.githubusercontent.com/20015929/67326877-1647c080-f4e5-11e9-9cdd-7aef393fd6d8.png)
2. Test the application again. This time, you can successfully create and delete items.
<img width="1657" alt="screen shot 2019-08-28 at 2 47 46 pm" src="https://user-images.githubusercontent.com/20015929/67326878-1647c080-f4e5-11e9-8615-1c73efb9e6e0.png">
<img width="1657" alt="screen shot 2019-08-28 at 2 48 47 pm" src="https://user-images.githubusercontent.com/20015929/67326879-1647c080-f4e5-11e9-9adb-ab1436668c31.png">
<img width="1643" alt="screen shot 2019-08-28 at 2 48 56 pm" src="https://user-images.githubusercontent.com/20015929/67326880-1647c080-f4e5-11e9-932c-2e4e714f94cf.png">

If you have successfully tested out the delete item function, then you have successfully completed this lab! 


Have a few more minutes? You could:
- Restart the backend container in debug mode from within Visual Studio Code, set breakpoints, and inspect the running code.
- Make further changes to the frontend or backend projects.
- Create a new microservice project in another language.
- Run a load test against the frontend or backend and see how it performs.

Thanks for taking a quick look at Codewind! To learn more about Codewind, visit https://www.eclipse.org/codewind/. If you have any questions/feedback, you can reach us via Github (https://github.com/eclipse/codewind/) or on mattermost (https://mattermost.eclipse.org/eclipse/channels/eclipse-codewind).
