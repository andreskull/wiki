# **Quantitative Analysis of Financial Media Influence: A Longitudinal Study of Market Reactions and Portfolio Performance of Recommendations by Jim Cramer**

The emergence of broadcast financial media as a primary source of information for retail investors has necessitated a rigorous quantitative evaluation of the "finfluencer" phenomenon. Among the most enduring figures in this landscape is Jim Cramer, whose "Mad Money" program has served as a laboratory for studying the intersection of media attention and market efficiency for over two decades.1 This report provides an exhaustive analysis of the performance metrics associated with Cramer’s stock picks, contrasting historical academic findings with modern algorithmic tracking methodologies and the performance of specialized inverse investment products.

## **The Evolution of the Finfluencer Tracking Framework**

The tracking of financial influencers has transitioned from manual, qualitative assessment to systematic, data-driven methodologies that utilize large language models (LLMs) to ensure consistency and transparency. The objective of such frameworks is to replace subjective opinion with verifiable evidence, creating a data trail that links media commentary to market outcomes \[User Query\].

### **Signal Extraction and Directional Attribution**

Central to the measurement of performance is the definition of a "Signal." A signal is characterized as a specific, verifiable stock pick or market call, distinct from vague commentary or generalized sentiment \[User Query\]. In the context of "Mad Money," these signals are extracted from broadcast audio and transcripts, mapping every directional position to a named instrument.

| Signal Badge | Meaning | Actionable Interpretation |
| :---- | :---- | :---- |
| Long | Bullish recommendation | Expectation of price appreciation over the measured horizon. |
| Hold Long | Conviction maintenance | Disclosure of existing ownership without immediate sell intent. |
| Close Long | Bullish exit | Selling a previously owned stock to take profits or cut losses. |
| Short | Bearish recommendation | Expectation of price depreciation or betting against the asset. |
| Hold Short | Bearish maintenance | Maintaining a short position in the face of current market dynamics. |
| Close Short | Bearish exit | Covering a short position, effectively ending the bearish bet. |

The extraction process prioritizes high-conviction recommendations, including explicit imperatives like "buy AAPL" and declarative picks such as "AAPL is my pick" \[User Query\]. General commentary, hedged statements, and conditional mentions are excluded to prevent false positives and ensure that the performance score reflects only definitive forward-looking commitments \[User Query\].

### **Performance Measurement Horizons**

To capture the temporal dynamics of the "Cramer Effect," performance is tracked across five distinct time horizons. This multi-window approach accounts for the documented short-term price pressure and subsequent long-term reversals associated with televised recommendations.1

| Horizon | Trading Days | Strategic Relevance |
| :---- | :---- | :---- |
| 1 Week | 5 days | Captures the immediate post-broadcast attention shock and volatility. |
| 1 Month | 21 days | Measures the potential reversal of initial price pressure. |
| 3 Months | 63 days | Evaluates medium-term fundamental alignment with the recommendation. |
| 6 Months | 126 days | Assesses the sustainability of the pick across a broader market cycle. |
| 1 Year | 252 days | Provides a long-term benchmark against index fund performance. |

The entry price is standardized to the market open price on the next trading day following the air date \[User Query\]. This methodology reflects the earliest realistic opportunity for a retail viewer to act on a recommendation made after the market close, thereby avoiding the inflation of returns caused by including the overnight price spike that occurs before trade execution is possible.3

## **Academic Foundations of the Cramer Effect**

Academic literature has extensively documented the market's reaction to recommendations on "Mad Money." These studies typically employ event-study methodologies to quantify the size of the abnormal returns surrounding the broadcast.4

### **Short-Term Attention Shocks and Price Pressure**

Research by Engelberg, Sasseville, and Williams (2012) identifies Cramer's show as a source of massive attention shocks to individual traders.1 Their analysis suggests that these shocks lead to large overnight returns that subsequently reverse over the following months.1 The average abnormal overnight return has been measured at over 3% for the entire sample, peaking at 6.7% for stocks in the smallest quintile of market capitalization.5

| Stock Quintile | Overnight Abnormal Return | Mechanism of Action |
| :---- | :---- | :---- |
| Smallest | 6.7% | High sensitivity to retail flow; limited institutional arbitrage. |
| Average | 3.0%+ | General attention-driven buying from naive investors. |
| Largest | Moderate | Higher liquidity and institutional presence dampen the media effect. |

The "Cramer Effect" is significantly influenced by viewership demographics. Using Nielsen ratings, researchers found that the overnight return is strongest when high-income viewership is high, suggesting that actionable capital is concentrated in specific viewer segments.1 Conversely, the number of low-income households viewing the show has a negligible impact on price response.5 This supports the "retail attention hypothesis," which posits that media recommendations induce buying behavior among investors with limited attention and significant capital but lacking sophisticated valuation models.1

### **The Reversal Pattern and Limits to Arbitrage**

A recurring theme in the literature is the "spike-reversal" pattern. While buy recommendations generate immediate positive returns, these gains often dissipate within twenty to twenty-five trading days.3 This reversal is indicative of price pressure from uninformed noise traders rather than the arrival of new, value-relevant information.7

The persistence of the reversal is strongest among small, illiquid stocks that are difficult for sophisticated investors to arbitrage.1 For these assets, the temporary imbalance between buying demand and available liquidity creates a "mispricing" that takes several months to fully correct.1 In contrast, the market reaction to sell recommendations is often weaker and slower to reverse, suggesting that Cramer may be identifying genuine negative information that persists over post-announcement windows.2

## **Longitudinal Analysis of the Charitable Trust**

While event studies focus on short-term shocks, the long-term efficacy of Cramer’s investment style is best observed through the Action Alerts PLUS (AAP) portfolio, which has functioned as a charitable trust since 2005\.10

### **Historical Return Benchmarks**

Research by Hartley and Olson (2016, 2018\) provides one of the most comprehensive datasets on the AAP portfolio, covering more than 17 years of performance.12 Their findings suggest a consistent pattern of underperformance relative to the S\&P 500 index on both an absolute and risk-adjusted basis.11

| Metric (2001 \- 2017\) | Cramer AAP Portfolio | S\&P 500 Total Return |
| :---- | :---- | :---- |
| Annualized Return | 4.08% | 7.07% |
| Cumulative Return | 64.53% | 126.06% |
| Sharpe Ratio | 0.16 | 0.41 |
| Standard Deviation | 17.65 | 14.16 |

The study concludes that the AAP portfolio generated lower annualized returns while subjecting investors to a "bumpier ride" characterized by higher volatility and lower risk-reward characteristics.12 This underperformance became particularly pronounced in the post-financial crisis years (2009–2016), as the portfolio failed to keep pace with the broader market rally.11

### **Factor Attribution and Structural Constraints**

To understand the drivers of this performance, researchers applied multifactor regression models, including the Capital Asset Pricing Model (CAPM) and the Fama-French Three-Factor Model.9

![][image1]  
Where:

* ![][image2]: Portfolio excess return over the risk-free rate.  
* ![][image3]: The idiosyncratic return (alpha) generated by the stock-picker.  
* ![][image4]: Small Minus Big (size factor).  
* ![][image5]: High Minus Low (value factor).

The analysis reveals that Cramer's returns are primarily driven by underlevered exposure to market returns (low beta) and a consistent tilt toward small-cap and growth-oriented stocks.9 Furthermore, the portfolio frequently exhibits a tilt toward stocks with "low quality of earnings," which can act as a drag on long-term performance.11

A critical structural factor in the AAP portfolio’s underperformance is the "cash drag." As a charitable trust, the portfolio often maintains significant cash positions to facilitate annual distributions to charities.11 During bull markets, this liquidity acts as a substantial hurdle to matching the performance of a fully invested index fund like the SPY.11

## **The Rise and Fall of Inverse Investment Products**

The cultural skepticism surrounding Cramer’s performance, often manifesting in the "Inverse Cramer" meme, led to the creation of institutionalized trading products designed to bet against his recommendations.14

### **The Launch of SJIM and LJIM**

In March 2023, Tuttle Capital Management launched the Inverse Cramer Tracker ETF (SJIM) and its bullish counterpart, the Long Cramer Tracker ETF (LJIM).16 These actively managed funds were designed to provide a "one-ticker" solution for investors to participate in or bet against the "Cramer Effect".17

* **SJIM Strategy**: This fund engaged in short sales of stocks recommended by Cramer on Twitter or CNBC, while going long on stocks he specifically recommended against.16 The portfolio typically held 20 to 50 securities and aimed to maintain inverse correlation with Cramer’s public utterances.18  
* **LJIM Strategy**: Replicated Cramer’s returns by investing at least 80% of net assets in his recommended securities.20 It also sought to capture momentum returns generated by the volatility inherent in his televised segments.20

| Product Feature | Inverse Cramer (SJIM) | Long Cramer (LJIM) |
| :---- | :---- | :---- |
| Expense Ratio | 1.20% (Net) | 1.20% (Net) |
| Inception Date | March 2, 2023 | March 2, 2023 |
| Asset Base (at Peak) | \~$3.4 Million | \~$1.3 Million |
| Primary Ticker | SJIM | LJIM |

### **Performance Divergence and Liquidation**

The actual performance of these funds during the 2023–2024 period highlights the risks of betting against mega-cap technology stocks during a growth-led bull market. Cramer's "buy" recommendations frequently included high-performing entities such as Nvidia, Meta, and Boeing.21

By February 2024, SJIM had lost approximately 15% on a total return basis since its inception, drastically underperforming the S\&P 500’s 25% gain over the same period.14 The LJIM fund, despite a modest 2.2% gain, failed to attract sufficient investor interest and was shuttered in September 2023\.23 SJIM followed into liquidation in February 2024\.24

Matthew Tuttle, the fund's manager, cited several reasons for the closure, including the administrative difficulty of tracking a media personality who makes a multitude of calls with a high "half-life" of conviction.14 The lack of institutional interest in a long-short portfolio and the retail preference for more volatile, single-directional products also contributed to the funds' demise.14

## **Methodological Discrepancies: Solving the Performance Paradox**

The user's preliminary finding that "Cramer stock picks are not too bad at all" stands in direct contradiction to the "Inverse Cramer" meme and some longitudinal academic findings. This discrepancy can be resolved by analyzing the differences in signal curation, entry pricing, and market cap weighting.

### **Automated vs. Subjective Signal Curation**

Traditional studies and the Tuttle ETFs relied on human monitoring, which is inherently subjective and prone to interpretation bias.14 Modern methodologies using LLMs provide a consistent "single source of truth" by extracting signals based on a fixed set of instructions that exclude ambiguous or unresolvable references \[User Query\].

| Curation Method | Subjectivity Level | Handling of Ambiguity | Consistency |
| :---- | :---- | :---- | :---- |
| Manual (Tuttle) | High | Discretionary decisions on "closing" bets. | Low; personnel-dependent. |
| Academic (Event Study) | Moderate | Focus on "Featured Stocks" to reduce noise. | Moderate; defined by paper scope. |
| Algorithmic (LLM) | Low | Explicitly excludes non-directional commitment. | High; instruction-driven. |

By filtering out "Lightning Round" rapid-fire comments—which account for 40% of Cramer's calls but often lack fundamental depth—the algorithmic approach may be inadvertently selecting for "higher quality" signals that have better hit rates than a naive aggregation of every stock ticker mentioned on air.25

### **The Impact of Entry Price Standardization**

Performance results are highly sensitive to the exact moment of trade entry. If a study uses the "Closing Price" of the broadcast day as the entry point, the result will include the 3% overnight "attention spike," making the performance appear superior to what an actual investor could achieve.3

The methodology of entering at the "Market Open" price of the next trading day eliminates this phantom alpha \[User Query\]. This approach align with the reality that retail investors cannot act until the following morning.3 Consequently, results that use "Next-Day Open" prices often show that Cramer’s picks merely perform in line with the market or experience a short-term "mean reversion" as the initial excitement cools.9

### **Market Cap and Sector Weighting**

Cramer’s portfolio often leans toward small-cap and growth stocks.9 In an equal-weighted backtest—where every recommendation is treated as a 1/N position—the performance of several successful small-cap picks can mask the poor performance of high-volume large-cap recommendations.26

Furthermore, during the 2015–2024 period, the dominance of "Mega-Cap Tech" has disproportionately benefited investors who remained bullish on the Nasdaq and S\&P 500 benchmarks. Because Cramer frequently recommends these "Magnificent Seven" stocks, his hit rate may appear high simply because he is "closet indexing" the most successful sector of the decade.22

## **The Role of AI and LLM Bias in Financial Signal Extraction**

The transition to LLM-driven research introduces a new set of variables: systematic biases within the models themselves. These biases can inflate reported performance and contaminate backtests, making them useless for institutional deployment if not explicitly addressed.29

### **Identification of Financial LLM Biases**

A review of 164 papers from 2023 to 2025 identifies five recurring biases in financial LLM applications that can distort the analysis of finfluencer performance.29

1. **Look-ahead Bias**: The model inadvertently uses information from the "future" (relative to the signal date) to extract sentiment or verify tickers. This is a common failure in systems that use non-point-in-time training data.29  
2. **Survivorship Bias**: LLMs may exhibit higher confidence in firms that still exist today, while failing to accurately extract signals for companies that have since gone bankrupt or were delisted.29  
3. **Narrative Bias**: Models may be swayed by the "persuasive communication" style of the influencer, prioritizing the flow of the argument over the underlying financial data.4  
4. **Objective Bias**: LLMs are optimized for likelihood maximization and human preference (helpfulness), not for risk-aware financial accuracy. This encourages "verbal overconfidence," where the model guesses a stock's direction with high certainty even when the signal is ambiguous.29  
5. **Representation Bias**: Models like the open-source Qwen family have been shown to favor large, well-known firms while undervaluing smaller ones. This "anchoring effect" can lead to higher confidence scores for stocks in the Technology sector, which aligns with Cramer’s own sectoral preferences.32

### **Mitigating Bias in Signal Extraction**

To ensure structural validity, researchers propose the Financial Bias Indicators (FBI) framework and the Bias Detection and Fairness Evaluation (BDFE) Framework.34 These systems utilize "System 2" slow thinking analysis and attention mechanism visualization to detect and mitigate belief bias.34

For the finfluencers.trade methodology, the primary defense against these biases is the "provenance chain," which links every signal to the original audio snippet and transcript \[User Query\]. By forcing the model to cite specific quotes, the system reduces the risk of "hallucinated" or "incentive-induced" signals.29

## **Quantitative Findings from Retail-Led Backtests**

Detailed analysis of large-scale datasets provides a more granular view of Cramer’s hit rate and alpha. A study of 23,000 recommendations over several years reveals the difficulty of generating consistent outperformance using a "naive" inverse strategy.26

| Metric | Naive Inverse Cramer (Short-Only) | Cramer-Neutral Benchmark (S\&P 500\) |
| :---- | :---- | :---- |
| Annual Return (since 2016\) | 1.27% | \~15.00% |
| Daily Volatility | 1.32% | Standard |
| Sharpe Ratio | 0.18 | \~0.80 \- 1.00 |
| Max Drawdown | 53.73% | Standard |
| Correlation with S\&P | 66.69% | 100% |

The extremely low Sharpe ratio of 0.18 for the naive inverse strategy suggests that simply doing the opposite of Cramer’s "sell" calls does not yield a superior risk-adjusted return.26 Interestingly, the study found that stocks Cramer liked "a bit better than absolute hatred" actually demonstrated 3x higher Sharpe ratios, suggesting that his "tepid" buy recommendations might possess more predictive power than his high-conviction "screaming" buys.26

### **Rebalance and Timing Strategies**

The "half-life" of a media recommendation is short. Backtests suggest that monthly rebalancing is often too slow to capture the "alpha" of an inverse strategy.25 The most effective "Inverse Cramer" results are found in high-frequency, daily-reset strategies that target the specific 5-day trading window where the price reversal is most aggressive.18 However, such high-turnover strategies are often cannibalized by transaction costs and short-borrowing fees, which can reach 1.20% or more, as seen in the SJIM ETF’s expense ratio.16

## **Comparative Synthesis: Academic vs. Algorithmic Evidence**

The discrepancy between the user’s "not too bad" results and the academic "underperformance" can be synthesized into a coherent picture of market dynamics.

### **Why Academic Research Shows Underperformance**

* **Time Period**: Studies like Hartley and Olson focus on the "Action Alerts PLUS" portfolio from 2001–2016, which included the dot-com crash and the 2008 financial crisis.11 During these periods, Cramer’s growth tilt was a major liability.  
* **Factor Adjustment**: Academics do not just look at raw returns; they subtract the "Small Cap" and "Growth" premiums.9 When these are removed, Cramer’s "Alpha" often disappears.  
* **Cash Drag**: The charitable trust's structural need to hold cash for donations makes it a poor proxy for a pure "trading" strategy.11

### **Why Algorithmic Tracking Shows Better Results**

* **Modern Market Regime**: Since 2015, the "Growth" and "Tech" factors have outperformed. Cramer’s natural bias toward these sectors has acted as a tailwind.21  
* **Signal Filtering**: Using LLMs to filter for "Entry" signals (new buys) and ignoring "Holdings" disclosures can isolate the most "active" part of the advice, which may have higher short-term momentum \[User Query\].  
* **Alpha vs. Benchmark**: The user’s methodology calculates alpha directly as "Signal Return minus Benchmark Return" \[User Query\]. Without adjusting for the Fama-French risk factors (Beta, Size, Value), this metric will inherently show higher "alpha" for any portfolio that takes on more risk than the S\&P 500 during a bull market.

## **Conclusion**

The analysis of Jim Cramer’s stock picks reveals a sophisticated interplay between media-driven attention shocks and market efficiency. While short-term event studies confirm the existence of a "Cramer Effect"—characterized by a 3% overnight price spike followed by a monthly reversal—the long-term performance of his curated portfolios demonstrates the difficulty of consistently beating a passive index on a risk-adjusted basis. The failure of inverse ETFs like SJIM highlights the peril of betting against the market's leading sectors, even when those sectors are championed by a polarizing media figure.

For the modern researcher, the integration of LLM-based signal extraction provides a path toward a more consistent and auditable performance trail. However, this transition requires a meticulous awareness of the inherent biases within AI models, particularly look-ahead and representation biases. The discrepancy between various tracking methodologies is largely a function of entry-price selection, time-horizon definition, and factor-adjustment rigor. Ultimately, while Cramer’s recommendations may not provide "extraordinarily good" idiosyncratic alpha, they are "not extraordinarily bad" in the context of a diversified, growth-oriented portfolio, provided that investors can navigate the volatility induced by the televised broadcast. Future research should prioritize sector-aware calibration and the development of "System 2" extraction frameworks to ensure that the measurement of finfluencer performance remains grounded in empirical reality rather than media-driven narrative.

#### **Works cited**

1. Market Madness? The Case of Mad Money \- IDEAS/RePEc, accessed on April 6, 2026, [https://ideas.repec.org/a/inm/ormnsc/v58y2012i2p351-364.html](https://ideas.repec.org/a/inm/ormnsc/v58y2012i2p351-364.html)  
2. (PDF) Investing in Mad Money \- ResearchGate, accessed on April 6, 2026, [https://www.researchgate.net/publication/401678377\_Investing\_in\_Mad\_Money](https://www.researchgate.net/publication/401678377_Investing_in_Mad_Money)  
3. Booyah\! An Analysis of Mad Money Stock Recommendations Matthew Dakken Minnesota State University Moorhead \- Federal Reserve Bank of Minneapolis, accessed on April 6, 2026, [https://www.minneapolisfed.org/-/media/files/mea/contest/2017papers/dakkenmatthew.pdf](https://www.minneapolisfed.org/-/media/files/mea/contest/2017papers/dakkenmatthew.pdf)  
4. Impact Of Mad Money Stock Recommendations: Merging Financial and Marketing Perspectives, accessed on April 6, 2026, [https://digitalcommons.chapman.edu/cgi/viewcontent.cgi?article=1006\&context=business\_articles](https://digitalcommons.chapman.edu/cgi/viewcontent.cgi?article=1006&context=business_articles)  
5. Market Madness? The Case of Mad Money, accessed on April 6, 2026, [https://cdr.lib.unc.edu/downloads/bv73c824b](https://cdr.lib.unc.edu/downloads/bv73c824b)  
6. Does the Mad Money Show cause investors to go madly attentive? \- ACFR \- AUT, accessed on April 6, 2026, [https://acfr.aut.ac.nz/\_\_data/assets/pdf\_file/0004/577246/Ali-R-Does-the-Mad-Money-Show-cause-investors-to-go-madly-attentive.pdf](https://acfr.aut.ac.nz/__data/assets/pdf_file/0004/577246/Ali-R-Does-the-Mad-Money-Show-cause-investors-to-go-madly-attentive.pdf)  
7. Mad Money Stock Recommendations: Market Reaction and Performance \- ResearchGate, accessed on April 6, 2026, [https://www.researchgate.net/publication/227591227\_Mad\_Money\_Stock\_Recommendations\_Market\_Reaction\_and\_Performance](https://www.researchgate.net/publication/227591227_Mad_Money_Stock_Recommendations_Market_Reaction_and_Performance)  
8. Semi-Strong Form Market Hypothesis: Evidence from CNBC's Jim Cramer's Mad Money Stock Recommendations \- ScholarWorks@UARK, accessed on April 6, 2026, [https://scholarworks.uark.edu/cgi/viewcontent.cgi?article=1153\&context=inquiry](https://scholarworks.uark.edu/cgi/viewcontent.cgi?article=1153&context=inquiry)  
9. (PDF) How Mad is Mad Money: Jim Cramer as a Stock Picker and Portfolio Manager, accessed on April 6, 2026, [https://www.researchgate.net/publication/260321449\_How\_Mad\_is\_Mad\_Money\_Jim\_Cramer\_as\_a\_Stock\_Picker\_and\_Portfolio\_Manager](https://www.researchgate.net/publication/260321449_How_Mad_is_Mad_Money_Jim_Cramer_as_a_Stock_Picker_and_Portfolio_Manager)  
10. Jim Cramer's Mad Moneyy Charitable Trust Performance and Factor Attribution, accessed on April 6, 2026, [https://www.researchgate.net/publication/314562188\_Jim\_Cramer's\_Mad\_Moneyy\_Charitable\_Trust\_Performance\_and\_Factor\_Attribution](https://www.researchgate.net/publication/314562188_Jim_Cramer's_Mad_Moneyy_Charitable_Trust_Performance_and_Factor_Attribution)  
11. Jim Cramer's 'Mad Money' Charitable Trust ... \- Squarespace, accessed on April 6, 2026, [https://static1.squarespace.com/static/568f03c8841abaff89043b9d/t/5734f6e2c2ea51b32cf53885/1463088868550/HartleyOlson2016+Jim+Cramer+Charitable+Trust+Performance+and+Factor+Attribution.pdf](https://static1.squarespace.com/static/568f03c8841abaff89043b9d/t/5734f6e2c2ea51b32cf53885/1463088868550/HartleyOlson2016+Jim+Cramer+Charitable+Trust+Performance+and+Factor+Attribution.pdf)  
12. Jim Cramer vs. S\&P 500: Chasing 'Mad Money' | Index Fund Advisors, Inc., accessed on April 6, 2026, [https://www.ifa.com/articles/cramer\_chasing\_mad\_money](https://www.ifa.com/articles/cramer_chasing_mad_money)  
13. Model Investing vs. Jim Cramer, accessed on April 6, 2026, [https://modelinvesting.com/articles/model-investing-vs-jim-cramer/](https://modelinvesting.com/articles/model-investing-vs-jim-cramer/)  
14. Inverse Jim Cramer ETF closes, accessed on April 6, 2026, [https://www.etfstream.com/articles/inverse-jim-cramer-etf-closes](https://www.etfstream.com/articles/inverse-jim-cramer-etf-closes)  
15. The Jim Cramer Inverse ETF and How to Pick Against Him | White Coat Investor, accessed on April 6, 2026, [https://www.whitecoatinvestor.com/jim-cramer-inverse-etf/](https://www.whitecoatinvestor.com/jim-cramer-inverse-etf/)  
16. INVERSE CRAMER ETF ETFs \- Markets Insider, accessed on April 6, 2026, [https://markets.businessinsider.com/etfs/inverse-cramer-etf-us66538h1462](https://markets.businessinsider.com/etfs/inverse-cramer-etf-us66538h1462)  
17. 2 ETFs: Inverse and Long Exposure to Cramer Stock Picks \- ETF Database, accessed on April 6, 2026, [https://etfdb.com/news/2023/03/02/2-etfs-inverse-and-long-exposure-to-cramer-stock-picks/](https://etfdb.com/news/2023/03/02/2-etfs-inverse-and-long-exposure-to-cramer-stock-picks/)  
18. Inverse Cramer Tracker ETF \- SEC.gov, accessed on April 6, 2026, [https://www.sec.gov/Archives/edgar/data/1644419/000158064223001117/inverse-cramer\_497k.htm](https://www.sec.gov/Archives/edgar/data/1644419/000158064223001117/inverse-cramer_497k.htm)  
19. filed with the Securities and Exchange Commission \- SEC.gov, accessed on April 6, 2026, [https://www.sec.gov/Archives/edgar/data/1644419/000158064222005066/tuttleetfs485a.htm](https://www.sec.gov/Archives/edgar/data/1644419/000158064222005066/tuttleetfs485a.htm)  
20. www.sec.gov, accessed on April 6, 2026, [https://www.sec.gov/Archives/edgar/data/1644419/000158064223001118/long-cramer\_497k.htm](https://www.sec.gov/Archives/edgar/data/1644419/000158064223001118/long-cramer_497k.htm)  
21. Jim Cramer Calls Boeing His Top Stock Pick for 2026 \- Gotrade, accessed on April 6, 2026, [https://www.heygotrade.com/en/news/jim-cramer-calls-boeing-his-top-stock-pick-for-2026](https://www.heygotrade.com/en/news/jim-cramer-calls-boeing-his-top-stock-pick-for-2026)  
22. Jim Cramer ETFs Are History With Closure of Struggling Short Fund \- Wealth Management, accessed on April 6, 2026, [https://www.wealthmanagement.com/etfs/jim-cramer-etfs-are-history-with-closure-of-struggling-short-fund](https://www.wealthmanagement.com/etfs/jim-cramer-etfs-are-history-with-closure-of-struggling-short-fund)  
23. 'Mad Money' ETF to close after attracting just $1.3M \- InvestmentNews, accessed on April 6, 2026, [https://www.investmentnews.com/etfs/mad-money-etf-to-close-after-attracting-just-13m/241291](https://www.investmentnews.com/etfs/mad-money-etf-to-close-after-attracting-just-13m/241291)  
24. The Inverse Cramer Tracker ETF to be Closed and Liquidated \- Nasdaq, accessed on April 6, 2026, [https://www.nasdaq.com/press-release/the-inverse-cramer-tracker-etf-to-be-closed-and-liquidated-2024-01-25](https://www.nasdaq.com/press-release/the-inverse-cramer-tracker-etf-to-be-closed-and-liquidated-2024-01-25)  
25. The Inverse Jim Cramer ETF Has Officially Arrived : r/wallstreetbets \- Reddit, accessed on April 6, 2026, [https://www.reddit.com/r/wallstreetbets/comments/11gb1ti/the\_inverse\_jim\_cramer\_etf\_has\_officially\_arrived/](https://www.reddit.com/r/wallstreetbets/comments/11gb1ti/the_inverse_jim_cramer_etf_has_officially_arrived/)  
26. I analyzed 23,000 recommendations made by Jim Cramer to make ..., accessed on April 6, 2026, [https://www.reddit.com/r/stocks/comments/uetsqa/i\_analyzed\_23000\_recommendations\_made\_by\_jim/](https://www.reddit.com/r/stocks/comments/uetsqa/i_analyzed_23000_recommendations_made_by_jim/)  
27. Journal of Accounting and Finance \- North American Business Press, accessed on April 6, 2026, [http://www.na-businesspress.com/Subscriptions/JAF/JAF\_13\_6\_\_Master.pdf](http://www.na-businesspress.com/Subscriptions/JAF/JAF_13_6__Master.pdf)  
28. The Inverse Jim Cramer ETF Has Officially Arrived : r/stocks \- Reddit, accessed on April 6, 2026, [https://www.reddit.com/r/stocks/comments/11gdduf/the\_inverse\_jim\_cramer\_etf\_has\_officially\_arrived/](https://www.reddit.com/r/stocks/comments/11gdduf/the_inverse_jim_cramer_etf_has_officially_arrived/)  
29. Evaluating LLMs in Finance Requires Explicit Bias Consideration \- arXiv, accessed on April 6, 2026, [https://arxiv.org/html/2602.14233v1](https://arxiv.org/html/2602.14233v1)  
30. \[2602.14233\] Evaluating LLMs in Finance Requires Explicit Bias Consideration \- arXiv, accessed on April 6, 2026, [https://arxiv.org/abs/2602.14233](https://arxiv.org/abs/2602.14233)  
31. Unmasking Bias in Financial AI: A Robust Framework for Evaluating and Mitigating Hidden Biases in LLMs | Request PDF \- ResearchGate, accessed on April 6, 2026, [https://www.researchgate.net/publication/397614364\_Unmasking\_Bias\_in\_Financial\_AI\_A\_Robust\_Framework\_for\_Evaluating\_and\_Mitigating\_Hidden\_Biases\_in\_LLMs](https://www.researchgate.net/publication/397614364_Unmasking_Bias_in_Financial_AI_A_Robust_Framework_for_Evaluating_and_Mitigating_Hidden_Biases_in_LLMs)  
32. Uncovering Representation Bias for Investment Decisions in Open-Source Large Language Models \- arXiv.org, accessed on April 6, 2026, [https://arxiv.org/html/2510.05702v1](https://arxiv.org/html/2510.05702v1)  
33. Tracing Positional Bias in Financial Decision-Making: Mechanistic Insights from Qwen2.5 | Request PDF \- ResearchGate, accessed on April 6, 2026, [https://www.researchgate.net/publication/397614063\_Tracing\_Positional\_Bias\_in\_Financial\_Decision-Making\_Mechanistic\_Insights\_from\_Qwen25](https://www.researchgate.net/publication/397614063_Tracing_Positional_Bias_in_Financial_Decision-Making_Mechanistic_Insights_from_Qwen25)  
34. Are LLMs Rational Investors? A Study on the Financial Bias in LLMs \- ACL Anthology, accessed on April 6, 2026, [https://aclanthology.org/2025.findings-acl.1239.pdf](https://aclanthology.org/2025.findings-acl.1239.pdf)  
35. Bias Detection and Fairness in Large Language Models for Financial Services, accessed on April 6, 2026, [https://www.researchgate.net/publication/389954152\_Bias\_Detection\_and\_Fairness\_in\_Large\_Language\_Models\_for\_Financial\_Services](https://www.researchgate.net/publication/389954152_Bias_Detection_and_Fairness_in_Large_Language_Models_for_Financial_Services)

[image1]: <data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAAmwAAAArCAYAAADFV9TYAAALxUlEQVR4Xu3dd7AlRRXH8aNizmIWYc0omOOK4hrQUhTFjGlLKErLgLlMpaCWopaZMqOlYvmHCmXAnBNmLUpKwcQaMOectb/2NO+887r7zuy+d+/l7e9T1bVvztwwd2buzJnTPXfNRERERERERERERERERERERERERERERERERERERERERERERERERERERERERERERERERERERERERERERERERERERERERESW0flTu3AMbhLniQH5v828zaMrx8CSOl8MLNjlYmBJHRQDS+g+MSAiMtXhqT0xBpNfpfbbof0ytZ+k9rnULugfNAeXSu3XlpfjN5aX5azU3p3avu5xPT9N7dwxuEnsYXnbPCe1v4Z5LUek9vgYtPo2/6zNf5tHV7W87cuy/SK1H6X2ltQu4B7XM3bdrIfvWt4ep6d2cJhXc16rbw88KrXfpXaC5e1zndSe4uaXbcb6qSXg57KVdce/Wy3vM3598jev87PU3pifdrZ32fwSt1undlpqJ6X2gTCvhf30QiF2X8ufhX3kx5b3Y3Ac2JHa91L7YWrHDfGiHGd4TktZb7yvx/QOy9v+B5b30ejNMSAiMsV/Y8DhoLbdTXNi4fEkUfPG++7lpjk5EePkMwuVi3/E4CZB8lo8PLVPuemW3jZn3kPc9CK3ecRyPM1NUznrfRbveqm9JwY3wIfd3yRLY5aPRKHmvam9w02XbcG/3j+H+JVCHJ+3PO9NIY6/pPb1EON78q8Q+0+Y3gjXTO3ubppljolY9LDUXh2DDq/BNvD+PcRbTrb2/FOs/poFiWbruSCJ88cwEZHRjk/tkBh0agcfYqfG4By0luWhMdjwN1uOpKOHqswUXLH7k8eJqX3FTddQQblrDDqt9RxP7PPGia61bAfGYENMRGahmjUF1b4j3fQlrb7MEVWbmtpzazEqecQfEGckV7E8r5YoEL9fiD16iHskMWMujIrW5+kpVbCCZbh4iEW9RPIAq1dVed0XxuCAC7t9bO3nB/FrWX1e8Xtb+zk8Euop6+b6MSAiuy8OPq3urj2tfnAidlQMbjC6SlrLcsUYbHh2au+MwSUzNWH7U5imOjKr67e3zS9t7fVM19wivdjWLhsJOLGxycTHbHUVZ5apCVus4L3e2slB8cnUXhGDg/h5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>

[image2]: <data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAAFcAAAAfCAYAAACSw1FtAAAC50lEQVR4Xu2YS6hNURjH/54Dj4Eoj9RNCmMyMjDwGBgZUQy8oogRI4yYmCN1w0WKpJSQUi4D5JVSlAzuRfIubyKP72+t7+x9vnP2Ovecu8+9Zx37V//2Xd//22ud7zv77rP2BgoKCgoKCtqYm6Lborted0TXRVdFF0XHRB2l7Di5JLoFV5vWybqvibpFF0R7Stk5clB0RfTH64OoS3Qc7kN99/HXPj9G9ouOil4iqfOE6JDoJFzjNc5xrsxFMnk1xiLsx8J5hOtQr8ca/eEUwosS9TdbIyK0hmfW8OxG7T7UjU74xBopNIe3iljRGtZYw3MWSc484zWMTthhDc8oJDlDjBcLbGitq1L9UE5drEbtCR/D+TusERG9qF2n+hOt0Si9CC/6EM5bbI3I0BoPW0MYI/oN5482Xr/QRX/ANZr33Z+pOMe5fZODiNbzFm438A5JQ6lzSWp+6OSzRONEE0SzRV98nJvt2FmLpE7WSE0VLU3FN5Wyc0Qnr4Z6W62REzMb0JR/Z9YHt15ZdQ5FtpcF/7Pfw32eTGbATdpjDc8v1L9wK6I1rLKGp54a7/sj84PN5aMek7L2fbroJ2tEBG93oeYNR9i39DUvOOlIJD5f5MTKaYTr3I6wb+lrXnDSdUh8vvRQtvnjFu/xS9gF98KH48nebxW0hqfW8GTdjzeYseaobpTb5SyES3puDc98JBMd8bHpfky4i3jgx4yTOSm/VdAa1lvD043K5nbBPQpb9iFQH+8/vMK4x3sB19hXoo+oftJOJAt/hjuP6C824xv932Sajw02nXC/5m/gamStfGXK7eWjVJ5yD+5z8xweD5TbJehdtsE8GGYDqGzk3iqxmOAePwRrW2mDzcI2kuOvJtZOsD7ui5vOCLjHR2US3OJ8Tm9HlqDyYmoa3MLwHrZAtAhuYf1ha0fOYACbO2ALtQisd4UNNov/pbmsc7w/Np1lom9wi/Fdb7vDrStfRy63RkFB/PwF89oCZ5g7q7sAAAAASUVORK5CYII=>

[image3]: <data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAABAAAAAeCAYAAAAl+Z4RAAAA1klEQVR4Xu2MvQ5BQRCFBxE/DY0OzyASD6CXKCmUIl5Ap9B6AYVapxY9r+ABiE5CIqFScObObO7euRG93C/5srvnzA5RQsLf0oBv+IRnvd9hyh/6xobkQ9rLMpqxjh3se++AOcnQ2hZgT9EF/j2grmGsUAYkXQG24Clah59nJnfUSPqenhF4q1uQM52jROFM23TU1CK22SNL0l9swRTp9wKG+4kNHQeSgaotwBHeSPqtZlNYdgOOK8nQEi7gQ98d7fkTv1fwpVmMChzBLsybjhnCsQ0TEj4IszRMmFyXogAAAABJRU5ErkJggg==>

[image4]: <data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAADoAAAAfCAYAAAC22t6tAAAC6ElEQVR4Xu2XS+gIURjFD/IsC0TJYylRWLBQFiTJQhG2+rNRskA2orwWFEnZeYYNhWIjGwl5Ux5bS4+FyPv9+I57P/PNNzN3LjuZX51m5p5z79w7r3sH6Ojo6PgH6BH9cFoXvdOiHXFfWSi6LroTdUu0ppRoZgSKetQN0Unj7xHdFN02Ge7zHCy/JrqE0K8/4g3CwOaZskmx7F7cDjUeWSo6LPqO4sK8KiWasRfzouiQaL7xt8eyBzFDHRcdER0QHROdNd6JUC2NDrKPN4TxKBprgt6uuE3llG2ilcjLr0V7Tv3V3vAwtMwXGujzrjVBf1jcpjpEeok+irYgL/8UIfPOGwZtJ9nWRLQEEPy9vtCg9VtPJrwU9UMYLLNXy3YFbXO9NwxZA72AEFjgDUOyAWFD3OrJxhrPMlA0JO5rdlBhVxiDjAGgyAz3hmU3iuBO5+WwBOEOEW2n57db5kvczkLeAPajPaePdupG/YIfIG3Mip/wHB6a/dcIdY+aMmUFijvPaYG5T4Vdi/blnDeEuSj8Ac5rxE4PXm3YzJl4/NiUKfZjpm1vNWV1+L548Y73/53OZLLoCqqNfbOhGuxAuVjQepZHKM/BmuE728Ro1LdleYbgs+9/DR/dthMtEn0wx1NQrcNFx3lzPBvVTB0HkZdrzUxDWGWkaGuE3syaMq3Du3LfeITvJX0+PSm0nU3ecGiucel5Gem5keQM1GPrpPw53nBoLvV4E81N9YZCk1NLCmaarvwopAeyT7TYecReiBQ5uVXIyNH86gsN45BugItq/kF49Av+1huR1o4hLBOZ+ewNAz9u2lbTAqU0Ydc929oIc3VMQPDrphG+EvTqfhD0KeDvVoqNCDmuh+vgtKT9v+u8Eu9Fy+P+ZhSVVPyb6Rt9Cy/Ac4TVyBPRC1Qn/RmiU66Mv26sx+mA4nqXa11716fHY95FTmm+T3xS6LEdvhaDQ7U0/r+S9BaN9IUdHR0dHR0d/x8/ARTyImWXO5oPAAAAAElFTkSuQmCC>

[image5]: <data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAAEEAAAAfCAYAAAC4c6DCAAADC0lEQVR4Xu2Y2ctOURTGlyHixlDixr0iSsaIUoYo3JDiL5ALJCXlQhE35EqSxIVZkiGEPmUoQ65Q7iTzlJmM6/nWXvY6y3v22ed+/+rpe89+nr32fs+wz34/okKhUChkcZ11nnWWdYp1hnWJddxkjoR2tB0MxydDrhNzWDdYt1l3WLdYmyuJenqR9FHdZF2uJCKYSxfrBOsAxXmdI/le2ewh6fAn6AdrH2u5yWxiHTMZCCdlh8lYFrP2sq5QtU8OHynmf7L2s1ZXEpFtJOM8pNjnPusoa7fJZYErp0X6Oc+imXXeqGEYtTsJ81k7KeYHVO1atpDkr3qjDbitmyY6k2IGt2wO21l3qbm2gsy88Dcnr3wlyU/2Rht00E/eMFyj9pPD7byA8vr1Zg0nWT+Q/Va1k+TUT4KrqkUmOc+iGbtgNqET075TjOfBWgQ0O814KcaR5N95ow1rKO9MamaUN2rA4/MrfNa+G6Jd4QWrP2sg5c3Fcpokv8obbXhPeQPnZCxYpLB6A129u/65kamsw+Ez8m3H0XxPb7RBi+QqF2QHhc+7wrHeGRZbEz6OL5q2JtrOqyNaZIw3DFtJMnj/5mIntiQc+8liE2QfL82k1g7LBOpctzU5RbBoITPSGzXMoupVH0r/jzOE5DGx+EwT2Bnm9JnhGyw44yiAdSFFzkAWnLSFrs3XeGU+K/A/+8YEWhOLYx1jKf3q774dUWStNwxzSTIvvZGg0wnTCY8mea15FpH4671RQ1+KNcc7z4KN1GzfaNEiqR0gfgQhs8IbNQym9ElYGf56HpC09/FGDRsp1qwD603K76apCNBMD2/UgDfBPd/IvKFYa6LzQM5cLLpOpfrA6zSXfywlCX3xhgFXpWkgD7KvfSNziMR77A2K43z3RgKdFxZHDx633yQ+tuIVppMsgpjkM9YTkt2abpgUHOPKPQ8ZZN+SrPh4Fj3Y6X0gyT8lyeIL4f8BygjWBXMMdBzMAeOgH55h3UJ7lpEscvAxF/2iVmjDbxb89ngk3QqFQqFQKBQKWfwFYw8itRiguJ4AAAAASUVORK5CYII=>