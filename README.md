# Grocery Store App Frontend

This is the frontend component of the grocery store sorting app. You'll be importing the two components of this application:
- groceryfrontend (this repo)
- grocerybackend (https://github.com/rajivnathan/grocerybackend)
You'll be exploring the Visual Studio Code plugin.
You'll be integrating the components and adding function to the application.


A generated IBM Cloud application

[![](https://img.shields.io/badge/IBM%20Cloud-powered-blue.svg)](https://bluemix.net)

## Prereqs
* Docker installed (version 17.06 minimum)
* Visual Studio Code, version 1.28 or later, has already been installed on your machine (https://code.visualstudio.com/)
* The Codewind for VS Code/Eclipse plugin has been installed [VS Code Getting Started](https://www.eclipse.org/codewind/mdt-vsc-getting-started.html) & [Eclipse Getting Started](https://www.eclipse.org/codewind/mdteclipsegettingstarted.html)
* (optional) Add your ssh key (public) to Github 

## Codewind
Codewind is an end to end development environment that lets you rapidly create, edit, and deploy applications. Applications are run in containers from day one and can be delivered into production on Kubernetes through an automated DevOps pipeline using Tekton. Codewind can be installed locally or on a cloud platform like IBM Cloud Private or OpenShift/OKD, and currently supports Java, Node.js, Swift, and several other languages. 

For this lab, we will be installing and running the Codewind plugin in Visual Studio Code. This means that integration with local tools like Visual Studio Code will be included, but we cannot show the Codewind pipeline (which supports deployment of application to Kubernetes on IBM Cloud Private or IBM Container Service) or other tools included in the hosted version of Codewind on IBM Cloud Private. 

## Grocery Store Inventory System
This lab uses a grocery store inventory system to demonstrate Codewind’s capabilities. The inventory system is comprised of a frontend and a backend project. The frontend is a Node.js application, and the backend is a Java MicroProfile microservice. The frontend displays the user interface of the grocery store inventory system and lets the user manage the inventory via adding, editing, deleting, searching and sorting. The backend microservice is a database to store the item’s status, name, unit price and quantity. In order to use the grocery store inventory system you need to start both projects so that the frontend can call the backend. 

We’ll be using this application to show the various capabilities of Codewind, then we’ll add an action to the backend project (already written for you) to show how changes are automatically synced into the running Docker image. 

## Grocery Store Inventory System Setup
The grocery store inventory front end and back end projects need to be created before you start the remaining exercises. Codewind has the capability to create a new project or import existing projects. In this setup phase, we’ll import an initial frontend application and create the backend application, and then connect them together. 

### Step 1: Import the frontend project 
1. Clone the frontend project from https://github.com/rajivnathan/groceryfrontend to the codewind-workspace directory.
2. Import the frontend project from the Codewind Explorer menu by right clicking on Projects and selecting `Add Existing Project`. (**Note**: For Eclipse, the Codewind Explorer can viewed by navigating to Window > Show View > Other… > Codewind > Codewind Explorer. Furthermore, the project first needs to be imported to the Eclipse workspace by using the Eclipse Import... > General > Projects from Folder or Archive, and importing the frontend project from the codewind-workspace directory, you cloned above. You can now view the project from `Add Existing Project...` menu option from the Codewind plugin in the Codewind Explorer)
  ![image](https://media.github.com/user/36567/files/d61d5880-ada4-11e9-9ec9-a406c79a9914)
3. As the project imports, Codewind will detect the project type. Verify that it detects the project type correctly (Node) and that there are no validation errors. Click **Yes** to complete the import project.

### Step 2: Create the backend project 
1. In the Codewind Explorer menu, right click **Projects** and select **Create New Project**
2. Select the **WebSphere Liberty MicroProfile** type and give it the name 'grocerybackend' and press **Enter**
3. Wait for the project to build and start. The project build status will change to *Build successful* and the project app status will change to *Running*. 
4. In your browser, navigate to https://github.com/rajivnathan/grocerybackend/tree/add-db. Download the contents of this branch to your local machine and extract the contents. Move the extracted contents (`db` and `src`) to the `grocerybackend` directory under the `codewind-workspace`.
5. Select **Replace** if there is an error or warning saying the folder application already exists. 
6. In VS Code, you should see the grocerybackend automatically start building once it detects the file changes. If not, simply right click the project and select `Build`.
7. Check the Codewind Explorer/Projects view and make sure both the groceryfrontend and grocerybackend projects have built successfully and are running. 

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
![image](https://media.github.com/user/36567/files/80c10200-c975-11e9-9417-0a9a5d44ea16)
![image](https://media.github.com/user/118508/files/dddf5a80-dba5-11e9-831e-861d1308779b)
2. Once the application appears, you'll see that the inventory system is blank, and no actions can be done yet. That is because the frontend application does not implement the backend yet. 
![image](https://media.github.com/user/36567/files/07bb9f00-c8d0-11e9-8c86-a6bb1b584238)
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
![image](https://media.github.com/user/36567/files/d80d9680-c8d1-11e9-95b6-be44f477ba60)
6. Save your changes and rebuild the groceryfrontend application. 
7. When the groceryfrontend project has built successfully and is back in Running state, open the application again to see that there is now an inventory list.
![screen shot 2019-08-27 at 3 53 39 pm](https://media.github.com/user/36567/files/304d9400-c8e4-11e9-9a00-d61c82f4ed99)
8. Test the add, edit, search, and sort features to make sure the front end and back end applications are functional.
- Add an item and double check it was added. 
![screen shot 2019-08-27 at 3 56 44 pm](https://media.github.com/user/36567/files/fc726e80-c8e3-11e9-9537-05bf2d8599bc)
- You can also use the search feature to double check.
![screen shot 2019-08-27 at 2 52 26 pm](https://media.github.com/user/36567/files/51a98280-c8da-11e9-8925-1a8d4640c5ff)
![screen shot 2019-08-27 at 4 03 53 pm](https://media.github.com/user/36567/files/4bb89f00-c8e4-11e9-807f-7ead2887acd7)
- You can also edit the price and quantity of an item:
![image](https://media.github.com/user/36567/files/6be85e00-c8e4-11e9-8717-88f20140a44e)
- Then check to see that the price and quantity have been updated
- You can also sort the items by Name, Unit Price, and Quantity
![screen shot 2019-08-27 at 4 06 52 pm](https://media.github.com/user/36567/files/ef09b400-c8e4-11e9-8a71-1817cd2c71d7)

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
   c. Save the file and codewind will pick up the changes automatically. The build status stays **Build successful** and the application will stay in **Running state**.
   d. You have now implemented the item quantity status feature; it is time to test it.
   
2. Test the item quantity status feature
   a. Open the app. Now click the **Add Item** button and add an item, but set the quantity to a number less than 10. 
   b. Verify that the new item's status is red.
   c. Then **Edit** the new item so that its quantity is greater than 10.
   d. Check that the status of that item is now green.
<img width="1364" alt="screen shot 2019-08-28 at 9 10 56 am" src="https://media.github.com/user/36567/files/0a6fd000-c974-11e9-9662-eb1f07ad6a37">
<img width="1364" alt="screen shot 2019-08-28 at 9 15 37 am" src="https://media.github.com/user/36567/files/92ee7080-c974-11e9-9c68-716fabcfe94f">


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
<img width="1626" alt="screen shot 2019-08-28 at 9 32 46 am" src="https://media.github.com/user/36567/files/40628380-c977-11e9-9e1a-778d3c18ddf1">

2. Expand the *Codewind* view on the left if it is not already open. You can see the backend project being rebuilt and restarted.
3. Wait until the project has finished building and restarting. If you want to see progress, right-click the project and select `Show all logs`.
<img width="1859" alt="screen shot 2019-08-28 at 2 42 32 pm" src="https://media.github.com/user/36567/files/8c76ed80-c9a2-11e9-873d-d9b0cbb816ec">
<img width="1859" alt="screen shot 2019-08-28 at 2 42 32 pm" src="https://media.github.com/user/118508/files/e59eff00-dba5-11e9-8759-d9fd39da567e">


### Step 3: Testing the delete capability
1. Once the project is rebuilt, you can use the *Open in Browser* function in the *Codewind* view from VSCode, or you can use the Application Endpoint provided on the project overview page.
![image](https://media.github.com/user/36567/files/aa042600-86d7-11e9-8708-52e2f7315d0a)
2. Test the application again. This time, you can successfully create and delete items.
<img width="1657" alt="screen shot 2019-08-28 at 2 47 46 pm" src="https://media.github.com/user/36567/files/82092380-c9a3-11e9-992d-dc302071104d">
<img width="1657" alt="screen shot 2019-08-28 at 2 48 47 pm" src="https://media.github.com/user/36567/files/ad8c0e00-c9a3-11e9-9c4a-3a89aa6e85b0">
<img width="1643" alt="screen shot 2019-08-28 at 2 48 56 pm" src="https://media.github.com/user/36567/files/4589f780-c9a4-11e9-986d-fadc6a96454d">

If you have successfully tested out the delete item function, then you have successfully completed this lab! 


Have a few more minutes? You could:
- Restart the backend container in debug mode from within Visual Studio Code, set breakpoints, and hit the application.
- Make furhter changes to the frontend of backend projects and see the change automatically updated into the running container.
- Create a new microservice project in another language.
- Run load against the frontend or backend and see how it performs.

Thanks for taking a quick look at Codewind! If you have any questions, please Google IBM Codewind and join us via Github or on mattermost (https://mattermost.eclipse.org/eclipse/channels/eclipse-codewind).
