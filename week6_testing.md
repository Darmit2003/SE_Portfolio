# Testing

This entry documents the practical work carried out in week 6, the purpose of week 6 was for some competitive testing to take place, so we would write some code and tests for it and then these tests would be ran on the code of other teams to see if they are versatile enough to pass on other code.

**<ins>Code Purpose<ins>**

Here is the code that I tested:

```
public void UpdateDisplay(bool isCorrect, string word, char letter, int remainingAttempts)
    {
        // Update RemainingAttemptsLabel
        RemainingAttemptsLabel.Text = $"Remaining Attempts: {remainingAttempts}";

        // Update word labels
        if (isCorrect)
        {
            for (int i = 0; i < word.Length; i++)
            {
                if (word[i] == letter)
                {
                    WordLabels[i].Text = letter.ToString();
                    WordLabels[i].IsVisible = true;
                }
            }
        }
    }
```

The purpose of this code is to take in a boolean for if the guess was correct, a string word which is the word to be guessed, a char letter which is the guessed letter and an int for the remaining attempts on the current game.

The method first updates the RemainingAttemptsLabel to show the remaining attempts that was passed in.  Next, if the guess was correct, it loops through each letter in the passed in word and if the current letter is equal to the passed in letter it sets the text of the corresponding word label to this letter and makes the label visible.

**<ins>Test Code<ins>**

### Test 1

```
[Fact]
        public void TestCorrectGuess()
        {
            // Setup
            var game = new GamePage("Easy")
            {
                RemainingAttemptsLabel = new Label(),
                WordLabels = new List<Label>
                {
                    new Label{ IsVisible = false },
                    new Label{ IsVisible = false },
                    new Label{ IsVisible = false }
                }
            };
            string word = "cat";
            char guessedLetter = 'a';

            // Call
            game.UpdateDisplay(true, word, guessedLetter, 3);

            // Assert
            Assert.Equal("Remaining Attempts: 3", game.RemainingAttemptsLabel.Text);
            Assert.False(game.WordLabels[0].IsVisible);
            Assert.Equal("a", game.WordLabels[1].Text);
            Assert.True(game.WordLabels[1].IsVisible);
            Assert.False(game.WordLabels[2].IsVisible);
        }
```

### Test 2

```
[Fact]
        public void TestIncorrectGuess()
        {
            // Setup
            var game = new GamePage("Easy")
            {
                RemainingAttemptsLabel = new Label(),
                WordLabels = new List<Label>
                {
                    new Label{ IsVisible = false },
                    new Label{ IsVisible = false },
                    new Label{ IsVisible = false }
                }
            };
            string word = "cat";
            char guessedLetter = 'z';

            // Call
            game.UpdateDisplay(false, word, guessedLetter, 2);

            // Assert
            Assert.Equal("Remaining Attempts: 2", game.RemainingAttemptsLabel.Text);
            Assert.False(game.WordLabels[0].IsVisible);
            Assert.False(game.WordLabels[1].IsVisible);
            Assert.False(game.WordLabels[2].IsVisible);
        }
```

**<ins>Test Explanations<ins>**

### Test 1

Test 1 simulates a correct guess being passed in to the method and tests that the approrpriate labels are changed.  First, it sets up the test by creating a new GamePage on "Easy" difficulty, it creates the labels for this page and ensures all word labels are invisible for the purposes of this test.  It also creates variables with values for the word and guessed letter.

Next, it calls the method with a true boolean value for a correct guess, the word and letter created and a value of 3 for the remaining attempts.

Finally, it asserts that the remaining attempts label is displaying a value equal to the passed in int, that the correct word label value is equal to the passed in letter and that it is visible and also that the other word labels are still invisible.

### Test 2

Test 2 simulates an incorrect guess being passed in to the method and tests that the appropriate labels are changed.  It sets up the same way as test 1 except this time the created letter is not in the created word.

It calls the method, this time with a false boolean value for an incorrect guess, the word and letter created and a value of 2 for the remaining attempts.

Finally, it asserts that the remaining attempts label is displaying a value equal to the passed in int, and that all word labels are still invisible.

**<ins>Test Importance<ins>**

The method is an important method to test in the code because if labels are not being properly changed based on guesses the player of the game will not know if they got a letter right or how many attempts they have left in the game.

The two tests shown here are the two most important general test cases for this method because they test the main two outcomes of the method, a correct guess and an incorrect guess, and whether or not the right things are changing in the right way for each of these outcomes.

**<ins>Limitations<ins>**

These test cases assume that all word labels are not visible except the one that is correctly guessed in the test case, so they are limited to cases where no letters have been previously guessed correctly.

These test cases both use small words on "Easy" difficulty so harder, longer words are never tested.

These tests are given sample data sets, but in the full program this method relies on other methods working correctly first, and if these are broken then the methods will provide incorrect results.

# Reflection

I have made the decision to submit this before the actual test battles take place as I think this is the best course of action in the current situation.  The team only came together and started communicating today on the Sunday night and most of the team haven't written any working code or tests by this point so I doubt there will be much to 'battle' with.

Myself and another member of the team used pair programming to write a working method and two unit tests for it that pass, but I'm not sure if this will even be useable as someone else on the team has to review it first to merge it with the main to ensure there are no conflicts and this probably won't happen in time.

This week has been a repeat of every other week so far where our team is collevtively lost on what our actual tasks are and we spend almost a full week trying to figure out our task and then we are rushed to carry out the work.

We are told we are the purpose of our team is to work together and share knowledge to help each other but not a single team member has the necesarry knowledge to help anyone else, so we are all just scrambling to find a way to do the work and run out of time to properly work together.
