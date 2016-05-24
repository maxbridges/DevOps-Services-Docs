#Track and plan a Bluemix DevOps Services project

###### Last updated: 01 December 2015

Time: 30 minutes

You can manage any type of project with an agile approach by using the Track & Plan feature in IBM&reg; Bluemix&trade; DevOps Services. The Track & Plan tools simplify project planning and speed your workflow. For example, you can drag work items from one project phase to another, such as from your project backlog to a sprint. 

<!-- After you enable the Track & Plan feature, you can start getting organized right away by creating and assigning work items for your project. -->

<!-- The Track & Plan feature is useful for supporting both large and small teams, and for managing complex and simple projects. -->

In this tutorial, you create a public, agile project that uses a Git repository. After you add the Track & Plan feature to the project, you plan a chat page for a website by creating and managing work items, triaging your backlog, and planning sprints. 

<!--- Commenting out until feature explorer is updated. -->
<!--- For a quick overview of features and functions, [explore the Track & Plan feature][9]. -->

<div class="table-of-contents">
 <table>
   <tr>
     <td colspan="8"><h4>Summary of steps</h4></td>
   </tr>
   <tr>
     <td><a href="#prereq">Before you begin</a></td>
     <td><a href="#set_up_an_IBM_devops_services_project">Set up a project</a></td>
     <td><a href="#create_your_first_work_items">Create your first work items</a></td>
     <td><a href="#create_work_items_in_the_backlog">Create parent and child work items</a></td>
     <td><a href="#triage_the_backlog">Triage the backlog</a></td>
     <td><a href="#plan_sprint_1">Plan Sprint 1</a></td>
     <td><a href="#work_through_the_sprint">Work through the sprint</a></td>
     <td><a href="#summary">Summary</a></td>
   </tr>
 </table>
</div>

<a name='prereq'></a>
##Before you begin
 
To do this tutorial, you need a Bluemix account. Bluemix accounts provide access to everything you need to develop, track, plan, and deploy apps. You can sign up for a free 30-day trial.  

<h5> </h5>
<div class="container-fluid small_bottom_space">
   <div class="row pbl button-links" id="overview-links">
		<a href="https://login.jazz.net/psso/proxy/jazzregister?redirect_uri=https%3A%2F%2Fhub.jazz.net%2F" target="_blank" alt-text="Sign up"> 
			<div class="hollowButton">SIGN UP<div class="extra-title">for Bluemix </div>
			</div>
		</a>
   </div>
</div>
<h5> </h5>

<a name='set_up_an_IBM_devops_services_project'></a>
##Set up a DevOps Services project

1. [Sign in to DevOps Services][1]. Your Projects page opens.  
![Bluemix DevOps Services new user landing page][2]

2. Start a new project by clicking the **Start coding** icon.  
If you see a list of projects, click **CREATE PROJECT**.

3. Name the project `ChatPage`.

4. Create a repository for your code by clicking the **Create a new repository** icon.  

5. Choose your repository location by clicking the **Create a Git repo on Bluemix** icon.   

5. Clear the **Private** check box.   
DevOps Services supports both public and private projects. Anyone can access your public projects, but only the DevOps Services users who you invite can access your private projects.  

6. Ensure that the **Add features for Scrum development** check box is selected. By enabling Scrum development, you can create sprints from your project's backlog of work items.  
**Note**: To use the full Track & Plan capabilities, you must enable Scrum development. However, if you plan traditional projects that use simple tasks and iterations without a backlog, clear this check box.

7. Ensure that the **Make this a Bluemix Project** check box is cleared.  

8. To set up your project and go to the Overview page for the ChatPage project, click **CREATE**.  

     ![New project selections][4]

9. To include other people in your project, click **Invite others to join your project**, enter their email addresses, and click **INVITE**.

<a name='create_your_first_work_items'></a>
##Create your first work items

1. Click **TRACK & PLAN**. 

2. Review the My Work view. From this view, you can see any project work items that are assigned to you. Because you're the only person who is working on this project, assign ownership of all work items to your alias. The fastest way to assign all of the work items to your alias is to create your work items from this view.

2. In the **Create a work item** field, type a summary for the first work item: `Create a simple chat page for our site.` Press **Enter** to create the task.  
**Note**: The default work item type is **Task** <img class="inline"  src="images/task.gif"  alt="Task type icon">. 

3. Add more attributes to your task. Click the
**Priority** icon <img  class="inline" src="images/nopriority.gif" alt="Priority icon" > to set the priority to **High**. 

4. Click the tag icon <img  class="inline" src="images/tag.gif" alt="Tag icon"> and in the tag field, type `website`. To assign the tag, press **Enter**.  
**Tip**: A tag can help you group related work items. For example, you can use the filtering options to view only the items that have a certain tag.    
<!--- Commenting out videos until they are updated -->
<!--- <iframe width="640" height="360" src="//www.youtube.com/embed/EvVPxazK5jU" frameborder="0" allowfullscreen></iframe>
<div align = center>Video: Adding attributes to an existing work item</div> -->

6. In the **Type a work item summary** field, enter the summary for the next work item: `Incoming messages are not reaching other chat participants.`

7. Hover over the icons that are under the summary.  
![Set Attributes at work item creation using these][5]  
You use these icons to set the work item attributes. Don't worry if you aren't sure about an attribute or think one might change. You can always change attributes later.

8. Use the icons to set these parameters for the work item:
 * Set the type to **Defect** <img  class="inline" src="images/defect.gif" alt="Defect type icon">.
 * Set the severity <img  class="inline" src="images/severity.gif" alt="Severity icon"> to **Major**.   
**Note**: You can assign severity only to defects.
 * Add a tag <img class="inline"  src="images/tag.gif" alt="Tag icon"> named `messages`. 

 * Set the priority <img class="inline"  src="images/priority.gif"  alt="Priority icon"> to **High**. When you set an attribute, the text code for that attribute is added after the summary.  
![Defect summary appended with attribute codes][7]

5. When you're finished, click **CREATE**.  
<!--- Commenting out videos until they are updated -->
<!--- <iframe width="640" height="360" src="//www.youtube.com/embed/t5xJnL_jinc" frameborder="0" allowfullscreen></iframe>
<div align = center>Video: Adding attributes while you create a work item</div> -->

6. Click **Incoming Work** and evaluate your new work items. They are ranked by severity so that you know which tasks are most important. You can send incoming work items to your backlog to be included in a sprint <img class="inline"  src="images/triagetobacklog.gif" alt="Send to backlog icon""> or you can delete <img class="inline"  src="images/trash.gif" alt="Delete work item icon"> them, which marks the work item as invalid and moves it to the **TRASHED** section.

  ![Incoming Work][26]

7. For each work item, click the **Triage to Backlog** icon <img class="inline" src="images/triagetobacklog.gif" alt="Triage to Backlog icon">.

<a name='create_work_items_in_the_backlog'></a>
##Create parent and child work items from the backlog
 
1. Create a story for your project:
 
  1. Click **Backlog**, and then click in the **Create a work item** field.

  2. Type a summary for the next work item: `As a user, I'd like your site to have a chat page.`  

  3. Make yourself the work item's owner <img  class="inline" src="images/owner.gif" alt="Make yourself the owner icon"> and add a `website` tag <img class="inline" src="images/tag.gif" alt="Add a website icon">. Set the priority <img class="inline" src="images/priority.gif" alt="Set priority icon"> to **Medium**.  

  4. Confirm that the work item type is "Story." If the type is not story, click the **Task** icon <img  class="inline" src="images/task.gif" alt="Task icon"> and then click the **Story** icon <img class="inline"  src="images/story.gif" alt="Story icon">.

  5. Click **CREATE**.  

2. Assign child work items to your story.

  1. Click the summary of your story, "As a user, I'd like your site to have a chat page," to open the work item editor.  
**Tip**: You can change most of the attributes on the **OVERVIEW** tab.  

     ![Work item editor page][8]    

  2. Click the **LINKS** tab, and then click the down arrow icon next to **Add Related**.

  3. Scroll down and select **Add Children**.

  4. In the search field, type `chat page`. From the results, select "Create a simple chat page,"  and click **OK**.

  5. Click **SAVE** and then click **Back to Backlog** <img class="inline"  src="images/restyle_backtobacklog.png" alt="Back to Backlog icon">.

  6. Click in the **Type a work item summary** field and type `Configure a chat server using Node.js.` 

  7. Slowly type `*task` to assign the task type. When you type the asterisk (` * `), notice how the **Type** menu is shown.  
**Tip**: You can enter most of the attributes by typing their text code instead of using the mouse. 

  8. Enter these attributes:
    * @:*< yourAlias >* 
    * \#server 
    * $high 

  8. Click **CREATE**.

  9. Make the "Configure a chat server using Node.js" task a child of the "As a user, I'd like your site to have a chat page" story. Click the task's title field and drag the task to the story's title field.
     ![Assign child relationship by dragging task][24]  
     
3. Add a few more work items to complete this project.

  1. Add the following work items and attributes by using the method that you like best. 
    * `As a user, I'd like to be able to log in to your chat with my Facebook or Google+ account. #login $high` 
    * `Integrate OAuth to the chat site. *task #login #website $high`
  2. Make the new task a child of the new story, "As a user, I'd like to be able to log in to your chat with my Facebook or Google+ account."

  3. Refresh your browser and then click the plus signs (+) next to your story summaries to expand your parent work items.
Make sure that your backlog looks like this image:  
     ![Unranked work items in the backlog][10]

<a name='triage_the_backlog'></a>
##Triage the backlog

1. Finish assigning work items owners.

  1. In the Filter work items by keyword field, type `@:unassigned` and click **SAVE**.  
![Empty text filter field][14]

  2. Next to your story summary, click the plus sign (+).

  3. For each work item, click the **Owner** icon and select your name from the list. After you assign your items, click in the filter field and click the **X** to clear the `@:unassigned` filter. When you work with other team members, you can use this method to assign related work items to them.  
**Tip**: If you want to see this view again, click **SAVE** to store it as a custom view. 

2. Create time estimates to ensure that you assign an appropriate number of stories to a sprint.

  1. Click in the title field of the "Integrate OAuth to the chat site" item. From the menu that opens, click **Estimate: Unassigned**.  
**Tip**: When you create estimates, remember to review your story points and work item owners.

  2. Set the duration to 3 days and click **OK**.  
     ![Location of Estimate: unassigned][12]  

   3. Assign 3-day estimates to your defect and remaining tasks.

3. Before you assign work items to a sprint, rank the work items in your backlog. 

  1. Collapse the parent items and notice how each work item's rank is not ranked. You can easily change this status. 
  2. Drag the "Incoming messages are not reaching other chat participants" task to the top of the list and see how its rank changes to 1.  <!--- Commenting out videos until they are updated -->
<!--- <iframe width="640" height="360" src="//www.youtube.com/embed/twr30-5wcfc" frameborder="0" allowfullscreen></iframe>
<div align = center>Video: Ranking work items in the backlog </div> -->

4. Rank the rest of the work items in this order:  
     ![Ranked work items in the backlog][11]
     
5. Assign story points to the stories. In agile projects, story points are a team-determined scale that reflects the effort that is required to implement a story. Next to each story, click **0 pts** and select **20 pts** from the list.  
**Note**: You can expand each story by clicking the plus sign (+) to review its child items.

<a name='plan_sprint_1'></a>	
##Plan Sprint 1

1. To start creating sprints for your project, click **Sprint Planning** and then click **Add Sprints**. From the **Quantity** and **Duration** lists, select two 2-week sprints.  
**Note**: Only a project owner or project admin can create and edit sprints.

  ![Sprint creation menu that shows two 2-week sprints][27]

2. Click **Save**. Lanes for your backlog and for Sprint 1 are shown. Scroll to the right to see Sprint 2.

3. In the backlog lane, click the title bar of the "Incoming messages are not reaching other chat participants" defect and drag it to the Sprint 1 lane.

4. Next to each story, click the plus sign (+) to show the child tasks.

4. Drag the rest of the work items to Sprint 1 in ascending order by rank.  
<!--- Commenting out videos until they are updated -->
<!--- <iframe width="640" height="360" src="//www.youtube.com/embed/SLQVK6V3qME" frameborder="0" allowfullscreen></iframe>
<div align = center>Video: Populating a sprint</div> -->

<a name='work_through_the_sprint'></a>
##Work through the sprint
 
You set up your sprint and are ready to start work.

1. To see the entire team's work for the current sprint, click **Team's Work**. This view is useful when you share work because work items are grouped by owner and you can see who owns which tasks.  
**Tip**: To view another sprint, click **Sprint 1 (Current Sprint)** and select a sprint.   

2. To see the number of hours that were worked, items that were resolved, and story points that were achieved, click **Team Progress**.

     ![Team progress showing current hours worked, items resolved, and story points achieved][25]
     
3. Click **My Work**. Because you own all of the tasks and you haven't started to work on them, you can see all of them in your Open lane.  
**Note**: If you do not see both the Open and In Progress lanes, click the **Display as Lanes** icon <img class="inline"  src="images/displaylanes.png" alt="Display as Lanes icon"> to switch from the list view.

3. For the "Integrate OAuth to the chat site" task, click the **Status** icon <img class="inline"  src="images/statusnew.png" alt="Status icon"> and from the menu that opens, click **Start Working**.

4. Notice how this item moves from the Open lane to the In Progress lane.

5. As you work on your tasks, update the number of hours spent on each task. Find the "Integrate OAuth to the chat site" task and click in its title bar to expand the menu. Use the **Time Spent** option to show that you spent 16 hours on the task.

6. Close the "Integrate OAuth to the chat site" task by clicking the **Status** icon <img  class="inline" src="images/statusopen.gif" alt="Status icon"> and click **Complete** from the menu.  
**Tip**: You can close a task from the Team's Work view, too. You might find the ability to assign hours and complete tasks useful during your daily scrum meeting.

7. Find the "Integrate OAuth to the chat site" task. Click its **Open parent task breakdown** icon.

  ![Location of open parent task breakdown icon][28]

8. Review the contents of the task view. You completed the only task that is associated with this story, so you can resolve the story.

9. Click **Back to My Work**.

10. For the "As a user, I'd like to be able to log in to your chat with my Facebook or Google+ account" story, click the Status icon.  Resolve the story by clicking **Set Done** from the menu. 

11. Create work items and address them.

  1. Click **Incoming Work**.

  2. Click in the **Create a work item** field and create three work items:
     1. `As a user, I'd like your chat site to look good on my phone. #mobile $low`
     1. `Test site on mobile devices and optimize Bootstrap and CSS files to improve performance. *task #mobile #website $low`
     1. `The server handles the requests too slowly. *defect #server $high !major` 

  3. Send all of the work items to the backlog by using the **Triage to Backlog** icon.

  4. Take a second look at the work items that you created. The defect seems important. Click **Sprint Planning** and drag the defect from the Backlog lane into the Sprint 1 lane.  
**Tip**: If you are leading a team, you can triage new work items in the Incoming Work view and the Backlog view during your sprints.

<a name='summary'></a> 
##Summary

You have a good overview of the Track & Plan feature and see that you can use it to track any type of project, no matter how small or complex.

<a name='nextsteps'></a> 
##Next steps

To learn more about the Track & Plan feature, see [Track and plan](/docs/trackplan).


[1]: https://hub.jazz.net
[2]: /tutorials/trackplan/images/myprojectslanding.png

[4]: /tutorials/trackplan/images/restyle_newprojectselection.png
[5]: /tutorials/trackplan/images/restyle_wiicons.png
[6]: https://bluemix.net/#/pricing/cloudOEPaneId=pricing 
[7]: /tutorials/trackplan/images/restyle_appendeddefect.png
[8]: /tutorials/trackplan/images/restyle_workitemeditor.png
[9]: /docs/trackplan/trackplanfg/
[10]: /tutorials/trackplan/images/restyle_unrankedbacklog.png
[11]: /tutorials/trackplan/images/restyle_rankedlist.png
[12]: /tutorials/trackplan/images/restyle_estimateunassigned.png

[14]: /tutorials/trackplan/images/restyle_querybar.png
[15]: https://hub.jazz.net/tutorials/jazzeditor
[16]: https://hub.jazz.net/tutorials/jazzeditorjava
[17]: https://bluemix.net/
[18]: https://www.ibmdw.net/answers?community=Devops_services (Bluemix DevOps Services forum)
[19]: mailto:hub%40jazz.net
[22]: https://ace.ng.bluemix.net/#/pricing

[24]: /tutorials/trackplan/images/restyle_dragdropchild.png
[25]: /tutorials/trackplan/images/restyle_teamprogress.png
[26]: /tutorials/trackplan/images/restyle_incomingwork.png
[27]: /tutorials/trackplan/images/restyle_editsprints.png
[28]: /tutorials/trackplan/images/restyle_parentbreakdown.png


[40]: /tutorials/jazzrtc (Developing Bluemix applications in Java with Eclipse and Bluemix DevOps Services)
[41]: /tutorials/jazzeditor (Getting Started with Bluemix and Bluemix DevOps Services using Node.js)
[42]: /tutorials/clients (Setting up Eclipse, Git, and Rational Team Concert Desktop Clients to access Bluemix DevOps Services)
[43]: /tutorials/jazzweb (Developing Bluemix applications in Node.js with the Bluemix DevOps Services Web IDE)
[44]: /tutorials/jazzeditorjava (Getting Started with Bluemix and Bluemix DevOps Services using Java)
