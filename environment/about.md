The file we are currently run is main2.py with the Trader class coming from trader3.py - all the others are older iterations of our alogorithm.<br/>

The strategy is quoting in PHILIPS_B (making profit off the bid-ask spread) and then hedging in PHILIPS_A (ensuring our net position is 0).<br/>

First, a brief overview of the functions powering the bid/ask prices:<br/>

idealPrice - takes current best bid/ask prices in PHILIPS_B and undercuts it by 0.10. To account for short term volatility we have also included a <br/> history function which averages the last N number of best bid/ask prices. This was especially useful in the unstable markets of yesterday.<br/>
If there is no current best bid/ask prices, we generate the price using previous prices.<br/>

hedgeAdjustor - we have attempted to keep the number of positions in PHILIPS_B. The further from 0, the less competitive we make our quote prices.<br/> E.g. If we are very long in PHILIPS_B, we wish to sell more, so we lower our ask price.<br/>

checkCrossover - there is the edge case where we work out the bid/ask prices thus far, and they have actually crossed over (sell low, buy high).<br/> If this has happened, we adjust the prices so the crossover is negated.<br/>

Secondly, we move to the hedging functions. The basic strategy is: we only quote prices in B such that we still make a profit when we hedge in A. <br/>
More detail: If we buy in B, we want to sell in A, ONLY WHEN we can sell in A higher than we bought in B (or vice versa)<br/>

checkSpread - simply checks the difference in the CURRENT spreads in A and B. <br/>
We then adjust the bid/ask prices in B such that spread in B is greater than spread in A<br/>

hedge - Executes the order for hedge trades in A. We ensure that the price that we order is not significantly different from the last known price.<br/>

decideVolume - varies the volume we order in PHILIPS_B depending on net position. We do not want to be far from 0, so we trade less volume if we are<br/>

Now, the main functions are done and we finish off the Trader class.<br/>

trade - executes order for PHILIPS_B. Contains various conditionals to ensure we respect the limits on positions etc.<br/>

close - closes the class and provides feedback on trades done in the last session.<br/>
