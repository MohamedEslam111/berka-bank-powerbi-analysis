# 📊 Berka Bank Dashboard: Questions, Insights & Recommendations
*(Revised version — tightened recommendations, minor wording fixes)*

---

## 📌 Data & Methodology

* **Source:** Berka dataset (Kaggle), an anonymised Czech bank from PKDD'99.
* **Currency:** CZK. All figures were recomputed from the source files.
* **Coverage:** Accounts opened 1 Jan 1993 to 29 Dec 1997, loans to 8 Dec 1998, cards to 29 Dec 1998, transactions 1 Jan 1993 to 31 Dec 1998 (1,056,320 rows, all 4,500 accounts). Flat 1998 account and client values are a data boundary, not a sign that growth stopped.
* **By Year charts:** Clients are cumulative (year of the client's first account). Accounts By Year counts accounts with at least one transaction that year. Loans and Cards By Issued are annual. Orders have no date field, so no orders-by-year view is analysed.
* **Customer transactions:** All rows except bank postings (interest `UROK`, service fees `SLUZBY`, penalty interest `SANKC. UROK`). Bank postings are 340,523 rows (32.2% of count, 0.48% of value). Fee rows are recorded under the `VYBER` operation.
* **Activity per account-year:** Transactions divided by account-years, counted from each account's opening date.
* **Geography & Loan Status:** By the account's district. Age at 31 Dec 1998. Loan status:
  * **A** = Finished OK
  * **B** = Finished unpaid
  * **C** = Running OK
  * **D** = Running in debt
* **Lens:** Recommendations are framed for a modern bank reading 1990s data, limited to tools that fit the period (branch/counter service, mail statements, basic ATM and card infrastructure — no mobile or internet banking).

---

## 🎯 1. Overview

### **Q1. Where is the business concentrated, and is Prague a risk?**
> 💡 **Insight:** Prague holds 12.9M of the 103.3M CZK loan book (12.5%), 554 of 4,500 accounts (12.3%) and 11.7% of the population. The three shares match, so it is proportionate. The top 10 cities hold 36.2% of loan value, and north and south Moravia hold 34.9% of accounts.
>
> 🚀 **Recommendation:** No exposure cap is needed on Prague itself. Two concrete follow-ups: (1) set a single-city concentration limit (e.g. flag if any city exceeds ~20% of the loan book) so future growth is monitored, not just today's snapshot; (2) treat north/south Moravia as the priority region for branch staffing and marketing spend, since it already holds over a third of accounts.

### **Q2. How deeply does the bank serve its customers?**
> 💡 **Insight:** 682 accounts have a loan (15.2%) and 892 a card (19.8%). 3,096 accounts (68.8%) have neither, 512 have a loan only, 722 a card only, and 170 both.
>
> 🚀 **Recommendation:** Build the 3,096 "neither" accounts into a ranked cross-sell list, ordered by 1998 transaction volume and account balance where available. Start outreach with the top 20% by activity — they're the most likely to convert and the cheapest to reach through existing statement mailings.

### **Q3. Is growth in activity converting into loans?**
> 💡 **Insight:** In 1998 cards rose 85.5% (242 to 449) and transaction amount 12.2% (1.69bn to 1.89bn), but loans fell 19.4% (196 to 158). The loan drop is entirely in H2 (61 loans versus 103 in H2 1997), while H1 was 97 versus 93.
>
> 🚀 **Recommendation:** Pull loan applications and rejections (not just approvals) for H2 1998 and compare against H2 1997 to see whether demand fell or approval criteria tightened — the current data only shows approved loans, so this is the first gap to close. In parallel, pilot pre-approved loan offers to the 512 loan-only and 3,096 neither-product accounts with stable H2 1998 inflows, to test whether demand is still there.

---

## 💳 2. Transaction

### **Q1. Do customers put in or take out more money?**
> 💡 **Insight:** Debits are 61.7% of transactions (`VYDAJ` 60.1% + `VYBER` 1.6%) against 38.3% credits. In value, credits are 51.6% (3.23bn) and debits 48.4% (3.03bn). The average credit is 7,967 CZK versus 4,653 for a debit (1.71x), and credits exceed debits by 197M CZK (6.5%).Cash deposits (`VKLAD`) are 14.8% of transactions but 38.7% of value.
>
> 🚀 **Recommendation:** Target accounts with large, regular credits (salary-sized deposits recurring monthly) for a savings or term-deposit product, structured as an automatic sweep of a fixed percentage or amount into savings right after each credit lands.

### **Q2. Is the transaction count a fair measure of customer activity?**
> 💡 **Insight:** 32.2% of rows (340,523) are bank postings: interest credits (183,114, average 150 CZK) and service fees (155,832, average 17 CZK). They add up to only 30.2M CZK, or 0.48% of value. Customer-initiated transactions number 715,797.
>
> 🚀 **Recommendation:** Report engagement using customer-initiated transactions only, and add a `is_bank_generated` flag at the data-model level so this exclusion is automatic in every future report, not a manual filter someone has to remember.

### **Q3. How much activity is cash versus cards?**
> 💡 **Insight:** Counter cash (withdrawals 277,509 plus deposits 156,743) is 60.7% of customer transactions and 76.4% of value. Card withdrawals (`VYBER KARTOU`) are 8,036, which is 1.1% of customer transactions and 0.29% of value. 807 of 892 card accounts (90.5%) used one, but the average card withdrawal is 2,261 CZK versus 8,421 for a counter withdrawal. In 1998 card holders made 18.8 counter withdrawals per account against 18.1 for non-holders, so cards added withdrawals instead of replacing cash.
>
> 🚀 **Recommendation:** Within the period's tools, push migration off the teller counter through wider ATM placement and higher ATM/card withdrawal limits, so a card withdrawal can substitute for a counter visit rather than supplement it. Track counter-withdrawal frequency per card holder before and after any limit increase as the success metric — flat or falling counter visits would confirm real substitution.

### **Q4. What drives transaction growth?**
> 💡 **Insight:** Amount grew 9.3x (204.3M to 1,894.8M CZK) and count 11.4x (28,205 to 322,277), with the average ticket stable at 5.8–5.9K from 1994. Activity per account-year is flat at 67.5 (1994) to 71.6 (1998), and every opening cohort made 68.8–72.7 transactions in 1998 regardless of age. 1998 grew 13.3% in count and 12.2% in amount with no new accounts, mostly because accounts opened in 1997 were active for a full year (account-years +10.9%). Per account-year it rose only 2.2%.
>
> 🚀 **Recommendation:** Since growth is base expansion and not deepening engagement, set transactions-per-account-year as the primary KPI going forward (replacing raw transaction count), and pair it with a product-adoption target — e.g. increase the 68.8% "neither loan nor card" segment's product holding by a set percentage per year — since volume alone won't move the flat per-account number.

### **Q5. Which regions are most active?**
> 💡 **Insight:** North Moravia (191,633) and south Moravia (181,751) lead, and Prague is fourth (132,613). Per account the range is only 223.8 (east Bohemia) to 241.7 (north Moravia), against an average of 234.7. In value per account Prague is highest (1.45M CZK) and east Bohemia lowest (1.33M), a 9% spread.
>
> 🚀 **Recommendation:** Keep service capacity allocated in line with each region's account base — the 9% spread is too narrow to justify reallocating staff. East Bohemia is the one region below average on both count and value per account, so it's the sole candidate for a modest engagement push (e.g. a targeted mailing on savings or standing-order products).

---

## 👥 3. Clients

### **Q1. Who are the clients, and who can borrow?**
> 💡 **Insight:** Average age is 44.8, with 2,724 men (50.7%) and 2,645 women (49.3%). Age groups are balanced, from 802 (Under 25, 14.9%) to 982 (45–54, 18.3%). No loan went to anyone 62 or older at loan date (the oldest was 61.8), although 639 owners were 65+ by end-1998. Loan penetration by owner age peaks at 25–34 (20.4%) and falls to 13.3% at 60–64.
>
> 🚀 **Recommendation:** First confirm with the credit policy team whether an age cutoff around 62 is deliberate or coincidental — 234 loans is not enough to rule out chance. If it is policy, the 65+ segment (15.2% of clients) is still open for savings products and low-risk secured credit. Separately, target starter accounts at Under-25 clients and standard lending offers at the 25–54 group, where penetration is already highest.

### **Q2. How did the client base grow?**
> 💡 **Insight:** Cumulative clients went 1,385 → 1,898 → 2,666 → 4,290 → 5,369 (1993–1997). New clients per year were 1,385 / 513 / 768 / 1,624 / 1,079, so 1996 was the biggest jump.
>
> 🚀 **Recommendation:** Treat 1993–1997 as the observed acquisition window and do not read the flat 1998 client figure as a stop in growth — it's a data boundary (see Methodology), not a business trend. If a 1998 acquisition number is needed, source it separately rather than inferring it from this cumulative series.

### **Q3. Where are clients, and how do they relate to accounts?**
> 💡 **Insight:** Prague has 671 clients (12.5%), then Karvina (177), Ostrava (164) and Brno (152). There are 1.19 clients per account: every account has one owner and 869 clients (16.2%) are disponents.
>
> 🚀 **Recommendation:** Design joint/family account packages aimed at the 869 disponents specifically, and flag them as a cross-sell list distinct from primary owners, since they already have a banking relationship but no product of their own.

---

## 🏥 4. Loan

### **Q1. How healthy is the portfolio?**
> 💡 **Insight:** By count, C is 59.1%, A 29.8%, D 6.6% and B 4.5%. Problem loans (B and D) are 11.1% of loans but 15.1% of value. D loans average 249K CZK versus 151K for the portfolio. Of 234 finished loans, 31 (13.2%) ended unpaid. Among the largest 25% of loans (210,654 CZK or more), 19.9% are B or D, versus 7.6% among the smallest 25%.
>
> 🚀 **Recommendation:** Add a mandatory second approval step and require collateral or a guarantor above the 210,654 CZK threshold, where the problem-loan rate is 2.6x higher. Prioritise collections on the 45 currently-running D loans (11.2M CZK exposure) ahead of any new large-ticket lending. Before wiring a `loan_risk_flag` into any early-warning model, confirm its definition with whoever built it — it's a derived column in this analysis, not a field in the source data.

### **Q2. How has lending evolved?**
> 💡 **Insight:** Loan count went 20 / 101 / 90 / 117 / 196 / 158, and amount peaked at 30.7M in 1997 before falling to 24.9M in 1998 (−19.1%). Average size rose from 131.0K to 156.6K (1996) and then stayed flat (157.4K in 1998). Problem rates for the 1994–1997 vintages are stable at 13.3–13.9%, so older cohorts show no deterioration that would explain a cutback. The 1998 vintage (2.5%) is too young to judge.
>
> 🚀 **Recommendation:** Since existing vintages show no rising default risk, the H2 1998 drop is unlikely to be a risk-driven pullback — check for a policy or pricing change, or a demand shift, using internal loan committee records if available. If nothing changed on the bank's side, run a targeted loan offer to active, in-good-standing customers (the C-status, loan-free segment) to test whether demand recovers.

### **Q3. Which cities drive lending?**
> 💡 **Insight:** Prague leads with 84 loans (12.9M CZK), then Karvina and Brno (24 each). Loans per account are 15.2% in Prague, 15.8% in Karvina and 14.8% in Ostrava, versus 18.8% in Brno and 24–26% in small cities (Prachatice 25.5%, Usti nad Orlici 24.6%, Louny 24.1%). Cities ranked by loan count are biased toward high penetration.
>
> 🚀 **Recommendation:** Prioritise Prague, Karvina and Ostrava for loan-book growth given their scale. Before copying the small high-penetration cities' approach elsewhere, check their B/D default rates specifically (not just penetration) — high penetration on a small base can mask concentrated risk that wouldn't show up in the portfolio-wide numbers.

---

## 🏦 5. Accounts

### **Q1. How did the account base build up?**
> 💡 **Insight:** New accounts per year were 1,139 / 439 / 661 / 1,363 / 898 (total 4,500). 1996 was the peak (+60.9% on the base) and 1997 added +24.9%. Openings were spread evenly across 1993 (75–115 a month). Almost all accounts stay active: 4,496 transacted in 1997 and 4,492 in 1998.
>
> 🚀 **Recommendation:** With account growth essentially flat since 1997 and retention already near-total, shift the primary growth lever from new accounts to products per existing account — cross-sell loans, cards and orders into the 68.8% "neither" segment identified in Overview Q2.

### **Q2. How are accounts distributed geographically?**
> 💡 **Insight:** Prague (554) is 3.6x Karvina (152), but that follows population: Prague has 0.46 accounts per 1,000 inhabitants versus 0.44 nationally. Across districts the range is 0.23 (Ceske Budejovice) to 1.12 (Jesenik). The top 10 cities hold 1,413 accounts (31.4%).
>
> 🚀 **Recommendation:** Rank all districts by accounts per 1,000 inhabitants and prioritise the lowest three for account-acquisition campaigns: Ceske Budejovice, Hodonin and Novy Jicin (0.23–0.26 per 1,000). Investigate whether low penetration there reflects genuinely weaker branch presence, since Jesenik's 1.12 shows the region as a whole isn't a low-potential market.

### **Q3. What do statement frequencies tell us?**
> 💡 **Insight:** 4,167 accounts are monthly (92.6%), 240 weekly (5.3%) and 93 after-transaction (2.1%). In 1998 the weekly and after-transaction accounts moved 0.82M and 0.88M CZK per account versus 0.39M for monthly (2.1x and 2.3x), with 79–81 transactions against 71.
>
> 🚀 **Recommendation:** Treat these 333 accounts as a defined high-value tier: offer a named premium service level (priority counter service, dedicated support line) and use statement frequency itself as a low-cost signal for identifying similar high-value accounts among the monthly majority.

---

## 💳 6. Card

### **Q1. What is the card mix?**
> 💡 **Insight:** Classic is 659 (73.9%), junior 145 (16.3%) and gold 88 (9.9%). All cards belong to account owners. "Junior" is a youth card: holders averaged 18.2 at issue and only 55 of 145 were under 18. Card accounts moved 0.64M CZK in 1998 versus 0.37M for accounts without a card (1.76x).
>
> 🚀 **Recommendation:** Build a defined junior-to-classic upgrade path timed around age 19–21, since junior holders are already close to that age at issue. Separately, run a gold-upgrade offer targeted at classic holders whose 1998 transaction value puts them in the top tier of classic accounts.

### **Q2. Where is card penetration weakest?**
> 💡 **Insight:** The average is 19.8% of accounts. Prague is 23.8% (132 cards, 14.8% of all cards). Brno is 12.5% (16 of 128), Karvina 15.8%, Olomouc 15.9% and Ostrava 16.3%. Some small districts are far higher (Litomerice 43.2%, Plzen-jih 41.0%), but on small account bases.
>
> 🚀 **Recommendation:** Target Brno first — reaching the 19.8% average would add roughly 9 cards there. Follow with Karvina (+6), Ostrava (+5) and Olomouc (+3). Don't use Litomerice or Plzen-jih as a benchmark; their high rates sit on small bases and aren't a reliable model to replicate at scale.

### **Q3. Is issuance growing, and are cards used?**
> 💡 **Insight:** Issuance went 1 / 21 / 63 / 116 / 242 / 449. 1998 is 50.3% of all cards ever issued, up 85.5% on 1997. It stepped up in July 1998, from 26–31 a month in H1 (171 in total) to 40–49 in H2 (278). Cards are used: 90.5% of card accounts made a card withdrawal, and 4,560 card withdrawals came in 1998 (5.1 per card account). But card withdrawals are 0.29% of transaction value.
>
> 🚀 **Recommendation:** Identify what specifically changed in July 1998 (a campaign, a fee waiver, a new branch process) and, if it was a deliberate action, repeat it in underperforming districts identified in Q2. Pair any renewed push with an activation incentive — ATM-fee waivers or a temporary higher withdrawal limit for new cardholders — and track card-withdrawal count per card by issue-year cohort to see whether newer cohorts use their cards more than older ones.

---

## 📝 7. Order

### **Q1. What are standing orders used for?**
> 💡 **Insight:** SIPO is 3,502 orders (54.1%) and 13.97M CZK (65.8% of value). Unspecified is 1,379 (21.3%), UVER 717 (11.1%), POJISTNE 532 (8.2%) and LEASING 341 (5.3%). UVER (loan payments) has the highest average (4,233 CZK) and POJISTNE the lowest (1,291).
>
> 🚀 **Recommendation:** Feed the presence and regularity of standing orders (especially UVER) into credit scoring as a cash-flow stability signal. Reach out to insurance and leasing partners about a bundled autopay offering, and prioritise classifying the 1,379 "unspecified" orders first, since at 21.3% of orders they're too large a blind spot to leave uncategorised.

### **Q2. How do orders differ by region?**
> 💡 **Insight:** North Moravia (1,144) and south Moravia (1,119) lead, and south Bohemia (523) is lowest. Orders per account run from 1.31 (east Bohemia) to 1.48 (north Bohemia), against an average of 1.44, so rankings follow account base.
>
> 🚀 **Recommendation:** No regional campaign is needed here — the spread is small and tracks account count. A light autopay-awareness push in east Bohemia is enough, consistent with that region's below-average standing across other metrics too (see Transaction Q5).

### **Q3. How widely are standing orders adopted, and what do they add?**
> 💡 **Insight:** 3,758 of 4,500 accounts (83.5%) have at least one order, averaging 1.72 each. The average order is 3,280.64 CZK (21,228,993.60 total). Accounts with orders made 74.7 transactions in 1998 versus 56.0 without (+33%), yet moved the same value (0.42M CZK each). Retention can't be tested, because 4,492 of 4,500 accounts were active in 1998.
>
> 🚀 **Recommendation:** Offer a first standing order at onboarding as a convenience feature — it correlates with more frequent (not larger-value) activity, which is worth having but shouldn't be sold internally as a proven retention driver. Retention specifically needs a dataset with account closures to test properly; flag this as an open question rather than an established result.

---

## 🌟 Overall Takeaways

1. **Base Expansion Growth:** Growth to 1997 was account-base expansion. Activity per account-year is flat at 67.5–71.6, and 1998 growth is mostly the full-year effect of 1997 openings.
2. **The Conversion Gap:** Conversion into credit is the main gap. 68.8% of accounts hold neither a loan nor a card, and lending fell 19.4% in 1998, all in H2 — the cause of the H2 drop (demand vs. policy) is still unconfirmed and should be the first follow-up.
3. **Risk Exposure:** Loan risk rises with size: 19.9% problem rate in the top quartile versus 7.6% in the bottom, and D loans average 249K versus 151K. Approval controls should scale with ticket size.
4. **Age Limits:** No loan went to anyone 62 or older, so verify whether that is deliberate policy before treating it as a rule.
5. **Dominance of Cash:** Counter cash is 60.7% of customer transactions and 76.4% of value. Cards are 1.1% and 0.29%, and card holders still withdraw the same amount of cash — card growth so far has added a channel, not replaced the costliest one.
6. **Data Cleaning Needs:** A third of the raw transaction count (32.2%) is interest and fee postings worth 0.48% of value, so report customer transactions only, and encode the exclusion in the data model, not as a manual filter.
7. **Proportionate Regional Spread:** Prague is proportionate on loans, accounts and population, and regional differences per account are small — resource allocation should track account base, with only east Bohemia and Brno singled out for modest, targeted pushes.
