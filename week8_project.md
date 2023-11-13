# Project Work 1

The work for this week invloves using everyting that has been learned or discussed in the module so far to complete an issue from start to finish.

**<ins>Issue Description<ins>**

The issue for this week is: As an UNDAC Team Support and Logistics Manager, I want to request the removal of access for uses when they leave so that effective security is maintained.

The end user goal for this issue it to prevent acess to mission system once it is no longer neeeded and the end business goal is to ensure the data security of the mission.

The acceptance criteraia for this issue to be considered completed are that requests for removal of access are automatically accepted and that records are not deleted and are instead just disabled.

**<ins>Issue Code<ins>**

This code builds off of the code used to complete the issue I cover in week 9 and changes some of the methods to fit the new functionality.

```
async void AccessToggled(object sender, ToggledEventArgs e)
    {
        if(e.Value == true)
        {
            string password = await DisplayPromptAsync("Admin Verification", "Enter admin password:");

            if (password != "ABC")
            {
                ((Switch)sender).IsToggled = false;
                await DisplayAlert("Access Denied", "Incorrect password.", "OK");
            }
        }
    }
```

This code is evidence of good software design practice for a few reasons, for example it follows DRY, KISS and YAGNI as much as possible as it is very simple code.  It utilises asynchronous programming to keep things running smoothly while the method waits for a password from the user.  It provides instant user feedback on an incorrect password which is a good practice to follow to ensure the user is always aware of what's happening.

```
public async Task<int> DeleteItemAsync(TeamMemberModel item)
        {
            await Init();
            item.status = true;
            item.access = false;
            Console.WriteLine(item.status);
            return await Database.UpdateAsync(item);


        }
```

Like the previous code snippet, this snippet adheres to KISS, DRY and YAGNI as well as asynchronous programming for the same reasons as above.  This method also follows the Single Responsibility Principle as its only purpose is to handle the disabling of a member.

```
async void OnDeleteClicked(object sender, EventArgs e)
    {
        if (Item.ID == 0)
            return;
        await database.DeleteItemAsync(Item);
        await Shell.Current.GoToAsync("..");
    }
```

The following already mentioned principles are being followed here: DRY, KISS, YAGNI, asynchronous programming.  Antother notable principle in this snippet is event-driven programming which is good practice when a user interface is involved, as the method is executed as a response to a specific user action.

**<ins>Test Code<ins>**

```
Fact]
    public void DeleteItemAsync_SetsCorrectStatus()
    {
        // Arrange
        var item = new TeamMemberModel(); // Assuming this is your model class
        var dbModel = new TeamMemberDB(); // Replace with your actual ViewModel class

        // Act
        dbModel.DeleteItemAsync(item); // Adjust this call based on your actual method signature

        // Assert
        Assert.True(item.status);
        Assert.False(item.access);
    }
```

This test asserts that when the DeleteItemASync method is used, that the status is set to true and the access is set to false.  This one test covers all the new functionality for the new issue.

**<ins>Code Review Fixes<ins>**

When my code was reviewed by another team member, one thing they requested that I change was the name of the status variable in the TeamMemberModel as it follows poor naming convetions as it isn't clear enough about what it is.  To fix this I changed the name to disabledStatus which better fits good coding principles as it is now much clearer reading it what it is for.

**<ins>Code Review Issues<ins>**

For the code that I reviewed this week, it was for the most part very good but I had a couple of suggestions for them to make things a little bit better.

I recommended adding Doxygen comments for better documentation and also I suggested that they should consider changing their database methods to async, as this way the application can carry out other tasks while it waits for these operations to be completed.

# Reflection

This week was submitted after week 9 for reasons that have already been discussed with lecturers, this is why the code from this week is building off of the code that I submitted for week 9.

This week a lot has changed, I have moved team as I felt that my old team just wasn't really working and straight away I think I am seeing improvements.  I have much better communication with the members of this team who are often available.

This week I feel like I've better learned how to code review, this of course will be getting better the more I do it but also because there were a lot of detailed code reviews in the team repo that I was able to read through to understand how other people were reviewing code and the suggestions that they gave which in turn helped me to know what to look out for in the future.

I've learned why asynchronous programming is a very useful principle to follow to keep the program running smoothly while a method waits for operation, while I've been using it for a while, I didn't fully understand it until now.

A common problem that can arise in a team development situation is that there can be big skill gaps between different members of the team which could lead to an issue during code reviews, if someone who is a very skilled coder writes some code for review, and someone much less experienced than them tries to review it, they may not be able to properly give feedback if they are not able to fully understand the code and how it works.
