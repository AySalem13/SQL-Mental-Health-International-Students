# Mental Health & Length of Stay

This is a small practice project where I used **PostgreSQL** and **Power BI** to explore how the length of stay in a foreign country might relate to mental health outcomes for international university students.

Instead of just dropping code, this README walks through my **step-by-step process** and what I was thinking at each stage.

---

## Step 1 – Understanding the question

The original prompt (from a SQL learning exercise on DataCamp) asked:

> Does going to university in a different country affect your mental health?  
> And does the **length of stay** in the host country play a role?

The dataset included:

- both **international** and **domestic** students  
- several psychological scores, including:
- `todep` – depression score (PHQ-9)
- `tosc` – social connectedness score (SCS)
- `toas` – acculturative stress score (ASISS)
- `stay` – current length of stay (years)  
- `inter_dom` – whether the student is international or domestic

The original study suggested:

- international students are at higher mental health risk  
- **social connectedness** is linked to lower depression  
- **acculturative stress** is linked to higher depression

My goal here was not to “prove” anything, but to see if the **patterns in this dataset** look similar.

---

## Step 2 – Focusing on the right population

Because the research question is about international students, the first decision was:

> Filter the data to **international students only**.

In SQL terms, that means:
```WHERE inter_dom = 'Inter'```

## Step 3 — Building the SQL Summary Table

To understand how mental health changes with length of stay, I grouped the data by stay and calculated:

-number of international students at each stay length
-average depression score (PHQ-9)
-average social connectedness score
-average acculturative stress score

The added the SQL query in its own file in this repository. I then exported the result and took it to Power BI for better analysis.

## Step 4 — First Look at the Full Trend (Years 1–10)

I created three line charts:

-PHQ-9 average vs stay
<img width="528" height="292" alt="image" src="https://github.com/user-attachments/assets/af70288c-05f6-4016-a2e7-a285d4506476" />
-Acculturative stress average vs stay
<img width="528" height="292" alt="image" src="https://github.com/user-attachments/assets/a0f461ee-68ae-4405-8de7-000cf1b8713b" />
-Social connectedness average vs stay
<img width="527" height="289" alt="image" src="https://github.com/user-attachments/assets/6d2ae152-4c20-4367-ab6b-2730436ed4a3" />

🧠 Initial observation

The lines showed patterns, but there were always some strange spikes/drops after year 5.
That made me wonder “Is the data actually reliable for those years?”

So I checked the sample sizes.

## Step 5 — Checking/Reducing the Sample Size by Stay Year

I went back to run a new query to check back the sample size: 

SELECT stay, COUNT(*) AS count_int
FROM students
WHERE inter_dom = 'Inter'
GROUP BY stay
ORDER BY stay;

The results: Years 1–4 → LARGE number of students / Years 5–10 → only 1–3 students per category

This immediately told me that anything that happens after year 5 is probably noise, not a real pattern as I cant really make any conclusion based on such a small sample size.
So I focused my analysis on the reliable portion of the data. I added a filter on PowerBI to show only Stay <= 4 and got the following line charts, which made me see a clear trend 
from eachh line chart and answered the questions I have: 
The PHQ-9 (depression) had an upwards trend
<img width="527" height="293" alt="image" src="https://github.com/user-attachments/assets/1d0ee414-a858-4ab9-86c6-751ebbaea9ed" />
The Acculturative stress had an upwards trend
<img width="525" height="292" alt="image" src="https://github.com/user-attachments/assets/55a1fc47-a7df-4d45-a52e-fc8c21fa8069" />
The Social connectedness had a downward trend 
<img width="527" height="286" alt="image" src="https://github.com/user-attachments/assets/34f8108c-092d-43ad-9b2e-74993fcfc0ec" />

I could see that both the PHQ and AS both move in the same direction over the years which sugggests that depression and stress may be related.
On the other hand, PHQ and SCS move in opposite directions which suggests that low connectedness may lead to higher depression.






