# Changelog

## Upcoming Breaking Changes
- Teku currently publishes a `head` event on the REST API 4 seconds into a slot even if a block has not been received. In a future release this will be changed so `head` event is only published when a new
  chain head block is imported. The `--Xvalidators-dependent-root-enabled` option can be used to switch to the new behaviour now for testing.
  Note: this should be applied to both the beacon node and validator client if running separately.
- The `/teku/v1/beacon/states/:state_id` endpoint has been deprecated in favor of the standard API `/eth/v1/debug/beacon/states/:state_id` which now returns the state as SSZ when the `Accept: application/octet-stream` header is specified on the request.

## Current Releases
For information on changes in released versions of Teku, see the [releases page](https://github.com/ConsenSys/teku/releases).

## Unreleased Changes

### Additions and Improvements
- Fixed issues in discv5 `PING` and `PONG` message handling which resulted in not updating peer's ENR records correctly.
- Implement alpha.7 spec updates to sync committee logic and rewards.
- `deposit_data` files generated by the deposit-cli tool in the validator keys directory are now automatically ignored. Thanks to Vishal Jhala.
- Added new metrics to report on the progress of Eth1 block voting:
  - `beacon_eth1_current_period_votes_max` - Maximum number of votes possible in the current voting period.
  - `beacon_eth1_current_period_votes_total` - Number of votes cast so far in the current voting period.
  - `beacon_eth1_current_period_votes_best` - Number of votes for the leading candidate in the current voting period.
  - `beacon_eth1_current_period_votes_unknown` - Number of votes for blocks this node considers invalid (e.g. unknown blocks).
  - `beacon_eth1_current_period_votes_current` - Number of votes for the value in the current state (the default vote per the spec).
  - `beacon_eth1_block_cache_size` - Total number of blocks stored in the Eth1 block cache.

### Bug Fixes
- Prevent LevelDB transactions from attempting to make any updates after the database is shut down.
- Update `/eth/v1/node/health` to return 206 while node is starting up rather than a 200.
- Fixed issue which cause a small reduction in attestation rewards when using `--Xvalidators-dependent-root-enabled`.
- Fixed issue with eth1 follow distance tracking revealed in Rayonism testnets. Teku incorrectly strictly follows 2048 blocks before the eth1 head but the follow distance should be based on timestamp, not block number.
  This change can be disabled with `--Xeth1-time-based-head-tracking-enabled=false` if required.


### Experimental: New Altair REST APIs
- implement POST `/eth/v1/beacon/pool/sync_committees` to allow validators to submit sync committee signatures to the beacon node.
- implement POST `/eth/v1/validator/duties/sync/{epoch}` for Altair fork.
- implement GET and POST `/eth/v1/validator/sync_committee_subscriptions` for Altair fork.
- implement GET `/eth/v2/validator/blocks/{slot}` for Altair fork.
- implement GET `/eth/v2/debug/beacon/states/:state_id` for Altair fork.
- implement GET `/eth/v1/beacon/states/{state_id}/sync_committees` for Altair fork.
- `/eth/v1/validator/blocks/{slot}` will now produce an altair block if an altair slot is requested.
- `/eth/v1/node/identity` will now include syncnets in metadata
