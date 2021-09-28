# Rest api error 

## Purpose
### This project was made to show that when using a case project in Business Central, the Rest work definition throws an error when running the case project with a Batch Execution Command. But runs normally when we run it as a new instance of a process definition.

## Requirements
- Red Hat Process Automation Manager

# Steps to reproduce with provided project

### Step 1: Import this project from the git clone link.
- On Business central, in your desired namespace click on `Add project` dropdown and select import project. Paste the clone link to this project there.

### Step 2: Build and deploy
- Click `Deploy` to build this project and launch the kie-server

### Step 3: Go to the SwaggerUI 
- Got to Menu > Excution Servers > Hello_World_1.0.0-SNAPSHOT
- click the link to go the kie-server
- Change your url to go to the swaggerui. This can be done by adding `/docs` after `/kie-server` in the URL
    - Ex. `http://localhost:8080/kie-server/services/rest/server/containers/Hello_World_1.0.0-SNAPSHOT`.  
    - Ex. `http://localhost:8080/kie-server/docs/`



### Testing through `Kie session assets`
- Select `POST /server/containers/instances/{containerId} Executes one or more runtime commands`
- Click try it out and enter the following Container ID and payload.

Container id:`Hello_World_1.0.0-SNAPSHOT`.  
Body: 
```
{
	"commands": [{
		"start-process": {
			"processId": "Hello_World.RestWorkflow",
			"parameter": [],
			"out-identifier": null
		}
	}]

}
```
* We expect the process to complete, but instead we get the error: 
```
{
  "type" : "FAILURE",
  "msg" : "Error calling container Hello_World_1.0.0-SNAPSHOT: [Hello_World.RestWorkflow:3 - Rest:3] -- Could not find work item handler for Rest",
  "result" : null
}
```
* Note: There is indeed a work item handler present for Rest Api calls. It can be found at Setting > Deployments > Work Item Handler.  

### testing through `Process instances`
- Select `/server/containers/{containerId}/processes/{processId}/instances Starts a new process instance of a specified process`
- Click try it out and enter the following Container ID, Process ID and payload.

Container id:`Hello_World_1.0.0-SNAPSHOT`.  
Process id: `Hello_World.RestWorkflow`.  
Body: 
```
{}
```
* We expect the process to not complete and receive a `Could not find work item handler for Rest` instead, we get a Code 201 status and a process instance id. This means that it is in fact completing the workflow unlike earlier when we ran it through `Kie session assets`.

<!---

###################### TO DO ###################


# Steps to make new project for testing
### Step 1: Create a new Case Project to recreate this error
- For this example, we will be using a workspace named Hello_World. 
- We can make our case project by clicking on the dropdown in Business Central and selecting `Case Project`
### Step 2: Create a Case Definition Asset
- In your project, select `Add Asset` and scroll down to see `Case Definition`.
- Click on it and fill in any required boxes.
- Name: `RestWorkflow`
### Step 3: Create our workflow
- You should now be seeing an empty canvas
- Open the side bar if not already open. You should be seeing various icons.
- Click on the top most icon.
- Drag and drop the item that is called `Start`. This denotes where our workflow will start from
- Back on the pallet. Select the bottom most icon. The picture should be of several gears.
- Drag and drop the item thats called `Rest` next to the start node. Leave some space as you see fit. 
- Click on the Green Start node you placed first and select the arrow icon and point it to the Rest Node. You should see some blue dots appear to show where it can be connected. 
- Click on the rest node and sleect the red circle icon. Grab the red cirle that appears and space it as you see fit.
    - This is our exit node. In other words, what tells the workflow to stop.

### Step 4: Add our Rest work item handler
- In your project go to Settings > Deployments > Work Item Handler
- click `+ Add work Item Handler`
- Insert the following info and then click save.

Name: `Rest` <br /> 
Value: `new org.jbpm.process.workitem.rest.RESTWorkItemHandler();`<br /> 
Resolver type: `MVEl` <br /> 
- Build and Deploy your project.


### Step 5: Go to the SwaggerUI 
- Got to Menu > Excution Servers > Hello_World_1.0.0-SNAPSHOT
- click the link to go the kie-server
- Change your url to go to the swaggerui. This can be done by adding `/docs` after `/kie-server` in the URL
    - Ex. `http://localhost:8080/kie-server/services/rest/server/containers/Hello_World_1.0.0-SNAPSHOT`.  
    - Ex. `http://localhost:8080/kie-server/docs/`




### Testing through `Kie session assets`
- Select `POST /server/containers/instances/{containerId} Executes one or more runtime commands`
- Click try it out and enter the following Container ID and payload.

Container id:`Hello_World_1.0.0-SNAPSHOT`.  
Body: 
```
{
	"commands": [{
		"start-process": {
			"processId": "Hello_World.RestWorkflow",
			"parameter": [],
			"out-identifier": null
		}
	}]

}
```


* We expect the process to complete, but instead we get the error: 
* Note: There is indeed a work item handler present for Rest Api calls. It can be found at Setting > Deployments > Work Item Handler

```
{
  "type" : "FAILURE",
  "msg" : "Error calling container Hello_World_1.0.0-SNAPSHOT: [Hello_World.RestWorkflow:3 - Rest:3] -- Could not find work item handler for Rest",
  "result" : null
}
```
### testing through `Process instances`
- Select `/server/containers/{containerId}/processes/{processId}/instances Starts a new process instance of a specified process`
- Click try it out and enter the following Container ID, Process ID and payload.

Container id:`Hello_World_1.0.0-SNAPSHOT`.  
Process id: `Hello_World.RestWorkflow`.  
Body: 
```
{}
```
* We expect the process to not complete and receive a `Could not find work item handler for Rest"` instead, we get a Code 201 status and a process instance id. This means that it is in fact completing the workflow unlike earlier when we ran it through `Kie session assets`.


```

-->

