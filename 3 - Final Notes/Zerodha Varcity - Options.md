
| Property         | Value                                              |
| ---------------- | -------------------------------------------------- |
| 📅 Date          | 11-08-2024, 11:36                                  |
| 🏷️ Tags         | #options #derivatives #investing #futures          |
| 🔗 Related Notes | [[Zerodha Varcity]], [[Zerodha Varcity - Futures]] |
##### #1 Intro to Options
These are types of derivative contracts. There are basically two types <span style="color:#e1db3d">Call</span> and <span style="color:#e1db3d">Put</span>. 
No actual shares are transferred to the accounts of the participants. Hence the name, <span style="color:#e1db3d">Derivatives</span>. The profit of a call or put buyer is from the difference in the strike and current price. This way the buyers have not cap on profit, and the loss is limited to the premium amount. For the sellers however the profit is capped in both cases by the premium amount and the losses have no limits.

<span style="color:#e1db3d">The Call Option</span>: The buyer pays a <span style="color:#e1db3d">premium</span>(an amount to reserve the right to buy later at a lower price) to the seller of the Call Option. This is the token on which the buyer may purchase a certain commodity from the seller in the future <span style="color:#e1db3d">regardless the price of the commodity</span> at a fixed price, one that is decided by both at the time of drawing up the agreement. If the price of the commodity does on increase at the time of the expiration of the contract, then <span style="color:#e1db3d">the buyer can simply refuse</span> to go through with the purchase but <span style="color:#e1db3d">lose out on the premium</span>. But if the price goes up, the buyer will make a purchase and get he higher valued commodity at a lower decided price, hence making a profit.

##### #2 Options Jargons
The call option instrument on a trading platform looks like this:  `SBIN OCT 460 CE`
`SBIN`: State bank of India stock is the commodity.
`OCT`: This is the <span style="color:#e1db3d">expiration date</span>. This happens on the <span style="color:#e1db3d">last thursday of the month</span>.
`460`: This is the <span style="color:#e1db3d">strike price</span>, This is the fixed price at which the deal will occur if the buyer wishes at the end of the expiration period.
`CE`: Call Option (European)
The premium in this example was `₹ 17.74`.

These options are not for just a single share, they are for lots, lot size vary for different shares. So total premium is `lotSize * perSharePremium` .
==The premium is just a fee for reserving the right to buy, and does not buy you any shares, one must buy the ordered lot at the full strike price at the time of expiration.==
##### #3 Long Call Payoff and Short Call Trade
The best case scenario is the commodity increasing in value. And the worst case is losing all the premium. The graph of maximum return with respect to the price of stock at expiration is called the <span style="color:#e1db3d">long call payoff</span>, if it stays below the strike price the profit is losing the premium, if it is above the profit becomes the current price minus the premium.
<span style="color:#e1db3d">The graph is inverted for the seller</span>.

##### #4 Put Options
Contrary to the Call option, the <span style="color:#e1db3d">Put relies on the stock price going down</span>, its type of a call option but on the falling edge of the stock (analogous a short). One decides to Sell the stock at a fix price and pays a premium to do so before the expiration period. Now if the actual price of the stock is down, one makes a profit but if it is up, one will have to live with the loss of the premium.

The put option instrument on a trading platform looks like this:  `SBIN OCT 460 PE`
`SBIN`: State bank of India stock is the commodity.
`OCT`: This is the <span style="color:#e1db3d">expiration date</span>. This happens on the <span style="color:#e1db3d">last thursday of the month</span>.
`460`: This is the <span style="color:#e1db3d">strike price</span>, This is the fixed price at which the deal will occur if the buyer wishes to sell the stock at the end of the expiration period.
`PE`: Put Option (European)
The premium in this example was `₹ 17.74`.

##### #5 concept summary
<span style="color:#e1db3d">Call Buyers</span> and <span style="color:#e1db3d">Put Sellers</span> think markets are <span style="color:#e1db3d">bullish</span>.
<span style="color:#e1db3d">Call Sellers</span> and <span style="color:#e1db3d">Put Buyers</span> think markets are <span style="color:#e1db3d">bearish</span>.
The options are available on <span style="color:#e1db3d">indexes</span> as well and we can square-off anytime before the expiration date.
There is a certain margin allotted by the broker to the trader. If one desires to take the sellers position in any of the two options, they must have to follow the margins and their theoretical maximum loss cannot exceed the margin values.

##### #6 option chains
Taking a look at all the options, we can classify them into three categories,
<span style="color:#e1db3d">ATM</span> (at the money): Strike price is same as LTP.
<span style="color:#e1db3d">ITM</span> (in the money): Strike price is lesser than the LTP.
<span style="color:#e1db3d">OTM</span> (out of money): Strike price is more than the LTP.
The action(volatility) is always more around the <span style="color:#e1db3d">ATM</span> options

##### #7 option greeks
When the underlying price changes, the premiums also change, for calls the correlation is positive, and for puts it is negative.
<span style="color:#e1db3d">The delta</span>: This is a value between 0 and 1 for calls and between -1 and 0 for puts (inverse nature to LTP). It indicates the change in the premium per unit change in the LTP.
If the delta value is 0.15, it means for every 1 point change in the value of the stock the premium changes by 0.15 for a call option and -0.15 if its a put option.

| Option category | Probable Value |
| --------------- | -------------- |
| Call ATM        | ~ 0.5          |
| Call OTM        | 0 - 0.5        |
| Call ITM        | 0.5 - 1        |
| Put ATM         | ~ -0.5         |
| Put OTM         | -0.5 - 0       |
| Put ITM         | -0.5 - -1      |
##### #8 gamma 
The rate of change of a delta.
##### #9 theta
With more time to expiry, the chances of an option to be profitable is high, due to general rising nature of the market. This means with a lot of time the ATM and even OTM could become ITM. So for a seller if there is a lot of time to expiry, chances of him failing are high, so the premium he requests is also high, if there is less time for the stock to go up, the premiums are low as they option is more likely to be worthless.

Theta is the value that indicates the change in premium with respect to time.
The premium can be broken into two parts, intrinsic value and time value. The time value will decline and come down to LTP 
##### #10 vega
The rate of change of <span style="color:#e1db3d">premium wrt the volatility</span> is the <span style="color:#e1db3d">vega</span> factor. If the volatility is higher then its more likely that the option will become ITM. So the premium for such option is also greater. Volatility is directly proportional to Premium and vega is the proportionality constant.

##### #11 Margins
Option selling requires margins from the end of the trader as losses are not capped. The margins are proportional to risk. If volatility increases its bad for sellers so the risk increases and margins increase too. Margins are calculated on the position of the entire portfolio, one may hedge and reduce the margin charges.

##### #12 physical settlement of stock options
SEBI has enforced physical settlement

