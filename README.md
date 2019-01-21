# LiveProject
My final project with the Tech Academy was a two week Live Project, where I worked on a Clock in/Schedule Management MVC program with other team members using Visual Studio in C# and the .NET framework. I worked on a variety of code, from front-end to back-end, as well as bug fixing; user stories were shared among the group so that everyone had a chance to work on various types of code. Working on a large project midway through it's lifecycle taught me that you need to be adaptable and open to change in order to best serve the project's and team needs.

Below are some examples of code from the project:

Bug Fix:
The bug was to remove this part of the code: @Html.DisplayNameFor(model => model.UnreadMessage)
 
 
 			
        <th>
            @Html.DisplayNameFor(model => model.DateRead)
        </th>
        <th>
            @Html.DisplayNameFor(model => model.MessageTitle)
        </th>
            @Html.DisplayNameFor(model => model.Content)
        </th>
        <th>
            @Html.DisplayNameFor(model => model.UnreadMessage)
        </th>
        
        <th></th>
    </tr>

    
  To then look like this:
  <th>

            @Html.DisplayNameFor(model => model.DateSent)
        </th>

        <th>
            @Html.DisplayNameFor(model => model.DateRead)
        </th>

        <th>
            @Html.DisplayNameFor(model => model.MessageTitle)
        </th>

        <th>
            @Html.DisplayNameFor(model => model.Content)
        </th>

Back-end:
Make Work Time Event / Index View visible to Admins only:
    
    @model IEnumerable<ScheduleIt2.Models.WorkTimeEvent>
    @if(User.IsInRole("Admin"))
    {
        <li><a asp-area="" asp-controller="Admin" asp-action="Index">Admin</a></li>
    }
    @{
        ViewBag.Title = "Index";
    }
    
On the Work Time Event Controller, change the Create function so that the Id property gets set to the Id of the user logged in:

      using System.Web;
      using System.Web.Mvc;
      using ScheduleIt2.Models;
      using Microsoft.AspNet.Identity;
      namespace ScheduleIt2.Controllers
      {
        //Changed the Start parameter so it is set to current time.
                workTimeEvent.Start = DateTime.Now;
                workTimeEvent.EventID = Guid.NewGuid();
                workTimeEvent.Id = User.Identity.GetUserId();
                db.WorkTimeEvents.Add(workTimeEvent);
                db.SaveChanges();
                return RedirectToAction("Index");
            }

Front End: 
On the Message / Create View, remove the Date Sent and Date Read input boxes. Add a "Send To" input that accepts an email address:
           
           [Display(Name = "Message Title")]
            public string MessageTitle { get; set; }
            public string Content { get; set; }
            [Display(Name = "Send To")]
            public string SendTo { get; set; }
            public string UnreadMessage { get; set; }
            public ApplicationUser Sender { get; set; }
            public ApplicationUser Recipient { get; set; }

            <h4>Message</h4>
            <hr />
            @Html.ValidationSummary(true, "", new { @class = "text-danger" })
            <div class="form-group">
                @Html.LabelFor(model => model.DateSent, htmlAttributes: new { @class = "control-label col-md-2" })
                <div class="col-md-10">
                    @Html.EditorFor(model => model.DateSent, new { htmlAttributes = new { @class = "form-control" } })
                    @Html.ValidationMessageFor(model => model.DateSent, "", new { @class = "text-danger" })
                </div>
            </div>
            <div class="form-group">
                @Html.LabelFor(model => model.DateRead, htmlAttributes: new { @class = "control-label col-md-2" })
                @Html.LabelFor(model => model.SendTo, htmlAttributes: new { @class = "control-label col-md-2" })
                <div class="col-md-10">
                    @Html.EditorFor(model => model.DateRead, new { htmlAttributes = new { @class = "form-control" } })
                    @Html.ValidationMessageFor(model => model.DateRead, "", new { @class = "text-danger" })
                    @Html.EditorFor(model => model.SendTo, new { htmlAttributes = new { @class = "form-control", @type = "email", @placeholder = "example@gmail.com" } })
                    @Html.ValidationMessageFor(model => model.SendTo, "", new { @class = "text-danger" })
                </div>
            </div>

Create missing form for Schedule Template:

       <div class="mb-100">
            <div class="form-group">
                  @Html.LabelFor(model => model.ScheduledWorkPeriods, new { @class = "control-label col-md-2" })
                      <div class="col-md-10">
                          @Html.TextBoxFor(model => model.ScheduledWorkPeriods, new { htmlAttributes = new { @class = "form-control" }})
                            @Html.ValidationMessageFor(model => model.ScheduledWorkPeriods)
                            <p>MM/DD/YYYY</p>
                        </div>
                    </div>
                </div>

Have Title of scheduled event displayed in User's Index page:

     @model IEnumerable<Schedule_It_2.Models.ScheduleTemplate>

     @{
        ViewBag.Title = "Index";
      }

      <h2>Index</h2>

      <p>
      @Html.ActionLink("Create New", "Create")
      </p>
      <table class="table">
          <tr>
              <th>
                  @Html.DisplayNameFor(model => model.Title)
              </th>
              <th></th>
          </tr>

          @foreach (var item in Model)
          {
              <tr>
                   <td>
                       @Html.DisplayFor(modelItem => item.Title)
                   </td>
             <td>
                @Html.ActionLink("Edit", "Edit", new { id = item.ScheduleTemplateId }) |
                @Html.ActionLink("Delete", "Delete", new { id = item.ScheduleTemplateId })
             </td>
             </tr>
          }

    </table>
                  
