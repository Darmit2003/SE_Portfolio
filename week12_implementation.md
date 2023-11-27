# Project Work 5

The final week of project work on code issues.

**<ins>Issue Description<ins>**

The issue fixed this week is: As an UNDAC Deputy Team Leader I want to view the status (location, health, current responsibilities) of team members so that I can make new task assignments.

The end user goal for the issue is to form operational sub-teams as efficiently as possible and the end business goal is to allow the mission to react in an agile fashion to local priorities.

The acceptance criteria for the issue to be considered completed is: 
* Operational sub-team requirements can be specified (e.g. size, skills required, start and end dates of assignment, etc.).
* Team members can be viewed and filtered by location, skill, and current availability.
* List filters can be cleared.
* Team members can be added to the operational sub-team.
* Team members are shown as unavailable while assigned to an operational sub-team.

**<ins>Issue Code<ins>**

```c#
public void AssignMembersToSubTeam(OperationalSubTeam subTeam)
    {
        foreach (var requiredSkill in subTeam.Requirement.RequiredSkills)
        {
            var availableMembersWithSkill = teamMembers
                .Where(m => m.IsAvailable && m.CurrentResponsibility.Contains(requiredSkill))
                .OrderBy(m => m.HealthStatus); // Prioritise members with better health status

            foreach (var member in availableMembersWithSkill)
            {
                if (subTeam.GetTeamMembers().Count() < subTeam.Requirement.TeamSize)
                {
                    subTeam.AddTeamMember(member);
                }
                else
                {
                    break; // The sub-team is full
                }
            }
        }
    }
```

The method above demonstrates good use of software design principles.  It adheres to KISS by avoiding any unnecessary complexity, being as straightforward as possible while still fulfilling its functionality.  The logic for member assignment is looped through foreach member avoiding code repetition adhering to the DRY principle.

The Open/Closed Principle is also followed by this code snippet, as the method is open to be extended but closed for modification, if the criteria for member assignment was to change for any reason the method can be extended to do so without changing any of this code.

Another design principle the code exhibits is the Dependency Inversion Principle as it relies on abstractions in the sense that it uses a list of TeamMember objects instead of concrete implementations.

```c#
public string GenerateSubTeamSummaryReport()
    {
        var report = new StringBuilder();
        foreach (var subTeam in subTeams)
        {
            report.AppendLine($"Sub-Team for {subTeam.Requirement.Purpose}:");
            foreach (var member in subTeam.GetTeamMembers())
            {
                report.AppendLine($"- {member.Name}, Skill: {member.CurrentResponsibility}, Location: {member.Location}");
            }
            report.AppendLine($"Start Date: {subTeam.Requirement.StartDate}, End Date: {subTeam.Requirement.EndDate}");
            report.AppendLine("-----");
        }

        return report.ToString();
    }
```

It feels like the end of an era to be saying this for the last time in my portfolio, but the above method adheres to DRY, KISS and YAGNI for reasons very similar to above.

The method follows the Single Responsibility Principle as its sole purpose is to generate a report of all operational sub-teams.

The Principle of Least Astonishment is followed as this principle is about creating software components that behave predictably and intuitively, which this method does.

Another principle adhered to in this code is the Law of Demeter which means that the method is only communicating with immediate objects and is not navigating through a complex network of objects to use in the method.

**<ins>Test Code<ins>**

```c#
[Fact]
    public void AssignMembersToSubTeam_Assigns_Members_With_Skills()
    {
        // Arrange
        var teamAssignment = new TeamAssignment();
        teamAssignment.AddTeamMember(new TeamMember { Name = "Alice", CurrentResponsibility = "Medic", IsAvailable = true });
        teamAssignment.AddTeamMember(new TeamMember { Name = "Bob", CurrentResponsibility = "Engineer", IsAvailable = true });

        var requirement = new OperationalRequirement
        {
            RequiredSkills = new List<string> { "Medic" },
            TeamSize = 1
        };
        var subTeam = new OperationalSubTeam(requirement);

        // Act
        teamAssignment.AssignMembersToSubTeam(subTeam);

        // Assert
        Assert.Single(subTeam.GetTeamMembers());
        Assert.Contains(subTeam.GetTeamMembers(), member => member.Name == "Alice");
    }
```

This test asserts that the method is correctly assigning members that have the required skills to the operational sub-team.  It checks that the sub-team contains the correct member after they have been assigned.

```c#
[Fact]
    public void GenerateSubTeamSummaryReport_Returns_Correct_Format()
    {
        // Arrange
        var teamAssignment = new TeamAssignment();
        var member = new TeamMember { Name = "Alice", CurrentResponsibility = "Medic", Location = "Area 1", IsAvailable = true };
        teamAssignment.AddTeamMember(member);

        var requirement = new OperationalRequirement
        {
            Purpose = "Emergency Response",
            TeamSize = 1,
            StartDate = new DateTime(2023, 1, 1),
            EndDate = new DateTime(2023, 1, 7)
        };
        var subTeam = new OperationalSubTeam(requirement);
        subTeam.AddTeamMember(member);

        teamAssignment.CreateSubTeam(requirement);

        // Act
        var report = teamAssignment.GenerateSubTeamSummaryReport();

        // Assert
        var expectedReportStart = "Sub-Team for Emergency Response:";
        var expectedMemberDetails = "- Alice, Skill: Medic, Location: Area 1";
        Assert.StartsWith(expectedReportStart, report);
        Assert.Contains(expectedMemberDetails, report);
    }
```

This test asserts that the method is generating the reports in the expected format and has the details of the team members and the operational requirement included.

**<ins>Code Review Fixes<ins>**

The team member who reviewed my code this week made some general comments about a lack of error handling in the code and that better commenting/documentation should be included.  This can easily be fixed by commenting the code, adding Doxygen comments and adding some extra validation to the methods to handle some possible errors.

While this code will work for the small number of members and sub-teams we currently have, my team member noted that with the way the issue code is implemented right now it may struggle to handle larger amounts of data but this isn't really concerning enough to change at the moment.

**<ins>Code Review Issues<ins>**

The code I reviewed this week was mostly pretty strong code but there were a couple of things that could have been improved.  Firstly, they had a method that seemed to be generating some sample data and also populating a list with data, I suggested that they split this method up into two separate methods so that their code better adheres to the SRP.

The other suggestion I gave them was to improve the naming conventions of their method and variable names as they had mostly generic and non-descriptive names which goes against good software design practice.

# Reflection

Seeing as how this is the last of 5 weeks working on these project tasks, this week has felt like some of my strongest work in the portfolio and I am getting more and more comfortable with identifying certain more common software design principles as well as what to do to fix them.

This week both to make better improvements to my code and to prepare for the upcoming interview, I have been researching some software design principles that I hadn't really heard of yet or had heard but didn't understand, and after spending some time on this I am happy with the extra knowledge I now have in the realm of quality code and I expect to make good use of this in the future.

Overall, I have learned a lot over the course of this portfolio.  In week 1 I had never heard of code smells as well as most of the important software design principles.  I now have a much better idea of how to create quality code and how to analyse the quality of other people's code.  In week 1 I had never used GitHub, MAUI apps, C#, SQLite or a proper team workflow.  I am now confident using GitHub and know most of its important features, I am still not great with SQLite or MAUI but I have still gained valuable experience with them as well as the C# language.  As for team workflows, after working through this portfolio the past few weeks I now have an understanding as well as some experience with some great workflows for a team to collaborate to create software.

While the work wasn't always easy and it definitely had its learning curves and low points, due to both learning new concepts and dealing with the troubles of team development, I do feel like after working through this course, I will be a better software engineer in the future.
