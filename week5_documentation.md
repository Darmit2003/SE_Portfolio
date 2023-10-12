# Documentation

This section is related to work done on clean code and documentation from week 5.

Here is the full snippet of code that I will be talking about below:

```
using StatusManagement.Models;
using System;
using System.Collections.Generic;
using StatusManagement.Views;

namespace StatusManagement
{
    public partial class MainStatusPage : ContentPage
    {
        public MainStatusPage()
        {
            InitializeComponent();
            BindUIElements();
        }

        private void ClearStatusMessageLabel()
        {
            statusMessageLabel.Text = "";
        }

        private void BindUIElements()
        {
            newStatusButton.Clicked += OnNewStatusButtonClicked;
            getStatusButton.Clicked += OnGetStatusButtonClicked;
            statusList.ItemTapped += OnStatusItemTapped;
        }

        private void OnNewStatusButtonClicked(object sender, EventArgs e)
        {
            ClearStatusMessageLabel();

            var newStatusEntryText = statusEntry.Text;
            StatusRepository.AddNewStatus(newStatusEntryText);
            statusMessageLabel.Text = StatusRepository.StatusMessage;
        }

        private void OnGetStatusButtonClicked(object sender, EventArgs e)
        {
            ClearStatusMessageLabel(); 

            var allStatuses = StatusRepository.GetAllStatuses();
            statusList.ItemsSource = allStatuses;
        }

        private async void OnStatusItemTapped(object sender, EventArgs e)
        {
            if (sender is Grid tappedGrid && tappedGrid.BindingContext is Status tappedStatus)
            {
                await Navigation.PushAsync(new EditStatusPage(tappedStatus));
            }
        }
    }
}
```
 

## Clean Code Rules

**<ins>Use Meaning Variable Names<ins>**

This clean code rule means that you should always use meaningful, self-explanatory names for all methods, classes and variables, this way anyone should be able to tell what these things do just by reading the name alone.

An example of this rule in the code snippet:

```
private void OnGetStatusButtonClicked(object sender, EventArgs e)
```

This is a decriptive and meaningful name that makes it obvious to anyone reading the code that this is an event handler method for when the 'Get Status' button is clicked.  All other names in the code are written in a similar way, always describing what that piece of code is for in its name.

**<ins>KISS (Keep It Simple, Stupid)<ins>**

The KISS principle is about keeping your code as simple as you possibly can while meeting your goals of functionality.  Don't overcomplicate anything, just write the code in the most straightforward way you can.

An example of this rule in the code snippet:

```
private async void OnStatusItemTapped(object sender, EventArgs e)
        {
            if (sender is Grid tappedGrid && tappedGrid.BindingContext is Status tappedStatus)
            {
                await Navigation.PushAsync(new EditStatusPage(tappedStatus));
            }
        }
```

The above method is written in an very straightforward way, handling the event of an item being tapped and keeping the lines of code to a minimum, only using 2 lines of code without including the bracket lines.

**<ins>DRY (Don't Repeat Yourself)<ins>**

The DRY principle is about keeping repeated lines of code to a minimum, having no code repetition if possible, a good way to do this is to move any bits of shared functionality into their own methods and/or classes so they only have to be written once.

An example of this rule in the code snippet:

```
private void ClearStatusMessageLabel()
        {
            statusMessageLabel.Text = "";
        }
```

Since there is more than one instance where the statusMessageLabel text needs to be cleared, the code to do so was moved to it's own method and then called when needed, instead of writing the text clearing code twice.

**<ins>Appropriate Comment Use<ins>**

In terms of a clean code rule, this means only using a comment when it is absolutely necessary.  Clean code is supposed to be self-documenting so comments shouldn't be necessary in most cases and when you think you need a comment, you should first try to rewrite the code to no longer need commented.  In the case that this is not plausible, a comment can be used.

An example of this in the code snippet would be the enitre snippet, since the code explains its self no comments are used meaning the code implements this rule.

**<ins>YAGNI (You Ain't Gonna Need It)<ins>**

The YAGNI principle involves only writing a piece of code when it is needed and not when you think you might need it later on, this way you avoid writing code that never gets used and it stops you from potentially breaking other important code with code you don't even need yet.

Again an example of this would be the entire snippet, since there is no code in the snippet that is not needed for something.

**<ins>Single Responsibilty Principle<ins>**

The Single Responsibilty Principle is part of the SOLID Design Principles and states that each method should only have one responsibility. If a method is performing multiple pieces of functionality then this should be split down into multiple methods each performing one task.

An example of this rule in the code snippet:

```
private void BindUIElements()
        {
            newStatusButton.Clicked += OnNewStatusButtonClicked;
            getStatusButton.Clicked += OnGetStatusButtonClicked;
            statusList.ItemTapped += OnStatusItemTapped;
        }
```

The BindUIElements() method follows the Single Repsonsibility Principle by only handling the logic of UI element binding.  The above method is one example of this but the other methods also follow this rule.
