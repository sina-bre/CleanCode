# Clean Code

## Naming

<font size="3"> Q1. Explain consistency in naming rules.</font>

<font size="2">An important part of using proper names is **consistency**.<br>
If you used `fetchUsers()` in one part of your code, you should also use `fetchProducts()` - and not `getProducts()` - in another part of that same code.

Generally, it doesn't matter if you prefer `fetch...()`, `get...()`, `retrieve...()` or any
other term but you should be consistent!
</font>

---

<font size="3"> Q2. What concepts are important in vertical formatting?</font>

<font size=2>

- Adding Blank Lines
- Vertical Density & Vertical Distance
- Ordering Functions & Methods
- Splitting Code Across Files
  </font>

---

<font size="3"> Q3. Please provide an explanation about vertical Vertical Density & Vertical Distance.</font>

<font size=2>Vertical density simply means that related concepts should be kept closely together.<br>
Vertical distance means that concepts which are not closely related should be separated.
That affects both individual statements inside of a function as well as functions/ methods
as a whole.</font>

---

<font size="3"> Q4. What concepts are important in Horizontal formatting?</font>

<font size=2>

- Breaking Lines Into Multiple Lines
- Using Short Names
  </font>

---

<font size="3"> Q5.What are the types of bad comments?</font>

<font size="2">

- Dividers & Markers
- Redundant Information
- Commented Out Code
- Misleading Comments
  </font>

---

<font size="3"> Q6.What are the types of good comments?</font>

<font size="2">

- "Required" Explanations
- Warnings
- Todo Notes
  </font>

---

<font size="3"> Q7. Please provide an explanation and example about adding blanck lines in vertiacl formatting.</font>

<font size="2">Have a look at this example:

```js
function login(email, password) {
  if (!email.includes("@") || password.length < 7) {
    throw new Error("Invalid input!");
  }
  const user = findUserByEmail(email);
  const passwordIsValid = compareEncryptedPassword(user.password, password);
  if (passwordIsValid) {
    createSession();
  } else {
    throw new Error("Invalid credentials!");
  }
}
function signup(email, password) {
  if (!email.includes("@") || password.length < 7) {
    throw new Error("Invalid input!");
  }
  const user = new User(email, password);
  user.saveToDatabase();
}
```

The above code uses good names and is not overly long - still it can be challenging to
digest. Compare this code snippet:

```js
function login(email, password) {
  if (!email.includes("@") || password.length < 7) {
    throw new Error("Invalid input!");
  }

  const user = findUserByEmail(email);

  const passwordIsValid = compareEncryptedPassword(user.password, password);

  if (passwordIsValid) {
    createSession();
  } else {
    throw new Error("Invalid credentials!");
  }
}

function signup(email, password) {
  if (!email.includes("@") || password.length < 7) {
    throw new Error("Invalid input!");
  }

  const user = new User(email, password);
  user.saveToDatabase();
}
```

It's the exact same code but the extra blank lines help with **readability**.

</font>

---

<font size="3"> Q8. Please provide an explanation and example about ordering functions and methods in vertical formatting?</font>

<font size="2">
When it comes to ordering functions and methods, it is a good practice to follow the
"stepdown rule".

A function A which is called by function B should be (closely) below function B - at least
if your programming language allows such an ordering.

```js
function login(email, password) {
validate(email, passsword);
...
}
function validate(email, password) {...}
```

</font>

---

<font size="3"> Q9. Please provide an explanation about splitting code across files in vertical formatting?</font>

<font size="2">If your **code file gets bigger** and or if you have a lot of different "things" in one file (e.g.multiple class definitions), it is considered a good practice to split that code across multiple files and to then use import and export statements to connect your code.

This ensures that your individual code files as a whole stay readable.</font>

---

<font size="3"> Q10. Please provide an explanation and example about breaking lines into multiple lines in horizontal formatting?</font>

<font size="2">

Good code should use **relatively short lines** and you should consider splitting code across multiple lines if it becomes too long.

```js
const loggedInUser =
  email && password
    ? login(email, password)
    : login(getValidatedEmail(), getValidatedPassword());
```

This is too long - and too much code in one line.

This snippet holds the same logic but is easier to read:

```js
if (!email && !password) {
  email = getValidatedEmail();
  password = getValidatedPassword();
}
const loggedInUser = login(email, password);
```

</font>

---

---

## Functions (& Methods)

<font size="3"> Q1. Why minimizing the number Of parameters is important concept in functions? Please explain it with example.</font>

<font size="2">

The **fewer parameters a function has, the easier it is to read and call** (and the easier it is
to read and understand statements where a function is called).

expmple:

```js
createUser("Max", "Max", "test@test.com", "testers", 31, ["Sports", "Cooking"]);
```

Calling this function is no fun. We have to remember which parameters are required and in which order they have to be provided.

Reading this code is no fun either - for example, it's not immediately clear why we have two `'Max'` values in the list.

</font>

---

<font size="3"> Q2. Why "no parameters" is not always an good option?</font>

<font size="2">because it is the capability to take parameters that makes functions dynamic and flexible.</font>

---

<font size="3"> Q3. Why function body should be kept small?</font>

<font size="2">

Because a smaller body means less code to read and understand. But in addition, it also forces you (ideally) to write highly readable code - for example by extracting other functions which use good naming.

Consider this example:

```js
function login(email, password) {
  if (!email.includes("@") || password.length < 7) {
    throw new Error("Invalid input!");
  }
  const existingUser = database.find("users", "email", "==", email);
  if (!existingUser) {
    throw new Error("Could not find a user for the provided email.");
  }
  if (existingUser.password === password) {
    // create a session
  } else {
    throw new Error("Invalid credentials!");
  }
}
```

If you read this snippet, you will probably understand pretty quickly what it's doing.
Because it's a short, simple snippet.

Nonetheless, it'll definitely take you a few moments.

Now consider this snippet which does the same thing:

```js
function login(email, password) {
  validateUserInput(email, password);

  const existingUser = findUserByEmail(email);

  existingUser.validatePassword(password);
}
```

And that's the goal! Having short, focused functions which are easy to read, to
understand and to maintain!

</font>

---

<font size="3"> Q4. What is "Do One Thing" in functions?</font>

<font size="2">

In order to be small, functions should just do one thing. Exactly one thing.
This ensures that a function doesn't do too much.
But what is "one thing"?

look at this function:

```js
function login(email, password) {
  validateUserInput(email, password);
  verifyCredentials(email, password);
  createSession();
}
```

Is this function doing one thing?

You could argue it does three things:

- Validate the user input
- Verify the credentials
- Create a session

A function is considered to do just one thing if all operations in the function body are on the same level of abstraction and one level below the function name.
</font>

<font size="3"> Q5. What is levels of abstraction?</font>
