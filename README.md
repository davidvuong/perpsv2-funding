# perpsv2-funding

**Welcome to perpsv2-funding!**

This project provides a Python based sample implementation of the new funding model described in [SIP-279](https://sips.synthetix.io/sips/sip-279/) for Perps V2. The new funding model aims to optimise for a perfectly balanced skew. To recap the basics,

- A perps market is a Synth/sUSD pair e.g. sETH/sUSD
- A market skew is present when longs and shorts don't balance out e.g. 1M short, 2M long has a 1M long skew
- When a skew is present, the skewed direction pays the opposite defined by a 'funding rate' e.g. 1M longs pay shorts. This is meant to incentivise arbers to short on Synthetix and long on another exchange, reducing skew back to equilibrium

The current mainnet implementation of SIP-80 poses two problems:

- There is no financial incentive to perform the arb as funding rates are calculated instantaneously. In the example above, a short to bring skew back to 0 will also result in a 0 funding rate. The arber makes no money
- Funding rate volatility. A funding rate that fluctuates too frequently is not attractive for traders

The new funding model in SIP-279 solves this by introducing a floating funding rate. The basic idea is as follows:

- As long as the market skew is not at equilibrium, increase the funding in the same direction. As some point, enough market participants will participate to bring it back. If there's free money on the table, and you had alpha to take advantage, why not?
- If/when a market skew is at equilibrium stop changing the funding rate. This is the floating funding rate. It doesn't instantaneously drop to 0, keeping the arbers on the exchange to maintain skew. We can denote this as the fair market funding rate.
- If the skew drifts past equilibrium into the other direction, the funding rate hence increases in the opposite direction.

## Notebook

The meat and potatos of this project are all within a single Python Notebook, which can be found here: https://github.com/davidvuong/perpsv2-funding/blob/master/main.ipynb
