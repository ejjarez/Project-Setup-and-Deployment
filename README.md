# Project Setup and Deployment
This README provides step-by-step instructions for setting up and deploying a Salesforce project using the Salesforce CLI (sf CLI). Follow these instructions to create a Salesforce Scratch Org, deploy custom objects and metadata, and work with your Salesforce project.

## Prerequisites
1. **Salesforce Developer Edition Org**: You should have a Salesforce Developer Edition Org. If you don't have one, you can sign up for a free Developer Edition Org at [Salesforce Developer](https://developer.salesforce.com/signup).
2. **Salesforce CLI**: Ensure you have the Salesforce CLI installed. You can download it from [here](https://developer.salesforce.com/tools/sfdxcli).
3. **VSCode**: Make sure you have Visual Studio Code (VSCode) installed, as it will be used to edit and manipulate files.

## Project Initialization

### Step 1: Generate Salesforce Project
```bash
sf project generate --name myorgdev --manifest
```

### Step 2: Create a Scratch Org
Create a Scratch Org for your project.
```bash
sf org create scratch -f config/project-scratch-def.json -a myorgdev -v vscodeOrg -s --wait=20 --duration-days=1
```

### Step 3: Open the Scratch Org
Open the newly created Scratch Org.
```bash
sf force org open
```

## Community Setup

### Step 4: Create a Community
Create a community with the "B2B Commerce (LWR)" template.
```bash
sf community create --name 'B2BStore' --template-name 'B2B Commerce (LWR)' --description 'My Temporary Store'
```

### Step 5: Query Data
Run a SOQL query to retrieve data from Salesforce to check the Store Creation status
```bash
sf data query --query "SELECT id, name, type, STATUS FROM BackgroundOperation"
```

## Custom Object Changes

#### Step 6: Edit the package.xml and call objects
Go to the folder `manifest/package.xml` and before the last line where the version is displayed, paste this:

```xml
<types>
   <members>OrderDeliveryMethod</members>
   <members>ElectronicMediaGroup</members>
   <members>Order</members>
</types>
```

### Step 7: Retrieve Custom Objects Metadata
Now is time to retrieve all custom object metadata we specified from your Salesforce project using the following command.
```bash
sf force source retrieve --manifest manifest/package.xml
```

### Step 8: Edit Custom Object Metadata
Navigate to the 'objects' directory and edit custom object metadata files using an editor (e.g., nano).
```bash
cd objects
cd ElectronicMediaGroup
nano ElectronicMediaGroup.object-meta.xml
```
You'll notice the XML file has two values:
```xml
<externalSharingModel></externalSharingModel>
<sharingModel></sharingModel>
```
We have to update every object with the value `Read`.
```xml
<externalSharingModel>Read</externalSharingModel>
<sharingModel>Read</sharingModel>
```
And then we do the same for the OrderDeliveryMethod file.

### Step 9: Deploy Custom Object Changes
Initiate a deployment to update the custom object metadata with the following command.
```bash
sf project deploy start --manifest manifest/package.xml --ignore-conflicts
```

## Custom Field Changes

## Step 10: Edit Custom Field Metadata
Now we go to our object Order, you'll find the folder called `values`, open it and find the file `SalesStoreId.field-meta.xml`
```bash
cd objects
cd Order
cd fields
nano ElectronicMediaGroup.object-meta.xml
```

## Step 11: Update the Field Visibility in SalesStoreId
Locate the object trackHistory:
```xml
<trackHistory>false</trackHistory
```
It will be false by default, we have to update it to true.
```xml
<trackHistory>true</trackHistory
```

## Step 12: 
Initiate the last deployment to update the custom field metadata with the following command.
```bash
sf project deploy start --manifest manifest/package.xml --ignore-conflicts
```

This revised README provides more descriptive explanations for each step, making it easier for users to follow the instructions and understand the purpose of each command.
