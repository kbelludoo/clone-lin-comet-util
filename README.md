# clone-lin-comet-util

Experimental i64-scale LIN clone of [compound-finance/comet](https://github.com/compound-finance/comet) `contracts/CometWithExtendedAssetList.sol` (`getUtilization`) plus `CometCore.sol` `FACTOR_SCALE=1e18`.

- **LIN clone:** [`src/lin_comet_util.lin`](src/lin_comet_util.lin)
- **Proofs (results only) live in lin-open:** https://github.com/kbelludoo/lin-open
- **Event receipt:** `docs/events/EVENT_COMET_UTIL_CLONE_LIN.rulel` in lin-open

This repository is the standalone LIN copy. It is **not** Compound III, **not** a Comet / USDC market, and does **not** claim Solidity uint256 / mainnet / PriceFeed parity. It is **not** the presentValue/principalValue kernel (`clone-lin-comet-pv`) and **not** the accruedInterestIndices kernel (`clone-lin-comet-accrue`).

Upstream was BUSL-1.1 with Change Date 2025-12-31; the Change License is GPL-2.0-or-later (this run is 2026-10-12). This LIN kernel is an original uint64-scale implementation of the published present-value utilization wrap, not a verbatim Solidity dump.

Pinned upstream: commit `f766f51583c23acc33b2a7824654ef2029a96804`.

Canonical goldens (uint64-scale, 128-bit product):

- `utilization(1e18, 1e18)` = **1e18** (100%)
- `utilization(2e18, 1e18)` = **5e17** (50%)
- `utilization(1e18, 2e18)` = **2e18** (200% — possible in III, not in V2 cash+borrows)
- `utilization(0, 1e18)` = **0** (empty supply fail-closed)
- `twostep(2e18, 1e18)` = **0** (rejected `floor(borrow/supply)*1e18` path)
- Compound V2 JumpRate `utilization(1e18, 1e18, 0)` = **5e17** (50%) — same numbers, **2x** below III
- `1e18 * 1e18 / 1` fail-closes to **0**
