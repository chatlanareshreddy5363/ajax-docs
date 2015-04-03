---
title: OnClientMouseOver
page_title: OnClientMouseOver | UI for ASP.NET AJAX Documentation
description: OnClientMouseOver
slug: treeview/client-side-programming/events/onclientmouseover
tags: onclientmouseover
published: True
position: 22
---

# OnClientMouseOver



## 

The __OnClientMouseOver__client-side event occurs when the mouse cursor passes over a node.

The event handler receives parameters:

1. The treeview instance that fired the event.

1. Event arguments with functions:

* __get_node()__ retrieves a reference to the node.

* __get_domEvent()__ retrieves a DOM event object of the mouse movement.

The example below retrieves text of the node the mouse is passing over and displays the text in a div.

````ASPNET
	
	    <script type="text/javascript" language="javascript">
	
	        function ClientMouseOver(sender, eventArgs) {
	            var node = eventArgs.get_node();
	            var myDiv = document.getElementById("myDiv");
	            myDiv.innerHTML = "OnClientMouseOver: " + node.get_text();
	        }
	    </script>
	
	    <telerik:RadTreeView ID="RadTreeView1" runat="server" OnClientMouseOver="ClientMouseOver">
	    </telerik:RadTreeView>
	    <br />
	    <div id="myDiv" style="width: 489px; height: 100px">
	    </div>
````



# See Also

 * [OnClientMouseOut]({%slug treeview/client-side-programming/events/onclientmouseout%})

 * [RadTreeNode]({%slug treeview/client-side-programming/objects/radtreenode%})