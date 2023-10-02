# Code review

This section documents your practical work from week 4 in which you attempt a series of 
code review challenges. For your portfolio, do the following:

1. Choose the code review challenge which best demonstrates your skills.
2. Copy the code into your portfolio using a Markdown
   [fenced code block](https://docs.github.com/en/get-started/writing-on-github/working-with-advanced-formatting/creating-and-highlighting-code-blocks).
3. Provide some descriptive commentary that identifies the problems.
4. Show your improved version of the code in a second code block.
5. Explain in one or more paragraphs why your solution is a good one.

**DO**

* Use grammatically correct sentences and paragraphs for your commentary.
* Make clear reference to the code in your commentary. GitHub Markdown does not support
  line numbers and so you need to make sure that the reader knows which line you are
  referring to from your description.
* Refer to recognised principles or rules when describing your solution. "I thought it
  would be better that way" is not sufficient: you need to have specific reasons.

**DON'T**

* Include multiple examples. Make the decision about which example shows your best
  work and use that one.

# Code Review

This section documents my practical work from week 4.  This week I was given some code review challenges which involved looking at different pieces of code and assessing whether or not they violate certain convention or exhibit certain coding smells.

Most of these concepts are either completely new ideas to me or some are even the opposite of what I was taught before which made these code reviews difficult to fully understand and this was my first time taking on this kind of task.

Also I feel that a lot of these concepts can become quite opinion based in some cases which led to me getting some answers wrong that I believe are right following my understanding of the concetps.

In the case of the challenge I have decided to showcase here, I did not get any of the answers right, but it is still the piece of code I want to talk about to try to justify my answers and showcase my skills at reviewing code.  Below is the code in question.

```
class Program
{
    public static void Main(string[] args)
    {
        // create a new invoice
        var invoice = new Invoice
        {
            InvoiceNo = 1,
            Customer = "John Doe",
            IssuedDate = new DateOnly(2023, 4, 1),
            Description = "Website Design",
            Amount = 1000,
            Tax = Amount * CURRENT_TAX_RATE
        };

        invoice.Save();

    }
} 
```

The first problem that I identified in this code is that the Amount and CURRENT_TAX_RATE variables aren't really descriptive enough names to fully explain their purpose, which I would argue makes the code less simple to read, violating the KISS principle.  Also this CURRENT_TAX_RATE is not seen anywhere else in the code so it's not obvious where this comes from violating the same principle.

Next inside the Invoice object the Tax variable is created by multiplying Amount and CURRENT_TAX_RATE.  Initially it seems that the repsonsibility of an Invoice object is to store the values of an Invoice, but now it is also carrying out a calculation giving it more than one responsibility violating the Single Responsibility Principle.

The CURRENT_TAX_RATE variable uses underscores in the name of the variable and the Standard C# Coding Conventions state that this should not be done meaning this variable name violates this principle as well as the best practice rule of using appropriate names for variables.

Finally I thought that this code exhibited the primitive obsession coding smell.  This is because the amount variable in the Invoice object from what we can tell is an int, and a more complex data type like a currency data type including a curreny symbol and a decimal point with two decimal places would be much better for representing this variable.

The below code is my suggested fixed code.

```
class Program
{
    public static void Main(string[] args)
    {
        // create a new invoice
        var invoice = new Invoice
        {
            InvoiceNo = 1,
            Customer = "John Doe",
            IssuedDate = new DateOnly(2023, 4, 1),
            Description = "Website Design",
            Price = 1000,
            WithTaxPrice = CalculateTax(Price);
        };

        invoice.Save();

    }

  public int CalculateTax(int price)
  {
      private int currentTaxAmount = 0.7;
      private int percentage = price * currentTaxAmount;
      return percentage
  }
}
```

In my suggested fix I have changed the name of the Amount variable to Price and the Tax variable to WithTaxPrice as they are slightly more descriptive names for the variables.

I taken the calculation out of the Invoice object and created a new method called CalculateTax(price) which takes an int as an arguement.  WithTaxPrice equals the return value of this method when the Price is entered as the arguement.

In the CalculateTax method it stores the currentTaxAmount variable.  Also it has a variable called percentage which multiplies the passed in value with the currentTaxAmount, then the percentage is returned.

From my changes, the problems with the KISS principle are resolved as the more descriptive variable names make the code easier to read and understand.  Moving the calculation from the Invoice object to a new method resolves the issue with the Single Responsibility principle.  Finally, the violation of the Standard C# Coding Conventions is fixed by taking the underscores out of the CURRENT_TAX_RATE variable name.

The only possible issue not addressed in my fix is the possible exhibition of the primitive obsession code smell which would require using a new better fitting data type for the amount variable.
