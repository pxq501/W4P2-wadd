# Week 4 Practical 2: Forms

In Stage 1 of this practical you will implement a contact form. It is important you get the form working as it will be the basis for future JS exercises on working with and storing data. Stage 2 is encouraged but optional and focuses on styling, including setting styles based on input values.

## Stage 1: Creating a Contact Form
For this stage, you will need an HTML file, a CSS file, and a JS file. The CSS and JS files should be linked to the HTML file.

### Exercise 1.1: Create the HTML input fields
You are creating a website for a cat rescue centre. Your target users are potential adopters who want to express interest in adopting a cat. Potential adopters need to provide:
1.	Their full name*
2.	An email address*
3.	A phone number
4.	Whether they have outside space*
5.	Whether or not they have children living in or regularly visiting the home.* If they have children, the age ranges should be provided
6.	Whether there are other pets in the home.* If there are other pets, then they need to provide the species of the other pets (e.g. dog, bird, fish)

As per web convention, the * is used to indicate that a field is required.

Using HTML, create a form that allows potential adopters to submit the above information. Don't worry about CSS or JS just yet; focus on choosing the right input types and structuring your form.

- Your form does not need an action attribute. You will eventually use JavaScript rather than a server-side script. 
- Make sure all your form fields have proper labels, preferably using the explicit labelling approach.
- All input fields should have unique name attributes.
- Don't forget a submit button!
- Make sure that required input fields have the `required` attribute. For radio button groups, only one of the buttons needs to have the `required` attribute. The requirement will be satisfied if any button in the group is selected, even if it is not the button with the `required` attribute.

### Exercise 1.2: Handling form submission

Your form should have a submit button with a unique id attribute. Mine looks like this:

```
<input type="submit" id="submit" value="Submit">
```

Fill in all required inputs (Chrome will prompt you if you forget one) and click Submit. What happens? … You should see that the page refreshes and all input is cleared. This is the default behaviour of input type submit. If your form had an action attribute with a server-side processing script, the values of all named inputs would be sent to the script before the page refreshed. As we're not using a server-side script, you'll need to add some JavaScript to process it instead.

**In your JavaScript file, add a click event listener and event handler to your submit button using the `addEventListener` method.** For the event handler, you can use a named function or an anonymous function.

All functions you've written to date are named - they have a name that you then use to call the function elsewhere in your code. Anonymous functions do not have names. This means they can't be called directly elsewhere in your code. Instead, they are usually passed as arguments to other functions. Here is an example:

```
// Anonymous function with no parameters
function () {
  // function contents
}
```

Here are the factors I consider when deciding if I want a named function or an anonymous function for an event handler:

- Is the event handler unique to a single element?
    - If yes -> Is there any reason why I might want to remove or change that event listener while the webpage is running? (rare!)
        - If yes -> use a named function as these can be removed
        - If no -> use an anonymous function as it is better for security (anonymous functions can't be called by other scripts)
    - If no -> use a named function

These considerations aren't hard and fast rules and there are always exceptions.
For this exercise, my event listener looks like this:

```
document.getElementById("submit").addEventListener("click", function (event) {
  // Function contents go here
});
```

I used an anonymous function. Make sure the id in the selector matches the id of your submit button. Notice that I have included the event parameter—you will need this in your event handler.

**Next, prevent the page from refreshing and clearing the form inputs by adding the following line as the first statement in your event handler:**

```
event.preventDefault();
```

This statement prevents any default behaviours associated with an event from happening and should be used very rarely and only with good reason. It is also important to be aware of the side effects of preventing a particular event: 
- In the case of a submit event, `preventDefault()` 
    - prevents the page from submitting and the form values from being sent to the script in the form's action attribute (if it exists). 
    - prevents the browser's default response to a required input being left blank.
- For all events, `preventDefault()` stops an event from bubbling up the DOM tree.

For this practical, you are temporarily preventing the default behaviour to do some basic client-side form processing. Next week, you will learn how to store submitted data in the client and allow default behaviour to occur in a meaningful manner.

### Exercise 1.3: Conditional rendering of form inputs

Two of the required inputs (whether there are kids and whether there are other pets) have follow up questions that are only relevant if the user selects a particular option. For example, the question about the age range of any kids is only relevant if the user indicates that they do have children at home. If they do have kids, it should also be a requirement for the user to provide the ages of the children.

In this exercise, you will use a combination of CSS and JavaScript to show these follow-up questions if and only if the relevant required fields are selected.

**In your CSS file, set the follow-up questions (including labels!) to have `display: none`.** Check the live preview of your form to make sure the two follow up questions are not visible. 

To show/hide input fields dependent on the value of some other input, you will need to add an event listener for a "change" event on the controlling questions (5 & 6). Questions 5 (children) and 6 (pets) should both have radio button inputs as there are two exclusive answer options (yes / no). If you have used different input types for either question, change them to radio buttons. The radio button options in each question should also be set up as groups—each radio button within a group should have the same name attribute, and the two groups should have different name attributes. For example, in my HTML, the yes and no options for question 5 have name "children" and the yes and no options for question 6 have name "pets".

You will handle change events on question 5 first. **In your script file, use the `getElementsByName()` selector to get all radio buttons relevant to questions 5.** Save the HTMLCollection returned by this method as a variable. Here is my code for reference (modify it to match your HTML):

```
const children = document.getElementsByName("children");
```

**Next, use a for loop to add a "change" event listener to each radio button.** Here is my code:

```
function childOptionChanged(event) {
}

for (let childOption of children) {
    childOption.addEventListener("change", childOptionChanged);
}
```

This time I used a named function for the event handler because multiple elements will use the same event handler.

**Inside your event handler function, add a conditional that checks the value of the event target (`event.target.value`) and changes the CSS display property of the follow up question as appropriate.** The event target value will be the `value` attribute you gave each radio button for questions 5. I used values of "yes" and "no". Here is my completed event handler function:

```
function childOptionChanged(event) {
  const input = document.getElementById("child-ages");
  const label = document.getElementById("child-ages-label");
  if (event.target.value === "yes") {
    input.style.display = "block";
    label.style.display = "block";
  }
  else {
    input.style.display = "none";
    label.style.display = "none";
  }
}
```

…where "child-ages" and "child-ages-label" are the ids I gave the elements I want to show / hide when the user changes their answer for question 6.

Finally, use the same approach to **implement a change event handler for question 7.**

### Exercise 1.4: Form input validation

The final step for this stage is to perform a little input validation when the form is submitted—you will check that, if the user has answered yes to question 6 or 7, that they have also entered something for the follow up question.

**In the click event listener you created for exercise 1.2, add conditionals that check if the user has indicated they have children or pets.** To do this, I recommend using the `checked` attribute on the relevant radio buttons. If you have labelled all your inputs appropriately, each radio button should have a unique id that you can use to select it. Here is a template you can use:

```
if (document.getElementById("id-of-yes-option").checked) {
    console.log("User has answered yes to this question");
}
```

**If the user has answered yes to either question, check that they have also provided a response to the follow up question. If they have skipped the follow up, print a warning message to the console.** Exactly how you do this will depend on the HTML elements you used for the follow up questions. For text inputs or textareas, check the value attribute. If it's empty, they have not answered the question e.g.:

```
if (document.getElementById("my-input").value === "") {
    console.log("Required follow up question not answered!");
}
```

For checkboxes, check that the checked attribute of at least one checkbox is true.

Make your console warning messages more informative than the example above! i.e. specify which question was missed.

## (Optional) Stage 2: Visual Design

### Exercise 2.1: Make the form pretty

Use the CSS techniques you have learned to make your form look more attractive and improve usability. 

### Exercise 2.2: Provide additional feedback

Use a combination of CSS and JavaScript (as appropriate) to notify the user when a field is invalid e.g. when they have indicated they have other pets but haven't provided any information.

Providing feedback on invalid input sometimes includes displaying text at the top of the form or (better) above the affected input that explains what has gone wrong. Sometimes, forms visually highlight invalid inputs by setting their outline or changing the text colour.

It can also be helpful to display a message that confirms when a form has submitted correctly. You will be doing this next week but you may like to get a head start now.
