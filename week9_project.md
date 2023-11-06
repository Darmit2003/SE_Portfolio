# Project Work 2

Like week 8, this week involves using everything gathered from this module to complete another issue.

**<ins>Issue Description<ins>**

The issue for this week is: As an UNDAC Team Support and Logistics Manager, I want to request access be reinstated for users who return to the team so that they have access to the information they need.

The end user goal for this issue is to contol access to mission systems and the end business goal is to ensure the data security of the mission.

The appectance criteria for the issue to be considered completed are that a member's current system access status can be viewed and that giving access to a team member must be approved by the Deputy Team Leader.

**<ins>Issue Code<ins>**

The code used this week was pair programmed with a member of my team as well as a member of another team as none of us feel confident enough to code these things on our own, but by sharing ideas and snippets we managed to come to a solution.

```
async void OnDeleteClicked(object sender, EventArgs e)
    {
        if (Item.ID == 0)
            return;
        await database.DeleteItemAsync(Item);
        await Shell.Current.GoToAsync("..");
    }
```

The above code snippet demostrates good software design practice, it follows DRY, KISS and YAGNI by being simple, not repeating anything and not using things that aren't needed.  It follows the Single Responsibility Principle (SRP) as all it does is deletes an item and navigates backwards.  It also uses Asynchronous Programming and uses the await keyword ensuring better responsiveness.

```
protected override async void OnNavigatedTo(NavigatedToEventArgs args)
    {
        base.OnNavigatedTo(args);
        var items = await database.GetItemsAsync();
        MainThread.BeginInvokeOnMainThread(() =>
        {
            Items.Clear();
            foreach (var item in items)
                Items.Add(item);

        });
    }
```

This code snippet follows all of the same software engineering principles as the first method for the same reasons.  There isn't really too much to talk about because the methods are really small so it can be kind of hard to break some of these principles.  Due to the limitations on time we have each week the tasks are usually completed with the simplest implementation possible making all the code really small and concise.

**<ins>Test Code<ins>**

```
public void GetUserFromList()
{
    var teamMemberDB = new TeamMemberDB();
 
    var result = teamMemberDB.getUserFromList("John");
 
    Assert.NotNull(result);
}
```

The test code above creates a new empty TeamMember variable, uses the getUserFromList method to get "John" from the list and store the member in the result variable and finally it asserts that the result variable is not null meaning it has successfully pulled a member from the list and stored it.

**<ins>Code Review Fixes<ins>**

This week the person who reviewed my code never requested any changes to be made so that meant that there was also nothing to fix, this is kind of what I was expecting since like I mention elsewhere the code is so concise that it would be unlikely that it has the chance to break any principles.

**<ins>Code Review Issues<ins>**

When I reviewed the code of one of my team members I wasn't actually able to find any issues with their code its self as all of their methods were small and to the point and as far as I could see the code followed software engineering design principles very closely.  The only bit of note I was able to give them was that they could be doing with some better commenting, like Doxygen style comments for example to explain each of the methods etc.

# Reflection

**<ins>New Things Realised/Learned<ins>**

The biggest thing that I learned this week is how to make an SQLite database work with a MAUI app, as I mentioned in one of the earlier weeks I wasn't able to make a database work using the tutorial that we were supplied and attempting multiple other methods also never got me anywhere.  This week a member of another team showed me a method they found to connect these databases to MAUI apps and I now have a working strategy I can use.

I realised that the issues we were given in one of the previous weeks were not displaying right in my team's GitHub repository so part of the reason that I had so much difficulty completing a task that week was because I couldn't see all the info on my task and didn't realise there was more I should be seeing, so this week having the issues display properly showing the goals and acceptance criteria has made it much easier to understand the task.

I am slowly getting more familiar with my workflow and how it is carried out and I am learning quicker ways to do certain things, I am a lot more familiar with GitHub at this point after a few weeks of working with it.

I have found that if I want to make a really big change to my code and I am not sure if it will work or if I may want to revert it, that it is actually very easy in Visual Studio to make a new branch off of the one being worked on, then make the changes and either merge them to the old branch or if it doesn't work you can completely delete this new branch discarding any changes, keeping things how they were before, this is very handy when I am still not very confident with coding these MAUI apps.

**<ins>Common Team Development Problems<ins>**

A very common team development problem that my team has definetly suffered from is that there are commonly people who don't communicate when they are forced into a team situation, in our team this is likely a big contrbutor to how we as a team have struggled so much as there are multiple members of my team who I have never heard speak once or send any messages and those people might have skills that could help the rest of us that are not being shared.

Another common problem that we are suffering from is that due to our tasks for the most part being carried out in our free time, people have other things that they prioritise with in their time, like other jobs, hobbies, family and the it often means that everyone on the team wants to work at different times, so if one of us needs help from someone else on the team it is sometimes not possible to get that help.

**<ins>Practice Comparison<ins>**

I don't know too much about how other people are carrying out the work in comparison to myself, but one thing I do know is that for example, at least one of the other teams has pretty consistent communication with each other, they hold group calls where they go over issues and fix things and they book classrooms to get some time to sit and work with each other on campus.  This differs greatly from how I have been working, as most of the time the other members of my team are unavailable or are looking for the same answers as me, so I spend most of the time working either alone or pair programming with one other member of my team who I have close contact with.