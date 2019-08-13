# Lightstep xMatters Integration

Trigger an xMatters Flow to alert xMatters groups and easily integrate with other applications when a Lightstep condition is met.

<kbd>
  <img src="https://github.com/xmatters/xMatters-Labs/raw/master/media/disclaimer.png">
</kbd>

# Pre-Requisites

- Lightstep [https://lightstep.com](https://lightstep.com)
- xMatters account - If you don't have one, [get one](https://www.xmatters.com)!

# Files

- [xMatters Lightstep Communication Plan](Lightstep.zip)

# How it works

Lightstep will create an xMatters alert when conditions in Lightstep are met.

# xMatters Configuration

### Create an xMatters Integration User

Although this step is not required, it is recommended. This will enhance security and ensure the integration does not unintentionally break when a password is changed.

**Note**: If you are installing this integration into an xMatters trial instance, you don't need to create a new user. Instead, locate the "Integration User" sample user that was automatically configured with the REST Web Service User role when your instance was created and assign them a new password. You can then skip ahead to the next section.

**To create an integration user:**

1. Log in to the target xMatters system.
2. On the **Users** tab, click the **Add New User** icon.
3. Enter the appropriate information for your new user.
   Example User Name **Lightstep_API_User**
4. Assign the user the **REST Web Service User** role.
5. Click **Save**.

<br><br>

## Import and Configure the xMatters Communication Plan

To import the communication plan:

1. In the target xMatters system, on the **Developer** tab, click **Import Plan**.
2. Click **Browse**, and then locate the downloaded communication plan: [Lightstep](Lightstep.zip)
3. Click **Import Plan**.
4. Once the communication plan has been imported, click **Plan Disabled** to enable the plan.
5. In the **Edit** drop-down list, select **Access Permissions**.

<kbd>
    <img src="/media/access_permissions.png" width="300">
</kbd>
<br><br>

6. Add the **Lightstep_API_User** or Make the Plan **Accessible by All**.
7. **Save Changes**.

<kbd>
    <img src="/media/set_access_permissions.png" width="300">
</kbd>
<br><br>

8. In the **Edit** drop-down list, select **Forms**.
9. For the **Lightstep Alert** form, in the **Not Deployed** drop-down list, click **Enable for Web Service**.

<kbd>
    <img src="/media/enable_ws.png" width="300">
</kbd>
<br><br>

10. After you Enable for Web Service, the drop-down list label will change to **Web Service**.
11. In the **Web Service** drop-down list, click **Sender Permissions**.
12. Add the **Lightstep_API_User** you created above, and then click **Save Changes**.

<kbd>
    <img src="/media/sender_permissions.png" width="300">
</kbd>

<br><br>

## Get the xMatters Inbound Integration Endpoint URL

1. In the **Edit** drop down list on the _Lightstep Communication Plan_ Click **Flow**.
2. Click on the _Lightstep Alert_ Flow.

<kbd>
    <img src="/media/access_flow.png">
</kbd>
<br><br>

3. On the Flow Canvas, Double Click on the step named **Lightstep Flow Trigger**. This is the inbound http trigger for the Flow.

<kbd>
    <img src="/media/trigger_step.png">
</kbd>
<br><br>

4. Change the _Authenticating User_ to the **Lightstep_API_User**

You must supervise this user and it must have a Web Service User Role. If you cannot select the **Lightstep_API_User** it is because you do not supervise that user or they do not have the REST Web Service User role. When you create a new user, you are automatically set as that users' supervisor. If you do not see the user that you created, most likely the REST Web Service role is missing.

5. Copy the URL listed under the **Trigger** section.

You will need this URL for configuring Lightstep.

  <kbd>
    <img src="/media/trigger_url.png">
  </kbd>

<br><br>

## Customize your xMatters Flow

By default, this integration will create an xMatters Alert with basic property values from Lightstep. You can customize this integration to include more properties and also add additional Flow Steps to post to chat ops tools, create tickets or anything else you can dream of.

**Note**:If you customize the flow, just make sure that you do not remove the **xMatters Create Event (Lightstep Alert)** Step. This step creates the actual xMatters alert. This step can come right after Create Lightstep Alert Step or after other steps that you add to the canvas, it's up to you.

_Here is an example of a possible Flow:_
<kbd>
<img src="/media/sample_flow.png">
</kbd>

## Customize **Lightstep Flow Trigger** Step

1. In the Triggers tab under HTTP Requests section, click the Cog and then Edit.

<kbd>
    <img src="/media/edit_trigger.png">
</kbd>
<br><br>

2. Go to the **OUTPUTS** Tab and add new **Step Outputs**. Outputs are values that will be available for subsequent steps.

<kbd>
    <img src="/media/create_outputs.png" width="550px">
</kbd>
<br><br>

**Note:** The Lightstep webhook format is predefined and most values are already part of this integration with the exception of **custom-data**. Custom Data can be defined in Lightstep for each notification stream.
This is already designed into the integration for the parameter **group**. This is how you determine the proper xMatters group to send an alert to when the integration is triggered.

For more information go here: https://docs.lightstep.com/docs/custom-data-on-saved-searches

<kbd>
    <img src="/media/custom_data.png" width="100px">
</kbd>
<kbd>
    <img src="/media/set_custom_data.png" width="250px">
</kbd>
<br><br>

3. Go to the **SCRIPT** tab and modify your script appropriately.

- Each Output added in Step 2 will need to be mapped to a value in your payload. This takes the incoming values from Lightstep and sets the xMatters properties to hold these values.

Example: _output['New Output'] = payload['Another value'];_

    output['New Output'] - this is an output added in step 2. The output name is "New Output"
    payload['Another value'] - this is a value in your payload with the key "Another value" sent to xMatters from Lightstep.

<kbd>
    <img src="/media/edit_script.png" width="650px">
</kbd>
<br><br>

## Add additional / custom properties to the Lightstep Alert Form.

1. Exit Flow Designer.

2. Navigate to the Forms section of Lightstep Communication Plan.

3. Beside Lightstep Alert Form, click **Edit** and go to **Layout**.

<kbd>
    <img src="/media/Lightstep_alert_form.png">
</kbd>
<br><br>

4. Create new Properties.

<kbd>
    <img src="/media/add_property.png">
</kbd>

5. Drag new Properties onto the Form.

6. Save Changes.

7. Make any required changes to **Messages** and **Responses**.

<kbd>
    <img src="/media/messages_responses.png" width="350px">
</kbd>

<br><br>

## Customize **xMatters Create Event (Lightstep Alert)** Step

Once you have added new properties to the Lightstep Alert form, they will become available in the **xMatters Create Event (Lightstep Alert)** Step for you to map payload properties to.

1. On the Flow Canvas double click the **xMatters Create Event (Lightstep Alert)** Step

<kbd>
    <img src="/media/create_event_step.png">
</kbd>
<br><br>

2. Drag items from the right to the appropriate fields on the left. Any new fields you have added to the form layout will be available here.

<kbd>
    <img src="/media/custom_create_event.png" width="650px">
</kbd>
<br><br>

# Configuring Lightstep

## Create a New Destination

Follow instructions here to create new Destination (Webhook):<br>
https://docs.lightstep.com/docs/webhook-configuration

Here is how you should configure your new Destination Webhook:

,,,
**Name**: xMatters Alert

**URL**:
Set the URL (hook url) field to the xMatters Inbound Integration Endpoint URL.
[Get the xMatters Inbound Integration Endpoint URL here](#get-the-xmatters-inbound-integration-endpoint-url)

**Special Note**: You can specify recipients and other parameters by adding url parameters at the end of the integration URL.
For instructions on how to do this see:
[Add URL Parameters to the Lightstep Destination Webhook](#add-url-parameters-to-the-lightstep-destination-webhook)

**Headers**: none
,,,

<kbd>
    <img src="/media/destinations.png" width="600px">
</kbd>
<br>
<kbd>
    <img src="/media/webhook.png">
</kbd>
<br>
<kbd>
    <img src="/media/create_destination.png" width="250px">
</kbd>
<br><br>

## Create New Condition

Follow instructions here to create new Alert Rule:<br>
https://docs.lightstep.com/docs/monitoring-and-alerting-in-lightstep#section-conditions

<kbd>
    <img src="/media/condition.png">
</kbd>
<br><br>
Make sure to __Add Alerting Rule__ and selecting the __xMatters (Desitination)__ __Webhook__ created in the last step.

<kbd>
    <img src="/media/alert_rule.png">
</kbd>

<br><br>

## Set xMatters Recipients in Lightstep

There are two ways to target xMatters Users and Groups from this integration.

1. Set Custom Data on your Lightstep Stream.
2. Add URL Parameters to the Lightstep Destination Webhook.

You can use either one or both of these methods. You must use at least one method, or your integration will not target anyone and will fail.

### Set Custom Data on your Lightstep Stream.

1. In Lightstep, go to the stream you have configured conditions that will alert the xMatters Destination Webhook.

<kbd>
    <img src="/media/condition.png" width="650px">
</kbd>
<br><br>

2. Click **Custom Data** button.

<kbd>
    <img src="/media/custom_data.png" width="100px">
</kbd>
<br><br>

3. Add Custom Data with Key Name **"Group"**.

4. Set **Group** key to the name of the xMatters group or user you want to notify. You can add multiple groups / users by separating with a comma.

<kbd>
    <img src="/media/set_custom_data.png" width="250px">
</kbd>
<br><br>

For more information on adding custom data go here: <br>
[https://docs.lightstep.com/docs/custom-data-on-saved-searches](https://docs.lightstep.com/docs/custom-data-on-saved-searches)

### Add URL Parameters to the Lightstep Destination Webhook.

You can specify recipients and other parameters by adding url parameters at the end of the destination webhook URL. See [Create a New Destination](#create-a-new-destination)

This integration is specifically configured to look for a url parameter **"recipients"** and add specified recipients to the xMatters alert.

This will help you customize the target xMatters groups / users from the Destination URL and make targeting appropriate people easier. You could create multiple destination url's targeting different groups / users in xMatters.

- You can specify xMatters **Groups** as well as **Users**.
- Specify more than one by separating with a comma.
- Add the recipients url parameter to the end of the inbound integration url.

**Examples**:

- Target a single group<br>
  **&recipients=Database**

- Target a single user<br>
  **&recipients=jsmith**

- Target a group and a user. Notice each value is comma separated.<br>
  **&recipients=Database,jsmith**

- To target a recipient with a space in the name, you must URL encode the space.<br>
  **%20** is the url encoded value for a space. If you have other special characters they could cause a problem and should be encoded. Here is a character encoded to assist you
  https://meyerweb.com/eric/tools/dencoder/

**&recipients=Cloud%20Devops**

**Full Example**:<br>
https://company.cs1.xmatters.com/api/integration/1/functions/769bc32d-07bc-4a720/triggers?apiKey=b6d5-3f1bd65ae91d&recipients=Cloud%20Devops

**Other Notes**:<br>

- Make sure to include the **&** before recipient or any other parameter you decide to add.

- If you want to add additional parameters, you can do so by adding **&parameter=value**<br>
  This is an easy way to add parameters like severity so xMatters can take different workflows for different values.

- If you add additional parameters you will need to also follow instructions here [Customize **Lightstep Flow Trigger** Step](customize-lightstep-flowtrigger-step)

# Troubleshooting

Trigger a new Lightstep Alert and check that it makes its way into xMatters.

You can check the Activity Log of Flow Designer with these instructions.
https://help.xmatters.com/ondemand/xmodwelcome/flowdesigner/activity-panel.htm
