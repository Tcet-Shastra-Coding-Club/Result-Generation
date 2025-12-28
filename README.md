# HackerRank Leaderboard Automation Pipeline

This project automates the **end-to-end result processing workflow** for a HackerRank coding contest (used for *TCET Shastra*), starting from raw participant data to a final merged results CSV.

The pipeline is designed to work with **private HackerRank contests**, where leaderboard access is restricted to **signed-in users**.

---

## Overview of the Pipeline

The workflow consists of **four clear stages**:

1. **Clean participant HackerRank IDs**
2. **Manual login to HackerRank (one-time per session)**
3. **Automated leaderboard scraping using the same browser session**
4. **Merging scraped scores with participant data**

The final output is a **ready-to-publish results CSV**.

---

##  Tech Stack

* **Python 3.9+**
* **Selenium** – browser automation
* **Pandas** – data cleaning & merging
* **Google Chrome + ChromeDriver**

---

##  Project Structure

```
project/
│
├── cleaned_test_2026_csv.csv      # Cleaned participant data
├── leaderboard_scraped.csv        # Raw scraped leaderboard data
├── final_shastra_results.csv      # Final merged results
├── main-pipeline.ipynb / script.py       # Main automation script
├── requirements.txt
└── README.md
```

---

##  Installation

###  Create a virtual environment (recommended)

```bash
python -m venv venv
source venv/bin/activate   # Windows: venv\Scripts\activate
```

###  Install dependencies

```bash
pip install -r requirements.txt
```

###  Ensure Chrome & ChromeDriver

* Install **Google Chrome**
* Install **ChromeDriver** matching your Chrome version
* Ensure `chromedriver` is in PATH

---

##  Input Files

### Participant CSV (Provided by Docs / T&P Team)

Must contain a column named:

```
HackerRank ID
```

Example:

| Name  | HackerRank ID |
| ----- | ------------- |
| Rohan | @rohan123     |

---

##  Step-by-Step Usage

###  Step 1: Clean HackerRank IDs

Purpose: Remove `@` prefix to match leaderboard IDs.

Output:

```
cleaned_test_2026_csv.csv
```

This step ensures accurate joins later.

---

### Step 2: Launch Browser & Manual Login

The script launches **one Chrome instance** with Selenium detection bypass.

You must:

1. Wait for Chrome to open
2. Log in to HackerRank manually
3. Ensure you reach the **Dashboard** page

This is required because:

* The contest leaderboard is **private**
* HackerRank blocks unauthenticated access

 **Do not close the browser** after login.

---

###  Step 3: Scrape Leaderboard (Same Session)

After login, the script:

* Opens the contest leaderboard URL
* Sets pagination to 100 entries per page
* Iterates through all pages
* Extracts:

  * HackerRank ID
  * Score
* Removes duplicate users

Output:

```
leaderboard_scraped.csv
```

---

### Step 4: Merge Results

The cleaned participant data is merged with leaderboard scores using:

```
HackerRank ID (LEFT JOIN)
```

Output:

```
final_shastra_results.csv
```

Participants who **did not attempt** the contest will have empty scores.

---

## Final Output

The final CSV contains:

* All original participant columns
* Score column from HackerRank

This file is **ready for**:

* Result declaration
* Rank calculation
* Certificate generation
* Internal reports

---

##  Important Notes

* Only **one Selenium driver** is used throughout
* Manual login is mandatory for private contests
* Do **not** run the scraper before logging in
* Re-running only requires refreshing the session if logged out

---

##  Common Issues & Fixes

### Leaderboard not loading

* Ensure you are logged in
* Check contest URL

### Empty scraped file

* HackerRank DOM changed
* Contest may be ended or hidden

### Session expired

* Close browser
* Re-run and login again

---



