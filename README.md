# Handy Flow Steps Volume 2
Another collection of ten custom steps for use with xMatters Flow Designer. The steps are designed to make working with xMatters users, groups and the xMatters API (xMAPI) easier. Whereas the steps in Volume 1 were generic, the steps in volume 2 all make use of xMAPI. If you're an xMatters admin, these steps will make your life easier. 

As with [Handy Flow Steps Volume 1](https://github.com/xmatters/xm-labs-handy-flow-steps), the steps come with a form-based test interface, called Run Flow Step kicker, which lets you select and try out each step with different input values, then view the output either via an email alert or the flow designer activity log.

---------
<kbd>
  <a href="https://support.xmatters.com/hc/en-us/community/topics"><img src="https://github.com/xmatters/xMatters-Labs/raw/master/media/disclaimer.png"></a>
</kbd>

----------

# Pre-Requisites
* Access to xMatters Flow Designer. Because these steps use the built-in xMatters endpoint to access xMAPI, you cannot use them on Everbridge Flow Designer.
* xMatters account - If you don't have one, [get one for free](https://www.xmatters.com/free)!

# Files
* [xMattersHandyFlowStepsvolume2.zip](xMattersHandyFlowStepsvolume2.zip) - xMatters workflow containing 2 forms, 1 flow canvas and 11 custom flow steps. Even though we said ten steps, you get a free bonus step, Stringify Object, used to format results.

# Custom Flow Steps
The custom flow steps are...
*	Get Site ID
  
	<kbd>  <img src="/media/get_site_id.png" width="200"> </kbd>
 
Every user in xMatters must be assigned to a site, even if that site is just the built-in Default Site. Use this step to obtain a site's UUID from its name. The UUID is required for xMAPI calls. Unusually, you cannot use the site's name in most xMAPI calls. Nor can you obtain the Site's UUID by looking in the xMatters UI, so retrieving via API is the only way. The step also returns a full site object with all site details, e.g. address. The site object is easy to parse in a subsequent step. You can run the <b>Get Site ID</b> step in reverse too, i.e. supply a UUID as input instead of a name.

*	Create xM User
  
	<kbd>  <img src="/media/create_xm_user.png" width="200"> </kbd>
 
Create a new user in xMatters, complete with an associated Work Email device.  Although this doesn't include every single "Person" field available through the API, we hope it includes the most useful ones.  The flow step is more forgiving than the naked API in several respects, e.g. you can pass in user names for Supervisors and a site name for Site (rather than UUIDs).  If no roles are specified, the Standard User role is assumed. Choose between Stakeholder and Full User licence types. You can even set custom property values on the user profile, e.g. Job Title, via a JSON object, although the custom properties have to be pre-congfigured in the UI. Please be aware, creating users via xMAPI counts towards your total User License count. You can view your count under Admin > General Settings. When you have reached your licese count, xMAPI will not allow you to create any more users. You can either use the step below, Delete xM User, to reduce the number of users in the system. Alternatively, your Everbridge account manager will be happy to discuss other options.

*	Delete xM User

	<kbd>  <img src="/media/delete_xm_user.png" width="200"> </kbd>
 
Delete a user from xMatters with either the User ID (aka user name, targetName, login) or UUID. This is a permanent operation. When a user has been deleted, they cannot be restored. You can recreate an equivalent account with Create xM User step but the underlying UUID will be different and the account history will be lost.

*	xM Get User

	<kbd>  <img src="/media/xm_get_user.png" width="200"> </kbd>
 
Get an xMatters user object by passing in either the User ID (a.k.a. username or targetName) or UUID.


*	xM Get User From Name

	<kbd>  <img src="/media/xm_get_user_from_name.png" width="200"> </kbd>
 
Get an xMatters user object by passing in a proper name, e.g. Taylor Swift. If you pass just one name, it is assumed to be the first name. Therefore both "Taylor" and "Taylor Swift" return Taylor Swift's user object. However, "Taylor Sw" does not work as the last name is Swift, not Sw. This step is not case-sensitive. 

*	Is User Member of Group?
	<kbd>  <img src="/media/is_user_member_of_group.png" width="2)0"> </kbd>
 
Is the User a member of a certain group in xMatters? The step tries to offer a reason the user is/isn't a member, e.g. no such group, which can be useful for trouble-shooting.

*	xM Remove User from SOME Groups

	<kbd>  <img src="/media/xm_remove_user_from_some_groups.png" width="250"> </kbd>
 
Remove a user from one of more xMatters groups. Also lets you remove directly-targeted devices, e.g taylor.swift@example.com . You can specify a user, a specific device, or a device type, or an 'AND' combination of user + device + types If you pass in a device name, any user who happens to have that device directly targeted will also be removed from the group. If you pass in only a deviceName/deviceType, it removes all such devices from the group. Inputs are not case-sensitive.

*	xM User Set Active

	<kbd>  <img src="/media/xm_user_set_active.png" width="200"> </kbd>
 
Ensure an xMatters user account is active. Can also be used to do the reverse, i.e. set inactive. You can even ask the step to toggle, i.e. flip the switch the other way. N.B. Inactive users still consume a license. The only way to reduce license-count is to delete the user.

*	Get Users From Email

	<kbd>  <img src="/media/get_users_from_email.png" width="400"> </kbd>
 
Returns a list of all xMatters users who have devices matching an email address. As there is no uniqueness requirement for email devices in xMatters, the same email can be attached to mulitple users. N.B. This is a combination of two steps, not a single step. The first step is the generic xMAPI Get Big Result, used to query every email in the instance and filter the results. The second step, Get Users From Email, parses the results. Think of it as a worked-up example with xmAPI Get Big Result. You can experiment with xMAPI Get Big Result in the section below.

*	xMAPI Get Big Result

	<kbd>  <img src="/media/xmapi_get_big_result.png" width="200"> </kbd>
 
Runs an arbitrary GET query from the xMatters API. Does a lot of the donkey-work for you, i.e. handles paging, trims the result object, filters the result set by searching for a specific property/value needle in the haystack.

*  Stringify Object

	<kbd>  <img src="/media/stringify_object.png" width="200"> </kbd>
 
Bonus step. Nothing to do with xMAPI. Used to stringify a JSON object, so it displays nicely in the results email. Without it, an object would be shown as \[Object object\].  


# Installation
1. Log into xMatters as a user with either the Developer or Full Access User role. Navigate to Workflows, click the Import button on the top right and import the [xMattersHandyFlowStepsvolume2.zip](xMattersHandyFlowStepsvolume2.zip) file. This is what you will see in the list of workflows:

 <kbd>  <img src="/media/xmatters_handy_flow_steps_volume2.png" width="850"> </kbd>
	
2. OPTIONAL. Navigate into the new workflow, either via the Open Workflow button after importing or from the workflow list. You will see a list of two forms. You need to set Sender Permissions on both forms. For <b>Run Flow Step kicker</b>, click the left-most of the two buttons to the right of the form (says Web UI) and then Sender Permissions at the bottom of the pop-up. Add the users or roles who can use the kicker form (form-based test interface). We recommend allowing all those with Full Access User or Developer roles to use the form. If you prefer, you can name specifc users. Do the same for the other form, <b>Run Flow Step results</b>. The same users/roles should be selected. (If you don't set Sender Permissions, only the user who imported the workflow can use/edit it.)

 <kbd>  <img src="/media/sender_permissions.png" width="350"> </kbd>
 
	
3. OPTIONAL. By default, only the importing user has permission to use and modify the custom steps within the workflow. We recommend broadening permissions to allow all users with the Developer role to be able to use and/or edit the new flow steps. To do this, shift to the FLOW DESIGNER tab, just to the right of FORMS. Open the <b>Run Flow Step</b> flow canvas.

	<kbd>  <img src="/media/run_flow_step_canvas.png" width="750"> </kbd>
 
	
4. To the right of the screen on the Palette, highlight the CUSTOM tab. Navigate to each new flow step in turn. Click the gear icon then Usage Permissions. In the pop-up window, grant ACCESS to other users/roles as required, e.g. select the Developer role and grant permission to Edit Step.
 
	<kbd>  <img src="/media/step_usage_permissions.png" width="500"> </kbd>


# Testing
You can try out each new custom step via the Run Flow Step kicker form.   Navigate to Messaging > xMatters Handy Flow Steps volume 2 > Run Flow Step kicker. Select a step in the <b>Step To Run</b> dropdown.  Each step has it’s own corresponding form section with some demo values pre-filled in it. You can use the demo values supplied or over-type with your own values. You can also edit the custom properties and choose more appropriate defaults for your system.

<kbd>  <img src="/media/run_flow_step_form.png" width="750"> </kbd>
	
At the bottom of the form, you can optionally set yourself as a Recipient. When you click Send Message, you will get an initial email notification confirming the flow has been launched. A short while later, a second email will arrive with the step’s output values clearly shown. Alternatively, you can leave Recipient blank and view step output in the Run Flow Step flow-canvas activity log.


# Troubleshooting
Navigate to xMatters Handy Flow Steps volume 2 > Flow Designer tab > Run Flow Step canvas. You can see the canvas used to select each step based on an initial Switch block. Each custom step outputs log notes. If a step fails, you can inspect the Activity Log and inspect the log messages. 

<kbd>  <img src="/media/run_flow_step_activity_log.png" width="750"> </kbd>

As per all xMatters Labs steps, the steps have been tested up to a certain point. They are provided on an as-is basis. We cannot guarantee they run in all situations. 
