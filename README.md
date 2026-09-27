# Villager Currency: Lightman's Economy

Minecraft 1.20.1 / Forge 47.4.10 / Java 17

This is a new addon and is not based on the old Create: Numismatics villager currency mod.

## What this version fixes

- Uses `VillagerTrades.ItemListing` for Forge 1.20.1.
- Uses `net.minecraftforge.event.village.VillagerTradesEvent`.
- Uses the 1.20.1 `MerchantOffer` API; there is no `setDemand(int)`.
- Preserves MerchantOffer state by rebuilding through `MerchantOffer(CompoundTag)`.
- Keeps uses, max uses, reward XP flag, demand, special price, price multiplier and merchant XP.
- Converts both first and second emerald inputs and emerald results.
- Wraps Forge villager and wandering-trader listings, so modded listings are covered too.
- Reads actual Lightman's Currency values through `CoinAPI`/`ChainData`; denomination values are not hard-coded.
- Selects among exact one-stack representations and prefers higher requested tiers as villager level rises.
- Never rounds a value downward.
- If a converted representation cannot exactly fit in one stack, the original representation is retained for fixed-value conversions. For dynamic first costs, the live vanilla emerald price is used as the safe fallback rather than undercharging.

## Important limitation imposed by vanilla MerchantOffer

Minecraft 1.20.1's `MerchantOffer` has a single `baseCostA` item identity. Demand and special-price calculations change the count of that item. This implementation therefore marks the selected Lightman's denomination and recalculates the live emerald-equivalent value in a mixin.

When a dynamic price no longer fits exactly in 64 of the selected denomination, it falls back to the exact vanilla emerald price for that offer state. This is deliberate: it is impossible to preserve exact value with a single ordinary ItemStack without either rounding or changing the MerchantOffer payment semantics.

## Build

Place the supplied Lightman's Currency jar at:

    libs/lightmanscurrency-1.20.1.jar

Use Java 17.

    .\gradlew.bat build

The output jar is produced under:

    build/libs/

The mod requires Lightman's Currency 1.20.1-2.3.0.5 (or a compatible 1.20.1 build) to be installed separately in the Minecraft `mods` folder.

## Test matrix

1. Novice villager with a normal emerald input trade.
2. Villager trade that gives emeralds to the player.
3. Trade with a second emerald cost.
4. Hero of the Village discount.
5. Cured-villager discount.
6. Repeated trades to increase demand.
7. Restocking.
8. Modded profession/trade listing.
9. Wandering Trader generic and rare trades.
10. Values that require different Lightman's denominations.
11. A price that cannot fit exactly in the selected denomination: verify it is never cheaper than vanilla.
12. Verify no converted fixed-value trade contains a vanilla emerald.
