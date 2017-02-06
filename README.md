= Talon.One Salesforce Integration Example

This example shows how to create a "coupon dispenser" button for your Salesforce Accounts. It is implemented in the form of a button in the Account layout that a sales agent can click to create an on-demand coupon and store it in a custom field of your Accounts.

== 1. Create a Custom Field on the object of your choice (In this example, we're using the Account object)
- Go to Setup -> Customize -> Accounts -> Fields
- Click "New" in the Account Custom Fields & Relationships section
- Select "Text" as the Data Type and click Next
- Enter "Coupon" (or a label of your choice) as the Field Label and Length 32. The Field Name can also be "Coupon" (it will be named "Coupon__c" internally).
- Click Next and save the field.

== 2. Create an Apex Class that you can use to talk to the Talon.One API.
- Go to Setup -> Develop -> Apex Classes
- Click "New" and paste in all of the example from the Talon.One Apex SDK: https://raw.githubusercontent.com/talon-one/TalonOne.apxc/master/TalonOneManagementAPI.apxc
- Customize the first line of the function NewCouponService() that is located at the end of the file:

````
TalonOneManagementAPI t = new TalonOneManagementAPI('yourdomain', 'email@example.org', 'password', 1, 1);
````

- Change "yourdomain" to your Talon.One subdomain name, "email@example.org" and "password" to the login credentials for the Talon.One user that should act as the API user. It is good practice to create a Talon.One user especially for this role. The last two parameters are the Application ID and the Campaign ID, in that order. You can find these numbers in the Developer section of Campaign Manager after creating an Application and a Campaign that will contain your coupons.
- Save the Class.

== 3. Create a Button for the Account Layout that calls the Talon.One Apex Class
- Go to Setup -> Customize -> Accounts -> Buttons, Links, and Actions
- Click "New Button or Link". Choose a descriptive name for the Label, for example "Create Coupon", and set the Name field to "Talon_One_Coupon". 
- As Behaviour choose "Execute JavaScript" and "OnClick JavaScript" as the Content Source.
- Paste the following code into the big code text field:

````
{!REQUIRESCRIPT("/soap/ajax/29.0/connection.js")}
{!REQUIRESCRIPT("/soap/ajax/29.0/apex.js")}
var result = sforce.apex.execute("TalonOneManagementAPI","NewCouponService",{id:"{!Account.Id}"});
alert(result);
````

- This script will pass the Account's Id to the NewCouponService function of the TalonOneManagementAPI Apex Class.
- The code executed by NewCouponService will ask the Talon.One API to create a new coupon and store it in the Account's custom Coupon__c field if everything goes well.
- It will also display the result of the coupon request as an alert.

== 4. Save the button and insert it into the Account Layout
- Go to Setup -> Customize -> Accounts -> Page Layouts -> Edit
- Select the Buttons section in the toolbox at the top and drag the "Create Coupon" button to an appropriate place in your Layout.
- Save and enjoy creating Talon.One Coupons directly from Salesforce!
