---
layout: post
title: Building a model to predict IPL match outcomes
subtitle: This post will walk you through how you can use Analytics & ML to predict winners of IPL matches.
tags: [python,Streamlit,IPL]
---

# Building a simple model to predict IPL Match Outcomes

#### App Link with final predictions - https://share.streamlit.io/arpitsolanki/ipl-prediction-engine/main/app.py

#### Code -https://github.com/arpitsolanki/IPL-Prediction-Engine

## Background
Over the last few months I've been trying to combine my two passions - sports & data analytics. It all started with using [analytics to get better at Fantasy League for Football](https://arpitsolanki.github.io/blog/2021/02/06/Using-ML-to-win-FPL.html), I also recently participated in annual Kaggle competition for predicting outcomes of NCAA matches. While I enjoyed both those projects, I found that my lack of knowledge for Football & Basketball limited my abilities to come up with good features & models to some extent. Therefore I decided to pick up Cricket, a sport which I understand really well and have been following for a long time. 

Cricket traditionally isn't a sport which has been known for widespread usage of data to inform tactics & team selection, at least not to the extent of some other sports like Baseball & Football. However with the growing popularity of T20 over the last 10 years, things have been changing very rapidly in Cricket as well, with teams employing performance analysts & using data to get any possible edge over the opposition. This has spilled over to cricket broadcasts & journalism as well, with commentators trying to bring in more visualizations & journalists using data to create stories around their articles. A great example of this is Jarrod Kimber, who uses a lot of analytics in his video blogs to deep dive into matches & explain why teams performed in a certain manner. I'd been watching all of this very closely over the years & decided to get my hands dirty & combine my love for the sport of cricket and my skills as a data analyst to create a project around predicting match outcomes for IPL matches using data analytics. 

### Why IPL is a difficult tournament to predict accurately?
I did check out a few solutions available on the internet predicting match outcomes, but they all had low accuracy scores, sometimes worse than a coin toss. I think this could be due to the following reasons - 
 1. Upsets are more likely in T20 than any other cricket format. In cricket or any other sport, the longer the match goes, the stronger team has much higher chance of overcoming poor passages of play and win the game, but since T20 cricket is just 120 balls per innings, few moments of individual brilliances can change outcomes of games. 
 2. IPL teams have a fixed purse amount to spend on players, which ensures a level playing field. The difference in quality in best & worst teams isn't as high as in some other sports like EPL, NBA etc. 
 3. The format for tournament isn't consistent - squads get changed every few years, gets played outside India every now and then, teams keep getting added or banned. This makes it difficult to get high volume of high quality data & there's too much noise added due to above factors
 4. As I mentioned earlier, except for a few there aren't many teams which have consistently dominated the IPL over a long time. A team having a good season can suddenly become poor the next season. 
![enter image description here](https://lh3.googleusercontent.com/RWTBDACkQohzdiBPQYuyvAZoog8-s67wQEbAtOzgcSyqZ9CrX3z2PKd6vRhjUwM0XumurRtqNMw0P419SN6EXEfkn-SAlpKd4QgTqGIs9cMqfEXKHX9CJ-CgcnxPElsOqFPaCfxzy9P47OcUvZ5CkSglftsBaqqobGNk1BwGb6Rgrxqv8gL6b6bFBP-p_UO1_MAAwTw0RYC_hBkbjfiDF8KL8D6V19VMmvZxvPW4Gzwy2TI94X4Wrf9XEsp_J7eDMA5nh-iDXuwTwZckJF3FN-xuIiSnRlGXiYoPOWp7ky89Z6bwNeNHa5ai4lfrHVA9tchE9n7DSe3eEqyVIFF1LjL6GnV89r-wGQYrBGpJJNdvsfKHlWEAMg0AW-B8fh6tAovgAFeNdVQy1nlcw3RkLiPYIvDonBKhsn3VQ-Mth_KMTqMFcxl7RfNaAw2TehZZkMCVAZ4M4XwsTwpe3wZqjvDXEz_PEa_JsmwyFQWxCwew82XzceMQ-MuI1weluEiPlTiayfyebE6ESdeOk3Bhuto1EnwpwsjGfZ33zL2AcWzEftHdQTvkitEoRZpxX96Mu3uD0Smt_tKQLJQvWeIvawycGG5cy8jdwmcIKljcAj1dNPvwAfnKrOYpu4EOe8dTDkt456I3ugyX-RZl45qJS9ntQWTrWNBoay9jnzKFixEX46J9Jf6ldar9agZRMKd3h5IAGtFB6AVy8-o7wpN8EXQ=w1141-h525-no?authuser=0)
The above chart shows the percentage of matches won by IPL teams over different seasons. There aren't too many relatively straight lines in the chart above - win rates for teams wildly fluctuates from one season to the other. There are two teams which have had longer successful spells - Mumbai Indians & Chennai Super Kings, but even they've had poor run of seasons every now and then. 

## Gathering Data
I had initially thought that getting access to good quality data for IPL matches would be a challenge, but a few google searches helped me discover [cricsheet.org](cricsheet.org) - absolute gem of a website when it comes to cricket data. It tracks data for all major domestic & international competitions going back several years. The dataset available for IPL is fairly granular - ball by ball data for all the IPL matches that have happened so far. This is how the data looks like - 

enter image description here
![enter image description here](https://lh3.googleusercontent.com/v_7vpLk4920gPOixZF46E2ojM1Wy31apIWmtHFHjcMD3uyzzjUJGiI9CRsH-X5cQ_kx3Z11Y8Ka5EyfluTPzRQmgdmv-TP8ZEKo6RFi3H8kHyz4No8Jlqv9zgXlP-coa4bkwuu5Rnw0z-zXTKE8ywQIx21LZQ3WS1LtCWyN50Mwdbrh7NAi8kc0uIYpmhdHbodNJLrDqxiH2dfOeMPcJ1JyT5SCRLJbeWU5zSmzvkLuwmUrh2zspAaOiWUCru0MtBDjjozdPSiDITyWwIbMrOeiLmEKvZP3u-mi2VqKLW3zi0WB3H0ngEGIzOb4PXO9MWhy8j9-WVqjvwZeDTDOFtVkXwcl_8mbul83hhKlHd6N7XCrK8FVz5d6q_f5HVs3kJNdbOgbSGCYAa_IGHWbTtJ_xpD7NdbIZqNIqLCBYxAMdNB-gbju9tE-Ji4NouQi7nbMJ-LomJ2xsxZRipRuBFQ__xvI26YnL4vRwm8XYI9zoXxZFiCjnvpDsINqoMlFz-xMJHEhudHLGfZ80O_P82B3zwrkIyAyfNZJP3MyfMuPYr5g6qHKf_EHRBb5dYFjBLW-JPzJWpAqp-N4LsujA0qdzcL40-3rB-1OJX4UAId3ts9pi0LRSxGq6yBta58jrOj8LBCNhzHIuT3nTl059Zvhgd5aqYnVaHzxQJ8B4u7ubisPrU6AlnekbicnILSKfpnOPGqkkCIjKoHAvwYYGT44=w981-h179-no?authuser=0)

The data is available for all the matches, tracking information around venue, batsman, bowler, extras, dismissals etc. However, it does require some amount of cleaning & wrangling to get it into a format ready for making predictions.

## Data Preparation
Although the data is available in a reasonably clean format, there are still some operations that need to be performed to get the data in a desired format - 
1. Ensure team & stadium across season are consistent. For ex. Hyderabad team shows up as Deccan Chargers, Sunrisers Hyderabad across multiple seasons. Ensuring that such instances are treated as same across seasons
2. Roll-up the ball level data to match by match data, with clear winner being identified for each match. 

## Feature Engineering
As with any modelling problem, domain knowledge is very important to come up with good features relevant to the problem statement. Very often the quality of data is just as important as the choice of the model for making predictions. Based on my experience of watching cricket broadcasts over the years, I came up with a list of factors which I think play a role in determining match outcomes. Exploratory analysis of data on these factors along with several iterations during the model building stage will help us select the optimal set of features which maximizes prediction accuracy. Here are the categories of features I came up with intuitively - 

**Venue Stats**
1. Home/Away fixture - Unlike football & some other sports, pitch conditions play a huge role in determining the outcome of the match. Teams create squads to suit their home ground pitch conditions - for ex. Chennai going spin heavy to suit spinning pitches at Chepauk.
2. Venue Win Rates - As some seasons are played at neutral venues, its good to keep a track of a team's performance at every individual venue

**Runs & Wicket Stats**
1. Powerplay Batting & Bowling Stats - Number of wickets & runs scored on average while batting & bowling during powerplay
2. Innings Batting & Bowling Stats - Number of wickets taken & runs scored on average while batting & bowling during the entire innings

**Win Rate Stats**
1. Team Overall Win Rates - Even though we saw that a team's win rate in a season can fluctuate a lot over multiple seasons, its still a good metric to separate the likes of MI & CSK
2. Head2Head - Head to head records of teams against each other
3. Season Performance Stats - Team's position on points table & number of matches won in a season is a good indicator of recent form and could be useful for predicting the winner. For ex. you would expect a team at the top of the table to beat the team at the bottom of the table most times during a season.

Let's start diving deep into some of these feature groups and look at which ones correlate with a team's win rate.

**Venue Stats**

![enter image description here](https://lh3.googleusercontent.com/mxCv6Xwx3YCiwhWgYoRX6Lh92Qmr83my6Rc7AyU-Ci_TLtnxOcQQ1DljWUtt92bw600uN2fmJ2Z5z31nZh1VocetJGjv8wx0IvfQf-xvHOwcAl9sEhBSxNb_7prRQsjcgBuKxhLzgT2_4YJKCwBk_qDo3vBEdruEnX0gZswE2_b828EMJ4RmRQkEdw49n7RzQKG8C57YaIAev7AWh6GoKpdNkTZQdWWQo8_PUJ4JSBy5CDT4h7455VvCv9HHYc_VUpON-yYS2iXwOpZg1uDuG1tOCHUiHfpYy5-D29VNoXLey0sKHVbifMdWikErAut7NhlXq6ZfN3t_9VHnUasNWtWg_WDgFb8m4la0c9AnqE_XpyYlM_9dbI576I-Ql38ySpcEd9IdZ6cmvvO74RJWuyfntxfMdYudoM31M1eIkpyTyPr0RqD2lTfhfpTHZXk0fmwz8baa3Prq1XG2nhtTy5iGqo7aMHKEwZpvNs37HN6Br8KNThQmpSK5hMcBaYh9Wh5Lg_ELjPa39UOpfzbl3QoR9wYBP8NL5rcUzHlo91y0UAcdyVHNYBMZLnwgpCBug911HDIPyoSMJSzzmt4uQ2a6pcOrjxDe_ehzRs4VWlfEJ5qRRmGBhauCWia6WkImSJZwQdaRY47HmmhiWygP9gPVzBtDkPdPK2N6XewltLFxz3SWEiqJNiMKpEfro_r-HE36YuWjlSrIZ2f6hIp4UW0=w1158-h525-no?authuser=0)

As you can see above, most teams seem to do well in their home conditions compared to away grounds. Teams play at least half of their matches at their home ground every season. Pitch characteristics & ground sizes vary a lot across India, with some pitches like Chennai being spin bowler friendly while others like Bangalore being a batting paradise. Teams therefore build their squad to best suit their home ground conditions which gives them an advantage over visiting teams. Also home support is a big factor as all IPL games are well attended and fan bases are quite strong. 

Rajasthan & Chennai both win almost 70% of their matches on their home grounds. No team other than Mumbai & Chennai has a win_rate of more than 50% on away grounds. This versatility across conditions is probably the reason why both Mumbai & Chennai are the strongest teams in the IPL. Let's also look at distribution of win rates across different grounds to see how much an individual team's performance varies across grounds - 

![enter image description here](https://lh3.googleusercontent.com/iqNIroEuKt8ywcFBvKN28EX6wXjXk7MWLICOBXJoGlU8CRYrZdgtbfAgGVcVPXWDYrY9v08u8QLLBt0YQrkwJ4kEiyoZ0mL96ZzuCp8fNj6fVSsbn12iScoS6C9_mTMKMy9ZkYmi8P9hzKCwtCl5pVFY-HKHqtXIKbofHD-68mISue0VOhWD5s7VEYBkhNC6_QolaPaxrWnLpmvI8w-XIkQFTuz9Yew1_E4LzgjLBg7OoanFQZfvZgpyF_7Tu7e_rNfUdz0-FxBrF8B9I_5ISX5uP97wuQoyqXVyf79rdBcM8MLW_BEZtF5VndIEKtJY9NzM-B63zLohPC3tFsAhx99U9u5SlaTrJQ6rZUChXPqE3tsykfJuIXI7VcbPzL9WloE5igkuPEsemIy3NjsXdYJONM_IrBZ_eUogzILg5uGAADNt4YjxjxdaA3B0hMJqgQ7s8nSpVZXm8AMKuO9Fkt-_Fq9GsUMuEZ25ytPQ6L3Z00hDrdJeHTEsxBb9ceReyUmGE6GiC24XukS4Lnkvp-qVH-dk5dr5GuRiPmIUHQiag11_7nuSD64TFFCDjovSa3hFzpZJH6BdZxzOftB2q4K2cJvwGxcVj01pjW22W4nd3HZkq8wkr_GyP9wX9xUMAxLsS1s9aI0JQ7dWlbTf-og0i6R1lFan6ppWmvzIhWe6LNlaTJpnLhd_X005BZoJlYb2QmbJ6mHQIHCPRTUlHpE=w1158-h525-no?authuser=0)

As you can see in the chart above, team's win rate at different venues fluctuates quite a bit. The violins above show that even within away venues there is a huge variation in win rates for teams. This proves the point that pitch & ground conditions play a huge role in deciding match outcomes and should not be ignored while creating the model. 

**Batting & Bowling Stats**
![enter image description here](https://lh3.googleusercontent.com/UdKrvn6kOKtsqoglazcRcw03uDn1KLW3Z2dv9VpKRJZuVxCY7R6KHil9UExsjZs8rxu4_OJV-iZLBQhpkRbMo0r2OIQ77MOCwjqA65IrNA3d14O_OOd_jLNU0-wBHWBSv2X0uT1AwUi0m1d5-bxnRt8SxOq-9py1Fnpcb2l8OgmLybIDOinBs5pxo954bkFIKePC5h0Oi1S4YVDvBmGgZs58mPaGMPcRWdj-d3p15BWt-ohma1J3Uqn5qsvM8U45-sAz-LaeutQIeSEKdVAwHIWEnb4aRMmbAb008orOTSRovBH74nY9iDC4tP9bfGlzLFus4Z6q8IXXV4QsPojx6jQQeJYkzKwwlD2b76JGwnwyXvtbDHEhWRz91fVgQ5m92_HtTC_1psc7N_6mD1YGWdVeJCJFoOhpT-hOcAT38izkUQcFPsOJxOIbCUMWHYNEk-6jnAkyP6sKUL3ufTgJNHmcJeETmy0X22iki6-xYza10s3zUW3tuiI81hQFzb4arNIXMIkGRMMdwqb4ftqyNNmLSjTtIKAo4HyabnDz7__1m0apvC03tjzhckj9kqi6i6q3SOxo-E9INyaKsqpQbmm_8nuzKofpEJfFdwNMXjojdmADsFksHGSXZWBuGb0JWez5m_9NkzjpJ_-Cb5eMunSAW3_HiyAEjcyBMUJMUy5cf7fQAG1pmj9Bq-lpj6iXoH-LebSVBGCHbZM6dToaLe0=w947-h830-no?authuser=0)

The above scatter plot shows us a scatter plot distribution between win rates for teams & their average bowling & batting stats for powerplay & innings. For some of these stats like mean runs scored while batting first have a correlation pattern with win rate, some others like numbers of runs scored during powerplay while batting first don't show any relationship with the win rates. A study of these scatter plots will help us understand which variables correlate with win rate & will help us in feature selection feature engineering stage of the exercise.

**Win Rate Stats**
We're looking at multiple win rate stats like head to head records of teams against each other, overall win rates since the beginning of the IPL, total number of wins in the ongoing season. Let's take a look at one feature - difference in number of wins for the ongoing season.

![enter image description here](https://lh3.googleusercontent.com/U4bU7kEKgMKzi-mA-47bM_UArGIboNFQZHnp23CjWqE9JJgC8yZRFDUtQr-IZJoY0-higGKxamC5519oRVN0Nt8KiQ65VuPB4bbEOlqOsuYZ_eB6tZxqm4CuqLD7OXVvTwgD2Sp3vCj0onClSK2RpUCPOV_TACZgARAPqGAO3dobKw8USHL3RTqF3SsRmveXRSVjH5ncn7Pb588e0oxEohbCnT2lhiAGfMj0BW8MpOCJfrj4t8sB2wT1AryYimtRvkXMVj9epHVO-iIcdUk-UEAUKaNBEd7x8iXzMdAY-ISGpd5P62NLqq9U0tmHcdl3Yaurdu3FDVzCql4BeWPjZZr4eAxBfRFLkejtOt4jG_mn6jda8JFCq5w3zI-2Uj7lWVGtMcQKJeE77zWpf1r9ISqydnE3Qp59kQHsQbf5BDdkbMpq_Udt4qIuPBdlsSh7SKhbNII12EZ966kKuvXu9EukX7bTro2WOKKVgnGe5Xp0c9dQjfJgaqcNz-RkPo_W5smd-UtEMzwa3CU8i5wNwEZ-Hi0DsKJrmJyp08V3JvhKNM0eTT0kHQBUvEdhFyxa0EM7Ezyl7OJv5UCBhtclKhWlRu0Yqkg57kt9TfsyKB5NBv97EUtLuuDg5ueX-hA3pAbAG6LyJIH3eJ2XY-cX4rwqvtJi9FsBEEg6ubZ9rhSe1splwSN_MV6cQnZbZ6Sg6VlwLJawp63ChkkiFD5QkIs=w1158-h525-no?authuser=0)

As we can see above, current form in the season plays an important role in determining match outcomes. Teams that have won a significantly higher number of matches compared to the opposition in the season tend to win more often. 

## Model Design & Development
![enter image description here](https://lh3.googleusercontent.com/cFwxuX_gRLq_kNsvulXduGAAFpZQqWWkL1y93gdSwUQcxUx1-A8tjXw7tLu5MKvTgo6SihXU9nEDD1EKFy5DjtoPbBJUdTZYT10rUm6fvmXnS15SvQFLLN5vk_6pcEZTVwdIxcwTHV8AOKQeLdiFSDYWCnZL4Ibl_I6n-2P0pdiyf-S6XN2rSlLA6r_MAeITf1sUckCNo4BdUhWNjf4DJkDrFeCAkGEMuv-lLHRaVMoeGLjVuvjdV9S0YMwJjuSqsjlLj72KNLi19BxjNTfb0qHAFXXSffKNk58HsWQ9AuIHh7lsu6HH3B0WG_oXlbf5wXplqVri61ZnqK7lDfhFSEExksEPAMjkZexsw-w3znbeD4FECK9W0M19WXQdQa7FQX47cvqpvz3TNjhrXXPGQ6AFNxRVHfV77yRVMTm2xgmsoVlU4z_Eavp9Z1Ed3iOtW2pBc_YIY1iQgd4iYYTYGdUvoDvW1NujamHzM01Dyh1t2ejfypLH7Z4YJtIXZa1wQQGMw5r4UkN6pA6U94MIrisFWgqbiANeUMtrWy-tIloaLlR_yNmiq_dHFRCD2vr33U4Ug2m0HBB2HTSLG2O1aQQ1FtcBq7vX-h-SMzSQXvvj5Qa1FZs0iSnwbs4Znhjss2cfq15qojSgSemGmaKJA22_69CSMswDDdcVdZj3rU8uCjUs_K5u0DgYAA0W7gPVlgNIsPFZAhPfLIKN7vdIOEI=w999-h490-no?authuser=0)

Summarizing the image above - 
1. Model development - Happens one per season using training data as data of all the previous seasons
2. Test Data Update - Happens once every week taking into account change in team's batting & bowling stats as well as current season's performance. This test data is used to make predictions every week. 

The aim of the model is to predict the winning team for every match. We use tree based ensemble Random Forest for making our predictions. After several iterations of model hyperparameter tuning & feature selection, here is the final feature importance plot we got for our model.

![enter image description here](https://lh3.googleusercontent.com/XLSqpjjZY80PYmzbCGwR-HYP3dAqD4BH7Gy47BXkNbwBkdLzhC-xazCDzsIECr1jWA-PgLLlQQ8wdz2YIoiZnPrHn4il4RTt9aZKPskEGTPjmMiXvNmwTDPOAD3PJalP-nZw43-Snu9S7iiYYvP5P8uh6Gs7rTpn5GfGl7lZlfgJcS3nvr5MTnTXMR_78Cc5uVoZZZ6zs3PoaTSX-Ewpo7UGIcbdfiSdW9cUidYGwrtHVg3HYYS54vJlPH3fufpv1_OdqaiLB_D8CvXdWYyCcdVRjsxNbPl1iG2AblSjq2S37Kl9V4rsxRFXthgZ2Ok0t2XlHnAw56NdiTjxkQOYTka1VjpVJtIH0X0AotP3inD2rfnriz-4QNnVia23K_J33PhA8bfyKf3vQzNXyKNztjyunmqpEy0E59HpTZL4YkZjlJb0gBjhlCNxY2UHfxTiXz2aNrE-6VdOt3qzBCt0cm8Fq9hfznryiuczK4WI20xg_Z9j9ghm9EG7IEtraX7N3micslrFdEMhNx0YCIuY3Jp-0DX2whqXO5VZewDYtFyXRUomQMu2i5t-ATzqu7OBQASfQqU4t4qBKQwKlBvjkeIQ7oswabY-QByWgurC0dB93TqhaL8BVu2HwryuBTrAi2AF4A9XmH7pj0LrPxqr7itMDynrfQUbd1y4vz98HxgHUZ-hdIHA1rt628PLpCRBBRV2HKLRfYE4dM4ZqGTnc5s=w942-h830-no?authuser=0)

Current season form is the most important feature, followed by difference in number of mean runs scored along with overall win rates in the IPL. Since we only have about 500 data points, its good to restrict depth of trees & the number of features going into the model to avoid overfitting. 

We also tested out the model for different seasons, looking to get an understanding of the average accuracy of the model over multiple seasons. As mentioned above, for making predictions for a season, we train the model with data up until the last season and make predictions on the current season. Using this method the general accuracy of the model lies in the range of 65%-70%, though it fell down to 61% for the 2018 season. 

Generally you would expect model to get better with every passing year due to availability of more data, but that isn't the case with IPL due to the reasons mentioned earlier -complete changes in squads due to mega auctions, tournament shifting to neutral venues etc.

![enter image description here](https://lh3.googleusercontent.com/OuR4u9M83eWzDa42VtKYL4WR7yHxmkVDxOcEUQCO9SNwSX3lg-JpeKZ8Qg1k0dcINdBhIy-0AB93kMyjpiYE2BHEQkown4o96d7hUrWzJ13UUvB4Phsi03BarPiXKCO1603A00gw9BIELE745RzcD7Dm1o9Ntu5RNlPq_RGtRtHuyG78xguQWd6CyeiT_QJz73Q_PTA7RmbCugp1b7bD5Qdqb-M3XLscowECp2zeaJG4om-c1zbpckE2Fgh6XZlT2rVOoRDB_NmP5fiI5t-yAZFBrlwKJYIpWvV0E86UExctdXv3g2LAykfoHYNhhNg3EZJXnZ731kOJUUvBBq4un9E9Jeu5ik04lUbKS2aVGGMgFf6obahng2wqBtSWX559yUxZ5gTT1ns_zvvD6C1jaiu-cvrq85bg2Lw2CRhttkgifAuE5Wc4MvjUUOPnWFSbGSTpYk0gU_b4cXlp6xoyShzaWQd7Zgci5MfdrBNHAjuzjSW3GkWBaTdZAPaRc7gJYalpQD1NjB8dMbLtcMRKMXjX5fQObZCw0D7ExJzSwMjjHHZF46h6dmrMj4M-j211800arcw6Mkt8BoMmQX-FoifARojMWlWutzNOLATpwkq9HQ5o0C-XwPCHPxu4_ALg163CbqMayquFIEguK0t-t264P0_z3gSEJahJxFQQ2_b43SEQiRwjxbTDCuTDTM3JgK3DXHynRJdHo55iW6JgVII=w485-h220-no?authuser=0)

## Alternative approaches
While this model gives us reasonable accuracy, there is still a lot of scope for improvement. We did not consider toss outcomes, match importance for teams, day/night status and squad strength based on players involved in the match during our prediction. Adding these & other features could potentially improve the model accuracy.

While researching for methods to make predictions on sports outcomes, I also came across the [fivethirtyeight.com](fivethirtyeight.com) method to create Elo ratings for teams based on squad strength and then run Monte Carlo simulations thousands of times and predict the outcomes of matches & seasons. They've used this approach number of times for sports like Football, Basketball, Baseball etc. I've found it to be fascinating and will be looking to try that out for upcoming seasons & tournaments as well.

## Final Comments
This was a fun exercise and took me about two weekends to complete the entire process from data gathering, manipulation to model building. The final predictions have been made available on a [Streamlit](https://share.streamlit.io/arpitsolanki/ipl-prediction-engine/main/app.py) app. Code is available [here](https://github.com/arpitsolanki/IPL-Prediction-Engine) 

Hope you guys enjoyed reading it. Please feel free to leave your feedback in comments. 

PS - Unfortunately the IPL season had to be postponed due to Covid situation in India, but this was a fun project & acted as a good starting point for me to use data for cricket. Will look to build on my work again when normalcy is restored to life & cricket.


