# When Was the Golden Era of Video Games?

This project tackles a nostalgic yet data-driven question: **When was the golden era of video games?** Using a curated dataset of the **top 400 video games released since 1977**, the analysis combines **critic scores**, **user ratings**, and **sales figures** to identify peak years in gaming history—both in terms of quality and popularity. By comparing aggregated annual metrics and examining discrepancies between critics and players, the project reveals whether gaming’s “golden age” occurred decades ago or is still unfolding today.

Built entirely in **PostgreSQL** within **DataCamp’s Datalab**, this project applies SQL skills like **JOINs**, **GROUP BY**, **filtering with HAVING**, and **set operations** to uncover trends across time.

---

## 🎯 Project Objectives

- Identify the **top 10 years** with the highest **average critic scores** (Metacritic), requiring **at least 5 reviewed games per year**
- Similarly analyze **user-rated golden years** using average user scores
- Compare critic and user perspectives to find years where **both agree** on exceptional quality
- Investigate whether **high-scoring years** also correlate with **high game sales**
- Define the “Golden Era” as years with **average scores > 9/10** from either critics or users

---

## 🗃️ Dataset Overview

The database contains **four tables**, each limited to 400 rows for this project:

| Table | Description |
|------|-------------|
| `game_sales` | Game name, platform, publisher, developer, release year, and sales (millions) |
| `reviews` | Game name, critic score, user score (from Metacritic) |
| `critics_avg_year_rating` | Pre-aggregated: year, number of games, average critic score |
| `users_avg_year_rating` | Pre-aggregated: year, number of games, average user score |

> 🔍 Note: The full dataset (13,000+ games) is available on **Kaggle**; this project uses a representative subset.

---

## 🔍 Methodology

### 1. Best-Selling Games (Exploratory)
```sql
SELECT * FROM game_sales ORDER BY games_sold DESC LIMIT 10;
```
→ Revealed classics like *Wii Sports*, *Super Mario Bros.*, and *Minecraft* dominate sales.

### 2. Top Critic-Rated Years
```sql
SELECT 
  year, 
  COUNT(*) AS num_games, 
  ROUND(AVG(critic_score), 2) AS avg_critic_score
FROM game_sales g
JOIN reviews r ON g.name = r.name
GROUP BY year
HAVING COUNT(*) > 4
ORDER BY avg_critic_score DESC
LIMIT 10;
```
→ **1998** emerged as the peak critic year (avg score: **9.32**), followed by **2004** and **2002**.

### 3. Identifying the Golden Era
Joined pre-aggregated critic and user tables to find years where **either average > 9**:
```sql
SELECT 
  c.year,
  c.num_games,
  c.avg_critic_score,
  u.avg_user_score,
  ROUND(c.avg_critic_score - u.avg_user_score, 2) AS diff
FROM critics_avg_year_rating c
JOIN users_avg_year_rating u ON c.year = u.year
WHERE c.avg_critic_score > 9 OR u.avg_user_score > 9
ORDER BY c.year ASC;
```

### 4. Key Observations
- **1998**: Critic avg = **9.32**, User avg = **9.40** → near-perfect alignment
- **Late 1990s to mid-2000s** show consistently high scores
- Surprisingly, **no year after 2010** surpassed a **9.0 average** from critics
- Users rated **1997** highly (9.5), even though critics did not

---

## 📊 Key Findings

- 🥇 **1998** stands out as the strongest candidate for the **Golden Era**—loved by both critics and players  
- 🎮 The **late '90s and early 2000s** represent a sustained “golden window” of innovation and quality  
- 📉 Despite massive sales growth, **modern games (post-2010)** rarely achieve the near-perfect consensus of earlier eras  
- 👥 **Critics and users often diverge**: users favor emotional/nostalgic titles; critics prioritize technical polish

> 💡 Conclusion: The **Golden Era of Video Games** likely occurred **between 1997 and 2004**, with **1998 as its pinnacle**.

---

## 🛠️ Tools Used

- **PostgreSQL** – for querying, joining, and aggregating gaming data  
- **DataCamp Datalab** – cloud SQL notebook environment  
- **SQL Joins, GROUP BY, HAVING, ROUND, ORDER BY** – core query techniques applied  
- **Set theory & comparative analysis** – to align critic and user perspectives

---

## 📌 How to Use

To reproduce:
1. Load the four tables (`game_sales`, `reviews`, `critics_avg_year_rating`, `users_avg_year_rating`) into DataCamp Datalab  
2. Run the provided SQL queries step-by-step  
3. Modify filters (e.g., raise the score threshold) to test alternative definitions of “golden era”

> 🔗 Inspired by real Metacritic and sales data. Ideal for gaming enthusiasts, data analysts, and nostalgic gamers alike!

---

## ✍️ Author

Completed by **Achraf Salimi** — blending pop culture curiosity with rigorous data analysis to answer timeless questions through the power of SQL.