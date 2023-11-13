# Project Work 3

This entry works exactly like the last two, I will work through a new issue, detail good principles in the code and test it and perform a code review.

**<ins>Issue Description<ins>**

The issue being tackled this week is: As an UNDAC Team Leader I want to view team alerts so that I can take appropriate action.

The end user goal of the issue is to receive timely information about team issues and the end business goal is to ensure that team members receive appropriate attention from the mission management.

The acceptance criteria for this issue is as follows: 
* Team alerts are represented by a visual flag on the app display.
* Team alerts can be viewed by status.
* Team alerts can be filtered by full-text search.
* Alert list filters can be cleared.

Another thing to note is that any team member can create an alert.

**<ins>Issue Code<ins>**

This code was written in collaboration with other members of my team.  Both methods shown are compliant with DRY, KISS and YAGNI as they are very simple pieces of code that don't repeat code anywhere and don't contain any code in them that is not needed.

```
private void SearchItemsInAlertList(string searchText)
    {
        if (string.IsNullOrWhiteSpace(searchText))
        {
            alertList.ItemsSource = _alerts.GetAlertData();
        }
        else
        {
            alertList.ItemsSource = _alerts.GetAlertData().Where(a => a.Detail.ToLower().Contains(searchText.ToLower())).ToList();
        }
    }
```

The method above adheres to the Single Responsibility Principle as its only purpose is to search and update the item source for alertList.  By converting all text to lowercase it performs a case-insensitive search which is good coding practice as it greatly limits the possible user errors when searching.  The method first checks if the text is null or whitespace and sets the source to all the data in this case, this kind of condition checking is good software design practice to exit the loop early and improve readability.

```
private void OnAlertType_Changed(object sender, EventArgs e)
    {
        if (alertTypePicker.SelectedItem is string selectedType)
        {
            alertList.ItemsSource = _alerts.GetAlertData().FindAll(a => a.Type == selectedType);
        }
    }
```

This next method again follows SRP by only performing one action.  FindAll is used to filter all the data of a selected type, efficiently filtering data like this is good coding practice when filtering on different criteria.  The method is private so that it can only be used in this class which helps prevent any interactions with other parts of the program that are not intended, this kind of encapsulation is an example of good software design practice.

**<ins>Test Code<ins>**

```
[Fact]
    public void FilterAlertsByType_ReturnsCorrectData()
    {
        // Arrange
        var mockAlerts = new Mock<IAlertsService>();
        var testAlerts = new List<AlertModel>
        {
            new AlertModel { Type = "Type1" },
            new AlertModel { Type = "Type2" },
        };

        mockAlerts.Setup(a => a.GetAlertData()).Returns(testAlerts);

        var teamAlertsPage = new TeamAlertsPage(mockAlerts.Object);

        // Act
        var result = teamAlertsPage.FilterAlertsByType("Type1");

        // Assert
        Assert.Single(result);
        Assert.All(result, item => Assert.Equal("Type1", item.Type));
    }
```

This test sets up a mock and then provides a list AlertModel objects.  After this the FilterAlertsByType method is called with a specified type.  Finally the test confirms that only the items of the specified type are displayed and that the right number of them are there in the assertion lines.

**<ins>Code Review Fixes<ins>**

The team member that reviewed my code this week made some suggestions to improve my code.  They recommended adding some error handling to some of my methods as none of them really include this and it is good practice to include this to minimise the possible errors at runtime.  To address this I added some simple condition checks to ensure that acceptable conditions are met in methods before the operation is carried out.

They also recommended making better use of comments in my code as it is lacking in this department and comments are very important for documentation purposes.  To fix this I added Doxygen comments to my page to better document the purpose of the methods and variables.

**<ins>Code Review Issues<ins>**

In the code I reviewed this week I recommended to my team member that they should consider splitting up some of their methods into smaller ones to better adhere to the SRP, as some methods seemed to be taking on more than one task, and simply splitting the method up in one for each task would fit this design principle.

I also recommended that they stick to consistent coding standards and naming conventions as they seemed to differ at a few points throughout the code, like switching from camel case to pascal case, this improves readability and consistency in code.

Lastly I gave one of my suggested fixes to them, as I suggested that some error handling code in their methods would be a good idea to minimise the chance of things going wrong later.

# Reflection

Working collaboratively with my team this week was a huge benefit as it meant that we arrived at a solution for the issue by helping each other and suggesting ideas much quicker than any of us would have on our own.

As the weeks have went on I have slowly started to feel much more proficient with GitHub, C#, MAUI apps and team workflows and in some of these areas I am starting to feel a lot more confident than I felt at in the initial weeks which in turn I feel is helping me provide better quality work.

My understanding of software design principles is also improving with each code review I carry out and also from the suggested changes my code gets from other team members.

I do think that these issues could use more time to be worked on as each week the work does feel kind of rushed and a very simple implementation and tests are usually produced.  Some more time to polish off these pieces of code and add better features and fit more design principles would be beneficial as sometimes there are ideas that don't come to fruition due to the weekly time constraint.

I did manage to develop a higher quality test this week than I have in the past two weeks which includes Mocking which is something I am still not very confident with but I am trying to learn how to better use it for future use.
