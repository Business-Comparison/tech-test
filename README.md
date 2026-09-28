# Junior Developer Technical Test

Thanks for taking the time to do this test. It's a small, realistic task: a customer fills in a form, you check what they entered, send it to an API and tell them it worked.

This repository is a **fresh Laravel 13 application running on PHP 8.5**. We've removed the default welcome page and route, so there are no routes, views or controllers yet. That's where you come in.

There's no single right answer. We're more interested in how you approach the problem than in a polished finish. If you run out of time, write down what you would have done next.

---

## Contents

1. [Getting set up](#1-getting-set-up)
2. [The task](#2-the-task)
3. [About webhook.site](#3-about-webhooksite)
4. [What we're looking for](#4-what-were-looking-for)
5. [Optional extras](#5-optional-extras)
6. [Submitting your work](#6-submitting-your-work)

---

## 1. Getting set up

### Requirements

- PHP 8.5
- [Composer](https://getcomposer.org/)
- Node.js 20+ and npm (only needed if you want to use Vite/Tailwind for your front end)

If you don't have PHP installed, [Laravel Herd](https://herd.laravel.com/) (macOS/Windows) or [php.new](https://php.new) is the quickest way to get PHP and Composer.

### Installation

```bash
git clone <this-repo-url> tech-test
cd tech-test
composer run setup
```

`composer run setup` installs the PHP and JavaScript dependencies, creates your `.env` file, generates an app key, sets up a local SQLite database and builds the front-end assets.

### Running the app

```bash
composer run dev
```

This starts the Laravel development server and Vite together. Open [http://localhost:8000](http://localhost:8000). You'll see a 404 at first because there are no routes yet.

If you're using Herd, you can visit `http://tech-test.test` instead.

---

## 2. The task

Build a small customer details form that sends its data to an external API and then shows the customer a results page.

### 2.1 The form

Create a **single page** with a form that collects these fields:

| Field              | Notes                                       |
| ------------------ | ------------------------------------------- |
| First name         | Required                                    |
| Last name          | Required                                    |
| Email address      | Required, must be a valid email address     |
| Phone number       | Required, must look like a real phone number |
| Date of birth      | Required, must be a valid date in the past  |
| Marketing consent  | Optional. A yes/no choice for whether the customer is happy to receive marketing |

Beyond these minimums, choose whatever validation rules you think make sense.

### 2.2 Validation

- The form **must be validated before anything is sent to the API**.
- If validation fails, send the user back to the form with clear error messages. The values they already entered should still be there so they don't have to type them again.

### 2.3 Sending the data to the API

Once the form passes validation, send the customer's details to an external API.

- **Endpoint:** your own unique [webhook.site](https://webhook.site) URL (see [section 3](#3-about-webhooksite))
- **Method:** `POST`
- **Body:** JSON containing the submitted form data. You decide the structure.
- **Authentication:** the request must include a **bearer token** in the `Authorization` header. Use this token:

  ```
  bc_test_9f4e2a7c1d8b6053e1a4
  ```

  Treat this token as if it were a real API credential.

### 2.4 The results page

- When the API request **succeeds**, show the user a results page confirming their submission and showing the details they submitted.
- When the API request **fails** (for example, the API returns an error or can't be reached), don't show the results page. Tell the user something went wrong in a friendly way and don't lose what they typed.

### Out of scope

- You **don't** need to save anything to the database.
- You **don't** need authentication, user accounts or an admin area.
- You **don't** need to spend lots of time on styling. Clean and usable is enough. Tailwind is already installed if you'd like to use it, but plain CSS or no CSS at all is fine.

---

## 3. About webhook.site

[webhook.site](https://webhook.site) is a free tool for testing code that sends HTTP requests. It gives you a unique URL that accepts any request you send to it and shows you exactly what arrived. You don't need to build or run a real API yourself.

### How to use it

1. Go to [https://webhook.site](https://webhook.site). You don't need an account.
2. You'll be given a **unique URL** straight away. It looks something like this:

   ```
   https://webhook.site/1b2c3d4e-aaaa-bbbb-cccc-123456789abc
   ```

   This is the endpoint your app should `POST` to.
3. Keep the webhook.site tab open. Every time your app sends a request to that URL, it appears in the left-hand list in real time.
4. Click a request to see its details:
   - the **HTTP method** (it should be `POST`)
   - the **headers** (look for `Authorization: Bearer bc_test_...` and `Content-Type: application/json`)
   - the **raw body** (your JSON)

This is the easiest way to check your request looks right.

### Things to know

- **It doesn't check your token.** webhook.site will accept the request whatever you put in the `Authorization` header. We still want you to send it correctly, just as you would with a real API. Use the request view to confirm the header is there and formatted properly.
- **By default it always returns `200 OK`.** To test how your app handles errors, click **Edit** at the top of the page and change the default status code (to `500`, for example). Then submit your form again and check that your error handling works.
- **Free URLs are temporary.** They expire after a while and have a limit on how many requests they'll accept. If yours stops working, just create a new one and update your app.
- **Anyone with the URL can see what's sent to it**, so only use made-up test data. Don't use your own real details.

---

## 4. What we're looking for

We're not expecting perfection. These are the kinds of things we'll talk through with you afterwards:

- **Does it work?** Can we fill in the form, see sensible validation errors, and reach the results page after a successful request?
- **Validation.** Have you validated on the server? Are your rules sensible, and are the error messages helpful?
- **The API request.** Is the request built correctly, with the method, headers, JSON body and bearer token? How do you handle a failed request?
- **Handling the token and URL.** Where have you put the bearer token and the webhook URL, and why?
- **Using Laravel.** Are you making use of what the framework gives you rather than reinventing it?
- **Code quality.** Is the code readable, sensibly named and organised so the next developer could pick it up easily?
- **User experience.** Is it clear to the user what to do, what went wrong and what happened?
- **Git history.** Small, meaningful commits are much easier to follow than one big "done" commit.

---

## 5. Optional extras

Only look at these if you've finished the core task and have time left. They're not required.

- Automated tests for your form and API call. Laravel's `Http::fake()` makes it possible to test the API request without actually sending one.
- Client-side validation as well as server-side validation.
- Stop users seeing the results page by going straight to its URL without submitting the form first.
- Stop the same submission being sent twice if the user refreshes or double-clicks.
- Accessibility improvements, such as labels, error messages linked to their fields, and keyboard navigation.

---

## 6. Submitting your work

1. Commit your work as you go.
2. Add a short **`NOTES.md`** file to the root of the project that covers:
   - how to run your solution, if it's any different from the setup above
   - any decisions or assumptions you made, for example your validation rules and where you put the token
   - anything you didn't get to, or would do differently with more time
   - your webhook.site URL, if it's still active, so we can see your requests
3. Share your work with us as instructed in your invitation email, either as a link to your repository or as a zip file. If you send a zip, include the `.git` folder so we can see your commit history, and leave out the `vendor` and `node_modules` folders.

If anything in these instructions is unclear, make a sensible assumption, write it down in `NOTES.md` and carry on.

Good luck!
