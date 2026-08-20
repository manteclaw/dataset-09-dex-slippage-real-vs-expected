# DEX Slippage: Real vs Expected (Sample)

Sample dataset analyzing actual execution slippage vs quoted slippage on Base L2 decentralized exchanges.

## What's Inside

This sample contains **25 real trades** across Aerodrome and Uniswap V3 on Base L2:

| Column | Type | Description |
|--------|------|-------------|
| timestamp | ISO datetime | When the trade executed |
| dex | string | Decentralized exchange (Aerodrome, Uniswap V3) |
| pool | string | Pool type (volatile, stable, or fee tier) |
| pair | string | Trading pair |
| trade_size_usd | int | USD value of the trade |
| quoted_price | float | Price shown in the DEX UI before trade |
| actual_execution_price | float | Price actually received |
| slippage_bps | float | Actual slippage in basis points (1 bps = 0.01%) |
| expected_slippage_bps | float | Slippage quoted by the DEX UI |
| slippage_excess_bps | float | Extra slippage beyond what was quoted (positive = worse) |
| user_loss_usd | float | Dollar value lost to excess slippage |
| tx_hash_prefix | string | First 6 chars of transaction hash |
| block_number | int | Block where trade was mined |
| gas_price_gwei | float | Gas price at time of trade |

## Key Observations

| Pool Type | Avg Excess Slippage | Worst Case | Cause |
|-----------|--------------------|------------|-------|
| Volatile (AERO/ETH) | +12.4 bps | +43.2 bps | Low liquidity, high trade size |
| Stable (USDC/DAI) | +0.2 bps | +1.0 bps | Deep liquidity, minimal impact |
| Uni V3 0.05% (USDC/ETH) | +8.5 bps | +37.8 bps | Concentrated liquidity gaps |
| Uni V3 0.3% (WBTC/ETH) | +15.6 bps | +48.4 bps | Wider fee tier, less competitive |

**Critical findings:**
- **Large trades (> $10K) consistently exceed quoted slippage** by 20-50 bps on volatile pairs
- **Stablecoin pairs are safest** â€” excess slippage rarely exceeds 1 bps even at $15K size
- **Gas price spikes correlate with worse execution** â€” trades during congestion see 2-3x excess slippage
- **Uniswap V3's quoted slippage is optimistic** â€” actual exceeds quoted in 72% of trades > $5K

## Use Cases

- DEX UI accuracy auditing and improvement
- Trading strategy optimization (optimal trade size splitting)
- MEV protection research
- Academic research on DEX execution quality
- Building slippage prediction models
- Wallet/DEX aggregator comparison

## Full Dataset

The complete dataset covers **5,000+ executed trades** across Aerodrome, Uniswap V3, and Curve on Base L2, with 90 days of data and full transaction hashes.

- **Payhip:** [Manteclaw Datasets](https://payhip.com/Manteclaw)
- **Full rows:** 5,000+
- **Price:** $16

## Methodology

Trades identified by monitoring DEX router contract events (`Swap` events on Uniswap V3, `swap` on Aerodrome). Quoted price captured from simulation at block start. Actual execution price from event logs. Expected slippage calculated from DEX's own `quoteExactInput` function at 1 block before execution. Excess slippage = actual - expected.

## License

CC BY 4.0 â€” Attribution required. Commercial use permitted.

---
*Sample dataset for evaluation. Full version available on Payhip.*
