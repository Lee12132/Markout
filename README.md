# Markout
Calculate price,net share position, trade volumn over self-defined time period

user could self-define the time period which they want to run the calculation on;
the exception will raise when the column names can not match;
be sure to change the coresponding file directory;

below are the explaination of each check-points:
Task#1: Familiarize with data, calculate statistical trend of HFT% in US Market
Task#2: Markout package, calculate markout for various types of trades, detect reasons for outliers
Task#3: RavenPack news package, converting time and merge database, calculate HFT/nHFT reaction towards market news events
Task#4: Calculate price trend under markout news (will explain); calculate profit under different liquidation time window; finding reasons for profits

Task#3
Main Objective: To observe the heterogeneous pattern of HFT/nHFT trades under market news 
Data Source: RavenPack, HFT
Ticker Range: 40 stocks only!
【Required #1】: For each news events, Under Time Span from -45,-40,…,0,5,…100s after news event timestamp, calculate:
	Markout (first store as price);
	Shares traded in each 5s bin (-50~-45, -45~-40, …, 95~100). Please store separately for HH,HN,NH,NN, and ‘B’ and ‘S’ (Important: so you have 8 combinations, such as HH_B, HH_S, …)
For this step, you are required to store information of each news event (including statistics) as intermediate outputs, since we will probably look at results under different clusters later!
【Required #2】: Based on the results you got above, calculate:
	Traded shares under HH,HN,NH,NN around markout event time. X: time bin; Y: Aggregated trading shares within each 5s bin;
Important question: How to plot to make best visualization effect?
	Classify news into 3 subgroups, ranked by absolute dollar change in 100s after event (i.e., |P_100-P_0 |). Subgroup1 with highest value, and Subgroup 3 with the lowest value. Within each subgroup, plot:
	Total volume for HFT/nHFT as aggressors. That means, plot two lines, one line is HH_B+HH_S+HN_B+HN_S, the other is NH_B+NH_S+NN_B+NN_S (so you have 6 lines, 3 subgroups with 2 lines in each subgroup)
X: Time after event. 0~5s, 5~10s, … , 95~100s
Y: Total volume 
	Net volume for HFT/nHFT as aggressors, aligning with the change direction of 100s stock price. That means, plot two lines, one line is HH_B-HH_S+HN_B-HN_S if 100s price change is positive (flip all signs if negative!), the other is NH_B-NH_S+NN_B-NN_S (flip all signs if negative!) (so you have 6 lines, 3 subgroups with 2 lines in each subgroup)
X: Time after event. 0~5s, 5~10s, … , 95~100s
Y: Total volume 
	Profitability test. Calculate the profit of HFT/nHFT as aggressors, both in terms of total dollar profit (per event), dollar event per share (per event) and percentage return per share (per event).
Example (fill by yourself here)

	Interpret all results from 2,3. What interpretation you can get?
	Redo 2,3,4 but now change classification methods in Step #2: Now classify into 3 subgroups by absolute value of percentage return (i.e., |ln(P_100)-ln⁡(P_0)|). Subgroup1 with highest value, and Subgroup 3 with the lowest value.
Will results change?

Task#4: 
Task #1 (easy) regression for detecting price trajectory after news event
【Required#1】Regression formula:
R_(k,10+k)=α+β_k R_0,10+γ_i X_i+ε,k=10,…,90
【Required#2】Placebo test:
R_(-10+k,k)=α+β_k R_(-10,0)+γ_i X_i+ε,k=10,…,90
R0,10=(P10-P0)/P0030
Task #2 HFT 7est (use HFT 120 stock data only!)
【Required#3】	
	Only use trades with trade type “HN” and “NH”, calculate each trade’s profit for HFT under different liquidation time window
e.g., HN, 100 shares, sell, $10/share, 10s later price goes to $12/share
10s liquidation window profit = (12-10)*100*(-1) (this is indicating trading direction!)
e.g., NH, 300 shares, sell, $25/share, 10s later price goes to $20/share
10s liquidation window profit = (20-25)*300*(1) (this is indicating trading direction!)
	Under this rule, calculates each trade’s profit for HFT under 0.1s, 0.2s, 0.5s, 1s, 2s, 5s, 10s, 20s, 50s, 100s, 200s, 500s, 1000s, 2000s, 5000s, 10000s.
	Take summation on all profits for “HN” and “NH” separately, for each day and aggregating all tickers; Then we have a series for “HN” daily profit and “NH” daily profit. Plot cumulative profits.
	You should have one line for each “liquidation window” + “trade type” combination, such as “HN”+”10s”. How to make the visualization results clear enough?
【Required#4】
	Now you assume you liquidate your position only when market ends. Calculate HN and NH’s daily profit separately, and plot the two cumulative profit lines. (liquidation window = 1day)



