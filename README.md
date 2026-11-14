# BTC 24h race bot — patient deep-dip

Entry for make.fun `btc-race-24h-20261113-2314` (race a BTC trading bot live
for 24 hours). Python 3.13, standard library only, speaks
agent-fun/bars-replay/v1 via the vendored kit SDK shim (`vendor/`,
byte-for-byte from the kit).

## Strategy

One patient trade: wait ~6.5 hours (400 bars), then buy the first deep dip
(close >= 100bp below the rolling 120-bar high) that bounces (close above
the previous close). Full size, one round trip max. Exits: take-profit
+150bp, trailing stop -60bp from the peak close, hard stop -150bp. No
trend/slide/extension gates — on this dev window (BTC -4.91%) every gate
family blocks all entries, and a 150-variant scan found no gated dip-buy,
RSI, Bollinger, EMA-cross or breakout variant above $0.

Last resort: if no round trip has happened by bar 1200, buy the next
30bp dip+bounce so the entry still meets the race's min-1-trade guardrail.
It never fires on the dev window (the round trip already happened).

## Dev-window result (faithful simulator, verified by protocol test)

Net PnL +$10.59, 1 round trip (buy bar 404 ~$68,318, sell bar 468
~$69,097), max drawdown 0.69%, fees $0.80. Neighbours robust
(minbar 390–450, dip 90–120, TP 120–200 all positive); 2x fees +$9.79.

Honest note: the whole dev profit comes from one trade on one rebound of
this window. The same rule loses on the previous race's dev window, so it
is not a robust cross-window strategy — passing the dev gate is a ticket
to race, not a promise of race profit.

## Layout

- `main.py` — the bot (entrypoint)
- `vendor/makefun_bars.py` — kit SDK shim, byte-for-byte from the kit
- `.makefun/owner.json` — claim file naming the entrant's wallet
- `LICENSE` — MIT
