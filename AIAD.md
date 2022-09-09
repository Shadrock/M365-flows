# App in a Day Notes

Don't put spaces in lists. Mostly b/c the `%` shows?
Also, for choices, there is a "default value" setting, this may be important for triggering.
Gear Icon "in upper right" is actually in the red MC banner, not in the top line nav of the list, which is confusing

## Power Apps
https://make.powerapps.com to make your app.
Connecting to Sharepoint was fairly straight forward.
Good naming protocol: use 3 lower case letter for control (e.g. 'Form' = 'frm') then a descriptor.
Position of card is relative to form: It's done in X, Y. With `0,0` being the upper left. But all these are contained within each other so the `0,0` is always relative to the top left of the card, form, etc.

For things like form fields with a set date, you basically change that up in the formula bar like Excel. String values (e.g. form field names) always require quotes.

You can set conditional formatting, e.g. `If(radPriority.Selected.Value="High", true)`

Everything you do after the _trigger_ are "actions".

Under approvals, "custom responses" all for things like "approve", "reject", and then something custom like "delay".
All Sharepoint lists have an audit trail with Created on, Modified by, etc.
You can see Approvals, in Power Automate! See left nav to get to the list of them.
She's written flows w/ up to 300 actions. "You can use Scope to group sets of actions."
Hovering over the dynamic fill will show you the underlying expression. Useful for checking things.
You can add "notes" to your triggers / actions to be more descriptive.
All sharepoint lists have the `ID` variable but it doesn't show it unless you go into list settings to show it. This can be helpful for seeing what you're working with in Power Flow.
Something about only using "When an item is created" versus "update item" in dynamic content for sending an email...
meaning of `V2`(V2)? Version. It just means there is also a "send an email" action floating around that is an older version. Sometimes they don't retire the older version of a flow action because it would break all the flows out there that depend on it. But generally if you're making a flow from scratch you'll want to use the latest version of any given trigger or action

The person field is fairly complex... make sure you're seeing your name with a grey box, versus a string looking email.

she makes an array and loops through w/ first to respond, record answer, and counts the approvals. She's passing the loop from power apps: a sharepoint list with people who's role is "approver"... reference this. For each item in this array send this approver an email. If this is "create and wait" for approval is could be awhile. You could put a counter on it to cancel and mark a non-responsive person or

For a complex approval, they suggest using tasks and have a scheduled flow to test things every day.

Also, flows time out after 28 days! See [this blog post](https://www.c-sharpcorner.com/blogs/how-to-increase-powerautomate-timeout-more-than-30-days#:~:text=As%20we%20know%20flow%20instance,29%20days%20we%20will%20take) for handling time outs. 
