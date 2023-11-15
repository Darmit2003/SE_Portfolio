# Project Work 4

Week 4 of the ongoing project work from the past few weeks.

**<ins>Issue Description<ins>**

The issue covered this week is: As an UNDAC Team Leader I want to make requests for volunteers so that I can recruit the appropriate number to the team.

The end user goal of the issue is to find appropriate team members efficiently and the end business goal is to ensure that the team has sufficient capacity and skills for the mission.

The acceptance criteria for the issue to be considered completed is: 
* Volunteer lists can be browsed and filtered by skill, geographical location and status.
* Filters can be cleared.
* Requests can be made by flagging the requested volunteers.
* The date of the most recent status change (e.g. 'invited', 'confirmed', etc.) is recorded.
* Arrival and departure dates are shown for confirmed volunteers.

**<ins>Issue Code<ins>**

```c#
private void LoadData()
        {
            // Clear existing items
            Volunteers.Clear();

            // Include Skill and Location for each Volunteer
            var allVolunteers = _dataService.GetAllVolunteers()
                .Include(v => v.Skill)
                .Include(v => v.Location)
                .ToList();

            foreach (var volunteer in allVolunteers)
            {
                Volunteers.Add(volunteer);
            }
        }
```

This code adheres to the DRY principle as it has no redundant code and it uses a loop to add each volunteer to the Volunteers collection instead of rewriting this code manually for each volunteer.

The method follows SRP by only having a single purpose which in this case is to load data into the Volunteers collection.
KISS and YAGNI are also followed as the code is very simple and doesn't include anything that is not needed.

```c#
private async void OnVolunteerSelected(object sender, SelectedItemChangedEventArgs e)
        {
            if (e.SelectedItem is Volunteer selectedVolunteer)
            {
                string action = await DisplayActionSheet("Choose an action:", "Cancel", null, "Edit", "Delete");
                switch (action)
                {
                    case "Edit":
                        await Navigation.PushAsync(new AddVolunteerPage(selectedVolunteer));
                        break;
                    case "Delete":
                        DeleteVolunteer(selectedVolunteer);
                        break;
                }
            }
        }
```

The usual suspects DRY, KISS and YAGNI are all followed here for reasons similar to the code above, it also once again follows SRP as it's only purpose is to handle the event of a volunteer being selected, it delegates actions like edit and delete but doesn't directly implement them in this method.

The method utilises asynchronous programming with the async and await keywords which helps provide a responsive UI.  The method also adheres to the Open/Closed principle since 'AddVolunteerPage' and 'DeleteVolunteer' can be extended without modifying this method.

**<ins>Test Code<ins>**

```c#
using Xunit;
using Moq;
using System.Collections.ObjectModel;
using UNDAC_Project.Models;
using UNDAC_Project.Views;
using UNDAC_Project.Data;

public class VolunteerRequestsViewTests
{
    [Fact]
    public void LoadData_ShouldPopulateVolunteersCollection()
    {
        // Arrange
        var mockDataService = new Mock<IDataService>();
        var volunteers = new ObservableCollection<Volunteer>();
        var testData = new List<Volunteer> { new Volunteer(), new Volunteer() };
        mockDataService.Setup(ds => ds.GetAllVolunteers()).Returns(testData.AsQueryable());

        var page = new VolunteerRequestsView(mockDataService.Object) { Volunteers = volunteers };

        // Act
        page.LoadData();

        // Assert
        Assert.Equal(testData.Count, volunteers.Count);
    }
}
```

This test checks the functionality of the LoadData method.  It first arranges the test by setting up a mock of 'IDataService' and testData is created which is a list of volunteer objects.  LoadData is then called on the instance of 'VolunteerRequestsView'.  Finally it asserts that the count of the objects in 'volunteers' after LoadData is the same as the testData list to make sure it worked.

```c#
using Xunit;
using Moq;
using Microsoft.Maui.Controls;
using UNDAC_Project.Models;
using UNDAC_Project.Views;
using UNDAC_Project.Data;
using System;

public class AddVolunteerPageTests
{
    [Fact]
    public void OnAddVolunteerClicked_ShouldAddNewVolunteer()
    {
        // Arrange
        var mockDataService = new Mock<IDataService>();
        var page = new AddVolunteerPage(null, mockDataService.Object);

        // Setting up the page's properties as if they were bound to UI elements
        page.FirstNameEntry.Text = "John";
        page.LastNameEntry.Text = "Doe";
        page.SkillPicker.SelectedItem = new Skill { SkillId = 1, Name = "Test Skill" };
        page.LocationPicker.SelectedItem = new VolunteerLocation { LocationId = 1, Name = "Test Location" };
        page.StatusPicker.SelectedItem = VolunteerStatus.Active.ToString();

        mockDataService.Setup(ds => ds.AddVolunteer(It.IsAny<Volunteer>()));

        // Act
        page.OnAddVolunteerClicked(null, EventArgs.Empty);

        // Assert
        mockDataService.Verify(ds => ds.AddVolunteer(It.IsAny<Volunteer>()), Times.Once());
    }
}
```

This test assess the behaviour of 'OnAddVolunteerClicked' when a new volunteer is added.  Like the first test, it is setup with a mock of 'IDataService' and an instance of 'AddVolunteerPage' is made in this mock.  Then 'OnAddVolunteerClicked' is called to create a new volunteer with the supplied info and call 'AddVolunteer' to save it.  Finally the test asserts that the 'AddVolunteer' is called only once to make sure the method correctly attempted to add a new volunteer.

**<ins>Code Review Fixes<ins>**

Here is the feedback my team member gave me after reviewing my code this week.  The validation logic inside 'OnAddVolunteerClicked' is embedded in the UI logic and should be moved, so I extracted this out into a new method to better adhere to SRP.

There is a lack of error handling tied to any interactions with the data service, it would be a good idea to add some in for improved robustness, so I added some exception handling code to help with this.

When a volunteer is added or updated there is no UI feedback to the user to tell them to operation was successful, this would be a good feature to add.  To fix this I added a display prompt upon adding and updating letting the user know it was successful.

**<ins>Code Review Issues<ins>**

When I reviewed the code of another team member here are the issues I found in their code.  I found they had some inconsistency with async methods, there was a method that was not async that called methods that were, so I suggested they make the calling method async as well to keep things consistent in their code.

They had some hardcoded strings in a few of their methods which can lead to maintenance issues so I recommended making them constants or using a resource file to facilitate any changes made.

# Reflection

This week I think my testing had improved, this is the first week in this project work cycle where I have managed to create two tests for my issue instead of just one, and I also have a much better handle on Moq and how to use it in tests, I have been trying to utilise it for weeks by it is a bit of a learning curve.

Although this is a very small point, another member of my team showed me that when adding a code block in markdown if you type 'c#' at the top, it colours the code like it would be in the c# language which makes the portfolio a little better and makes the code more eye-catching.  Being in a team can be great even for small things like this that aren't at all necessary, but add a little extra value that I wouldn't have known myself.

This week my team had a discussion about our workflow and the issues we have completed and we came to a decision that some of us were going to split up and create our own issues to work on for this week, as it would give us great improvements to add to the UNDAC project.  Some of my team have split off to create a login/authentication page for the app and another team member is trying to convert their pages to MVVM so they can fully understand it then teach us how to do it to possibly implement it into the entire app.


