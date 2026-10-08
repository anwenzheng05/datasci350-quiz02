# DATASCI 350 - Data Science Computing

## Quiz 02 - Building a website with Quarto and GitHub Pages

### Scenario

A film magazine has hired you to turn its box-office dataset into a small public website. The repository holds `films.csv`: 104 well-known films from 1980 to 2025, with approximate budgets, worldwide revenues, runtimes, and ratings. Your job is to build a four-page Quarto website from it and publish the site on GitHub Pages.

This quiz is worth 6% of the final grade, and covers lectures 10 and 11. It is open-book and open-notes. It is an individual assessment: do not discuss the questions with your colleagues during class. You have 75 minutes.

You must be able to explain every command and every line you submit. The instructor may ask any student to walk through part of their work, during the quiz or right after it.

### Data

`films.csv` has one row per film and seven columns:

| Column | Meaning |
|--------|---------|
| `title` | Film title |
| `year` | Release year |
| `franchise` | Series the film belongs to, or `Standalone` |
| `budget_musd` | Production budget, millions of US dollars |
| `revenue_musd` | Worldwide revenue, millions of US dollars |
| `runtime_min` | Runtime in minutes |
| `imdb_rating` | Public rating, 0 to 10 |

The figures are rounded and not adjusted for inflation, which is fine for a teaching dataset. The file `create-dataset.py` shows how the dataset was built.

The quiz grades your Quarto and Git work, not your Python. Any working plot earns the marks. The line plot follows the lecture 11 examples. The scatter plots use `scatter()` from your earlier work. Where a task needs code you have not seen, the code is given in the task.

Each page runs its own Python, so start every page with this chunk:

```python
import pandas as pd
import matplotlib.pyplot as plt

films = pd.read_csv("films.csv")
```

The path is relative, so it works while your pages sit next to `films.csv`. On a `FileNotFoundError`, check where your pages are (task 1). Never use an absolute path like `/Users/you/...` (lecture 10).

### Rules

- Work from the command line and your editor throughout. Files created or uploaded through the GitHub website lose points: the grader reads your commit history.
- Record every command you run in a file named `commands.txt` in the repository's root directory. Where a task asks for a short explanation, write it in `commands.txt` too.
- Type every Git command yourself, in any terminal (VS Code's is fine). Do not use the Source Control panel's buttons: they leave no command to record in `commands.txt`.
- When you finish, post the link to your published website AND the link to your fork on Canvas, in the "Assignments" tab under Quiz 02.
- State your AI usage on the index page (see task 6). The syllabus AI policy applies.

### If `git push` asks for credentials

Your machine should already be logged in to GitHub. If a push fails with an authentication error, do not waste time creating tokens: run `gh auth login`, choose GitHub.com, then HTTPS, and log in with the browser. After that, `git push` works normally.

### Setup

1. Fork this repository to your GitHub account.
2. Clone your fork to your machine with the command line.
3. Change directory into the cloned repository.

### Tasks

1. Create a new Quarto website project inside the cloned repository folder. From the repository folder, run `quarto create project website .` in the terminal (the `.` means "the current folder"). Press Enter at the title question, and at the next one press the down arrow to choose `(don't open)`, then Enter. Do not use VS Code's `Quarto: Create Project` here: in a folder that is not empty, it puts the site in a new subfolder. The folder gains `_quarto.yml`, `index.qmd`, `about.qmd`, `styles.css`, and a hidden `.gitignore`.

2. Quarto's `.gitignore` already ignores `/.quarto/` and its temporary notebooks. Add these two lines to the end of it, then stage and commit it with the message "Add gitignore" before you render anything:

    ```text
    /_site/
    __pycache__/
    ```

    If there is no `.gitignore` (check with `ls -a`), you are not inside your cloned repository: go back to setup step 3 and redo task 1.

3. In `_quarto.yml`, set the website title to `Box Office Numbers`.

4. Delete `about.qmd` and remove it from the navigation bar. Then, in `_quarto.yml`, add `budget-revenue.qmd`, `runtime-rating.qmd` and `bond.qmd` (tasks 7 to 9 create them) to the navigation bar with exactly these link texts: `Budget and Revenue`, `Runtime and Ratings`, `The Bond Films`.

5. In `_quarto.yml`, change the theme to just one theme of your choice from [Quarto's theme list](https://quarto.org/docs/output-formats/html-themes.html). Then add `freeze: auto` under a new `execute:` key.

6. Edit `index.qmd` so the home page has: a title, two or three sentences describing the dataset, links to the three analysis pages, and one final line stating which AI tools you used during the quiz (or that you used none).

7. Create `budget-revenue.qmd`: a short introduction and a scatter plot of budget against revenue. Show the code (`echo: true`), and give the plot a caption with `fig-cap` and a label starting with `fig-`. Refer to the plot in your introduction with `@fig-...`, so it renders as a numbered link.

8. Create `runtime-rating.qmd`. Plot runtime against rating as a scatter plot, again with the code visible, a caption and a new `fig-` label. Above the plot, write a sentence or two that cites it with `@fig-`. Below it, add a table of mean rating by decade with this code (given because we have not covered it):

    ```python
    films["decade"] = films["year"] // 10 * 10
    films.groupby("decade")["imdb_rating"].mean().round(2)
    ```

9. Create `bond.qmd`. Select the Bond films with `bond = films[films["franchise"] == "James Bond"]`. Add a line plot of their revenue over time, and one sentence that reports the first and last Bond year using inline code, so it updates if the data changes. Wrap each value in `int()`, or it prints as `np.int64(...)`.

10. Render the website. Confirm that a `_freeze/` folder appeared, then stage and commit everything with the message "Add site pages and freeze".

11. Publish the site with `quarto publish gh-pages`, as shown in lecture 11. (If that command fails on your machine, the fallback is the manual route: set `output-dir: docs` in `_quarto.yml`, render, commit, push, and enable GitHub Pages from the `docs` folder in your fork's settings. Note in `commands.txt` which route you used.)

12. Open the published link and check that all four pages load and the navigation works. Fix and republish if not. On the fallback route, the first build can take a couple of minutes.

13. Update `commands.txt` with every command you used, then stage and commit it with the message "Add command log". Write the `git add`, `git commit` and `git push` lines into `commands.txt` before you run them.

14. Push everything to your fork (`quarto publish` does not push your `main` branch), then submit the URLs of your published website and of your fork on Canvas. Done! 😊

### Bonus tasks

Attempt these only after finishing the main tasks, if you still have time. Document every step in `commands.txt`. Afterwards, republish, commit and push again.

1. Give the site a custom look: edit `styles.css` (change at least the link colour and one font setting) and make sure `_quarto.yml` points at it.
2. Give the site paired light and dark themes in `_quarto.yml`, so the toggle appears in the navigation bar.

Best of luck!
