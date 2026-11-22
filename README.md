# Put your capstone online with Streamlit

You will turn your capstone notebooks into a public web app that anyone can open from a link. Follow the steps **in order**. Each step tells you what to do and how to know it worked (**Done when**). Don't go on to the next step until the current one is done.

| Part | What | Who does it | Time |
| --- | --- | --- | --- |
| 0 | Check your computer is ready | You | 10 min (longer if you need to install something) |
| 1 | Create your Streamlit account | You | 5 min |
| 2 | Connect Streamlit to GitHub | You | 5 min |
| 3 | Design, build and test the app | You + AI assistant | 2–4 h |
| 4 | Deploy the app | You | 15 min |
| 5 | Check it and share it | You | 15 min |

---

## Part 0: Set up your computer

You'll build and test the app on your own computer, in **VS Code**, with an AI assistant that can work on your files. Tick each box below. If you can't tick one, follow the matching step in **[SETUP_GUIDE.md](SETUP_GUIDE.md)**. It has instructions for Windows, macOS and Linux.

- [ ] VS Code is installed. ([Step 0.1](SETUP_GUIDE.md#step-01-install-vs-code))
- [ ] `git --version` prints a version number. ([Step 0.2](SETUP_GUIDE.md#step-02-install-git))
- [ ] `python --version` (Windows) or `python3 --version` (macOS and Linux) prints 3.11, 3.12 or 3.13. ([Step 0.3](SETUP_GUIDE.md#step-03-install-python))
- [ ] Your final notebooks and data are in your GitHub repository, and the repository is **public**. ([Step 0.4](SETUP_GUIDE.md#step-04-make-sure-your-repository-on-github-is-complete))
- [ ] Your repository is cloned and open in VS Code. ([Step 0.5](SETUP_GUIDE.md#step-05-clone-your-repository-onto-your-computer))
- [ ] An AI assistant runs **inside VS Code in agent mode** (Copilot, Claude Code or Gemini Code Assist) and passed the test. Gemini in Colab and chat websites won't work. ([Step 0.6](SETUP_GUIDE.md#step-06-set-up-an-ai-assistant-that-works-on-your-computer))
- [ ] You have the file **`AI_APP_BUILDER.md`** from your instructor.

---

## Part 1: Create your Streamlit account (manual)

**Step 1.** Go to <https://share.streamlit.io> and choose to sign up or sign in.

**Step 2.** Choose **Continue with GitHub** and sign in with the **same GitHub account that owns your capstone repository**. Signing up with GitHub makes Part 2 shorter. Google or email also work, but you'll have to connect GitHub separately in Part 2.

**Done when:** you are signed in to Streamlit. If it shows a form asking for your name and other details, fill it in.

---

## Part 2: Connect Streamlit to GitHub (manual)

**Step 3.** When Streamlit asks you to connect or authorize GitHub, click **Connect GitHub account** or **Authorize streamlit**. If you're asked to choose an account, pick the one that owns your capstone.
- If you signed up with Google or email, Streamlit asks you to do this right after signing up.
- If you missed that screen, click your profile picture (top right) → **Settings** → **Linked accounts**, and connect GitHub there.

**Step 4.** Check that the connection works:
1. Click **Create app**, then choose the option to deploy a public app from GitHub.
2. Click the **Repository** field.
3. Look for your capstone repository in the list.
4. Close the form **without deploying**. You'll come back to it in Part 4.

**Done when:** your capstone repository appears in the Repository list.

If it doesn't appear:
- Check that the repository is public (see Part 0).
- Check that you signed in with the GitHub account that owns the repository.
- If you can type the repository's GitHub link into the field instead, that works too.

---

## Part 3: Design, build and test the app (with your AI assistant)

**Step 5.** Copy `AI_APP_BUILDER.md` into the **top folder of your repository**, the same folder as your `README.md`. You can drag it into VS Code's file panel. Don't commit it: the assistant keeps it out of your repository, and you delete it at the end.

**Step 6.** In VS Code, with your repository open, open your AI assistant's chat (in **Agent** mode for Copilot or Gemini) and paste this message:

```
Read AI_APP_BUILDER.md and follow it step by step.
Ask me one question at a time and wait for my answer before moving on.
```

**Step 7. Design the app (this part is your job, not the AI's).** The assistant first summarises your project back to you. Correct it if anything is wrong. Then it asks you four things:

1. **Who is the app for?** One main person, called a *persona*. For example: *"As a maritime inspector, I need to know which vessels to inspect first."*
2. **What does that person want to do?** These are *user stories*, written like this: *"As a maritime inspector, I want to see each vessel's risk score, so that I inspect the riskiest ones first."*
3. **What do they see first, next and later?** This is the order of the pages, and the order of things on each page.
4. **What can they change, and what must they not misread?** For example: filters, inputs and warnings about the model's limits.

If you don't know an answer, ask for suggestions, or say *"pick for me and explain why"*. Before you answer, picture one real person who could use your results in their work. Your instructor is not the persona.

The assistant writes your answers into **`app/APP_DESIGN.md`**. Read it carefully, then reply **approved** or tell it what to change.

**Done when:** `app/APP_DESIGN.md` exists and you have approved it.

**Step 8. Build and test.** The assistant puts the whole app in one new folder, **`app/`**, so your data, notebooks and docs stay as they are. It works on your `main` branch; your repository keeps a single branch. It will ask for permission to run commands. Read each request before you approve it.

**Done when:**
- the assistant reports that all tests pass, and
- the app opens in your browser at <http://localhost:8501>.

**Step 9. Check the app yourself.**
1. Go through each user story in `app/APP_DESIGN.md`. Do what your persona would do, and check that it works.
2. Tell the assistant about anything that is wrong, confusing or missing, and check again after it fixes it.
3. When you're happy, let the assistant commit and push to `main`. It shows you the list of files first: check that they are all inside `app/`, apart from a small `README.md` update and the hidden `.streamlit/` settings folder.

**Done when:**
- the assistant shows you a **Deploy settings** card, and
- the `app/` folder is on your repository's GitHub page.

---

## Part 4: Deploy the app (manual)

**Step 10.** Go to <https://share.streamlit.io>, click **Create app**, and choose to deploy a public app from GitHub. Fill in the form with the values from your Deploy settings card:

| Field | Value |
| --- | --- |
| Repository | your capstone repository |
| Branch | `main` |
| Main file path | `app/streamlit_app.py` |
| App URL | optional: pick a short name, such as `yourname-topic` |

**Step 11.**
1. Click **Advanced settings**.
2. Set **Python version** to the version on your card.
3. Leave **Secrets** empty, unless the card says otherwise.
4. Click **Save**, then click **Deploy**.

**Step 12.** Wait for the build, which usually takes 2–10 minutes. You can watch progress in the log panel (*Manage app*, bottom right of the app page).

**Done when:** your app opens at its `….streamlit.app` address.

If the build fails:
1. Copy the red error lines from the log.
2. Paste them into your AI assistant with: *"The Streamlit Cloud build failed with this error. Fix it and push to main."*
3. Streamlit redeploys automatically after each push to `main`. Wait a minute and check again.

---

## Part 5: Check it and share it

**Step 13.** Open your app's link in a **private or incognito window**. Go through your user stories once more. Then open *Manage app* and check that the log has no red errors.

**Step 14.** Tell your AI assistant: *"The app is live at <your URL>. Add the link to the README and push."*

**Done when:** your README on GitHub shows the link.

**Step 15.** Send the link to your instructor, and delete `AI_APP_BUILDER.md` from your repository folder.

---

## Good to know

- **Changing the app later:** ask the assistant to make the change and push it to `main`. The live app updates by itself within a minute or two, so every push to `main` changes the live app.
- **"This app has gone to sleep":** free apps sleep when nobody has visited for a while. Click the button to wake it up, which takes about 30 seconds.
- **"Page not found" pop-up:** with some layouts, this can appear once when someone opens a direct link to an inner page while the app is waking up. The page still loads behind it, so close the pop-up.

## When to ask your instructor

Ask your instructor only after you have tried the fixes above and asked your AI assistant, and only if:
- your repository still doesn't appear in Step 4,
- the build still fails after two rounds of fixes with your assistant, or
- you are not sure your data may be published.
