Ballpark Summary:

First call (uncached)

          trivial fn | long fn | "integrated" bench

  0.17      20.5ms       130ms      23ms
  1.0       17.5ms        33ms      22ms

Subsequent calls (cached)

          trivial fn | long fn | "integrated" bench

  0.17      4.0ms       114ms      6.0ms
  1.0       0.7ms        17ms      1.5ms


Raw data:

Short function:

```
12:42:06|~/projects/nearcore|wamer1-benchmarks⚡*
λ cargo test --release --package near-vm-runner-standalone --bin near-vm-runner-standalone --all-features -- script::short_computation --exact --nocapture
    Finished release [optimized] target(s) in 0.19s
     Running target/release/deps/near_vm_runner_standalone-69f61a48292627ed

running 1 test
vm kind: Wasmer0
      6.07µs get_key
      20.00ms compile_and_serialize_wasmer
        203.04µs run_method/instantiate
        15.62µs run_method/call
        100.48µs run_method/drop_instance
      338.01µs run_method
    20.56ms run_wasmer
  20.56ms run_vm

      1.35µs get_key
      3.51ms deserialize_wasmer
        172.40µs run_method/instantiate
        4.28µs run_method/call
        86.94µs run_method/drop_instance
      280.14µs run_method
    4.12ms run_wasmer
  4.13ms run_vm

      1.20µs get_key
      3.22ms deserialize_wasmer
        172.04µs run_method/instantiate
        4.20µs run_method/call
        87.45µs run_method/drop_instance
      281.73µs run_method
    3.83ms run_wasmer
  3.83ms run_vm

vm kind: Wasmer1
      1.31µs get_key
      17.23ms compile_and_serialize_wasmer1
        95.38µs run_method/instantiate
        10.77µs run_method/call
        1.57µs run_method/drop_instance
      132.78µs run_method
    17.52ms run_wasmer1
  17.52ms run_vm

      1.42µs get_key
      493.88µs deserialize_wasmer1
        77.51µs run_method/instantiate
        3.48µs run_method/call
        1.77µs run_method/drop_instance
      106.13µs run_method
    762.37µs run_wasmer1
  769.05µs run_vm

      1.30µs get_key
      405.01µs deserialize_wasmer1
        64.14µs run_method/instantiate
        3.07µs run_method/call
        1.34µs run_method/drop_instance
      92.09µs run_method
    666.47µs run_wasmer1
  672.58µs run_vm

test script::short_computation ... ok

test result: ok. 1 passed; 0 failed; 0 ignored; 0 measured; 2 filtered out
```

Long function:
```
12:42:08|~/projects/nearcore|wamer1-benchmarks⚡*
λ cargo test --release --package near-vm-runner-standalone --bin near-vm-runner-standalone --all-features -- script::long_computation --exact --nocapture
    Finished release [optimized] target(s) in 0.18s
     Running target/release/deps/near_vm_runner_standalone-69f61a48292627ed

running 1 test
vm kind: Wasmer0
      3.81µs get_key
      19.97ms compile_and_serialize_wasmer
        201.34µs run_method/instantiate
        109.84ms run_method/call
        100.50µs run_method/drop_instance
      110.16ms run_method
    130.35ms run_wasmer
  130.36ms run_vm

      1.40µs get_key
      3.56ms deserialize_wasmer
        175.69µs run_method/instantiate
        110.18ms run_method/call
        88.50µs run_method/drop_instance
      110.46ms run_method
    114.36ms run_wasmer
  114.36ms run_vm

      1.19µs get_key
      3.37ms deserialize_wasmer
        174.48µs run_method/instantiate
        126.94ms run_method/call
        89.22µs run_method/drop_instance
      127.22ms run_method
    130.92ms run_wasmer
  130.92ms run_vm

vm kind: Wasmer1
      1.35µs get_key
      16.46ms compile_and_serialize_wasmer1
        96.90µs run_method/instantiate
        16.58ms run_method/call
        1.73µs run_method/drop_instance
      16.71ms run_method
    33.33ms run_wasmer1
  33.33ms run_vm

      1.50µs get_key
      525.56µs deserialize_wasmer1
        75.58µs run_method/instantiate
        16.66ms run_method/call
        1.72µs run_method/drop_instance
      16.76ms run_method
    17.46ms run_wasmer1
  17.47ms run_vm

      1.41µs get_key
      428.26µs deserialize_wasmer1
        68.91µs run_method/instantiate
        16.69ms run_method/call
        1.61µs run_method/drop_instance
      16.79ms run_method
    17.40ms run_wasmer1
  17.41ms run_vm

test script::long_computation ... ok

test result: ok. 1 passed; 0 failed; 0 ignored; 0 measured; 2 filtered out
```

Integrated Wasmer0:

```
12:59:54|~/projects/nearcore|wamer1-benchmarks⚡*?
λ cargo build --release -p neard --features wasmer0_default
    Finished release [optimized] target(s) in 0.19s

12:59:57|~/projects/nearcore|wamer1-benchmarks⚡*?
λ pushd pytest; pipenv run python3 tests/sanity/concurrent_function_calls.py; popd
Use default config {'local': True, 'preexist': False, 'near_root': '../target/release/', 'binary_name': 'neard', 'release': False, 'bridge': {'bridge_repo': 'https://github.com/near/rainbow-bridge.git', 'bridge_dir': '~/.rainbow-bridge', 'config_dir': '~/.rainbow', 'ganache_dir': 'testing/vendor/ganache', 'ganache_bin': 'testing/vendor/ganache/node_modules/.bin/ganache-cli', 'ganache_block_prod_time': 10}}
Creating LOCAL cluster configuration with 4 nodes
Search for stdout and stderr in ['/home/matklad/.near/test0', '/home/matklad/.near/test1', '/home/matklad/.near/test2', '/home/matklad/.near/test3']
Starting node 0 as BOOT NODE
node 0 started
Starting node 1 with boot=ed25519:He7QeRuwizNEhBioYG3u4DZ8jWXyETiyNzFD3MkTjDMf@127.0.0.1:24577
Starting node 2 with boot=ed25519:He7QeRuwizNEhBioYG3u4DZ8jWXyETiyNzFD3MkTjDMf@127.0.0.1:24577
Starting node 3 with boot=ed25519:He7QeRuwizNEhBioYG3u4DZ8jWXyETiyNzFD3MkTjDMf@127.0.0.1:24577
node 1 started
node 2 started
node 3 started
Cleaning up node 127.0.0.1:24578 on script exit
Executed store validity tests: 0
Cleaning up node 127.0.0.1:24579 on script exit
Executed store validity tests: 0
Cleaning up node 127.0.0.1:24580 on script exit
Executed store validity tests: 0
Cleaning up node 127.0.0.1:24577 on script exit
Executed store validity tests: 0

13:00:27|~/projects/nearcore|wamer1-benchmarks⚡*?
λ cat ~/.near/test0_finished/stderr
Mar 25 13:00:14.313  INFO near: Version: 1.2.0, Build: eceb375b-modified, Latest Protocol: 43
Mar 25 13:00:14.314  INFO near: Did not find "/home/matklad/.near/test0/data" path, will be creating new store database
Mar 25 13:00:14.700  INFO runtime: Genesis state has 10 records, computing state roots
Mar 25 13:00:14.700  INFO runtime: Tracking shards: {}
Mar 25 13:00:14.703  INFO network: Starting http server at 0.0.0.0:3040
Mar 25 13:00:14.704 DEBUG network: Found known peers: 0 (boot nodes=0)
Mar 25 13:00:14.704  INFO client: Starting validator node: test0
Mar 25 13:00:14.704 DEBUG network: Blacklist: {}
Mar 25 13:00:14.704  INFO stats: Server listening at ed25519:He7QeRuwizNEhBioYG3u4DZ8jWXyETiyNzFD3MkTjDMf@0.0.0.0:24577
Mar 25 13:00:14.704 DEBUG runtime: add validator proposals at block height 0 []
Mar 25 13:00:14.704  INFO chain: Init: saved genesis: `8nF7fhxhmtKAxDjM1ek63WtUK6iHK8TuJe1FwTUiyZaT` / [`iiSeMk1WQEKmMWFcFxL3YC5tsZqMcc1bCynHLzf8FYv`, `BTdKNrn4aDgXdq2depAR3V36e9fGCEKt3THm8Rs2zHXS`, `8YijkEqArew3cMs7MoeZbbnqPEP8LbCUN85qVb668V6i`, `95uBRr8aRH8wuqGCNdQ5TEyfkk6MX22U9HHTpQjYVQZi`]
Mar 25 13:00:14.705  INFO chain: Init: head @ 0 [8nF7fhxhmtKAxDjM1ek63WtUK6iHK8TuJe1FwTUiyZaT]
Mar 25 13:00:18.428 DEBUG network: ed25519:He7QeRuwizNEhBioYG3u4DZ8jWXyETiyNzFD3MkTjDMf: Peer 127.0.0.1:59920 Inbound started
Mar 25 13:00:18.428 DEBUG network: ed25519:He7QeRuwizNEhBioYG3u4DZ8jWXyETiyNzFD3MkTjDMf: Received handshake Handshake { version: 43, oldest_supported_version: 34, peer_id: ed25519:5BVKQ5zZKtEUM3AHgj6KAerTpuh3CVZHtAbmVfxLbLhc, target_peer_id: ed25519:He7QeRuwizNEhBioYG3u4DZ8jWXyETiyNzFD3MkTjDMf, listen_port: Some(24580), chain_info: PeerChainInfoV2 { genesis_id: GenesisId { chain_id: "test-chain-T4xnn", hash: `8nF7fhxhmtKAxDjM1ek63WtUK6iHK8TuJe1FwTUiyZaT` }, height: 0, tracked_shards: [], archival: false }, edge_info: EdgeInfo { nonce: 1, signature: ed25519:2ZVafxhbwWLgYrfTTvBvkSq9QB7vPEouvAQi78DMC2f9YgmP3wBdMRSNbai9PHFYVRHRmjqiXQufLxzDHCEkd8J7 } }
Mar 25 13:00:18.428 DEBUG network: Consolidated connection with FullPeerInfo { peer_info: PeerInfo { id: ed25519:5BVKQ5zZKtEUM3AHgj6KAerTpuh3CVZHtAbmVfxLbLhc, addr: Some(127.0.0.1:24580), account_id: None }, chain_info: PeerChainInfoV2 { genesis_id: GenesisId { chain_id: "test-chain-T4xnn", hash: `8nF7fhxhmtKAxDjM1ek63WtUK6iHK8TuJe1FwTUiyZaT` }, height: 0, tracked_shards: [], archival: false }, edge_info: EdgeInfo { nonce: 1, signature: ed25519:2ZVafxhbwWLgYrfTTvBvkSq9QB7vPEouvAQi78DMC2f9YgmP3wBdMRSNbai9PHFYVRHRmjqiXQufLxzDHCEkd8J7 } }
Mar 25 13:00:18.430 DEBUG network: ed25519:He7QeRuwizNEhBioYG3u4DZ8jWXyETiyNzFD3MkTjDMf: Peer 127.0.0.1:59922 Inbound started
Mar 25 13:00:18.431 DEBUG network: ed25519:He7QeRuwizNEhBioYG3u4DZ8jWXyETiyNzFD3MkTjDMf: Received handshake Handshake { version: 43, oldest_supported_version: 34, peer_id: ed25519:Eo9W44tRMwcYcoua11yM7Xfr1DjgR4EWQFM3RU27MEX8, target_peer_id: ed25519:He7QeRuwizNEhBioYG3u4DZ8jWXyETiyNzFD3MkTjDMf, listen_port: Some(24579), chain_info: PeerChainInfoV2 { genesis_id: GenesisId { chain_id: "test-chain-T4xnn", hash: `8nF7fhxhmtKAxDjM1ek63WtUK6iHK8TuJe1FwTUiyZaT` }, height: 0, tracked_shards: [], archival: false }, edge_info: EdgeInfo { nonce: 1, signature: ed25519:34VUqMLTphHv5t3wNXbrQnvRHNiSdYKHL41sQSRiRXMGYtBEnFvGhS6yHM4Bn2wk1cfHjz3KiWBWJcvSScd83g1F } }
Mar 25 13:00:18.431 DEBUG network: Consolidated connection with FullPeerInfo { peer_info: PeerInfo { id: ed25519:Eo9W44tRMwcYcoua11yM7Xfr1DjgR4EWQFM3RU27MEX8, addr: Some(127.0.0.1:24579), account_id: None }, chain_info: PeerChainInfoV2 { genesis_id: GenesisId { chain_id: "test-chain-T4xnn", hash: `8nF7fhxhmtKAxDjM1ek63WtUK6iHK8TuJe1FwTUiyZaT` }, height: 0, tracked_shards: [], archival: false }, edge_info: EdgeInfo { nonce: 1, signature: ed25519:34VUqMLTphHv5t3wNXbrQnvRHNiSdYKHL41sQSRiRXMGYtBEnFvGhS6yHM4Bn2wk1cfHjz3KiWBWJcvSScd83g1F } }
Mar 25 13:00:18.434 DEBUG network: ed25519:He7QeRuwizNEhBioYG3u4DZ8jWXyETiyNzFD3MkTjDMf: Peer 127.0.0.1:59924 Inbound started
Mar 25 13:00:18.435 DEBUG network: ed25519:He7QeRuwizNEhBioYG3u4DZ8jWXyETiyNzFD3MkTjDMf: Received handshake Handshake { version: 43, oldest_supported_version: 34, peer_id: ed25519:FXXrTXiKWpXj1R6r5fBvMLpstd8gPyrBq3qMByqKVzKF, target_peer_id: ed25519:He7QeRuwizNEhBioYG3u4DZ8jWXyETiyNzFD3MkTjDMf, listen_port: Some(24578), chain_info: PeerChainInfoV2 { genesis_id: GenesisId { chain_id: "test-chain-T4xnn", hash: `8nF7fhxhmtKAxDjM1ek63WtUK6iHK8TuJe1FwTUiyZaT` }, height: 0, tracked_shards: [], archival: false }, edge_info: EdgeInfo { nonce: 1, signature: ed25519:5ca6A3uzrK3FKGjeAEi7pLNDiBbrzQ1ZPseXF1Rw8rfr3RWYNXPckVDUGGKMZQFE8eKWXeeCws6ZvmeQW8PZEzP6 } }
Mar 25 13:00:18.435 DEBUG network: Consolidated connection with FullPeerInfo { peer_info: PeerInfo { id: ed25519:FXXrTXiKWpXj1R6r5fBvMLpstd8gPyrBq3qMByqKVzKF, addr: Some(127.0.0.1:24578), account_id: None }, chain_info: PeerChainInfoV2 { genesis_id: GenesisId { chain_id: "test-chain-T4xnn", hash: `8nF7fhxhmtKAxDjM1ek63WtUK6iHK8TuJe1FwTUiyZaT` }, height: 0, tracked_shards: [], archival: false }, edge_info: EdgeInfo { nonce: 1, signature: ed25519:5ca6A3uzrK3FKGjeAEi7pLNDiBbrzQ1ZPseXF1Rw8rfr3RWYNXPckVDUGGKMZQFE8eKWXeeCws6ZvmeQW8PZEzP6 } }
Mar 25 13:00:18.452  INFO client: Sync: synced at 0 [8nF7fh], ed25519:Eo9W44tRMwcYcoua11yM7Xfr1DjgR4EWQFM3RU27MEX8, highest height peer: 0
Mar 25 13:00:18.452 DEBUG client: Some("test0") transitions to no sync
Mar 25 13:00:18.452 DEBUG client: Check announce account for test0, last announce time None
Mar 25 13:00:18.452 DEBUG client: Sending announce account for test0
Mar 25 13:00:18.452 DEBUG network: Some("test0") Account announce: AnnounceAccount { account_id: "test0", peer_id: ed25519:He7QeRuwizNEhBioYG3u4DZ8jWXyETiyNzFD3MkTjDMf, epoch_id: EpochId(`11111111111111111111111111111111`), signature: ed25519:5WwBXDt5PKXtkQCfQABWfUSMTDa9TGPaizRd7cr2KW6oLbF9hRFkpGpy99WniEHwXYRPvJEcAvENiqMyCs6SUM6j }
Mar 25 13:00:18.535 DEBUG network: Some("test0") Received new accounts: [AnnounceAccount { account_id: "test1", peer_id: ed25519:FXXrTXiKWpXj1R6r5fBvMLpstd8gPyrBq3qMByqKVzKF, epoch_id: EpochId(`11111111111111111111111111111111`), signature: ed25519:2E11kCw6Whe3WSYDFCEAYmqJvrQBjSHAVSVggmZfRDsM9EmgjPM2BZvaNuxytXsg9C32dbKzZ4X5rdSJSR8XxVD5 }]
Mar 25 13:00:18.539 DEBUG network: Some("test0") Received new accounts: [AnnounceAccount { account_id: "test3", peer_id: ed25519:5BVKQ5zZKtEUM3AHgj6KAerTpuh3CVZHtAbmVfxLbLhc, epoch_id: EpochId(`11111111111111111111111111111111`), signature: ed25519:2HAeqNB9a2pfQkvUUfqKeMkafkJU3GDTQSyV5xNuirQwRUpiW1XnPKH8NvZfMSekfbiHwZTe8ZKtnZHFLJonMvPe }]
Mar 25 13:00:18.541 DEBUG network: Some("test0") Received new accounts: [AnnounceAccount { account_id: "test2", peer_id: ed25519:Eo9W44tRMwcYcoua11yM7Xfr1DjgR4EWQFM3RU27MEX8, epoch_id: EpochId(`11111111111111111111111111111111`), signature: ed25519:5wyZZuDVShPRcSDoDMs6y8SjCjJw1JCjCU6mJH5Xm85YiTQfEs4epqCpimg1Ep5bXt5AQAPkEAMCcMKXc2PzYy23 }]
Mar 25 13:00:18.546 DEBUG client: Sending an approval Endorsement(`8nF7fhxhmtKAxDjM1ek63WtUK6iHK8TuJe1FwTUiyZaT`) from test0 to test3 for 1
Mar 25 13:00:18.546 DEBUG network: Some("test0") Drop signed message to PeerId(ed25519:5BVKQ5zZKtEUM3AHgj6KAerTpuh3CVZHtAbmVfxLbLhc) Reason PeerNotFound. Num known peers: 0 Message Approval(1, test0, Endorsement(`8nF7fhxhmtKAxDjM1ek63WtUK6iHK8TuJe1FwTUiyZaT`))
Mar 25 13:00:19.429 DEBUG network: Received peers from Some(ed25519:5BVKQ5zZKtEUM3AHgj6KAerTpuh3CVZHtAbmVfxLbLhc@127.0.0.1:24580): 1 peers.
Mar 25 13:00:19.429 DEBUG network: Peers request from Some(ed25519:5BVKQ5zZKtEUM3AHgj6KAerTpuh3CVZHtAbmVfxLbLhc@127.0.0.1:24580): sending 3 peers.
Mar 25 13:00:19.433 DEBUG network: Peers request from Some(ed25519:Eo9W44tRMwcYcoua11yM7Xfr1DjgR4EWQFM3RU27MEX8@127.0.0.1:24579): sending 3 peers.
Mar 25 13:00:19.433 DEBUG network: Received peers from Some(ed25519:Eo9W44tRMwcYcoua11yM7Xfr1DjgR4EWQFM3RU27MEX8@127.0.0.1:24579): 1 peers.
Mar 25 13:00:19.436 DEBUG network: Received peers from Some(ed25519:FXXrTXiKWpXj1R6r5fBvMLpstd8gPyrBq3qMByqKVzKF@127.0.0.1:24578): 1 peers.
Mar 25 13:00:19.436 DEBUG network: Peers request from Some(ed25519:FXXrTXiKWpXj1R6r5fBvMLpstd8gPyrBq3qMByqKVzKF@127.0.0.1:24578): sending 3 peers.
Mar 25 13:00:20.568 DEBUG client: Sending an approval Skip(0) from test0 to test1 for 2
Mar 25 13:00:20.660 DEBUG client: Some("test0") Received block FmrbnpS4uBcc7X3tyJ9M1reqqqT5Ydb7aNi6criEof4D <- 8nF7fhxhmtKAxDjM1ek63WtUK6iHK8TuJe1FwTUiyZaT at 2 from ed25519:FXXrTXiKWpXj1R6r5fBvMLpstd8gPyrBq3qMByqKVzKF, requested: false
Mar 25 13:00:20.660 DEBUG chain: Process block header: FmrbnpS4uBcc7X3tyJ9M1reqqqT5Ydb7aNi6criEof4D at 2
Mar 25 13:00:20.661 DEBUG chain: Process block FmrbnpS4uBcc7X3tyJ9M1reqqqT5Ydb7aNi6criEof4D at 2, approvals: 4, me: Some("test0")
Mar 25 13:00:20.661 DEBUG chain: Some("test0") Process block FmrbnpS4uBcc7X3tyJ9M1reqqqT5Ydb7aNi6criEof4D, is_caught_up: false, need_to_start_fetching_state: true
Mar 25 13:00:20.661 DEBUG chain: Header head updated to FmrbnpS4uBcc7X3tyJ9M1reqqqT5Ydb7aNi6criEof4D at 2
Mar 25 13:00:20.661 DEBUG chain: Verifying challenges []
Mar 25 13:00:20.661 DEBUG runtime: block height: 2, is next_block_epoch_start true
Mar 25 13:00:20.661 DEBUG epoch_manager: epoch id: EpochId(`8nF7fhxhmtKAxDjM1ek63WtUK6iHK8TuJe1FwTUiyZaT`), prev_epoch_id: EpochId(`11111111111111111111111111111111`), prev_prev_epoch_id: EpochId(`11111111111111111111111111111111`)
Mar 25 13:00:20.661 DEBUG epoch_manager: prev_prev_stake_change: {"test0": 50000000000000000000000000000000, "test1": 50000000000000000000000000000000, "test2": 50000000000000000000000000000000, "test3": 50000000000000000000000000000000}, prev_stake_change: {"test0": 50000000000000000000000000000000, "test1": 50000000000000000000000000000000, "test2": 50000000000000000000000000000000, "test3": 50000000000000000000000000000000}, stake_change: {"test0": 50000000000000000000000000000000, "test1": 50000000000000000000000000000000, "test2": 50000000000000000000000000000000, "test3": 50000000000000000000000000000000}, slashed: {}
Mar 25 13:00:20.661 DEBUG epoch_manager: stake_info: {"test3": 50000000000000000000000000000000, "test0": 50000000000000000000000000000000, "test2": 50000000000000000000000000000000, "test1": 50000000000000000000000000000000}, validator_reward: {"near": 0}
Mar 25 13:00:20.661 DEBUG runtime: account test2 stake 50000000000000000000000000000000 max_of_stakes: 50000000000000000000000000000000
Mar 25 13:00:20.661 DEBUG runtime: account test2 return stake 0
Mar 25 13:00:20.661 DEBUG runtime: add validator proposals at block height 2 []
Mar 25 13:00:20.661 DEBUG chain: Head updated to FmrbnpS4uBcc7X3tyJ9M1reqqqT5Ydb7aNi6criEof4D at 2
Mar 25 13:00:20.662 DEBUG chain: Downloading state for block FmrbnpS4uBcc7X3tyJ9M1reqqqT5Ydb7aNi6criEof4D
Mar 25 13:00:20.662 DEBUG chain: Downloading state for [], I'm Some("test0")
Mar 25 13:00:20.662 DEBUG chain: Check orphans: from FmrbnpS4uBcc7X3tyJ9M1reqqqT5Ydb7aNi6criEof4D, # orphans 0
Mar 25 13:00:20.662 DEBUG client: Producing chunk at height 3 for shard 0, I'm test0
Mar 25 13:00:20.662 DEBUG client: Produced chunk at height 3 for shard 0 with 0 txs and 0 receipts, I'm test0, chunk_hash: A4sZ13hHCGFo9wV7W39RzZb42zhck3aw8T6mMGPRnYUr
Mar 25 13:00:20.662 DEBUG chunks: Reconstructed and decoded chunk A4sZ13hHCGFo9wV7W39RzZb42zhck3aw8T6mMGPRnYUr, encoded length was 8, num txs: 0, I'm Some("test0")
Mar 25 13:00:20.668 DEBUG client: Catchup me: Some("test0"): sync_hash: `FmrbnpS4uBcc7X3tyJ9M1reqqqT5Ydb7aNi6criEof4D`, sync_info: {}
Mar 25 13:00:20.668 DEBUG chain: Verifying challenges []
Mar 25 13:00:20.668 DEBUG chain: Catching up: removing prev=`FmrbnpS4uBcc7X3tyJ9M1reqqqT5Ydb7aNi6criEof4D` from the queue. I'm Some("test0")
Mar 25 13:00:20.668 DEBUG chain: Check orphans: from FmrbnpS4uBcc7X3tyJ9M1reqqqT5Ydb7aNi6criEof4D, # orphans 0
Mar 25 13:00:20.670 DEBUG client: Sending an approval Endorsement(`FmrbnpS4uBcc7X3tyJ9M1reqqqT5Ydb7aNi6criEof4D`) from test0 to test2 for 3
Mar 25 13:00:20.856 DEBUG client: Some("test0") Received block EoBNntvm6mpkZvWqKPHS4NwEfnLaw8F5nW8kyTRxmoe7 <- FmrbnpS4uBcc7X3tyJ9M1reqqqT5Ydb7aNi6criEof4D at 3 from ed25519:Eo9W44tRMwcYcoua11yM7Xfr1DjgR4EWQFM3RU27MEX8, requested: false
Mar 25 13:00:20.856 DEBUG chain: Process block header: EoBNntvm6mpkZvWqKPHS4NwEfnLaw8F5nW8kyTRxmoe7 at 3
Mar 25 13:00:20.857 DEBUG chain: Process block EoBNntvm6mpkZvWqKPHS4NwEfnLaw8F5nW8kyTRxmoe7 at 3, approvals: 4, me: Some("test0")
Mar 25 13:00:20.857 DEBUG chain: Some("test0") Process block EoBNntvm6mpkZvWqKPHS4NwEfnLaw8F5nW8kyTRxmoe7, is_caught_up: true, need_to_start_fetching_state: false
Mar 25 13:00:20.857 DEBUG chain: Header head updated to EoBNntvm6mpkZvWqKPHS4NwEfnLaw8F5nW8kyTRxmoe7 at 3
Mar 25 13:00:20.858 DEBUG chain: Verifying challenges []
Mar 25 13:00:20.858 DEBUG runtime: block height: 3, is next_block_epoch_start false
Mar 25 13:00:20.858 DEBUG chain: Verifying challenges []
Mar 25 13:00:20.858 DEBUG runtime: add validator proposals at block height 3 []
Mar 25 13:00:20.858 DEBUG chain: Head updated to EoBNntvm6mpkZvWqKPHS4NwEfnLaw8F5nW8kyTRxmoe7 at 3
Mar 25 13:00:20.858 DEBUG chain: Check orphans: from EoBNntvm6mpkZvWqKPHS4NwEfnLaw8F5nW8kyTRxmoe7, # orphans 0
Mar 25 13:00:20.858 DEBUG client: Producing chunk at height 4 for shard 0, I'm test0
Mar 25 13:00:20.858 DEBUG runtime: Transaction filtering results 0 valid out of 0 pulled from the pool
Mar 25 13:00:20.858 DEBUG client: Produced chunk at height 4 for shard 0 with 0 txs and 0 receipts, I'm test0, chunk_hash: 8QwnvtVJJkkFemef3AMHbsCZw1BnZeWnTLkZXkNDGhUm
Mar 25 13:00:20.858 DEBUG chunks: Reconstructed and decoded chunk 8QwnvtVJJkkFemef3AMHbsCZw1BnZeWnTLkZXkNDGhUm, encoded length was 8, num txs: 0, I'm Some("test0")
Mar 25 13:00:21.377 DEBUG client: "test0" Producing block at height 4, parent 3 @ EoBNnt
Mar 25 13:00:21.377 DEBUG chain: Process block FREK3eApdS91hnGwe8o2fUGRHrnaPkLGZAZnqr6L5ssr at 4, approvals: 4, me: Some("test0")
Mar 25 13:00:21.377 DEBUG chain: Some("test0") Process block FREK3eApdS91hnGwe8o2fUGRHrnaPkLGZAZnqr6L5ssr, is_caught_up: true, need_to_start_fetching_state: false
Mar 25 13:00:21.377 DEBUG chain: Header head updated to FREK3eApdS91hnGwe8o2fUGRHrnaPkLGZAZnqr6L5ssr at 4
Mar 25 13:00:21.378 DEBUG chain: Verifying challenges []
Mar 25 13:00:21.378 DEBUG runtime: block height: 4, is next_block_epoch_start false
Mar 25 13:00:21.378 DEBUG chain: Verifying challenges []
Mar 25 13:00:21.378 DEBUG runtime: add validator proposals at block height 4 []
Mar 25 13:00:21.378 DEBUG chain: Head updated to FREK3eApdS91hnGwe8o2fUGRHrnaPkLGZAZnqr6L5ssr at 4
Mar 25 13:00:21.378 DEBUG chain: Check orphans: from FREK3eApdS91hnGwe8o2fUGRHrnaPkLGZAZnqr6L5ssr, # orphans 0
Mar 25 13:00:21.378 DEBUG client: Producing chunk at height 5 for shard 0, I'm test0
Mar 25 13:00:21.378 DEBUG runtime: Transaction filtering results 0 valid out of 0 pulled from the pool
Mar 25 13:00:21.378 DEBUG client: Produced chunk at height 5 for shard 0 with 0 txs and 0 receipts, I'm test0, chunk_hash: Hv9C4wRWCsSe6jZen9WXW9t4Tek5XRdMfLbJTHouTCcq
Mar 25 13:00:21.378 DEBUG chunks: Reconstructed and decoded chunk Hv9C4wRWCsSe6jZen9WXW9t4Tek5XRdMfLbJTHouTCcq, encoded length was 8, num txs: 0, I'm Some("test0")
Mar 25 13:00:21.586 DEBUG client: I'm Some("test0"), routing a transaction SignedTransaction { transaction: Transaction { signer_id: "test0", public_key: ed25519:He7QeRuwizNEhBioYG3u4DZ8jWXyETiyNzFD3MkTjDMf, nonce: 10, receiver_id: "test0", block_hash: `FREK3eApdS91hnGwe8o2fUGRHrnaPkLGZAZnqr6L5ssr`, actions: [DeployContract(DeployContractAction { code: (50498)[0, 97, … 116, 101] })] }, signature: ed25519:u6k2wxVMh662iyURVUSuAPQBd3Uy5tFAB3zCAKW3CAG1ZtrHrWyTF5gEEXP1FmsSFxeu5qJ7YYsGhJ4V1XBiLCR, hash: `JA9BSnuVyy3jvN6UQmAmVMQEfQtEjcJn3wjboqyTQz5o`, size: 50598 } to test3, shard_id = 1
Mar 25 13:00:21.881 DEBUG client: Sending an approval Endorsement(`FREK3eApdS91hnGwe8o2fUGRHrnaPkLGZAZnqr6L5ssr`) from test0 to test3 for 5
Mar 25 13:00:22.067 DEBUG client: Some("test0") Received block Bg6kzbJkDsqsj4GFuzqdJRDvgrAkTrnES9Zpr49jczHV <- FREK3eApdS91hnGwe8o2fUGRHrnaPkLGZAZnqr6L5ssr at 5 from ed25519:5BVKQ5zZKtEUM3AHgj6KAerTpuh3CVZHtAbmVfxLbLhc, requested: false
Mar 25 13:00:22.067 DEBUG chain: Process block header: Bg6kzbJkDsqsj4GFuzqdJRDvgrAkTrnES9Zpr49jczHV at 5
Mar 25 13:00:22.067 DEBUG chain: Process block Bg6kzbJkDsqsj4GFuzqdJRDvgrAkTrnES9Zpr49jczHV at 5, approvals: 4, me: Some("test0")
Mar 25 13:00:22.067 DEBUG chain: Some("test0") Process block Bg6kzbJkDsqsj4GFuzqdJRDvgrAkTrnES9Zpr49jczHV, is_caught_up: true, need_to_start_fetching_state: false
Mar 25 13:00:22.068 DEBUG chain: Header head updated to Bg6kzbJkDsqsj4GFuzqdJRDvgrAkTrnES9Zpr49jczHV at 5
Mar 25 13:00:22.068 DEBUG chain: Verifying challenges []
Mar 25 13:00:22.068 DEBUG runtime: block height: 5, is next_block_epoch_start false
Mar 25 13:00:22.068 DEBUG chain: Verifying challenges []
Mar 25 13:00:22.068 DEBUG runtime: add validator proposals at block height 5 []
Mar 25 13:00:22.068 DEBUG chain: Head updated to Bg6kzbJkDsqsj4GFuzqdJRDvgrAkTrnES9Zpr49jczHV at 5
Mar 25 13:00:22.068 DEBUG chain: Check orphans: from Bg6kzbJkDsqsj4GFuzqdJRDvgrAkTrnES9Zpr49jczHV, # orphans 0
Mar 25 13:00:22.068 DEBUG client: Producing chunk at height 6 for shard 0, I'm test0
Mar 25 13:00:22.068 DEBUG runtime: Transaction filtering results 0 valid out of 0 pulled from the pool
Mar 25 13:00:22.068 DEBUG client: Produced chunk at height 6 for shard 0 with 0 txs and 0 receipts, I'm test0, chunk_hash: GR4nHr37dicenys2RAKGQzpXMzKAU2PSFof1k1gz8XAR
Mar 25 13:00:22.068 DEBUG chunks: Reconstructed and decoded chunk GR4nHr37dicenys2RAKGQzpXMzKAU2PSFof1k1gz8XAR, encoded length was 8, num txs: 0, I'm Some("test0")
Mar 25 13:00:22.489 DEBUG client: Sending an approval Endorsement(`Bg6kzbJkDsqsj4GFuzqdJRDvgrAkTrnES9Zpr49jczHV`) from test0 to test1 for 6
Mar 25 13:00:22.583 DEBUG client: Some("test0") Received block EdGsAA9iTqy9gvpbrANpxfzYahc97mU1yzdfMnER5rd1 <- Bg6kzbJkDsqsj4GFuzqdJRDvgrAkTrnES9Zpr49jczHV at 6 from ed25519:FXXrTXiKWpXj1R6r5fBvMLpstd8gPyrBq3qMByqKVzKF, requested: false
Mar 25 13:00:22.583 DEBUG chain: Process block header: EdGsAA9iTqy9gvpbrANpxfzYahc97mU1yzdfMnER5rd1 at 6
Mar 25 13:00:22.583 DEBUG chain: Process block EdGsAA9iTqy9gvpbrANpxfzYahc97mU1yzdfMnER5rd1 at 6, approvals: 4, me: Some("test0")
Mar 25 13:00:22.583 DEBUG chain: Some("test0") Process block EdGsAA9iTqy9gvpbrANpxfzYahc97mU1yzdfMnER5rd1, is_caught_up: true, need_to_start_fetching_state: false
Mar 25 13:00:22.584 DEBUG chain: Header head updated to EdGsAA9iTqy9gvpbrANpxfzYahc97mU1yzdfMnER5rd1 at 6
Mar 25 13:00:22.584 DEBUG chain: Verifying challenges []
Mar 25 13:00:22.584 DEBUG runtime: block height: 6, is next_block_epoch_start false
Mar 25 13:00:22.584 DEBUG chain: Verifying challenges []
Mar 25 13:00:22.584 DEBUG runtime: add validator proposals at block height 6 []
Mar 25 13:00:22.584 DEBUG chain: Head updated to EdGsAA9iTqy9gvpbrANpxfzYahc97mU1yzdfMnER5rd1 at 6
Mar 25 13:00:22.584 DEBUG chain: Check orphans: from EdGsAA9iTqy9gvpbrANpxfzYahc97mU1yzdfMnER5rd1, # orphans 0
Mar 25 13:00:22.584 DEBUG client: Producing chunk at height 7 for shard 0, I'm test0
Mar 25 13:00:22.584 DEBUG runtime: Transaction filtering results 0 valid out of 0 pulled from the pool
Mar 25 13:00:22.584 DEBUG client: Produced chunk at height 7 for shard 0 with 0 txs and 0 receipts, I'm test0, chunk_hash: HKTruWdXmAa64vxAEDjQ1EUaRJ4YJuB9vJUJ7kGAE566
Mar 25 13:00:22.584 DEBUG chunks: Reconstructed and decoded chunk HKTruWdXmAa64vxAEDjQ1EUaRJ4YJuB9vJUJ7kGAE566, encoded length was 8, num txs: 0, I'm Some("test0")
Mar 25 13:00:23.095 DEBUG client: Sending an approval Endorsement(`EdGsAA9iTqy9gvpbrANpxfzYahc97mU1yzdfMnER5rd1`) from test0 to test2 for 7
Mar 25 13:00:23.184 DEBUG client: Some("test0") Received block 9H5ZQRKNKQ79xRQb1tAFEXMMtYpmkyZTQmmqoxdr7Q9c <- EdGsAA9iTqy9gvpbrANpxfzYahc97mU1yzdfMnER5rd1 at 7 from ed25519:Eo9W44tRMwcYcoua11yM7Xfr1DjgR4EWQFM3RU27MEX8, requested: false
Mar 25 13:00:23.184 DEBUG chain: Process block header: 9H5ZQRKNKQ79xRQb1tAFEXMMtYpmkyZTQmmqoxdr7Q9c at 7
Mar 25 13:00:23.185 DEBUG chain: Process block 9H5ZQRKNKQ79xRQb1tAFEXMMtYpmkyZTQmmqoxdr7Q9c at 7, approvals: 3, me: Some("test0")
Mar 25 13:00:23.185 DEBUG chain: Some("test0") Process block 9H5ZQRKNKQ79xRQb1tAFEXMMtYpmkyZTQmmqoxdr7Q9c, is_caught_up: true, need_to_start_fetching_state: false
Mar 25 13:00:23.185 DEBUG chain: Header head updated to 9H5ZQRKNKQ79xRQb1tAFEXMMtYpmkyZTQmmqoxdr7Q9c at 7
Mar 25 13:00:23.186 DEBUG chain: Verifying challenges []
Mar 25 13:00:23.186 DEBUG runtime: block height: 7, is next_block_epoch_start false
Mar 25 13:00:23.186 DEBUG chain: Verifying challenges []
Mar 25 13:00:23.186 DEBUG runtime: add validator proposals at block height 7 []
Mar 25 13:00:23.186 DEBUG chain: Head updated to 9H5ZQRKNKQ79xRQb1tAFEXMMtYpmkyZTQmmqoxdr7Q9c at 7
Mar 25 13:00:23.186 DEBUG chain: Check orphans: from 9H5ZQRKNKQ79xRQb1tAFEXMMtYpmkyZTQmmqoxdr7Q9c, # orphans 0
Mar 25 13:00:23.186 DEBUG client: Producing chunk at height 8 for shard 0, I'm test0
Mar 25 13:00:23.186 DEBUG runtime: Transaction filtering results 0 valid out of 0 pulled from the pool
Mar 25 13:00:23.186 DEBUG client: Produced chunk at height 8 for shard 0 with 0 txs and 0 receipts, I'm test0, chunk_hash: FBibnmeMwKNvPctntyfyZrVxDmWLP1eDbeZhfqmDvsDQ
Mar 25 13:00:23.186 DEBUG chunks: Reconstructed and decoded chunk FBibnmeMwKNvPctntyfyZrVxDmWLP1eDbeZhfqmDvsDQ, encoded length was 8, num txs: 0, I'm Some("test0")
Mar 25 13:00:23.802 DEBUG client: "test0" Producing block at height 8, parent 7 @ 9H5ZQR
Mar 25 13:00:23.802 DEBUG chain: Process block JDpfF6oMtr5uShGhu4ejsaBjdvfFZaWAPFGfr7iQ613B at 8, approvals: 4, me: Some("test0")
Mar 25 13:00:23.802 DEBUG chain: Some("test0") Process block JDpfF6oMtr5uShGhu4ejsaBjdvfFZaWAPFGfr7iQ613B, is_caught_up: true, need_to_start_fetching_state: false
Mar 25 13:00:23.802 DEBUG chain: Header head updated to JDpfF6oMtr5uShGhu4ejsaBjdvfFZaWAPFGfr7iQ613B at 8
Mar 25 13:00:23.803 DEBUG chain: Verifying challenges []
Mar 25 13:00:23.803 DEBUG runtime: block height: 8, is next_block_epoch_start false
Mar 25 13:00:23.803 DEBUG chain: Verifying challenges []
Mar 25 13:00:23.803 DEBUG runtime: add validator proposals at block height 8 []
Mar 25 13:00:23.803 DEBUG chain: Head updated to JDpfF6oMtr5uShGhu4ejsaBjdvfFZaWAPFGfr7iQ613B at 8
Mar 25 13:00:23.803 DEBUG chain: Check orphans: from JDpfF6oMtr5uShGhu4ejsaBjdvfFZaWAPFGfr7iQ613B, # orphans 0
Mar 25 13:00:23.803 DEBUG client: Producing chunk at height 9 for shard 0, I'm test0
Mar 25 13:00:23.803 DEBUG runtime: Transaction filtering results 0 valid out of 0 pulled from the pool
Mar 25 13:00:23.803 DEBUG client: Produced chunk at height 9 for shard 0 with 0 txs and 0 receipts, I'm test0, chunk_hash: 7JSBdiJrX2ZbcYwmY8CtcFmZdbiY8vP3Cij6CZQpurcT
Mar 25 13:00:23.803 DEBUG chunks: Reconstructed and decoded chunk 7JSBdiJrX2ZbcYwmY8CtcFmZdbiY8vP3Cij6CZQpurcT, encoded length was 8, num txs: 0, I'm Some("test0")
Mar 25 13:00:24.307 DEBUG client: Sending an approval Endorsement(`JDpfF6oMtr5uShGhu4ejsaBjdvfFZaWAPFGfr7iQ613B`) from test0 to test3 for 9
Mar 25 13:00:24.392 DEBUG client: Some("test0") Received block 96XBFXYwYdbv84QBzsTwmPe4Rg4LAZhZpL5PbZgFeXjr <- JDpfF6oMtr5uShGhu4ejsaBjdvfFZaWAPFGfr7iQ613B at 9 from ed25519:5BVKQ5zZKtEUM3AHgj6KAerTpuh3CVZHtAbmVfxLbLhc, requested: false
Mar 25 13:00:24.392 DEBUG chain: Process block header: 96XBFXYwYdbv84QBzsTwmPe4Rg4LAZhZpL5PbZgFeXjr at 9
Mar 25 13:00:24.392 DEBUG chain: Process block 96XBFXYwYdbv84QBzsTwmPe4Rg4LAZhZpL5PbZgFeXjr at 9, approvals: 3, me: Some("test0")
Mar 25 13:00:24.392 DEBUG chain: Some("test0") Process block 96XBFXYwYdbv84QBzsTwmPe4Rg4LAZhZpL5PbZgFeXjr, is_caught_up: true, need_to_start_fetching_state: false
Mar 25 13:00:24.393 DEBUG chain: Header head updated to 96XBFXYwYdbv84QBzsTwmPe4Rg4LAZhZpL5PbZgFeXjr at 9
Mar 25 13:00:24.393 DEBUG chain: Verifying challenges []
Mar 25 13:00:24.393 DEBUG runtime: block height: 9, is next_block_epoch_start false
Mar 25 13:00:24.393 DEBUG chain: Verifying challenges []
Mar 25 13:00:24.393 DEBUG runtime: add validator proposals at block height 9 []
Mar 25 13:00:24.393 DEBUG chain: Head updated to 96XBFXYwYdbv84QBzsTwmPe4Rg4LAZhZpL5PbZgFeXjr at 9
Mar 25 13:00:24.393 DEBUG chain: Check orphans: from 96XBFXYwYdbv84QBzsTwmPe4Rg4LAZhZpL5PbZgFeXjr, # orphans 0
Mar 25 13:00:24.393 DEBUG client: Producing chunk at height 10 for shard 0, I'm test0
Mar 25 13:00:24.393 DEBUG runtime: Transaction filtering results 0 valid out of 0 pulled from the pool
Mar 25 13:00:24.393 DEBUG client: Produced chunk at height 10 for shard 0 with 0 txs and 0 receipts, I'm test0, chunk_hash: 6AadaBvcwfjLGJXq5h2LNvwFcWTuXsUe1ME9trsKBh6A
Mar 25 13:00:24.393 DEBUG chunks: Reconstructed and decoded chunk 6AadaBvcwfjLGJXq5h2LNvwFcWTuXsUe1ME9trsKBh6A, encoded length was 8, num txs: 0, I'm Some("test0")
Mar 25 13:00:24.706  INFO stats: #       9 96XBFXYwYdbv84QBzsTwmPe4Rg4LAZhZpL5PbZgFeXjr V/4  3/3/40 peers ⬇ 73 B/s ⬆ 67 B/s 0.80 bps 127.36 Ggas/s CPU: 0%, Mem: 0 B
Mar 25 13:00:24.914 DEBUG client: Sending an approval Endorsement(`96XBFXYwYdbv84QBzsTwmPe4Rg4LAZhZpL5PbZgFeXjr`) from test0 to test1 for 10
Mar 25 13:00:25.008 DEBUG client: Some("test0") Received block J6QDUXBjEcZ7uFdvARkbm6iy9m34DZa2GP1Uo9qa3wbA <- 96XBFXYwYdbv84QBzsTwmPe4Rg4LAZhZpL5PbZgFeXjr at 10 from ed25519:FXXrTXiKWpXj1R6r5fBvMLpstd8gPyrBq3qMByqKVzKF, requested: false
Mar 25 13:00:25.008 DEBUG chain: Process block header: J6QDUXBjEcZ7uFdvARkbm6iy9m34DZa2GP1Uo9qa3wbA at 10
Mar 25 13:00:25.009 DEBUG chain: Process block J6QDUXBjEcZ7uFdvARkbm6iy9m34DZa2GP1Uo9qa3wbA at 10, approvals: 4, me: Some("test0")
Mar 25 13:00:25.009 DEBUG chain: Some("test0") Process block J6QDUXBjEcZ7uFdvARkbm6iy9m34DZa2GP1Uo9qa3wbA, is_caught_up: true, need_to_start_fetching_state: false
Mar 25 13:00:25.009 DEBUG chain: Header head updated to J6QDUXBjEcZ7uFdvARkbm6iy9m34DZa2GP1Uo9qa3wbA at 10
Mar 25 13:00:25.010 DEBUG chain: Verifying challenges []
Mar 25 13:00:25.010 DEBUG runtime: block height: 10, is next_block_epoch_start false
Mar 25 13:00:25.010 DEBUG chain: Verifying challenges []
Mar 25 13:00:25.010 DEBUG runtime: add validator proposals at block height 10 []
Mar 25 13:00:25.010 DEBUG chain: Head updated to J6QDUXBjEcZ7uFdvARkbm6iy9m34DZa2GP1Uo9qa3wbA at 10
Mar 25 13:00:25.010 DEBUG chain: Check orphans: from J6QDUXBjEcZ7uFdvARkbm6iy9m34DZa2GP1Uo9qa3wbA, # orphans 0
Mar 25 13:00:25.010 DEBUG client: Producing chunk at height 11 for shard 0, I'm test0
Mar 25 13:00:25.010 DEBUG runtime: Transaction filtering results 0 valid out of 0 pulled from the pool
Mar 25 13:00:25.010 DEBUG client: Produced chunk at height 11 for shard 0 with 0 txs and 0 receipts, I'm test0, chunk_hash: Avgd2WqfAatUSDGSQUYXPaCMVjxYTmn1EvgxDP1d2FSu
Mar 25 13:00:25.010 DEBUG chunks: Reconstructed and decoded chunk Avgd2WqfAatUSDGSQUYXPaCMVjxYTmn1EvgxDP1d2FSu, encoded length was 8, num txs: 0, I'm Some("test0")
Mar 25 13:00:25.519 DEBUG client: Sending an approval Endorsement(`J6QDUXBjEcZ7uFdvARkbm6iy9m34DZa2GP1Uo9qa3wbA`) from test0 to test2 for 11
Mar 25 13:00:25.608 DEBUG client: Some("test0") Received block 2Bu42LaW6A3LkLtDFjdbgs4pEffhtyLqqfAzTMVd669y <- J6QDUXBjEcZ7uFdvARkbm6iy9m34DZa2GP1Uo9qa3wbA at 11 from ed25519:Eo9W44tRMwcYcoua11yM7Xfr1DjgR4EWQFM3RU27MEX8, requested: false
Mar 25 13:00:25.609 DEBUG chain: Process block header: 2Bu42LaW6A3LkLtDFjdbgs4pEffhtyLqqfAzTMVd669y at 11
Mar 25 13:00:25.609 DEBUG chain: Process block 2Bu42LaW6A3LkLtDFjdbgs4pEffhtyLqqfAzTMVd669y at 11, approvals: 4, me: Some("test0")
Mar 25 13:00:25.609 DEBUG chain: Some("test0") Process block 2Bu42LaW6A3LkLtDFjdbgs4pEffhtyLqqfAzTMVd669y, is_caught_up: true, need_to_start_fetching_state: false
Mar 25 13:00:25.610 DEBUG chain: Header head updated to 2Bu42LaW6A3LkLtDFjdbgs4pEffhtyLqqfAzTMVd669y at 11
Mar 25 13:00:25.610 DEBUG chain: Verifying challenges []
Mar 25 13:00:25.610 DEBUG runtime: block height: 11, is next_block_epoch_start false
Mar 25 13:00:25.610 DEBUG chain: Verifying challenges []
Mar 25 13:00:25.610 DEBUG runtime: add validator proposals at block height 11 []
Mar 25 13:00:25.610 DEBUG epoch_manager: All proposals: [], Kickouts: {"test3": NotEnoughBlocks { produced: 2, expected: 3 }}, Block Tracker: {1: ValidatorStats { produced: 3, expected: 3 }, 2: ValidatorStats { produced: 3, expected: 3 }, 0: ValidatorStats { produced: 2, expected: 2 }, 3: ValidatorStats { produced: 2, expected: 3 }}, Shard Tracker: {0: {0: ValidatorStats { produced: 9, expected: 10 }}, 3: {2: ValidatorStats { produced: 9, expected: 10 }}, 1: {3: ValidatorStats { produced: 9, expected: 10 }}, 2: {1: ValidatorStats { produced: 9, expected: 10 }}}
Mar 25 13:00:25.610 DEBUG chain: Head updated to 2Bu42LaW6A3LkLtDFjdbgs4pEffhtyLqqfAzTMVd669y at 11
Mar 25 13:00:25.610 DEBUG chain: Check orphans: from 2Bu42LaW6A3LkLtDFjdbgs4pEffhtyLqqfAzTMVd669y, # orphans 0
Mar 25 13:00:25.610 DEBUG client: Producing chunk at height 12 for shard 0, I'm test0
Mar 25 13:00:25.610 DEBUG runtime: Transaction filtering results 0 valid out of 0 pulled from the pool
Mar 25 13:00:25.610 DEBUG client: Produced chunk at height 12 for shard 0 with 0 txs and 0 receipts, I'm test0, chunk_hash: 9fQRvECXQe6VzNbZtLF4NCbu3ZELEJBMGbK63LbdDdQj
Mar 25 13:00:25.610 DEBUG chunks: Reconstructed and decoded chunk 9fQRvECXQe6VzNbZtLF4NCbu3ZELEJBMGbK63LbdDdQj, encoded length was 8, num txs: 0, I'm Some("test0")
Mar 25 13:00:25.611 DEBUG chunks: Reconstructed and decoded chunk 8zGL9HMeoBpeibEEM1yCt5dSKbj2M3Rhp9hYRgB1e9JQ, encoded length was 8, num txs: 0, I'm Some("test0")
Mar 25 13:00:25.693 DEBUG chunks: Reconstructed and decoded chunk 4e4oh2WdmGcKeZ5Mabamndbo1x41cQipdNUoLsmnaTTE, encoded length was 1398, num txs: 0, I'm Some("test0")
Mar 25 13:00:26.125 DEBUG client: "test0" Producing block at height 12, parent 11 @ 2Bu42L
Mar 25 13:00:26.125 DEBUG chain: Process block FfZjASXkfgT58WoZ6cLy7BEPEQswJF1xYzhtdHR5pQfc at 12, approvals: 3, me: Some("test0")
Mar 25 13:00:26.125 DEBUG chain: Some("test0") Process block FfZjASXkfgT58WoZ6cLy7BEPEQswJF1xYzhtdHR5pQfc, is_caught_up: false, need_to_start_fetching_state: true
Mar 25 13:00:26.125 DEBUG chain: Header head updated to FfZjASXkfgT58WoZ6cLy7BEPEQswJF1xYzhtdHR5pQfc at 12
Mar 25 13:00:26.126 DEBUG chain: Verifying challenges []
Mar 25 13:00:26.126 DEBUG runtime: block height: 12, is next_block_epoch_start true
Mar 25 13:00:26.126 DEBUG epoch_manager: epoch id: EpochId(`2Bu42LaW6A3LkLtDFjdbgs4pEffhtyLqqfAzTMVd669y`), prev_epoch_id: EpochId(`8nF7fhxhmtKAxDjM1ek63WtUK6iHK8TuJe1FwTUiyZaT`), prev_prev_epoch_id: EpochId(`11111111111111111111111111111111`)
Mar 25 13:00:26.126 DEBUG epoch_manager: prev_prev_stake_change: {"test0": 50000000000000000000000000000000, "test1": 50000000000000000000000000000000, "test2": 50000000000000000000000000000000, "test3": 50000000000000000000000000000000}, prev_stake_change: {"test0": 50000000000000000000000000000000, "test1": 50000000000000000000000000000000, "test2": 50000000000000000000000000000000, "test3": 50000000000000000000000000000000}, stake_change: {"test0": 50000009909309994923908913331185, "test1": 50000009909309994923908913331185, "test2": 50000009909309994923908913331185, "test3": 0}, slashed: {}
Mar 25 13:00:26.126 DEBUG epoch_manager: stake_info: {"test3": 50000000000000000000000000000000, "test2": 50000009909309994923908913331185, "test1": 50000009909309994923908913331185, "test0": 50000009909309994923908913331185}, validator_reward: {"near": 7927447995939127130664947, "test1": 9909309994923908913331185, "test0": 9909309994923908913331185, "test2": 9909309994923908913331185}
Mar 25 13:00:26.126 DEBUG runtime: account test2 adding reward 9909309994923908913331185 to stake 50000000000000000000000000000000
Mar 25 13:00:26.126 DEBUG runtime: account test2 stake 50000009909309994923908913331185 max_of_stakes: 50000009909309994923908913331185
Mar 25 13:00:26.126 DEBUG runtime: account test2 return stake 0
Mar 25 13:00:26.126 DEBUG runtime: add validator proposals at block height 12 []
Mar 25 13:00:26.126 DEBUG chain: Head updated to FfZjASXkfgT58WoZ6cLy7BEPEQswJF1xYzhtdHR5pQfc at 12
Mar 25 13:00:26.126 DEBUG chain: Downloading state for block FfZjASXkfgT58WoZ6cLy7BEPEQswJF1xYzhtdHR5pQfc
Mar 25 13:00:26.126 DEBUG chain: Downloading state for [1, 2], I'm Some("test0")
Mar 25 13:00:26.126 DEBUG chain: Check orphans: from FfZjASXkfgT58WoZ6cLy7BEPEQswJF1xYzhtdHR5pQfc, # orphans 0
Mar 25 13:00:26.126 DEBUG client: Producing chunk at height 13 for shard 0, I'm test0
Mar 25 13:00:26.126 DEBUG runtime: Transaction filtering results 0 valid out of 0 pulled from the pool
Mar 25 13:00:26.126 DEBUG client: Produced chunk at height 13 for shard 0 with 0 txs and 0 receipts, I'm test0, chunk_hash: 6TBm8NwPkZGeqkyp1q3bq4yxtsfiwPhdiaSSTjsjHDuR
Mar 25 13:00:26.127 DEBUG chunks: Reconstructed and decoded chunk 6TBm8NwPkZGeqkyp1q3bq4yxtsfiwPhdiaSSTjsjHDuR, encoded length was 8, num txs: 0, I'm Some("test0")
Mar 25 13:00:26.128 DEBUG chunks: Reconstructed and decoded chunk 8najn7TfFNwUj3XeExjHZKSUeQwgqZiUn3r84hXyaCQv, encoded length was 8, num txs: 0, I'm Some("test0")
Mar 25 13:00:26.128 DEBUG chunks: Reconstructed and decoded chunk ApCnhf358sPLj7AHrKsidd7MHv1LYL7k2qYbiEqAMUDC, encoded length was 8, num txs: 0, I'm Some("test0")
Mar 25 13:00:26.128 DEBUG client: Catchup me: Some("test0"): sync_hash: `FfZjASXkfgT58WoZ6cLy7BEPEQswJF1xYzhtdHR5pQfc`, sync_info: {}
Mar 25 13:00:26.229 DEBUG client: Catchup me: Some("test0"): sync_hash: `FfZjASXkfgT58WoZ6cLy7BEPEQswJF1xYzhtdHR5pQfc`, sync_info: {1: ShardSyncDownload { downloads: [DownloadStatus { start_time: 2021-03-25T10:00:26.128510397Z, prev_update_time: 2021-03-25T10:00:26.128510397Z, run_me: false, error: false, done: true, state_requests_count: 1, last_target: Some(AccountId("test3")) }], status: StateDownloadHeader }, 2: ShardSyncDownload { downloads: [DownloadStatus { start_time: 2021-03-25T10:00:26.128510397Z, prev_update_time: 2021-03-25T10:00:26.128510397Z, run_me: false, error: false, done: true, state_requests_count: 1, last_target: Some(AccountId("test1")) }], status: StateDownloadHeader }}
[core/store/src/db.rs:732] path = "/home/matklad/.near/test0/data"
Mar 25 13:00:26.331 DEBUG client: Catchup me: Some("test0"): sync_hash: `FfZjASXkfgT58WoZ6cLy7BEPEQswJF1xYzhtdHR5pQfc`, sync_info: {1: ShardSyncDownload { downloads: [DownloadStatus { start_time: 2021-03-25T10:00:26.229403665Z, prev_update_time: 2021-03-25T10:00:26.229403665Z, run_me: false, error: false, done: true, state_requests_count: 1, last_target: Some(AccountId("test3")) }, DownloadStatus { start_time: 2021-03-25T10:00:26.229403665Z, prev_update_time: 2021-03-25T10:00:26.229403665Z, run_me: false, error: false, done: true, state_requests_count: 1, last_target: Some(AccountId("test3")) }, DownloadStatus { start_time: 2021-03-25T10:00:26.229403665Z, prev_update_time: 2021-03-25T10:00:26.229403665Z, run_me: false, error: false, done: true, state_requests_count: 1, last_target: Some(AccountId("test3")) }], status: StateDownloadParts }, 2: ShardSyncDownload { downloads: [DownloadStatus { start_time: 2021-03-25T10:00:26.229403665Z, prev_update_time: 2021-03-25T10:00:26.229403665Z, run_me: false, error: false, done: true, state_requests_count: 1, last_target: Some(AccountId("test1")) }, DownloadStatus { start_time: 2021-03-25T10:00:26.229403665Z, prev_update_time: 2021-03-25T10:00:26.229403665Z, run_me: false, error: false, done: true, state_requests_count: 1, last_target: Some(AccountId("test1")) }, DownloadStatus { start_time: 2021-03-25T10:00:26.229403665Z, prev_update_time: 2021-03-25T10:00:26.229403665Z, run_me: false, error: false, done: true, state_requests_count: 1, last_target: Some(AccountId("test1")) }], status: StateDownloadParts }}
Mar 25 13:00:26.414 DEBUG client: Some("test0") Received block 55jqTxo3s4tjEkqfL67x9P1wqMEuPDgyLzNfjP29agYB <- FfZjASXkfgT58WoZ6cLy7BEPEQswJF1xYzhtdHR5pQfc at 13 from ed25519:5BVKQ5zZKtEUM3AHgj6KAerTpuh3CVZHtAbmVfxLbLhc, requested: false
Mar 25 13:00:26.414 DEBUG chain: Process block header: 55jqTxo3s4tjEkqfL67x9P1wqMEuPDgyLzNfjP29agYB at 13
Mar 25 13:00:26.414 DEBUG chain: Process block 55jqTxo3s4tjEkqfL67x9P1wqMEuPDgyLzNfjP29agYB at 13, approvals: 3, me: Some("test0")
Mar 25 13:00:26.414 DEBUG chain: Some("test0") Process block 55jqTxo3s4tjEkqfL67x9P1wqMEuPDgyLzNfjP29agYB, is_caught_up: false, need_to_start_fetching_state: false
Mar 25 13:00:26.415 DEBUG chain: Header head updated to 55jqTxo3s4tjEkqfL67x9P1wqMEuPDgyLzNfjP29agYB at 13
Mar 25 13:00:26.415 DEBUG chain: Verifying challenges []
Mar 25 13:00:26.415 DEBUG runtime: block height: 13, is next_block_epoch_start false
Mar 25 13:00:26.415 DEBUG runtime: add validator proposals at block height 13 []
Mar 25 13:00:26.415 DEBUG chain: Head updated to 55jqTxo3s4tjEkqfL67x9P1wqMEuPDgyLzNfjP29agYB at 13
Mar 25 13:00:26.415 DEBUG chain: Check orphans: from 55jqTxo3s4tjEkqfL67x9P1wqMEuPDgyLzNfjP29agYB, # orphans 0
Mar 25 13:00:26.415 DEBUG client: Producing chunk at height 14 for shard 0, I'm test0
Mar 25 13:00:26.415 DEBUG runtime: Transaction filtering results 0 valid out of 0 pulled from the pool
Mar 25 13:00:26.415 DEBUG client: Produced chunk at height 14 for shard 0 with 0 txs and 0 receipts, I'm test0, chunk_hash: GCcHdHsWByes1P6Hs5pkgYHDQtyqQpKHZUU8N1h6YLBY
Mar 25 13:00:26.415 DEBUG chunks: Reconstructed and decoded chunk GCcHdHsWByes1P6Hs5pkgYHDQtyqQpKHZUU8N1h6YLBY, encoded length was 8, num txs: 0, I'm Some("test0")
Mar 25 13:00:26.416 DEBUG chunks: Reconstructed and decoded chunk CbZkdWyGL6JvicBwfCZgwrSjrKgAE2FWxGFMVXBFtWdF, encoded length was 8, num txs: 0, I'm Some("test0")
Mar 25 13:00:26.416 DEBUG chunks: Reconstructed and decoded chunk D72LRxyNFKkSUFKJE8ExCdD2cyTu5u5JhAJYtAiB1mvt, encoded length was 8, num txs: 0, I'm Some("test0")
Mar 25 13:00:26.432 DEBUG client: Catchup me: Some("test0"): sync_hash: `FfZjASXkfgT58WoZ6cLy7BEPEQswJF1xYzhtdHR5pQfc`, sync_info: {1: ShardSyncDownload { downloads: [], status: StateDownloadFinalize }, 2: ShardSyncDownload { downloads: [], status: StateDownloadFinalize }}
Mar 25 13:00:26.432 DEBUG runtime: block height: 11, is next_block_epoch_start false
Mar 25 13:00:26.433 DEBUG runtime: Calling the contract at account test0
Mar 25 13:00:26.433  INFO run_vm:run_wasmer:get_key: near_vm_runner::cache: close time.busy=3.79µs time.idle=1.28µs
Mar 25 13:00:26.450  INFO run_vm:run_wasmer:compile_and_serialize_wasmer: near_vm_runner::cache::wasmer0_cache: close time.busy=0.00ns time.idle=17.1ms
Mar 25 13:00:26.450  INFO run_vm:run_wasmer:run_method:run_method/instantiate: near_vm_runner::wasmer_runner: close time.busy=300µs time.idle=540ns
Mar 25 13:00:26.456  INFO run_vm:run_wasmer:run_method:run_method/call: near_vm_runner::wasmer_runner: close time.busy=5.34ms time.idle=450ns
Mar 25 13:00:26.456  INFO run_vm:run_wasmer:run_method:run_method/drop_instance: near_vm_runner::wasmer_runner: close time.busy=137µs time.idle=440ns
Mar 25 13:00:26.456  INFO run_vm:run_wasmer:run_method: near_vm_runner::wasmer_runner: close time.busy=5.80ms time.idle=1.46µs
Mar 25 13:00:26.456  INFO run_vm:run_wasmer: near_vm_runner::wasmer_runner: close time.busy=23.0ms time.idle=370ns
Mar 25 13:00:26.456  INFO run_vm: near_vm_runner::runner: close time.busy=23.0ms time.idle=1.64µs
Mar 25 13:00:26.456 DEBUG runtime: Done.
Mar 25 13:00:26.456 DEBUG runtime: Calling the contract at account test0
Mar 25 13:00:26.456  INFO run_vm:run_wasmer:get_key: near_vm_runner::cache: close time.busy=1.41µs time.idle=370ns
Mar 25 13:00:26.456  INFO run_vm:run_wasmer:run_method:run_method/instantiate: near_vm_runner::wasmer_runner: close time.busy=266µs time.idle=420ns
Mar 25 13:00:26.461  INFO run_vm:run_wasmer:run_method:run_method/call: near_vm_runner::wasmer_runner: close time.busy=5.36ms time.idle=480ns
Mar 25 13:00:26.462  INFO run_vm:run_wasmer:run_method:run_method/drop_instance: near_vm_runner::wasmer_runner: close time.busy=117µs time.idle=520ns
Mar 25 13:00:26.462  INFO run_vm:run_wasmer:run_method: near_vm_runner::wasmer_runner: close time.busy=5.77ms time.idle=350ns
Mar 25 13:00:26.462  INFO run_vm:run_wasmer: near_vm_runner::wasmer_runner: close time.busy=5.80ms time.idle=300ns
Mar 25 13:00:26.462  INFO run_vm: near_vm_runner::runner: close time.busy=5.81ms time.idle=420ns
Mar 25 13:00:26.462 DEBUG runtime: Done.
Mar 25 13:00:26.462 DEBUG runtime: Calling the contract at account test0
Mar 25 13:00:26.462  INFO run_vm:run_wasmer:get_key: near_vm_runner::cache: close time.busy=970ns time.idle=300ns
Mar 25 13:00:26.462  INFO run_vm:run_wasmer:run_method:run_method/instantiate: near_vm_runner::wasmer_runner: close time.busy=254µs time.idle=410ns
Mar 25 13:00:26.469  INFO run_vm:run_wasmer:run_method:run_method/call: near_vm_runner::wasmer_runner: close time.busy=7.48ms time.idle=360ns
Mar 25 13:00:26.470  INFO run_vm:run_wasmer:run_method:run_method/drop_instance: near_vm_runner::wasmer_runner: close time.busy=137µs time.idle=390ns
Mar 25 13:00:26.470  INFO run_vm:run_wasmer:run_method: near_vm_runner::wasmer_runner: close time.busy=7.89ms time.idle=350ns
Mar 25 13:00:26.470  INFO run_vm:run_wasmer: near_vm_runner::wasmer_runner: close time.busy=7.92ms time.idle=310ns
Mar 25 13:00:26.470  INFO run_vm: near_vm_runner::runner: close time.busy=7.93ms time.idle=380ns
Mar 25 13:00:26.470 DEBUG runtime: Done.
Mar 25 13:00:26.470 DEBUG runtime: Calling the contract at account test0
Mar 25 13:00:26.470  INFO run_vm:run_wasmer:get_key: near_vm_runner::cache: close time.busy=1.02µs time.idle=350ns
Mar 25 13:00:26.470  INFO run_vm:run_wasmer:run_method:run_method/instantiate: near_vm_runner::wasmer_runner: close time.busy=263µs time.idle=360ns
Mar 25 13:00:26.475  INFO run_vm:run_wasmer:run_method:run_method/call: near_vm_runner::wasmer_runner: close time.busy=5.25ms time.idle=390ns
Mar 25 13:00:26.475  INFO run_vm:run_wasmer:run_method:run_method/drop_instance: near_vm_runner::wasmer_runner: close time.busy=113µs time.idle=400ns
Mar 25 13:00:26.475  INFO run_vm:run_wasmer:run_method: near_vm_runner::wasmer_runner: close time.busy=5.65ms time.idle=300ns
Mar 25 13:00:26.475  INFO run_vm:run_wasmer: near_vm_runner::wasmer_runner: close time.busy=5.68ms time.idle=320ns
Mar 25 13:00:26.475  INFO run_vm: near_vm_runner::runner: close time.busy=5.68ms time.idle=310ns
Mar 25 13:00:26.475 DEBUG runtime: Done.
Mar 25 13:00:26.475 DEBUG runtime: Calling the contract at account test0
Mar 25 13:00:26.475  INFO run_vm:run_wasmer:get_key: near_vm_runner::cache: close time.busy=970ns time.idle=330ns
Mar 25 13:00:26.476  INFO run_vm:run_wasmer:run_method:run_method/instantiate: near_vm_runner::wasmer_runner: close time.busy=261µs time.idle=420ns
Mar 25 13:00:26.482  INFO run_vm:run_wasmer:run_method:run_method/call: near_vm_runner::wasmer_runner: close time.busy=6.77ms time.idle=350ns
Mar 25 13:00:26.483  INFO run_vm:run_wasmer:run_method:run_method/drop_instance: near_vm_runner::wasmer_runner: close time.busy=117µs time.idle=410ns
Mar 25 13:00:26.483  INFO run_vm:run_wasmer:run_method: near_vm_runner::wasmer_runner: close time.busy=7.17ms time.idle=350ns
Mar 25 13:00:26.483  INFO run_vm:run_wasmer: near_vm_runner::wasmer_runner: close time.busy=7.20ms time.idle=330ns
Mar 25 13:00:26.483  INFO run_vm: near_vm_runner::runner: close time.busy=7.20ms time.idle=390ns
Mar 25 13:00:26.483 DEBUG runtime: Done.
Mar 25 13:00:26.483 DEBUG runtime: Calling the contract at account test0
Mar 25 13:00:26.483  INFO run_vm:run_wasmer:get_key: near_vm_runner::cache: close time.busy=920ns time.idle=310ns
Mar 25 13:00:26.483  INFO run_vm:run_wasmer:run_method:run_method/instantiate: near_vm_runner::wasmer_runner: close time.busy=250µs time.idle=350ns
Mar 25 13:00:26.490  INFO run_vm:run_wasmer:run_method:run_method/call: near_vm_runner::wasmer_runner: close time.busy=6.71ms time.idle=430ns
Mar 25 13:00:26.490  INFO run_vm:run_wasmer:run_method:run_method/drop_instance: near_vm_runner::wasmer_runner: close time.busy=111µs time.idle=420ns
Mar 25 13:00:26.490  INFO run_vm:run_wasmer:run_method: near_vm_runner::wasmer_runner: close time.busy=7.09ms time.idle=320ns
Mar 25 13:00:26.490  INFO run_vm:run_wasmer: near_vm_runner::wasmer_runner: close time.busy=7.11ms time.idle=310ns
Mar 25 13:00:26.490  INFO run_vm: near_vm_runner::runner: close time.busy=7.12ms time.idle=390ns
Mar 25 13:00:26.490 DEBUG runtime: Done.
Mar 25 13:00:26.490 DEBUG runtime: Calling the contract at account test0
Mar 25 13:00:26.490  INFO run_vm:run_wasmer:get_key: near_vm_runner::cache: close time.busy=950ns time.idle=340ns
Mar 25 13:00:26.490  INFO run_vm:run_wasmer:run_method:run_method/instantiate: near_vm_runner::wasmer_runner: close time.busy=246µs time.idle=410ns
Mar 25 13:00:26.496  INFO run_vm:run_wasmer:run_method:run_method/call: near_vm_runner::wasmer_runner: close time.busy=5.67ms time.idle=440ns
Mar 25 13:00:26.496  INFO run_vm:run_wasmer:run_method:run_method/drop_instance: near_vm_runner::wasmer_runner: close time.busy=109µs time.idle=440ns
Mar 25 13:00:26.496  INFO run_vm:run_wasmer:run_method: near_vm_runner::wasmer_runner: close time.busy=6.05ms time.idle=300ns
Mar 25 13:00:26.496  INFO run_vm:run_wasmer: near_vm_runner::wasmer_runner: close time.busy=6.08ms time.idle=300ns
Mar 25 13:00:26.496  INFO run_vm: near_vm_runner::runner: close time.busy=6.08ms time.idle=410ns
Mar 25 13:00:26.496 DEBUG runtime: Done.
Mar 25 13:00:26.496 DEBUG runtime: Calling the contract at account test0
Mar 25 13:00:26.496  INFO run_vm:run_wasmer:get_key: near_vm_runner::cache: close time.busy=980ns time.idle=290ns
Mar 25 13:00:26.496  INFO run_vm:run_wasmer:run_method:run_method/instantiate: near_vm_runner::wasmer_runner: close time.busy=246µs time.idle=370ns
Mar 25 13:00:26.501  INFO run_vm:run_wasmer:run_method:run_method/call: near_vm_runner::wasmer_runner: close time.busy=5.27ms time.idle=350ns
Mar 25 13:00:26.502  INFO run_vm:run_wasmer:run_method:run_method/drop_instance: near_vm_runner::wasmer_runner: close time.busy=117µs time.idle=370ns
Mar 25 13:00:26.502  INFO run_vm:run_wasmer:run_method: near_vm_runner::wasmer_runner: close time.busy=5.65ms time.idle=340ns
Mar 25 13:00:26.502  INFO run_vm:run_wasmer: near_vm_runner::wasmer_runner: close time.busy=5.68ms time.idle=300ns
Mar 25 13:00:26.502  INFO run_vm: near_vm_runner::runner: close time.busy=5.69ms time.idle=360ns
Mar 25 13:00:26.502 DEBUG runtime: Done.
Mar 25 13:00:26.502 DEBUG runtime: Calling the contract at account test0
Mar 25 13:00:26.502  INFO run_vm:run_wasmer:get_key: near_vm_runner::cache: close time.busy=920ns time.idle=330ns
Mar 25 13:00:26.502  INFO run_vm:run_wasmer:run_method:run_method/instantiate: near_vm_runner::wasmer_runner: close time.busy=247µs time.idle=330ns
Mar 25 13:00:26.507  INFO run_vm:run_wasmer:run_method:run_method/call: near_vm_runner::wasmer_runner: close time.busy=5.28ms time.idle=420ns
Mar 25 13:00:26.507  INFO run_vm:run_wasmer:run_method:run_method/drop_instance: near_vm_runner::wasmer_runner: close time.busy=109µs time.idle=390ns
Mar 25 13:00:26.507  INFO run_vm:run_wasmer:run_method: near_vm_runner::wasmer_runner: close time.busy=5.66ms time.idle=310ns
Mar 25 13:00:26.507  INFO run_vm:run_wasmer: near_vm_runner::wasmer_runner: close time.busy=5.68ms time.idle=270ns
Mar 25 13:00:26.507  INFO run_vm: near_vm_runner::runner: close time.busy=5.69ms time.idle=370ns
Mar 25 13:00:26.507 DEBUG runtime: Done.
Mar 25 13:00:26.507 DEBUG runtime: Calling the contract at account test0
Mar 25 13:00:26.507  INFO run_vm:run_wasmer:get_key: near_vm_runner::cache: close time.busy=950ns time.idle=320ns
Mar 25 13:00:26.508  INFO run_vm:run_wasmer:run_method:run_method/instantiate: near_vm_runner::wasmer_runner: close time.busy=248µs time.idle=400ns
Mar 25 13:00:26.513  INFO run_vm:run_wasmer:run_method:run_method/call: near_vm_runner::wasmer_runner: close time.busy=5.27ms time.idle=340ns
Mar 25 13:00:26.513  INFO run_vm:run_wasmer:run_method:run_method/drop_instance: near_vm_runner::wasmer_runner: close time.busy=108µs time.idle=370ns
Mar 25 13:00:26.513  INFO run_vm:run_wasmer:run_method: near_vm_runner::wasmer_runner: close time.busy=5.64ms time.idle=320ns
Mar 25 13:00:26.513  INFO run_vm:run_wasmer: near_vm_runner::wasmer_runner: close time.busy=5.67ms time.idle=310ns
Mar 25 13:00:26.513  INFO run_vm: near_vm_runner::runner: close time.busy=5.67ms time.idle=350ns
Mar 25 13:00:26.513 DEBUG runtime: Done.
Mar 25 13:00:26.513 DEBUG runtime: block height: 11, is next_block_epoch_start false
Mar 25 13:00:26.614 DEBUG client: Catchup me: Some("test0"): sync_hash: `FfZjASXkfgT58WoZ6cLy7BEPEQswJF1xYzhtdHR5pQfc`, sync_info: {1: ShardSyncDownload { downloads: [], status: StateDownloadComplete }, 2: ShardSyncDownload { downloads: [], status: StateDownloadComplete }}
Mar 25 13:00:26.614 DEBUG chain: Verifying challenges []
Mar 25 13:00:26.614 DEBUG runtime: block height: 12, is next_block_epoch_start true
Mar 25 13:00:26.614 DEBUG epoch_manager: epoch id: EpochId(`2Bu42LaW6A3LkLtDFjdbgs4pEffhtyLqqfAzTMVd669y`), prev_epoch_id: EpochId(`8nF7fhxhmtKAxDjM1ek63WtUK6iHK8TuJe1FwTUiyZaT`), prev_prev_epoch_id: EpochId(`11111111111111111111111111111111`)
Mar 25 13:00:26.614 DEBUG epoch_manager: prev_prev_stake_change: {"test0": 50000000000000000000000000000000, "test1": 50000000000000000000000000000000, "test2": 50000000000000000000000000000000, "test3": 50000000000000000000000000000000}, prev_stake_change: {"test0": 50000000000000000000000000000000, "test1": 50000000000000000000000000000000, "test2": 50000000000000000000000000000000, "test3": 50000000000000000000000000000000}, stake_change: {"test0": 50000009909309994923908913331185, "test1": 50000009909309994923908913331185, "test2": 50000009909309994923908913331185, "test3": 0}, slashed: {}
Mar 25 13:00:26.614 DEBUG epoch_manager: stake_info: {"test2": 50000009909309994923908913331185, "test0": 50000009909309994923908913331185, "test1": 50000009909309994923908913331185, "test3": 50000000000000000000000000000000}, validator_reward: {"near": 7927447995939127130664947, "test1": 9909309994923908913331185, "test0": 9909309994923908913331185, "test2": 9909309994923908913331185}
Mar 25 13:00:26.614 DEBUG runtime: account test3 stake 50000000000000000000000000000000 max_of_stakes: 50000000000000000000000000000000
Mar 25 13:00:26.614 DEBUG runtime: account test3 return stake 0
Mar 25 13:00:26.614 DEBUG runtime: account test0 adding reward 9909309994923908913331185 to stake 50000000000000000000000000000000
Mar 25 13:00:26.614 DEBUG runtime: account test0 stake 50000009909309994923908913331185 max_of_stakes: 50000009909309994923908913331185
Mar 25 13:00:26.614 DEBUG runtime: account test0 return stake 0
Mar 25 13:00:26.614 DEBUG runtime: block height: 12, is next_block_epoch_start true
Mar 25 13:00:26.614 DEBUG epoch_manager: epoch id: EpochId(`2Bu42LaW6A3LkLtDFjdbgs4pEffhtyLqqfAzTMVd669y`), prev_epoch_id: EpochId(`8nF7fhxhmtKAxDjM1ek63WtUK6iHK8TuJe1FwTUiyZaT`), prev_prev_epoch_id: EpochId(`11111111111111111111111111111111`)
Mar 25 13:00:26.614 DEBUG epoch_manager: prev_prev_stake_change: {"test0": 50000000000000000000000000000000, "test1": 50000000000000000000000000000000, "test2": 50000000000000000000000000000000, "test3": 50000000000000000000000000000000}, prev_stake_change: {"test0": 50000000000000000000000000000000, "test1": 50000000000000000000000000000000, "test2": 50000000000000000000000000000000, "test3": 50000000000000000000000000000000}, stake_change: {"test0": 50000009909309994923908913331185, "test1": 50000009909309994923908913331185, "test2": 50000009909309994923908913331185, "test3": 0}, slashed: {}
Mar 25 13:00:26.614 DEBUG epoch_manager: stake_info: {"test3": 50000000000000000000000000000000, "test0": 50000009909309994923908913331185, "test2": 50000009909309994923908913331185, "test1": 50000009909309994923908913331185}, validator_reward: {"near": 7927447995939127130664947, "test1": 9909309994923908913331185, "test0": 9909309994923908913331185, "test2": 9909309994923908913331185}
Mar 25 13:00:26.615 DEBUG chain: Verifying challenges []
Mar 25 13:00:26.615 DEBUG runtime: block height: 13, is next_block_epoch_start false
Mar 25 13:00:26.615 DEBUG runtime: block height: 13, is next_block_epoch_start false
Mar 25 13:00:26.615 DEBUG chain: Catching up: removing prev=`FfZjASXkfgT58WoZ6cLy7BEPEQswJF1xYzhtdHR5pQfc` from the queue. I'm Some("test0")
Mar 25 13:00:26.615 DEBUG chain: Catching up: removing prev=`55jqTxo3s4tjEkqfL67x9P1wqMEuPDgyLzNfjP29agYB` from the queue. I'm Some("test0")
Mar 25 13:00:26.615 DEBUG chain: Check orphans: from 55jqTxo3s4tjEkqfL67x9P1wqMEuPDgyLzNfjP29agYB, # orphans 0
Mar 25 13:00:26.615 DEBUG chain: Check orphans: from FfZjASXkfgT58WoZ6cLy7BEPEQswJF1xYzhtdHR5pQfc, # orphans 0
Mar 25 13:00:26.733 DEBUG client: Sending an approval Endorsement(`55jqTxo3s4tjEkqfL67x9P1wqMEuPDgyLzNfjP29agYB`) from test0 to test1 for 14
Mar 25 13:00:26.929 DEBUG client: Some("test0") Received block 3dnuCnvPnjQoA93kFiusd6kYbpJ52GhvKWvyrRrfH23A <- 55jqTxo3s4tjEkqfL67x9P1wqMEuPDgyLzNfjP29agYB at 14 from ed25519:FXXrTXiKWpXj1R6r5fBvMLpstd8gPyrBq3qMByqKVzKF, requested: false
Mar 25 13:00:26.929 DEBUG chain: Process block header: 3dnuCnvPnjQoA93kFiusd6kYbpJ52GhvKWvyrRrfH23A at 14
Mar 25 13:00:26.929 DEBUG chain: Process block 3dnuCnvPnjQoA93kFiusd6kYbpJ52GhvKWvyrRrfH23A at 14, approvals: 4, me: Some("test0")
Mar 25 13:00:26.929 DEBUG chain: Some("test0") Process block 3dnuCnvPnjQoA93kFiusd6kYbpJ52GhvKWvyrRrfH23A, is_caught_up: true, need_to_start_fetching_state: false
Mar 25 13:00:26.930 DEBUG chain: Header head updated to 3dnuCnvPnjQoA93kFiusd6kYbpJ52GhvKWvyrRrfH23A at 14
Mar 25 13:00:26.930 DEBUG chain: Verifying challenges []
Mar 25 13:00:26.930 DEBUG runtime: block height: 14, is next_block_epoch_start false
Mar 25 13:00:26.930 DEBUG chain: Verifying challenges []
Mar 25 13:00:26.930 DEBUG runtime: block height: 14, is next_block_epoch_start false
Mar 25 13:00:26.930 DEBUG runtime: block height: 14, is next_block_epoch_start false
Mar 25 13:00:26.930 DEBUG runtime: add validator proposals at block height 14 []
Mar 25 13:00:26.930 DEBUG chain: Head updated to 3dnuCnvPnjQoA93kFiusd6kYbpJ52GhvKWvyrRrfH23A at 14
Mar 25 13:00:26.930 DEBUG chain: Check orphans: from 3dnuCnvPnjQoA93kFiusd6kYbpJ52GhvKWvyrRrfH23A, # orphans 0
Mar 25 13:00:26.930 DEBUG client: Producing chunk at height 15 for shard 0, I'm test0
Mar 25 13:00:26.930 DEBUG runtime: Transaction filtering results 0 valid out of 0 pulled from the pool
Mar 25 13:00:26.930 DEBUG client: Produced chunk at height 15 for shard 0 with 0 txs and 0 receipts, I'm test0, chunk_hash: 7TG4vyHH2rz7Z3ooWmSRqMPyAcpSqnjh3zGA3JTULwsu
Mar 25 13:00:26.931 DEBUG chunks: Reconstructed and decoded chunk 7TG4vyHH2rz7Z3ooWmSRqMPyAcpSqnjh3zGA3JTULwsu, encoded length was 8, num txs: 0, I'm Some("test0")
Mar 25 13:00:26.931 DEBUG chunks: Reconstructed and decoded chunk 9jAerGxs9u2D73A2UPc1n8N32rkV5e8vsC817KxYaqMR, encoded length was 8, num txs: 0, I'm Some("test0")
Mar 25 13:00:26.931 DEBUG chunks: Reconstructed and decoded chunk AmdQJZRCF2PMirboEb1ne9ZD6qNDU63MD64rAb82mbhf, encoded length was 8, num txs: 0, I'm Some("test0")
Mar 25 13:00:27.338 DEBUG client: Sending an approval Endorsement(`3dnuCnvPnjQoA93kFiusd6kYbpJ52GhvKWvyrRrfH23A`) from test0 to test2 for 15
Mar 25 13:00:27.528 DEBUG client: Some("test0") Received block 3HhHEiZifvh5s3CaAAssHUvtVm6NcWnHpKn63WARS2Se <- 3dnuCnvPnjQoA93kFiusd6kYbpJ52GhvKWvyrRrfH23A at 15 from ed25519:Eo9W44tRMwcYcoua11yM7Xfr1DjgR4EWQFM3RU27MEX8, requested: false
Mar 25 13:00:27.528 DEBUG chain: Process block header: 3HhHEiZifvh5s3CaAAssHUvtVm6NcWnHpKn63WARS2Se at 15
Mar 25 13:00:27.528 DEBUG chain: Process block 3HhHEiZifvh5s3CaAAssHUvtVm6NcWnHpKn63WARS2Se at 15, approvals: 3, me: Some("test0")
Mar 25 13:00:27.528 DEBUG chain: Some("test0") Process block 3HhHEiZifvh5s3CaAAssHUvtVm6NcWnHpKn63WARS2Se, is_caught_up: true, need_to_start_fetching_state: false
Mar 25 13:00:27.529 DEBUG chain: Header head updated to 3HhHEiZifvh5s3CaAAssHUvtVm6NcWnHpKn63WARS2Se at 15
Mar 25 13:00:27.529 DEBUG chain: Verifying challenges []
Mar 25 13:00:27.529 DEBUG runtime: block height: 15, is next_block_epoch_start false
Mar 25 13:00:27.529 DEBUG chain: Verifying challenges []
Mar 25 13:00:27.529 DEBUG runtime: block height: 15, is next_block_epoch_start false
Mar 25 13:00:27.529 DEBUG runtime: block height: 15, is next_block_epoch_start false
Mar 25 13:00:27.529 DEBUG runtime: add validator proposals at block height 15 []
Mar 25 13:00:27.529 DEBUG chain: Head updated to 3HhHEiZifvh5s3CaAAssHUvtVm6NcWnHpKn63WARS2Se at 15
Mar 25 13:00:27.529 DEBUG chain: Check orphans: from 3HhHEiZifvh5s3CaAAssHUvtVm6NcWnHpKn63WARS2Se, # orphans 0
Mar 25 13:00:27.529 DEBUG client: Producing chunk at height 16 for shard 0, I'm test0
Mar 25 13:00:27.529 DEBUG runtime: Transaction filtering results 0 valid out of 0 pulled from the pool
Mar 25 13:00:27.529 DEBUG client: Produced chunk at height 16 for shard 0 with 0 txs and 0 receipts, I'm test0, chunk_hash: 3JAXyRkn9JBSWM11LafZT5hizk7DCoBvcMPJyV8tRDAa
Mar 25 13:00:27.529 DEBUG chunks: Reconstructed and decoded chunk 3JAXyRkn9JBSWM11LafZT5hizk7DCoBvcMPJyV8tRDAa, encoded length was 8, num txs: 0, I'm Some("test0")
Mar 25 13:00:27.530 DEBUG chunks: Reconstructed and decoded chunk 59XeWDad9duJCQYkhf4nZEEmw29iAnq1L35dcFzua4MV, encoded length was 8, num txs: 0, I'm Some("test0")
Mar 25 13:00:27.530 DEBUG chunks: Reconstructed and decoded chunk eWwbXMe4nWtxMESMGGBSUrMvpFfkU4rLyqsDwaBY6Cd, encoded length was 8, num txs: 0, I'm Some("test0")

```

Integrated Wasmer 1:

```
12:55:55|~/projects/nearcore|wamer1-benchmarks⚡*?
λ cargo build --release -p neard --features wasmer1_default
   Compiling near-vm-runner v3.0.0 (/home/matklad/projects/nearcore/runtime/near-vm-runner)
   Compiling node-runtime v3.0.0 (/home/matklad/projects/nearcore/runtime/runtime)
   Compiling neard v1.2.0 (/home/matklad/projects/nearcore/neard)
    Finished release [optimized] target(s) in 14.59s

12:57:47|~/projects/nearcore|wamer1-benchmarks⚡*?
λ pushd pytest; pipenv run python3 tests/sanity/concurrent_function_calls.py; popd
Use default config {'local': True, 'preexist': False, 'near_root': '../target/release/', 'binary_name': 'neard', 'release': False, 'bridge': {'bridge_repo': 'https://github.com/near/rainbow-bridge.git', 'bridge_dir': '~/.rainbow-bridge', 'config_dir': '~/.rainbow', 'ganache_dir': 'testing/vendor/ganache', 'ganache_bin': 'testing/vendor/ganache/node_modules/.bin/ganache-cli', 'ganache_block_prod_time': 10}}
Creating LOCAL cluster configuration with 4 nodes
Search for stdout and stderr in ['/home/matklad/.near/test0', '/home/matklad/.near/test1', '/home/matklad/.near/test2', '/home/matklad/.near/test3']
Starting node 0 as BOOT NODE
node 0 started
Starting node 1 with boot=ed25519:He7QeRuwizNEhBioYG3u4DZ8jWXyETiyNzFD3MkTjDMf@127.0.0.1:24577
Starting node 2 with boot=ed25519:He7QeRuwizNEhBioYG3u4DZ8jWXyETiyNzFD3MkTjDMf@127.0.0.1:24577
Starting node 3 with boot=ed25519:He7QeRuwizNEhBioYG3u4DZ8jWXyETiyNzFD3MkTjDMf@127.0.0.1:24577
node 3 started
node 2 started
node 1 started
Cleaning up node 127.0.0.1:24578 on script exit
Executed store validity tests: 0
Cleaning up node 127.0.0.1:24580 on script exit
Executed store validity tests: 0
Cleaning up node 127.0.0.1:24579 on script exit
Executed store validity tests: 0
Cleaning up node 127.0.0.1:24577 on script exit
Executed store validity tests: 0

12:58:28|~/projects/nearcore|wamer1-benchmarks⚡*?
λ cat ~/.near/test0_finished/stderr
Mar 25 12:58:15.532  INFO near: Version: 1.2.0, Build: eceb375b-modified, Latest Protocol: 43
Mar 25 12:58:15.532  INFO near: Did not find "/home/matklad/.near/test0/data" path, will be creating new store database
Mar 25 12:58:15.919  INFO runtime: Genesis state has 10 records, computing state roots
Mar 25 12:58:15.919  INFO runtime: Tracking shards: {}
Mar 25 12:58:15.922  INFO network: Starting http server at 0.0.0.0:3040
Mar 25 12:58:15.923 DEBUG network: Found known peers: 0 (boot nodes=0)
Mar 25 12:58:15.923  INFO client: Starting validator node: test0
Mar 25 12:58:15.923 DEBUG network: Blacklist: {}
Mar 25 12:58:15.923 DEBUG runtime: add validator proposals at block height 0 []
Mar 25 12:58:15.924  INFO stats: Server listening at ed25519:He7QeRuwizNEhBioYG3u4DZ8jWXyETiyNzFD3MkTjDMf@0.0.0.0:24577
Mar 25 12:58:15.924  INFO chain: Init: saved genesis: `F49EPnYw9MfdhabugbMr8Ywu85ziPPkpbZtCpZHTxsP` / [`iiSeMk1WQEKmMWFcFxL3YC5tsZqMcc1bCynHLzf8FYv`, `BTdKNrn4aDgXdq2depAR3V36e9fGCEKt3THm8Rs2zHXS`, `8YijkEqArew3cMs7MoeZbbnqPEP8LbCUN85qVb668V6i`, `95uBRr8aRH8wuqGCNdQ5TEyfkk6MX22U9HHTpQjYVQZi`]
Mar 25 12:58:15.924  INFO chain: Init: head @ 0 [F49EPnYw9MfdhabugbMr8Ywu85ziPPkpbZtCpZHTxsP]
Mar 25 12:58:19.679 DEBUG network: ed25519:He7QeRuwizNEhBioYG3u4DZ8jWXyETiyNzFD3MkTjDMf: Peer 127.0.0.1:59762 Inbound started
Mar 25 12:58:19.680 DEBUG network: ed25519:He7QeRuwizNEhBioYG3u4DZ8jWXyETiyNzFD3MkTjDMf: Received handshake Handshake { version: 43, oldest_supported_version: 34, peer_id: ed25519:5BVKQ5zZKtEUM3AHgj6KAerTpuh3CVZHtAbmVfxLbLhc, target_peer_id: ed25519:He7QeRuwizNEhBioYG3u4DZ8jWXyETiyNzFD3MkTjDMf, listen_port: Some(24580), chain_info: PeerChainInfoV2 { genesis_id: GenesisId { chain_id: "test-chain-Q1s6S", hash: `F49EPnYw9MfdhabugbMr8Ywu85ziPPkpbZtCpZHTxsP` }, height: 0, tracked_shards: [], archival: false }, edge_info: EdgeInfo { nonce: 1, signature: ed25519:2ZVafxhbwWLgYrfTTvBvkSq9QB7vPEouvAQi78DMC2f9YgmP3wBdMRSNbai9PHFYVRHRmjqiXQufLxzDHCEkd8J7 } }
Mar 25 12:58:19.680 DEBUG network: Consolidated connection with FullPeerInfo { peer_info: PeerInfo { id: ed25519:5BVKQ5zZKtEUM3AHgj6KAerTpuh3CVZHtAbmVfxLbLhc, addr: Some(127.0.0.1:24580), account_id: None }, chain_info: PeerChainInfoV2 { genesis_id: GenesisId { chain_id: "test-chain-Q1s6S", hash: `F49EPnYw9MfdhabugbMr8Ywu85ziPPkpbZtCpZHTxsP` }, height: 0, tracked_shards: [], archival: false }, edge_info: EdgeInfo { nonce: 1, signature: ed25519:2ZVafxhbwWLgYrfTTvBvkSq9QB7vPEouvAQi78DMC2f9YgmP3wBdMRSNbai9PHFYVRHRmjqiXQufLxzDHCEkd8J7 } }
Mar 25 12:58:19.680 DEBUG network: ed25519:He7QeRuwizNEhBioYG3u4DZ8jWXyETiyNzFD3MkTjDMf: Peer 127.0.0.1:59764 Inbound started
Mar 25 12:58:19.681 DEBUG network: ed25519:He7QeRuwizNEhBioYG3u4DZ8jWXyETiyNzFD3MkTjDMf: Received handshake Handshake { version: 43, oldest_supported_version: 34, peer_id: ed25519:FXXrTXiKWpXj1R6r5fBvMLpstd8gPyrBq3qMByqKVzKF, target_peer_id: ed25519:He7QeRuwizNEhBioYG3u4DZ8jWXyETiyNzFD3MkTjDMf, listen_port: Some(24578), chain_info: PeerChainInfoV2 { genesis_id: GenesisId { chain_id: "test-chain-Q1s6S", hash: `F49EPnYw9MfdhabugbMr8Ywu85ziPPkpbZtCpZHTxsP` }, height: 0, tracked_shards: [], archival: false }, edge_info: EdgeInfo { nonce: 1, signature: ed25519:5ca6A3uzrK3FKGjeAEi7pLNDiBbrzQ1ZPseXF1Rw8rfr3RWYNXPckVDUGGKMZQFE8eKWXeeCws6ZvmeQW8PZEzP6 } }
Mar 25 12:58:19.681 DEBUG network: Consolidated connection with FullPeerInfo { peer_info: PeerInfo { id: ed25519:FXXrTXiKWpXj1R6r5fBvMLpstd8gPyrBq3qMByqKVzKF, addr: Some(127.0.0.1:24578), account_id: None }, chain_info: PeerChainInfoV2 { genesis_id: GenesisId { chain_id: "test-chain-Q1s6S", hash: `F49EPnYw9MfdhabugbMr8Ywu85ziPPkpbZtCpZHTxsP` }, height: 0, tracked_shards: [], archival: false }, edge_info: EdgeInfo { nonce: 1, signature: ed25519:5ca6A3uzrK3FKGjeAEi7pLNDiBbrzQ1ZPseXF1Rw8rfr3RWYNXPckVDUGGKMZQFE8eKWXeeCws6ZvmeQW8PZEzP6 } }
Mar 25 12:58:19.681 DEBUG network: ed25519:He7QeRuwizNEhBioYG3u4DZ8jWXyETiyNzFD3MkTjDMf: Peer 127.0.0.1:59766 Inbound started
Mar 25 12:58:19.682 DEBUG network: ed25519:He7QeRuwizNEhBioYG3u4DZ8jWXyETiyNzFD3MkTjDMf: Received handshake Handshake { version: 43, oldest_supported_version: 34, peer_id: ed25519:Eo9W44tRMwcYcoua11yM7Xfr1DjgR4EWQFM3RU27MEX8, target_peer_id: ed25519:He7QeRuwizNEhBioYG3u4DZ8jWXyETiyNzFD3MkTjDMf, listen_port: Some(24579), chain_info: PeerChainInfoV2 { genesis_id: GenesisId { chain_id: "test-chain-Q1s6S", hash: `F49EPnYw9MfdhabugbMr8Ywu85ziPPkpbZtCpZHTxsP` }, height: 0, tracked_shards: [], archival: false }, edge_info: EdgeInfo { nonce: 1, signature: ed25519:34VUqMLTphHv5t3wNXbrQnvRHNiSdYKHL41sQSRiRXMGYtBEnFvGhS6yHM4Bn2wk1cfHjz3KiWBWJcvSScd83g1F } }
Mar 25 12:58:19.682 DEBUG network: Consolidated connection with FullPeerInfo { peer_info: PeerInfo { id: ed25519:Eo9W44tRMwcYcoua11yM7Xfr1DjgR4EWQFM3RU27MEX8, addr: Some(127.0.0.1:24579), account_id: None }, chain_info: PeerChainInfoV2 { genesis_id: GenesisId { chain_id: "test-chain-Q1s6S", hash: `F49EPnYw9MfdhabugbMr8Ywu85ziPPkpbZtCpZHTxsP` }, height: 0, tracked_shards: [], archival: false }, edge_info: EdgeInfo { nonce: 1, signature: ed25519:34VUqMLTphHv5t3wNXbrQnvRHNiSdYKHL41sQSRiRXMGYtBEnFvGhS6yHM4Bn2wk1cfHjz3KiWBWJcvSScd83g1F } }
Mar 25 12:58:19.768  INFO client: Sync: synced at 0 [F49EPn], ed25519:FXXrTXiKWpXj1R6r5fBvMLpstd8gPyrBq3qMByqKVzKF, highest height peer: 0
Mar 25 12:58:19.768 DEBUG client: Some("test0") transitions to no sync
Mar 25 12:58:19.768 DEBUG client: Check announce account for test0, last announce time None
Mar 25 12:58:19.768 DEBUG client: Sending announce account for test0
Mar 25 12:58:19.768 DEBUG network: Some("test0") Account announce: AnnounceAccount { account_id: "test0", peer_id: ed25519:He7QeRuwizNEhBioYG3u4DZ8jWXyETiyNzFD3MkTjDMf, epoch_id: EpochId(`11111111111111111111111111111111`), signature: ed25519:5WwBXDt5PKXtkQCfQABWfUSMTDa9TGPaizRd7cr2KW6oLbF9hRFkpGpy99WniEHwXYRPvJEcAvENiqMyCs6SUM6j }
Mar 25 12:58:19.792 DEBUG network: Some("test0") Received new accounts: [AnnounceAccount { account_id: "test1", peer_id: ed25519:FXXrTXiKWpXj1R6r5fBvMLpstd8gPyrBq3qMByqKVzKF, epoch_id: EpochId(`11111111111111111111111111111111`), signature: ed25519:2E11kCw6Whe3WSYDFCEAYmqJvrQBjSHAVSVggmZfRDsM9EmgjPM2BZvaNuxytXsg9C32dbKzZ4X5rdSJSR8XxVD5 }]
Mar 25 12:58:19.792 DEBUG network: Some("test0") Received new accounts: [AnnounceAccount { account_id: "test3", peer_id: ed25519:5BVKQ5zZKtEUM3AHgj6KAerTpuh3CVZHtAbmVfxLbLhc, epoch_id: EpochId(`11111111111111111111111111111111`), signature: ed25519:2HAeqNB9a2pfQkvUUfqKeMkafkJU3GDTQSyV5xNuirQwRUpiW1XnPKH8NvZfMSekfbiHwZTe8ZKtnZHFLJonMvPe }]
Mar 25 12:58:19.792 DEBUG network: Some("test0") Received new accounts: [AnnounceAccount { account_id: "test2", peer_id: ed25519:Eo9W44tRMwcYcoua11yM7Xfr1DjgR4EWQFM3RU27MEX8, epoch_id: EpochId(`11111111111111111111111111111111`), signature: ed25519:5wyZZuDVShPRcSDoDMs6y8SjCjJw1JCjCU6mJH5Xm85YiTQfEs4epqCpimg1Ep5bXt5AQAPkEAMCcMKXc2PzYy23 }]
Mar 25 12:58:19.866 DEBUG client: Sending an approval Endorsement(`F49EPnYw9MfdhabugbMr8Ywu85ziPPkpbZtCpZHTxsP`) from test0 to test3 for 1
Mar 25 12:58:19.866 DEBUG network: Some("test0") Drop signed message to PeerId(ed25519:5BVKQ5zZKtEUM3AHgj6KAerTpuh3CVZHtAbmVfxLbLhc) Reason PeerNotFound. Num known peers: 0 Message Approval(1, test0, Endorsement(`F49EPnYw9MfdhabugbMr8Ywu85ziPPkpbZtCpZHTxsP`))
Mar 25 12:58:20.681 DEBUG network: Peers request from Some(ed25519:5BVKQ5zZKtEUM3AHgj6KAerTpuh3CVZHtAbmVfxLbLhc@127.0.0.1:24580): sending 3 peers.
Mar 25 12:58:20.682 DEBUG network: Received peers from Some(ed25519:5BVKQ5zZKtEUM3AHgj6KAerTpuh3CVZHtAbmVfxLbLhc@127.0.0.1:24580): 1 peers.
Mar 25 12:58:20.682 DEBUG network: Received peers from Some(ed25519:FXXrTXiKWpXj1R6r5fBvMLpstd8gPyrBq3qMByqKVzKF@127.0.0.1:24578): 1 peers.
Mar 25 12:58:20.682 DEBUG network: Peers request from Some(ed25519:FXXrTXiKWpXj1R6r5fBvMLpstd8gPyrBq3qMByqKVzKF@127.0.0.1:24578): sending 3 peers.
Mar 25 12:58:20.683 DEBUG network: Peers request from Some(ed25519:Eo9W44tRMwcYcoua11yM7Xfr1DjgR4EWQFM3RU27MEX8@127.0.0.1:24579): sending 3 peers.
Mar 25 12:58:20.683 DEBUG network: Received peers from Some(ed25519:Eo9W44tRMwcYcoua11yM7Xfr1DjgR4EWQFM3RU27MEX8@127.0.0.1:24579): 1 peers.
Mar 25 12:58:21.888 DEBUG client: Sending an approval Skip(0) from test0 to test1 for 2
Mar 25 12:58:22.004 DEBUG client: Some("test0") Received block AcRhfbBgfeQ8PMdZnqJPoinQyQK8bZ4aQu78eV7i3FvJ <- F49EPnYw9MfdhabugbMr8Ywu85ziPPkpbZtCpZHTxsP at 2 from ed25519:FXXrTXiKWpXj1R6r5fBvMLpstd8gPyrBq3qMByqKVzKF, requested: false
Mar 25 12:58:22.004 DEBUG chain: Process block header: AcRhfbBgfeQ8PMdZnqJPoinQyQK8bZ4aQu78eV7i3FvJ at 2
Mar 25 12:58:22.005 DEBUG chain: Process block AcRhfbBgfeQ8PMdZnqJPoinQyQK8bZ4aQu78eV7i3FvJ at 2, approvals: 4, me: Some("test0")
Mar 25 12:58:22.005 DEBUG chain: Some("test0") Process block AcRhfbBgfeQ8PMdZnqJPoinQyQK8bZ4aQu78eV7i3FvJ, is_caught_up: false, need_to_start_fetching_state: true
Mar 25 12:58:22.005 DEBUG chain: Header head updated to AcRhfbBgfeQ8PMdZnqJPoinQyQK8bZ4aQu78eV7i3FvJ at 2
Mar 25 12:58:22.006 DEBUG chain: Verifying challenges []
Mar 25 12:58:22.006 DEBUG runtime: block height: 2, is next_block_epoch_start true
Mar 25 12:58:22.006 DEBUG epoch_manager: epoch id: EpochId(`F49EPnYw9MfdhabugbMr8Ywu85ziPPkpbZtCpZHTxsP`), prev_epoch_id: EpochId(`11111111111111111111111111111111`), prev_prev_epoch_id: EpochId(`11111111111111111111111111111111`)
Mar 25 12:58:22.006 DEBUG epoch_manager: prev_prev_stake_change: {"test0": 50000000000000000000000000000000, "test1": 50000000000000000000000000000000, "test2": 50000000000000000000000000000000, "test3": 50000000000000000000000000000000}, prev_stake_change: {"test0": 50000000000000000000000000000000, "test1": 50000000000000000000000000000000, "test2": 50000000000000000000000000000000, "test3": 50000000000000000000000000000000}, stake_change: {"test0": 50000000000000000000000000000000, "test1": 50000000000000000000000000000000, "test2": 50000000000000000000000000000000, "test3": 50000000000000000000000000000000}, slashed: {}
Mar 25 12:58:22.006 DEBUG epoch_manager: stake_info: {"test1": 50000000000000000000000000000000, "test3": 50000000000000000000000000000000, "test0": 50000000000000000000000000000000, "test2": 50000000000000000000000000000000}, validator_reward: {"near": 0}
Mar 25 12:58:22.006 DEBUG runtime: account test2 stake 50000000000000000000000000000000 max_of_stakes: 50000000000000000000000000000000
Mar 25 12:58:22.006 DEBUG runtime: account test2 return stake 0
Mar 25 12:58:22.006 DEBUG runtime: add validator proposals at block height 2 []
Mar 25 12:58:22.006 DEBUG chain: Head updated to AcRhfbBgfeQ8PMdZnqJPoinQyQK8bZ4aQu78eV7i3FvJ at 2
Mar 25 12:58:22.006 DEBUG chain: Downloading state for block AcRhfbBgfeQ8PMdZnqJPoinQyQK8bZ4aQu78eV7i3FvJ
Mar 25 12:58:22.006 DEBUG chain: Downloading state for [], I'm Some("test0")
Mar 25 12:58:22.006 DEBUG chain: Check orphans: from AcRhfbBgfeQ8PMdZnqJPoinQyQK8bZ4aQu78eV7i3FvJ, # orphans 0
Mar 25 12:58:22.006 DEBUG client: Producing chunk at height 3 for shard 0, I'm test0
Mar 25 12:58:22.006 DEBUG client: Produced chunk at height 3 for shard 0 with 0 txs and 0 receipts, I'm test0, chunk_hash: 5F69LDExN7i4p1FgMUwXManxT16pzgCteFPABLniMWbZ
Mar 25 12:58:22.006 DEBUG chunks: Reconstructed and decoded chunk 5F69LDExN7i4p1FgMUwXManxT16pzgCteFPABLniMWbZ, encoded length was 8, num txs: 0, I'm Some("test0")
Mar 25 12:58:22.090 DEBUG client: Sending an approval Endorsement(`AcRhfbBgfeQ8PMdZnqJPoinQyQK8bZ4aQu78eV7i3FvJ`) from test0 to test2 for 3
Mar 25 12:58:22.090 DEBUG client: Catchup me: Some("test0"): sync_hash: `AcRhfbBgfeQ8PMdZnqJPoinQyQK8bZ4aQu78eV7i3FvJ`, sync_info: {}
Mar 25 12:58:22.090 DEBUG chain: Verifying challenges []
Mar 25 12:58:22.090 DEBUG chain: Catching up: removing prev=`AcRhfbBgfeQ8PMdZnqJPoinQyQK8bZ4aQu78eV7i3FvJ` from the queue. I'm Some("test0")
Mar 25 12:58:22.090 DEBUG chain: Check orphans: from AcRhfbBgfeQ8PMdZnqJPoinQyQK8bZ4aQu78eV7i3FvJ, # orphans 0
Mar 25 12:58:22.107 DEBUG client: Some("test0") Received block A6vrg2iM2WF5s4C9sfKCoReiDMAWXpE6yw3kuEJFwGaU <- AcRhfbBgfeQ8PMdZnqJPoinQyQK8bZ4aQu78eV7i3FvJ at 3 from ed25519:Eo9W44tRMwcYcoua11yM7Xfr1DjgR4EWQFM3RU27MEX8, requested: false
Mar 25 12:58:22.107 DEBUG chain: Process block header: A6vrg2iM2WF5s4C9sfKCoReiDMAWXpE6yw3kuEJFwGaU at 3
Mar 25 12:58:22.108 DEBUG chain: Process block A6vrg2iM2WF5s4C9sfKCoReiDMAWXpE6yw3kuEJFwGaU at 3, approvals: 4, me: Some("test0")
Mar 25 12:58:22.108 DEBUG chain: Some("test0") Process block A6vrg2iM2WF5s4C9sfKCoReiDMAWXpE6yw3kuEJFwGaU, is_caught_up: true, need_to_start_fetching_state: false
Mar 25 12:58:22.109 DEBUG chain: Header head updated to A6vrg2iM2WF5s4C9sfKCoReiDMAWXpE6yw3kuEJFwGaU at 3
Mar 25 12:58:22.109 DEBUG chain: Verifying challenges []
Mar 25 12:58:22.109 DEBUG runtime: block height: 3, is next_block_epoch_start false
Mar 25 12:58:22.109 DEBUG chain: Verifying challenges []
Mar 25 12:58:22.109 DEBUG runtime: add validator proposals at block height 3 []
Mar 25 12:58:22.109 DEBUG chain: Head updated to A6vrg2iM2WF5s4C9sfKCoReiDMAWXpE6yw3kuEJFwGaU at 3
Mar 25 12:58:22.110 DEBUG chain: Check orphans: from A6vrg2iM2WF5s4C9sfKCoReiDMAWXpE6yw3kuEJFwGaU, # orphans 0
Mar 25 12:58:22.110 DEBUG client: Producing chunk at height 4 for shard 0, I'm test0
Mar 25 12:58:22.110 DEBUG runtime: Transaction filtering results 0 valid out of 0 pulled from the pool
Mar 25 12:58:22.110 DEBUG client: Produced chunk at height 4 for shard 0 with 0 txs and 0 receipts, I'm test0, chunk_hash: 2kJaLw5ajVGgC8v3ZpgVDJk77dSLuvPozGZCeTmn7ASP
Mar 25 12:58:22.110 DEBUG chunks: Reconstructed and decoded chunk 2kJaLw5ajVGgC8v3ZpgVDJk77dSLuvPozGZCeTmn7ASP, encoded length was 8, num txs: 0, I'm Some("test0")
Mar 25 12:58:22.798 DEBUG client: "test0" Producing block at height 4, parent 3 @ A6vrg2
Mar 25 12:58:22.798 DEBUG chain: Process block FdNvsX6pJmam768cV3e8vdNSRSAC3ZCE9jiE8R6vBtnM at 4, approvals: 4, me: Some("test0")
Mar 25 12:58:22.798 DEBUG chain: Some("test0") Process block FdNvsX6pJmam768cV3e8vdNSRSAC3ZCE9jiE8R6vBtnM, is_caught_up: true, need_to_start_fetching_state: false
Mar 25 12:58:22.798 DEBUG chain: Header head updated to FdNvsX6pJmam768cV3e8vdNSRSAC3ZCE9jiE8R6vBtnM at 4
Mar 25 12:58:22.799 DEBUG chain: Verifying challenges []
Mar 25 12:58:22.799 DEBUG runtime: block height: 4, is next_block_epoch_start false
Mar 25 12:58:22.799 DEBUG chain: Verifying challenges []
Mar 25 12:58:22.799 DEBUG runtime: add validator proposals at block height 4 []
Mar 25 12:58:22.799 DEBUG chain: Head updated to FdNvsX6pJmam768cV3e8vdNSRSAC3ZCE9jiE8R6vBtnM at 4
Mar 25 12:58:22.799 DEBUG chain: Check orphans: from FdNvsX6pJmam768cV3e8vdNSRSAC3ZCE9jiE8R6vBtnM, # orphans 0
Mar 25 12:58:22.799 DEBUG client: Producing chunk at height 5 for shard 0, I'm test0
Mar 25 12:58:22.799 DEBUG runtime: Transaction filtering results 0 valid out of 0 pulled from the pool
Mar 25 12:58:22.799 DEBUG client: Produced chunk at height 5 for shard 0 with 0 txs and 0 receipts, I'm test0, chunk_hash: 3NJbfLWcuCHMKXcM1mYEod6nEWPdzQ3HbasJUyQr2X5D
Mar 25 12:58:22.799 DEBUG chunks: Reconstructed and decoded chunk 3NJbfLWcuCHMKXcM1mYEod6nEWPdzQ3HbasJUyQr2X5D, encoded length was 8, num txs: 0, I'm Some("test0")
Mar 25 12:58:22.816 DEBUG client: I'm Some("test0"), routing a transaction SignedTransaction { transaction: Transaction { signer_id: "test0", public_key: ed25519:He7QeRuwizNEhBioYG3u4DZ8jWXyETiyNzFD3MkTjDMf, nonce: 10, receiver_id: "test0", block_hash: `A6vrg2iM2WF5s4C9sfKCoReiDMAWXpE6yw3kuEJFwGaU`, actions: [DeployContract(DeployContractAction { code: (50498)[0, 97, … 116, 101] })] }, signature: ed25519:ActAj2HKkBzAFUM17evEWWgMxvMobzTYYWvpwPVnN4JH8E3kGnqjpaR6SpJoy5QY7yTF3r9gbETT5CbhwiqbCcV, hash: `7xUAxc1LHDEePk9yAaiQAva593iYpA4ZfgqFgDbMGnod`, size: 50598 } to test3, shard_id = 1
Mar 25 12:58:23.303 DEBUG client: Sending an approval Endorsement(`FdNvsX6pJmam768cV3e8vdNSRSAC3ZCE9jiE8R6vBtnM`) from test0 to test3 for 5
Mar 25 12:58:23.319 DEBUG client: Some("test0") Received block DUoFtsDvNiNcD1z1FWQvWvqwVBHc91t7njuST4TzNhvw <- FdNvsX6pJmam768cV3e8vdNSRSAC3ZCE9jiE8R6vBtnM at 5 from ed25519:5BVKQ5zZKtEUM3AHgj6KAerTpuh3CVZHtAbmVfxLbLhc, requested: false
Mar 25 12:58:23.319 DEBUG chain: Process block header: DUoFtsDvNiNcD1z1FWQvWvqwVBHc91t7njuST4TzNhvw at 5
Mar 25 12:58:23.320 DEBUG chain: Process block DUoFtsDvNiNcD1z1FWQvWvqwVBHc91t7njuST4TzNhvw at 5, approvals: 3, me: Some("test0")
Mar 25 12:58:23.320 DEBUG chain: Some("test0") Process block DUoFtsDvNiNcD1z1FWQvWvqwVBHc91t7njuST4TzNhvw, is_caught_up: true, need_to_start_fetching_state: false
Mar 25 12:58:23.321 DEBUG chain: Header head updated to DUoFtsDvNiNcD1z1FWQvWvqwVBHc91t7njuST4TzNhvw at 5
Mar 25 12:58:23.321 DEBUG chain: Verifying challenges []
Mar 25 12:58:23.321 DEBUG runtime: block height: 5, is next_block_epoch_start false
Mar 25 12:58:23.321 DEBUG chain: Verifying challenges []
Mar 25 12:58:23.321 DEBUG runtime: add validator proposals at block height 5 []
Mar 25 12:58:23.321 DEBUG chain: Head updated to DUoFtsDvNiNcD1z1FWQvWvqwVBHc91t7njuST4TzNhvw at 5
Mar 25 12:58:23.321 DEBUG chain: Check orphans: from DUoFtsDvNiNcD1z1FWQvWvqwVBHc91t7njuST4TzNhvw, # orphans 0
Mar 25 12:58:23.321 DEBUG client: Producing chunk at height 6 for shard 0, I'm test0
Mar 25 12:58:23.321 DEBUG runtime: Transaction filtering results 0 valid out of 0 pulled from the pool
Mar 25 12:58:23.321 DEBUG client: Produced chunk at height 6 for shard 0 with 0 txs and 0 receipts, I'm test0, chunk_hash: 5JWYDWZdJRDpPo5YBL8mNPkFgcGB7GoiQAYD6VTnosim
Mar 25 12:58:23.322 DEBUG chunks: Reconstructed and decoded chunk 5JWYDWZdJRDpPo5YBL8mNPkFgcGB7GoiQAYD6VTnosim, encoded length was 8, num txs: 0, I'm Some("test0")
Mar 25 12:58:23.911 DEBUG client: Sending an approval Endorsement(`DUoFtsDvNiNcD1z1FWQvWvqwVBHc91t7njuST4TzNhvw`) from test0 to test1 for 6
Mar 25 12:58:23.925 DEBUG client: Some("test0") Received block 9UjSriAeN6uf9yscXsiv2tH3Z7h8SREvQFD6iuFgiwui <- DUoFtsDvNiNcD1z1FWQvWvqwVBHc91t7njuST4TzNhvw at 6 from ed25519:FXXrTXiKWpXj1R6r5fBvMLpstd8gPyrBq3qMByqKVzKF, requested: false
Mar 25 12:58:23.925 DEBUG chain: Process block header: 9UjSriAeN6uf9yscXsiv2tH3Z7h8SREvQFD6iuFgiwui at 6
Mar 25 12:58:23.926 DEBUG chain: Process block 9UjSriAeN6uf9yscXsiv2tH3Z7h8SREvQFD6iuFgiwui at 6, approvals: 3, me: Some("test0")
Mar 25 12:58:23.926 DEBUG chain: Some("test0") Process block 9UjSriAeN6uf9yscXsiv2tH3Z7h8SREvQFD6iuFgiwui, is_caught_up: true, need_to_start_fetching_state: false
Mar 25 12:58:23.926 DEBUG chain: Header head updated to 9UjSriAeN6uf9yscXsiv2tH3Z7h8SREvQFD6iuFgiwui at 6
Mar 25 12:58:23.927 DEBUG chain: Verifying challenges []
Mar 25 12:58:23.927 DEBUG runtime: block height: 6, is next_block_epoch_start false
Mar 25 12:58:23.927 DEBUG chain: Verifying challenges []
Mar 25 12:58:23.927 DEBUG runtime: add validator proposals at block height 6 []
Mar 25 12:58:23.927 DEBUG chain: Head updated to 9UjSriAeN6uf9yscXsiv2tH3Z7h8SREvQFD6iuFgiwui at 6
Mar 25 12:58:23.927 DEBUG chain: Check orphans: from 9UjSriAeN6uf9yscXsiv2tH3Z7h8SREvQFD6iuFgiwui, # orphans 0
Mar 25 12:58:23.927 DEBUG client: Producing chunk at height 7 for shard 0, I'm test0
Mar 25 12:58:23.927 DEBUG runtime: Transaction filtering results 0 valid out of 0 pulled from the pool
Mar 25 12:58:23.927 DEBUG client: Produced chunk at height 7 for shard 0 with 0 txs and 0 receipts, I'm test0, chunk_hash: MQVdg5RJTtSfsb944RQj6Nd3zGgYNeUUnJ2yNCEWtsh
Mar 25 12:58:23.927 DEBUG chunks: Reconstructed and decoded chunk MQVdg5RJTtSfsb944RQj6Nd3zGgYNeUUnJ2yNCEWtsh, encoded length was 8, num txs: 0, I'm Some("test0")
Mar 25 12:58:24.517 DEBUG client: Sending an approval Endorsement(`9UjSriAeN6uf9yscXsiv2tH3Z7h8SREvQFD6iuFgiwui`) from test0 to test2 for 7
Mar 25 12:58:24.533 DEBUG client: Some("test0") Received block 81R7cYWafPuk6QowNYQqqW5urkXgaaiSD4a2dPPb83vQ <- 9UjSriAeN6uf9yscXsiv2tH3Z7h8SREvQFD6iuFgiwui at 7 from ed25519:Eo9W44tRMwcYcoua11yM7Xfr1DjgR4EWQFM3RU27MEX8, requested: false
Mar 25 12:58:24.534 DEBUG chain: Process block header: 81R7cYWafPuk6QowNYQqqW5urkXgaaiSD4a2dPPb83vQ at 7
Mar 25 12:58:24.534 DEBUG chain: Process block 81R7cYWafPuk6QowNYQqqW5urkXgaaiSD4a2dPPb83vQ at 7, approvals: 4, me: Some("test0")
Mar 25 12:58:24.534 DEBUG chain: Some("test0") Process block 81R7cYWafPuk6QowNYQqqW5urkXgaaiSD4a2dPPb83vQ, is_caught_up: true, need_to_start_fetching_state: false
Mar 25 12:58:24.535 DEBUG chain: Header head updated to 81R7cYWafPuk6QowNYQqqW5urkXgaaiSD4a2dPPb83vQ at 7
Mar 25 12:58:24.535 DEBUG chain: Verifying challenges []
Mar 25 12:58:24.535 DEBUG runtime: block height: 7, is next_block_epoch_start false
Mar 25 12:58:24.536 DEBUG chain: Verifying challenges []
Mar 25 12:58:24.536 DEBUG runtime: add validator proposals at block height 7 []
Mar 25 12:58:24.536 DEBUG chain: Head updated to 81R7cYWafPuk6QowNYQqqW5urkXgaaiSD4a2dPPb83vQ at 7
Mar 25 12:58:24.536 DEBUG chain: Check orphans: from 81R7cYWafPuk6QowNYQqqW5urkXgaaiSD4a2dPPb83vQ, # orphans 0
Mar 25 12:58:24.536 DEBUG client: Producing chunk at height 8 for shard 0, I'm test0
Mar 25 12:58:24.536 DEBUG runtime: Transaction filtering results 0 valid out of 0 pulled from the pool
Mar 25 12:58:24.536 DEBUG client: Produced chunk at height 8 for shard 0 with 0 txs and 0 receipts, I'm test0, chunk_hash: 6Eu2nxL9Ds2UmkVwSzXG58gmjijFduwCx1m11j15BiEo
Mar 25 12:58:24.536 DEBUG chunks: Reconstructed and decoded chunk 6Eu2nxL9Ds2UmkVwSzXG58gmjijFduwCx1m11j15BiEo, encoded length was 8, num txs: 0, I'm Some("test0")
Mar 25 12:58:25.126 DEBUG client: "test0" Producing block at height 8, parent 7 @ 81R7cY
Mar 25 12:58:25.126 DEBUG chain: Process block HzkutgjiJaZgj9zZvdQuWwS94FEcGxQwNq4oRVDwzAa9 at 8, approvals: 3, me: Some("test0")
Mar 25 12:58:25.126 DEBUG chain: Some("test0") Process block HzkutgjiJaZgj9zZvdQuWwS94FEcGxQwNq4oRVDwzAa9, is_caught_up: true, need_to_start_fetching_state: false
Mar 25 12:58:25.126 DEBUG chain: Header head updated to HzkutgjiJaZgj9zZvdQuWwS94FEcGxQwNq4oRVDwzAa9 at 8
Mar 25 12:58:25.127 DEBUG chain: Verifying challenges []
Mar 25 12:58:25.127 DEBUG runtime: block height: 8, is next_block_epoch_start false
Mar 25 12:58:25.127 DEBUG chain: Verifying challenges []
Mar 25 12:58:25.127 DEBUG runtime: add validator proposals at block height 8 []
Mar 25 12:58:25.127 DEBUG chain: Head updated to HzkutgjiJaZgj9zZvdQuWwS94FEcGxQwNq4oRVDwzAa9 at 8
Mar 25 12:58:25.127 DEBUG chain: Check orphans: from HzkutgjiJaZgj9zZvdQuWwS94FEcGxQwNq4oRVDwzAa9, # orphans 0
Mar 25 12:58:25.127 DEBUG client: Producing chunk at height 9 for shard 0, I'm test0
Mar 25 12:58:25.127 DEBUG runtime: Transaction filtering results 0 valid out of 0 pulled from the pool
Mar 25 12:58:25.127 DEBUG client: Produced chunk at height 9 for shard 0 with 0 txs and 0 receipts, I'm test0, chunk_hash: 5ncyLU5rwAaVeHZDCjic5sUbTfW2dSWsXXS1n8NvkaG7
Mar 25 12:58:25.127 DEBUG chunks: Reconstructed and decoded chunk 5ncyLU5rwAaVeHZDCjic5sUbTfW2dSWsXXS1n8NvkaG7, encoded length was 8, num txs: 0, I'm Some("test0")
Mar 25 12:58:25.732 DEBUG client: Sending an approval Endorsement(`HzkutgjiJaZgj9zZvdQuWwS94FEcGxQwNq4oRVDwzAa9`) from test0 to test3 for 9
Mar 25 12:58:25.748 DEBUG client: Some("test0") Received block J4fDZks9Zh9e5SyJBknGUR6kGj7pAQZgxEQPfzmfng9x <- HzkutgjiJaZgj9zZvdQuWwS94FEcGxQwNq4oRVDwzAa9 at 9 from ed25519:5BVKQ5zZKtEUM3AHgj6KAerTpuh3CVZHtAbmVfxLbLhc, requested: false
Mar 25 12:58:25.748 DEBUG chain: Process block header: J4fDZks9Zh9e5SyJBknGUR6kGj7pAQZgxEQPfzmfng9x at 9
Mar 25 12:58:25.748 DEBUG chain: Process block J4fDZks9Zh9e5SyJBknGUR6kGj7pAQZgxEQPfzmfng9x at 9, approvals: 4, me: Some("test0")
Mar 25 12:58:25.748 DEBUG chain: Some("test0") Process block J4fDZks9Zh9e5SyJBknGUR6kGj7pAQZgxEQPfzmfng9x, is_caught_up: true, need_to_start_fetching_state: false
Mar 25 12:58:25.749 DEBUG chain: Header head updated to J4fDZks9Zh9e5SyJBknGUR6kGj7pAQZgxEQPfzmfng9x at 9
Mar 25 12:58:25.749 DEBUG chain: Verifying challenges []
Mar 25 12:58:25.749 DEBUG runtime: block height: 9, is next_block_epoch_start false
Mar 25 12:58:25.749 DEBUG chain: Verifying challenges []
Mar 25 12:58:25.749 DEBUG runtime: add validator proposals at block height 9 []
Mar 25 12:58:25.749 DEBUG chain: Head updated to J4fDZks9Zh9e5SyJBknGUR6kGj7pAQZgxEQPfzmfng9x at 9
Mar 25 12:58:25.749 DEBUG chain: Check orphans: from J4fDZks9Zh9e5SyJBknGUR6kGj7pAQZgxEQPfzmfng9x, # orphans 0
Mar 25 12:58:25.749 DEBUG client: Producing chunk at height 10 for shard 0, I'm test0
Mar 25 12:58:25.749 DEBUG runtime: Transaction filtering results 0 valid out of 0 pulled from the pool
Mar 25 12:58:25.749 DEBUG client: Produced chunk at height 10 for shard 0 with 0 txs and 0 receipts, I'm test0, chunk_hash: 9dHS4rx7x5N1NLQao53hAMeoTm85Cr7RyRtfDmnjFNZ6
Mar 25 12:58:25.749 DEBUG chunks: Reconstructed and decoded chunk 9dHS4rx7x5N1NLQao53hAMeoTm85Cr7RyRtfDmnjFNZ6, encoded length was 8, num txs: 0, I'm Some("test0")
Mar 25 12:58:25.925  INFO stats: #       9 J4fDZks9Zh9e5SyJBknGUR6kGj7pAQZgxEQPfzmfng9x V/4  3/3/40 peers ⬇ 73 B/s ⬆ 67 B/s 0.80 bps 127.36 Ggas/s CPU: 0%, Mem: 0 B
Mar 25 12:58:26.339 DEBUG client: Sending an approval Endorsement(`J4fDZks9Zh9e5SyJBknGUR6kGj7pAQZgxEQPfzmfng9x`) from test0 to test1 for 10
Mar 25 12:58:26.353 DEBUG client: Some("test0") Received block Bm4XjTZJJJDQUG7aPEjfz6RvqsxQRrMTwA4SarPmWaV6 <- J4fDZks9Zh9e5SyJBknGUR6kGj7pAQZgxEQPfzmfng9x at 10 from ed25519:FXXrTXiKWpXj1R6r5fBvMLpstd8gPyrBq3qMByqKVzKF, requested: false
Mar 25 12:58:26.353 DEBUG chain: Process block header: Bm4XjTZJJJDQUG7aPEjfz6RvqsxQRrMTwA4SarPmWaV6 at 10
Mar 25 12:58:26.354 DEBUG chain: Process block Bm4XjTZJJJDQUG7aPEjfz6RvqsxQRrMTwA4SarPmWaV6 at 10, approvals: 4, me: Some("test0")
Mar 25 12:58:26.354 DEBUG chain: Some("test0") Process block Bm4XjTZJJJDQUG7aPEjfz6RvqsxQRrMTwA4SarPmWaV6, is_caught_up: true, need_to_start_fetching_state: false
Mar 25 12:58:26.354 DEBUG chain: Header head updated to Bm4XjTZJJJDQUG7aPEjfz6RvqsxQRrMTwA4SarPmWaV6 at 10
Mar 25 12:58:26.355 DEBUG chain: Verifying challenges []
Mar 25 12:58:26.355 DEBUG runtime: block height: 10, is next_block_epoch_start false
Mar 25 12:58:26.355 DEBUG chain: Verifying challenges []
Mar 25 12:58:26.355 DEBUG runtime: add validator proposals at block height 10 []
Mar 25 12:58:26.355 DEBUG chain: Head updated to Bm4XjTZJJJDQUG7aPEjfz6RvqsxQRrMTwA4SarPmWaV6 at 10
Mar 25 12:58:26.355 DEBUG chain: Check orphans: from Bm4XjTZJJJDQUG7aPEjfz6RvqsxQRrMTwA4SarPmWaV6, # orphans 0
Mar 25 12:58:26.355 DEBUG client: Producing chunk at height 11 for shard 0, I'm test0
Mar 25 12:58:26.355 DEBUG runtime: Transaction filtering results 0 valid out of 0 pulled from the pool
Mar 25 12:58:26.355 DEBUG client: Produced chunk at height 11 for shard 0 with 0 txs and 0 receipts, I'm test0, chunk_hash: B922fT6hc7q8Mx622pHAUnRHmKXxB8vGqLhMG1pKKcgc
Mar 25 12:58:26.355 DEBUG chunks: Reconstructed and decoded chunk B922fT6hc7q8Mx622pHAUnRHmKXxB8vGqLhMG1pKKcgc, encoded length was 8, num txs: 0, I'm Some("test0")
Mar 25 12:58:26.862 DEBUG client: Some("test0") Received block BPifyBjxJWq4pwWnCKSzEj6a694JjDwAxwR6uP4qfpiA <- Bm4XjTZJJJDQUG7aPEjfz6RvqsxQRrMTwA4SarPmWaV6 at 11 from ed25519:Eo9W44tRMwcYcoua11yM7Xfr1DjgR4EWQFM3RU27MEX8, requested: false
Mar 25 12:58:26.862 DEBUG chain: Process block header: BPifyBjxJWq4pwWnCKSzEj6a694JjDwAxwR6uP4qfpiA at 11
Mar 25 12:58:26.862 DEBUG chain: Process block BPifyBjxJWq4pwWnCKSzEj6a694JjDwAxwR6uP4qfpiA at 11, approvals: 3, me: Some("test0")
Mar 25 12:58:26.862 DEBUG chain: Some("test0") Process block BPifyBjxJWq4pwWnCKSzEj6a694JjDwAxwR6uP4qfpiA, is_caught_up: true, need_to_start_fetching_state: false
Mar 25 12:58:26.863 DEBUG chain: Header head updated to BPifyBjxJWq4pwWnCKSzEj6a694JjDwAxwR6uP4qfpiA at 11
Mar 25 12:58:26.863 DEBUG chain: Verifying challenges []
Mar 25 12:58:26.863 DEBUG runtime: block height: 11, is next_block_epoch_start false
Mar 25 12:58:26.863 DEBUG chain: Verifying challenges []
Mar 25 12:58:26.863 DEBUG runtime: add validator proposals at block height 11 []
Mar 25 12:58:26.863 DEBUG epoch_manager: All proposals: [], Kickouts: {"test3": NotEnoughBlocks { produced: 2, expected: 3 }}, Block Tracker: {0: ValidatorStats { produced: 2, expected: 2 }, 1: ValidatorStats { produced: 3, expected: 3 }, 2: ValidatorStats { produced: 3, expected: 3 }, 3: ValidatorStats { produced: 2, expected: 3 }}, Shard Tracker: {1: {3: ValidatorStats { produced: 9, expected: 10 }}, 2: {1: ValidatorStats { produced: 9, expected: 10 }}, 0: {0: ValidatorStats { produced: 9, expected: 10 }}, 3: {2: ValidatorStats { produced: 9, expected: 10 }}}
Mar 25 12:58:26.863 DEBUG chain: Head updated to BPifyBjxJWq4pwWnCKSzEj6a694JjDwAxwR6uP4qfpiA at 11
Mar 25 12:58:26.863 DEBUG chain: Check orphans: from BPifyBjxJWq4pwWnCKSzEj6a694JjDwAxwR6uP4qfpiA, # orphans 0
Mar 25 12:58:26.863 DEBUG client: Producing chunk at height 12 for shard 0, I'm test0
Mar 25 12:58:26.863 DEBUG runtime: Transaction filtering results 0 valid out of 0 pulled from the pool
Mar 25 12:58:26.864 DEBUG client: Produced chunk at height 12 for shard 0 with 0 txs and 0 receipts, I'm test0, chunk_hash: 7FD9STBB8VJCbkU3BxhYxVxfP8fSekc922KGk5xXAsnj
Mar 25 12:58:26.864 DEBUG chunks: Reconstructed and decoded chunk 7FD9STBB8VJCbkU3BxhYxVxfP8fSekc922KGk5xXAsnj, encoded length was 8, num txs: 0, I'm Some("test0")
Mar 25 12:58:26.864 DEBUG chunks: Reconstructed and decoded chunk BU2FjSQk7xu7R76RHYsCPMq34smQ2Cgxg2Hseu6vQwhd, encoded length was 8, num txs: 0, I'm Some("test0")
Mar 25 12:58:26.902 DEBUG chunks: Reconstructed and decoded chunk C4uZftgkFUp4HL4NVAXVnnGYsbQp5mN5Lt38bDPdhHGf, encoded length was 1398, num txs: 0, I'm Some("test0")
Mar 25 12:58:27.148 DEBUG client: "test0" Producing block at height 12, parent 11 @ BPifyB
Mar 25 12:58:27.148 DEBUG chain: Process block DeWDHSEzzTF4Dp6ZYipwAUZ3egtrVpcH8LictRx9KGf3 at 12, approvals: 3, me: Some("test0")
Mar 25 12:58:27.148 DEBUG chain: Some("test0") Process block DeWDHSEzzTF4Dp6ZYipwAUZ3egtrVpcH8LictRx9KGf3, is_caught_up: false, need_to_start_fetching_state: true
Mar 25 12:58:27.148 DEBUG chain: Header head updated to DeWDHSEzzTF4Dp6ZYipwAUZ3egtrVpcH8LictRx9KGf3 at 12
Mar 25 12:58:27.149 DEBUG chain: Verifying challenges []
Mar 25 12:58:27.149 DEBUG runtime: block height: 12, is next_block_epoch_start true
Mar 25 12:58:27.149 DEBUG epoch_manager: epoch id: EpochId(`BPifyBjxJWq4pwWnCKSzEj6a694JjDwAxwR6uP4qfpiA`), prev_epoch_id: EpochId(`F49EPnYw9MfdhabugbMr8Ywu85ziPPkpbZtCpZHTxsP`), prev_prev_epoch_id: EpochId(`11111111111111111111111111111111`)
Mar 25 12:58:27.149 DEBUG epoch_manager: prev_prev_stake_change: {"test0": 50000000000000000000000000000000, "test1": 50000000000000000000000000000000, "test2": 50000000000000000000000000000000, "test3": 50000000000000000000000000000000}, prev_stake_change: {"test0": 50000000000000000000000000000000, "test1": 50000000000000000000000000000000, "test2": 50000000000000000000000000000000, "test3": 50000000000000000000000000000000}, stake_change: {"test0": 50000009909309994923908913331185, "test1": 50000009909309994923908913331185, "test2": 50000009909309994923908913331185, "test3": 0}, slashed: {}
Mar 25 12:58:27.149 DEBUG epoch_manager: stake_info: {"test1": 50000009909309994923908913331185, "test0": 50000009909309994923908913331185, "test3": 50000000000000000000000000000000, "test2": 50000009909309994923908913331185}, validator_reward: {"test2": 9909309994923908913331185, "near": 7927447995939127130664947, "test1": 9909309994923908913331185, "test0": 9909309994923908913331185}
Mar 25 12:58:27.149 DEBUG runtime: account test2 adding reward 9909309994923908913331185 to stake 50000000000000000000000000000000
Mar 25 12:58:27.149 DEBUG runtime: account test2 stake 50000009909309994923908913331185 max_of_stakes: 50000009909309994923908913331185
Mar 25 12:58:27.149 DEBUG runtime: account test2 return stake 0
Mar 25 12:58:27.149 DEBUG runtime: add validator proposals at block height 12 []
Mar 25 12:58:27.149 DEBUG chain: Head updated to DeWDHSEzzTF4Dp6ZYipwAUZ3egtrVpcH8LictRx9KGf3 at 12
Mar 25 12:58:27.149 DEBUG chain: Downloading state for block DeWDHSEzzTF4Dp6ZYipwAUZ3egtrVpcH8LictRx9KGf3
Mar 25 12:58:27.149 DEBUG chain: Downloading state for [1, 2], I'm Some("test0")
Mar 25 12:58:27.149 DEBUG chain: Check orphans: from DeWDHSEzzTF4Dp6ZYipwAUZ3egtrVpcH8LictRx9KGf3, # orphans 0
Mar 25 12:58:27.149 DEBUG client: Producing chunk at height 13 for shard 0, I'm test0
Mar 25 12:58:27.149 DEBUG runtime: Transaction filtering results 0 valid out of 0 pulled from the pool
Mar 25 12:58:27.149 DEBUG client: Produced chunk at height 13 for shard 0 with 0 txs and 0 receipts, I'm test0, chunk_hash: U3tD3jJmGnkjZwZyZcvDWwGGFKdSj2WmJWFdkdYAfaG
Mar 25 12:58:27.149 DEBUG chunks: Reconstructed and decoded chunk U3tD3jJmGnkjZwZyZcvDWwGGFKdSj2WmJWFdkdYAfaG, encoded length was 8, num txs: 0, I'm Some("test0")
Mar 25 12:58:27.151 DEBUG chunks: Reconstructed and decoded chunk DKB2uZP2QafV7q2dEkwGNjjsbNEa3PqXCxjtsUPW9tHw, encoded length was 8, num txs: 0, I'm Some("test0")
Mar 25 12:58:27.151 DEBUG chunks: Reconstructed and decoded chunk GnCbfaF4MmJ9xsuP2d7k95UzHMMFbWh3ZrZruTwuUrcw, encoded length was 8, num txs: 0, I'm Some("test0")
Mar 25 12:58:27.248 DEBUG client: Catchup me: Some("test0"): sync_hash: `DeWDHSEzzTF4Dp6ZYipwAUZ3egtrVpcH8LictRx9KGf3`, sync_info: {}
Mar 25 12:58:27.350 DEBUG client: Catchup me: Some("test0"): sync_hash: `DeWDHSEzzTF4Dp6ZYipwAUZ3egtrVpcH8LictRx9KGf3`, sync_info: {2: ShardSyncDownload { downloads: [DownloadStatus { start_time: 2021-03-25T09:58:27.249023656Z, prev_update_time: 2021-03-25T09:58:27.249023656Z, run_me: false, error: false, done: true, state_requests_count: 1, last_target: Some(AccountId("test1")) }], status: StateDownloadHeader }, 1: ShardSyncDownload { downloads: [DownloadStatus { start_time: 2021-03-25T09:58:27.249023656Z, prev_update_time: 2021-03-25T09:58:27.249023656Z, run_me: false, error: false, done: true, state_requests_count: 1, last_target: Some(AccountId("test3")) }], status: StateDownloadHeader }}
[core/store/src/db.rs:732] path = "/home/matklad/.near/test0/data"
Mar 25 12:58:27.451 DEBUG client: Catchup me: Some("test0"): sync_hash: `DeWDHSEzzTF4Dp6ZYipwAUZ3egtrVpcH8LictRx9KGf3`, sync_info: {2: ShardSyncDownload { downloads: [DownloadStatus { start_time: 2021-03-25T09:58:27.350722210Z, prev_update_time: 2021-03-25T09:58:27.350722210Z, run_me: false, error: false, done: true, state_requests_count: 1, last_target: Some(AccountId("test1")) }, DownloadStatus { start_time: 2021-03-25T09:58:27.350722210Z, prev_update_time: 2021-03-25T09:58:27.350722210Z, run_me: false, error: false, done: true, state_requests_count: 1, last_target: Some(AccountId("test1")) }, DownloadStatus { start_time: 2021-03-25T09:58:27.350722210Z, prev_update_time: 2021-03-25T09:58:27.350722210Z, run_me: false, error: false, done: true, state_requests_count: 1, last_target: Some(AccountId("test1")) }], status: StateDownloadParts }, 1: ShardSyncDownload { downloads: [DownloadStatus { start_time: 2021-03-25T09:58:27.350722210Z, prev_update_time: 2021-03-25T09:58:27.350722210Z, run_me: false, error: false, done: true, state_requests_count: 1, last_target: Some(AccountId("test3")) }, DownloadStatus { start_time: 2021-03-25T09:58:27.350722210Z, prev_update_time: 2021-03-25T09:58:27.350722210Z, run_me: false, error: false, done: true, state_requests_count: 1, last_target: Some(AccountId("test3")) }, DownloadStatus { start_time: 2021-03-25T09:58:27.350722210Z, prev_update_time: 2021-03-25T09:58:27.350722210Z, run_me: false, error: false, done: true, state_requests_count: 1, last_target: Some(AccountId("test3")) }], status: StateDownloadParts }}
Mar 25 12:58:27.553 DEBUG client: Catchup me: Some("test0"): sync_hash: `DeWDHSEzzTF4Dp6ZYipwAUZ3egtrVpcH8LictRx9KGf3`, sync_info: {2: ShardSyncDownload { downloads: [], status: StateDownloadFinalize }, 1: ShardSyncDownload { downloads: [], status: StateDownloadFinalize }}
Mar 25 12:58:27.553 DEBUG runtime: block height: 11, is next_block_epoch_start false
Mar 25 12:58:27.554 DEBUG runtime: Calling the contract at account test0
Mar 25 12:58:27.554  INFO run_vm:run_wasmer1:get_key: near_vm_runner::cache: close time.busy=1.73µs time.idle=780ns
Mar 25 12:58:27.575  INFO run_vm:run_wasmer1:compile_and_serialize_wasmer1: near_vm_runner::cache::wasmer1_cache: close time.busy=0.00ns time.idle=21.4ms
Mar 25 12:58:27.576  INFO run_vm:run_wasmer1:run_method:run_method/instantiate: near_vm_runner::wasmer1_runner: close time.busy=103µs time.idle=530ns
Mar 25 12:58:27.576  INFO run_vm:run_wasmer1:run_method:run_method/call: near_vm_runner::wasmer1_runner: close time.busy=717µs time.idle=380ns
Mar 25 12:58:27.576  INFO run_vm:run_wasmer1:run_method:run_method/drop_instance: near_vm_runner::wasmer1_runner: close time.busy=820ns time.idle=310ns
Mar 25 12:58:27.576  INFO run_vm:run_wasmer1:run_method: near_vm_runner::wasmer1_runner: close time.busy=845µs time.idle=630ns
Mar 25 12:58:27.576  INFO run_vm:run_wasmer1: near_vm_runner::wasmer1_runner: close time.busy=22.4ms time.idle=460ns
Mar 25 12:58:27.576  INFO run_vm: near_vm_runner::runner: close time.busy=22.4ms time.idle=1.32µs
Mar 25 12:58:27.576 DEBUG runtime: Done.
Mar 25 12:58:27.576 DEBUG runtime: Calling the contract at account test0
Mar 25 12:58:27.576  INFO run_vm:run_wasmer1:get_key: near_vm_runner::cache: close time.busy=1.16µs time.idle=450ns
Mar 25 12:58:27.577  INFO run_vm:run_wasmer1:deserialize_wasmer1: near_vm_runner::cache::wasmer1_cache: close time.busy=0.00ns time.idle=408µs
Mar 25 12:58:27.577  INFO run_vm:run_wasmer1:run_method:run_method/instantiate: near_vm_runner::wasmer1_runner: close time.busy=71.5µs time.idle=510ns
Mar 25 12:58:27.578  INFO run_vm:run_wasmer1:run_method:run_method/call: near_vm_runner::wasmer1_runner: close time.busy=730µs time.idle=410ns
Mar 25 12:58:27.578  INFO run_vm:run_wasmer1:run_method:run_method/drop_instance: near_vm_runner::wasmer1_runner: close time.busy=750ns time.idle=360ns
Mar 25 12:58:27.578  INFO run_vm:run_wasmer1:run_method: near_vm_runner::wasmer1_runner: close time.busy=822µs time.idle=410ns
Mar 25 12:58:27.578  INFO run_vm:run_wasmer1: near_vm_runner::wasmer1_runner: close time.busy=1.65ms time.idle=350ns
Mar 25 12:58:27.578  INFO run_vm: near_vm_runner::runner: close time.busy=1.66ms time.idle=380ns
Mar 25 12:58:27.578 DEBUG runtime: Done.
Mar 25 12:58:27.578 DEBUG runtime: Calling the contract at account test0
Mar 25 12:58:27.578  INFO run_vm:run_wasmer1:get_key: near_vm_runner::cache: close time.busy=990ns time.idle=380ns
Mar 25 12:58:27.579  INFO run_vm:run_wasmer1:deserialize_wasmer1: near_vm_runner::cache::wasmer1_cache: close time.busy=0.00ns time.idle=358µs
Mar 25 12:58:27.579  INFO run_vm:run_wasmer1:run_method:run_method/instantiate: near_vm_runner::wasmer1_runner: close time.busy=69.3µs time.idle=410ns
Mar 25 12:58:27.580  INFO run_vm:run_wasmer1:run_method:run_method/call: near_vm_runner::wasmer1_runner: close time.busy=699µs time.idle=410ns
Mar 25 12:58:27.580  INFO run_vm:run_wasmer1:run_method:run_method/drop_instance: near_vm_runner::wasmer1_runner: close time.busy=750ns time.idle=310ns
Mar 25 12:58:27.580  INFO run_vm:run_wasmer1:run_method: near_vm_runner::wasmer1_runner: close time.busy=788µs time.idle=450ns
Mar 25 12:58:27.580  INFO run_vm:run_wasmer1: near_vm_runner::wasmer1_runner: close time.busy=1.57ms time.idle=330ns
Mar 25 12:58:27.580  INFO run_vm: near_vm_runner::runner: close time.busy=1.57ms time.idle=370ns
Mar 25 12:58:27.580 DEBUG runtime: Done.
Mar 25 12:58:27.580 DEBUG runtime: Calling the contract at account test0
Mar 25 12:58:27.580  INFO run_vm:run_wasmer1:get_key: near_vm_runner::cache: close time.busy=1.00µs time.idle=380ns
Mar 25 12:58:27.580  INFO run_vm:run_wasmer1:deserialize_wasmer1: near_vm_runner::cache::wasmer1_cache: close time.busy=0.00ns time.idle=331µs
Mar 25 12:58:27.580  INFO run_vm:run_wasmer1:run_method:run_method/instantiate: near_vm_runner::wasmer1_runner: close time.busy=69.9µs time.idle=480ns
Mar 25 12:58:27.581  INFO run_vm:run_wasmer1:run_method:run_method/call: near_vm_runner::wasmer1_runner: close time.busy=704µs time.idle=310ns
Mar 25 12:58:27.581  INFO run_vm:run_wasmer1:run_method:run_method/drop_instance: near_vm_runner::wasmer1_runner: close time.busy=680ns time.idle=320ns
Mar 25 12:58:27.581  INFO run_vm:run_wasmer1:run_method: near_vm_runner::wasmer1_runner: close time.busy=794µs time.idle=420ns
Mar 25 12:58:27.581  INFO run_vm:run_wasmer1: near_vm_runner::wasmer1_runner: close time.busy=1.52ms time.idle=310ns
Mar 25 12:58:27.581  INFO run_vm: near_vm_runner::runner: close time.busy=1.52ms time.idle=360ns
Mar 25 12:58:27.581 DEBUG runtime: Done.
Mar 25 12:58:27.581 DEBUG runtime: Calling the contract at account test0
Mar 25 12:58:27.581  INFO run_vm:run_wasmer1:get_key: near_vm_runner::cache: close time.busy=980ns time.idle=420ns
Mar 25 12:58:27.582  INFO run_vm:run_wasmer1:deserialize_wasmer1: near_vm_runner::cache::wasmer1_cache: close time.busy=0.00ns time.idle=336µs
Mar 25 12:58:27.582  INFO run_vm:run_wasmer1:run_method:run_method/instantiate: near_vm_runner::wasmer1_runner: close time.busy=67.4µs time.idle=440ns
Mar 25 12:58:27.583  INFO run_vm:run_wasmer1:run_method:run_method/call: near_vm_runner::wasmer1_runner: close time.busy=728µs time.idle=390ns
Mar 25 12:58:27.583  INFO run_vm:run_wasmer1:run_method:run_method/drop_instance: near_vm_runner::wasmer1_runner: close time.busy=670ns time.idle=320ns
Mar 25 12:58:27.583  INFO run_vm:run_wasmer1:run_method: near_vm_runner::wasmer1_runner: close time.busy=815µs time.idle=420ns
Mar 25 12:58:27.583  INFO run_vm:run_wasmer1: near_vm_runner::wasmer1_runner: close time.busy=1.51ms time.idle=310ns
Mar 25 12:58:27.583  INFO run_vm: near_vm_runner::runner: close time.busy=1.52ms time.idle=400ns
Mar 25 12:58:27.583 DEBUG runtime: Done.
Mar 25 12:58:27.583 DEBUG runtime: Calling the contract at account test0
Mar 25 12:58:27.583  INFO run_vm:run_wasmer1:get_key: near_vm_runner::cache: close time.busy=960ns time.idle=410ns
Mar 25 12:58:27.583  INFO run_vm:run_wasmer1:deserialize_wasmer1: near_vm_runner::cache::wasmer1_cache: close time.busy=0.00ns time.idle=335µs
Mar 25 12:58:27.584  INFO run_vm:run_wasmer1:run_method:run_method/instantiate: near_vm_runner::wasmer1_runner: close time.busy=67.2µs time.idle=420ns
Mar 25 12:58:27.584  INFO run_vm:run_wasmer1:run_method:run_method/call: near_vm_runner::wasmer1_runner: close time.busy=700µs time.idle=360ns
Mar 25 12:58:27.584  INFO run_vm:run_wasmer1:run_method:run_method/drop_instance: near_vm_runner::wasmer1_runner: close time.busy=630ns time.idle=310ns
Mar 25 12:58:27.584  INFO run_vm:run_wasmer1:run_method: near_vm_runner::wasmer1_runner: close time.busy=786µs time.idle=440ns
Mar 25 12:58:27.584  INFO run_vm:run_wasmer1: near_vm_runner::wasmer1_runner: close time.busy=1.48ms time.idle=330ns
Mar 25 12:58:27.584  INFO run_vm: near_vm_runner::runner: close time.busy=1.48ms time.idle=350ns
Mar 25 12:58:27.584 DEBUG runtime: Done.
Mar 25 12:58:27.584 DEBUG runtime: Calling the contract at account test0
Mar 25 12:58:27.584  INFO run_vm:run_wasmer1:get_key: near_vm_runner::cache: close time.busy=970ns time.idle=420ns
Mar 25 12:58:27.585  INFO run_vm:run_wasmer1:deserialize_wasmer1: near_vm_runner::cache::wasmer1_cache: close time.busy=0.00ns time.idle=332µs
Mar 25 12:58:27.585  INFO run_vm:run_wasmer1:run_method:run_method/instantiate: near_vm_runner::wasmer1_runner: close time.busy=67.3µs time.idle=380ns
Mar 25 12:58:27.586  INFO run_vm:run_wasmer1:run_method:run_method/call: near_vm_runner::wasmer1_runner: close time.busy=713µs time.idle=350ns
Mar 25 12:58:27.586  INFO run_vm:run_wasmer1:run_method:run_method/drop_instance: near_vm_runner::wasmer1_runner: close time.busy=660ns time.idle=300ns
Mar 25 12:58:27.586  INFO run_vm:run_wasmer1:run_method: near_vm_runner::wasmer1_runner: close time.busy=799µs time.idle=390ns
Mar 25 12:58:27.586  INFO run_vm:run_wasmer1: near_vm_runner::wasmer1_runner: close time.busy=1.49ms time.idle=350ns
Mar 25 12:58:27.586  INFO run_vm: near_vm_runner::runner: close time.busy=1.49ms time.idle=370ns
Mar 25 12:58:27.586 DEBUG runtime: Done.
Mar 25 12:58:27.586 DEBUG runtime: Calling the contract at account test0
Mar 25 12:58:27.586  INFO run_vm:run_wasmer1:get_key: near_vm_runner::cache: close time.busy=950ns time.idle=360ns
Mar 25 12:58:27.586  INFO run_vm:run_wasmer1:deserialize_wasmer1: near_vm_runner::cache::wasmer1_cache: close time.busy=0.00ns time.idle=336µs
Mar 25 12:58:27.587  INFO run_vm:run_wasmer1:run_method:run_method/instantiate: near_vm_runner::wasmer1_runner: close time.busy=66.6µs time.idle=380ns
Mar 25 12:58:27.587  INFO run_vm:run_wasmer1:run_method:run_method/call: near_vm_runner::wasmer1_runner: close time.busy=701µs time.idle=360ns
Mar 25 12:58:27.587  INFO run_vm:run_wasmer1:run_method:run_method/drop_instance: near_vm_runner::wasmer1_runner: close time.busy=640ns time.idle=340ns
Mar 25 12:58:27.587  INFO run_vm:run_wasmer1:run_method: near_vm_runner::wasmer1_runner: close time.busy=786µs time.idle=400ns
Mar 25 12:58:27.587  INFO run_vm:run_wasmer1: near_vm_runner::wasmer1_runner: close time.busy=1.47ms time.idle=310ns
Mar 25 12:58:27.587  INFO run_vm: near_vm_runner::runner: close time.busy=1.48ms time.idle=350ns
Mar 25 12:58:27.587 DEBUG runtime: Done.
Mar 25 12:58:27.587 DEBUG runtime: Calling the contract at account test0
Mar 25 12:58:27.587  INFO run_vm:run_wasmer1:get_key: near_vm_runner::cache: close time.busy=970ns time.idle=420ns
Mar 25 12:58:27.588  INFO run_vm:run_wasmer1:deserialize_wasmer1: near_vm_runner::cache::wasmer1_cache: close time.busy=0.00ns time.idle=333µs
Mar 25 12:58:27.588  INFO run_vm:run_wasmer1:run_method:run_method/instantiate: near_vm_runner::wasmer1_runner: close time.busy=66.9µs time.idle=440ns
Mar 25 12:58:27.589  INFO run_vm:run_wasmer1:run_method:run_method/call: near_vm_runner::wasmer1_runner: close time.busy=705µs time.idle=350ns
Mar 25 12:58:27.589  INFO run_vm:run_wasmer1:run_method:run_method/drop_instance: near_vm_runner::wasmer1_runner: close time.busy=760ns time.idle=310ns
Mar 25 12:58:27.589  INFO run_vm:run_wasmer1:run_method: near_vm_runner::wasmer1_runner: close time.busy=793µs time.idle=390ns
Mar 25 12:58:27.589  INFO run_vm:run_wasmer1: near_vm_runner::wasmer1_runner: close time.busy=1.48ms time.idle=350ns
Mar 25 12:58:27.589  INFO run_vm: near_vm_runner::runner: close time.busy=1.49ms time.idle=370ns
Mar 25 12:58:27.589 DEBUG runtime: Done.
Mar 25 12:58:27.589 DEBUG runtime: Calling the contract at account test0
Mar 25 12:58:27.589  INFO run_vm:run_wasmer1:get_key: near_vm_runner::cache: close time.busy=960ns time.idle=350ns
Mar 25 12:58:27.589  INFO run_vm:run_wasmer1:deserialize_wasmer1: near_vm_runner::cache::wasmer1_cache: close time.busy=0.00ns time.idle=337µs
Mar 25 12:58:27.590  INFO run_vm:run_wasmer1:run_method:run_method/instantiate: near_vm_runner::wasmer1_runner: close time.busy=66.8µs time.idle=410ns
Mar 25 12:58:27.590  INFO run_vm:run_wasmer1:run_method:run_method/call: near_vm_runner::wasmer1_runner: close time.busy=710µs time.idle=380ns
Mar 25 12:58:27.590  INFO run_vm:run_wasmer1:run_method:run_method/drop_instance: near_vm_runner::wasmer1_runner: close time.busy=600ns time.idle=310ns
Mar 25 12:58:27.590  INFO run_vm:run_wasmer1:run_method: near_vm_runner::wasmer1_runner: close time.busy=795µs time.idle=400ns
Mar 25 12:58:27.590  INFO run_vm:run_wasmer1: near_vm_runner::wasmer1_runner: close time.busy=1.49ms time.idle=340ns
Mar 25 12:58:27.590  INFO run_vm: near_vm_runner::runner: close time.busy=1.49ms time.idle=370ns
Mar 25 12:58:27.590 DEBUG runtime: Done.
Mar 25 12:58:27.591 DEBUG runtime: block height: 11, is next_block_epoch_start false
Mar 25 12:58:27.591 DEBUG client: Sending an approval Endorsement(`DeWDHSEzzTF4Dp6ZYipwAUZ3egtrVpcH8LictRx9KGf3`) from test0 to test3 for 13
Mar 25 12:58:27.669 DEBUG client: Some("test0") Received block Eff2zpCxid5yNYRNTzqueUGwuCPsiMaj1nGFBZpTxHVz <- DeWDHSEzzTF4Dp6ZYipwAUZ3egtrVpcH8LictRx9KGf3 at 13 from ed25519:5BVKQ5zZKtEUM3AHgj6KAerTpuh3CVZHtAbmVfxLbLhc, requested: false
Mar 25 12:58:27.669 DEBUG chain: Process block header: Eff2zpCxid5yNYRNTzqueUGwuCPsiMaj1nGFBZpTxHVz at 13
Mar 25 12:58:27.670 DEBUG chain: Process block Eff2zpCxid5yNYRNTzqueUGwuCPsiMaj1nGFBZpTxHVz at 13, approvals: 3, me: Some("test0")
Mar 25 12:58:27.670 DEBUG chain: Some("test0") Process block Eff2zpCxid5yNYRNTzqueUGwuCPsiMaj1nGFBZpTxHVz, is_caught_up: false, need_to_start_fetching_state: false
Mar 25 12:58:27.670 DEBUG chain: Header head updated to Eff2zpCxid5yNYRNTzqueUGwuCPsiMaj1nGFBZpTxHVz at 13
Mar 25 12:58:27.671 DEBUG chain: Verifying challenges []
Mar 25 12:58:27.671 DEBUG runtime: block height: 13, is next_block_epoch_start false
Mar 25 12:58:27.671 DEBUG runtime: add validator proposals at block height 13 []
Mar 25 12:58:27.671 DEBUG chain: Head updated to Eff2zpCxid5yNYRNTzqueUGwuCPsiMaj1nGFBZpTxHVz at 13
Mar 25 12:58:27.671 DEBUG chain: Check orphans: from Eff2zpCxid5yNYRNTzqueUGwuCPsiMaj1nGFBZpTxHVz, # orphans 0
Mar 25 12:58:27.671 DEBUG client: Producing chunk at height 14 for shard 0, I'm test0
Mar 25 12:58:27.671 DEBUG runtime: Transaction filtering results 0 valid out of 0 pulled from the pool
Mar 25 12:58:27.671 DEBUG client: Produced chunk at height 14 for shard 0 with 0 txs and 0 receipts, I'm test0, chunk_hash: pqQVeJNRjxzjJxhQvH5We8G83LdMPWewrCngLdvh5dA
Mar 25 12:58:27.671 DEBUG chunks: Reconstructed and decoded chunk pqQVeJNRjxzjJxhQvH5We8G83LdMPWewrCngLdvh5dA, encoded length was 8, num txs: 0, I'm Some("test0")
Mar 25 12:58:27.671 DEBUG chunks: Reconstructed and decoded chunk C2zvH3Rg6PbEcYxP99bq73PJHjAW9zS2tP1Wv7zB7QQ2, encoded length was 8, num txs: 0, I'm Some("test0")
Mar 25 12:58:27.672 DEBUG chunks: Reconstructed and decoded chunk E8agy1baS9QUt52y9R1rDYipciUqpsgHnigwpdZmvKQx, encoded length was 8, num txs: 0, I'm Some("test0")
Mar 25 12:58:27.692 DEBUG client: Catchup me: Some("test0"): sync_hash: `DeWDHSEzzTF4Dp6ZYipwAUZ3egtrVpcH8LictRx9KGf3`, sync_info: {2: ShardSyncDownload { downloads: [], status: StateDownloadComplete }, 1: ShardSyncDownload { downloads: [], status: StateDownloadComplete }}
Mar 25 12:58:27.692 DEBUG chain: Verifying challenges []
Mar 25 12:58:27.692 DEBUG runtime: block height: 12, is next_block_epoch_start true
Mar 25 12:58:27.692 DEBUG epoch_manager: epoch id: EpochId(`BPifyBjxJWq4pwWnCKSzEj6a694JjDwAxwR6uP4qfpiA`), prev_epoch_id: EpochId(`F49EPnYw9MfdhabugbMr8Ywu85ziPPkpbZtCpZHTxsP`), prev_prev_epoch_id: EpochId(`11111111111111111111111111111111`)
Mar 25 12:58:27.692 DEBUG epoch_manager: prev_prev_stake_change: {"test0": 50000000000000000000000000000000, "test1": 50000000000000000000000000000000, "test2": 50000000000000000000000000000000, "test3": 50000000000000000000000000000000}, prev_stake_change: {"test0": 50000000000000000000000000000000, "test1": 50000000000000000000000000000000, "test2": 50000000000000000000000000000000, "test3": 50000000000000000000000000000000}, stake_change: {"test0": 50000009909309994923908913331185, "test1": 50000009909309994923908913331185, "test2": 50000009909309994923908913331185, "test3": 0}, slashed: {}
Mar 25 12:58:27.692 DEBUG epoch_manager: stake_info: {"test3": 50000000000000000000000000000000, "test2": 50000009909309994923908913331185, "test0": 50000009909309994923908913331185, "test1": 50000009909309994923908913331185}, validator_reward: {"test2": 9909309994923908913331185, "near": 7927447995939127130664947, "test1": 9909309994923908913331185, "test0": 9909309994923908913331185}
Mar 25 12:58:27.692 DEBUG runtime: account test3 stake 50000000000000000000000000000000 max_of_stakes: 50000000000000000000000000000000
Mar 25 12:58:27.692 DEBUG runtime: account test3 return stake 0
Mar 25 12:58:27.692 DEBUG runtime: account test0 adding reward 9909309994923908913331185 to stake 50000000000000000000000000000000
Mar 25 12:58:27.692 DEBUG runtime: account test0 stake 50000009909309994923908913331185 max_of_stakes: 50000009909309994923908913331185
Mar 25 12:58:27.692 DEBUG runtime: account test0 return stake 0
Mar 25 12:58:27.692 DEBUG runtime: block height: 12, is next_block_epoch_start true
Mar 25 12:58:27.692 DEBUG epoch_manager: epoch id: EpochId(`BPifyBjxJWq4pwWnCKSzEj6a694JjDwAxwR6uP4qfpiA`), prev_epoch_id: EpochId(`F49EPnYw9MfdhabugbMr8Ywu85ziPPkpbZtCpZHTxsP`), prev_prev_epoch_id: EpochId(`11111111111111111111111111111111`)
Mar 25 12:58:27.692 DEBUG epoch_manager: prev_prev_stake_change: {"test0": 50000000000000000000000000000000, "test1": 50000000000000000000000000000000, "test2": 50000000000000000000000000000000, "test3": 50000000000000000000000000000000}, prev_stake_change: {"test0": 50000000000000000000000000000000, "test1": 50000000000000000000000000000000, "test2": 50000000000000000000000000000000, "test3": 50000000000000000000000000000000}, stake_change: {"test0": 50000009909309994923908913331185, "test1": 50000009909309994923908913331185, "test2": 50000009909309994923908913331185, "test3": 0}, slashed: {}
Mar 25 12:58:27.692 DEBUG epoch_manager: stake_info: {"test2": 50000009909309994923908913331185, "test1": 50000009909309994923908913331185, "test3": 50000000000000000000000000000000, "test0": 50000009909309994923908913331185}, validator_reward: {"test2": 9909309994923908913331185, "near": 7927447995939127130664947, "test1": 9909309994923908913331185, "test0": 9909309994923908913331185}
Mar 25 12:58:27.692 DEBUG chain: Verifying challenges []
Mar 25 12:58:27.692 DEBUG runtime: block height: 13, is next_block_epoch_start false
Mar 25 12:58:27.692 DEBUG runtime: block height: 13, is next_block_epoch_start false
Mar 25 12:58:27.692 DEBUG chain: Catching up: removing prev=`DeWDHSEzzTF4Dp6ZYipwAUZ3egtrVpcH8LictRx9KGf3` from the queue. I'm Some("test0")
Mar 25 12:58:27.692 DEBUG chain: Catching up: removing prev=`Eff2zpCxid5yNYRNTzqueUGwuCPsiMaj1nGFBZpTxHVz` from the queue. I'm Some("test0")
Mar 25 12:58:27.692 DEBUG chain: Check orphans: from DeWDHSEzzTF4Dp6ZYipwAUZ3egtrVpcH8LictRx9KGf3, # orphans 0
Mar 25 12:58:27.692 DEBUG chain: Check orphans: from Eff2zpCxid5yNYRNTzqueUGwuCPsiMaj1nGFBZpTxHVz, # orphans 0
Mar 25 12:58:28.199 DEBUG client: Sending an approval Endorsement(`Eff2zpCxid5yNYRNTzqueUGwuCPsiMaj1nGFBZpTxHVz`) from test0 to test1 for 14
Mar 25 12:58:28.276 DEBUG client: Some("test0") Received block 5ogz8AamjDmRmFkeGQ8hinaEkiy6WyZBLa9ztrg677cK <- Eff2zpCxid5yNYRNTzqueUGwuCPsiMaj1nGFBZpTxHVz at 14 from ed25519:FXXrTXiKWpXj1R6r5fBvMLpstd8gPyrBq3qMByqKVzKF, requested: false
Mar 25 12:58:28.276 DEBUG chain: Process block header: 5ogz8AamjDmRmFkeGQ8hinaEkiy6WyZBLa9ztrg677cK at 14
Mar 25 12:58:28.277 DEBUG chain: Process block 5ogz8AamjDmRmFkeGQ8hinaEkiy6WyZBLa9ztrg677cK at 14, approvals: 3, me: Some("test0")
Mar 25 12:58:28.277 DEBUG chain: Some("test0") Process block 5ogz8AamjDmRmFkeGQ8hinaEkiy6WyZBLa9ztrg677cK, is_caught_up: true, need_to_start_fetching_state: false
Mar 25 12:58:28.277 DEBUG chain: Header head updated to 5ogz8AamjDmRmFkeGQ8hinaEkiy6WyZBLa9ztrg677cK at 14
Mar 25 12:58:28.277 DEBUG chain: Verifying challenges []
Mar 25 12:58:28.277 DEBUG runtime: block height: 14, is next_block_epoch_start false
Mar 25 12:58:28.277 DEBUG chain: Verifying challenges []
Mar 25 12:58:28.277 DEBUG runtime: block height: 14, is next_block_epoch_start false
Mar 25 12:58:28.277 DEBUG runtime: block height: 14, is next_block_epoch_start false
Mar 25 12:58:28.277 DEBUG runtime: add validator proposals at block height 14 []
Mar 25 12:58:28.277 DEBUG chain: Head updated to 5ogz8AamjDmRmFkeGQ8hinaEkiy6WyZBLa9ztrg677cK at 14
Mar 25 12:58:28.278 DEBUG chain: Check orphans: from 5ogz8AamjDmRmFkeGQ8hinaEkiy6WyZBLa9ztrg677cK, # orphans 0
Mar 25 12:58:28.278 DEBUG client: Producing chunk at height 15 for shard 0, I'm test0
Mar 25 12:58:28.278 DEBUG runtime: Transaction filtering results 0 valid out of 0 pulled from the pool
Mar 25 12:58:28.278 DEBUG client: Produced chunk at height 15 for shard 0 with 0 txs and 0 receipts, I'm test0, chunk_hash: DVdiVKu1zXqakgtfAYtECXJjvkV7MG1qHbJWh3nXSrqK
Mar 25 12:58:28.278 DEBUG chunks: Reconstructed and decoded chunk DVdiVKu1zXqakgtfAYtECXJjvkV7MG1qHbJWh3nXSrqK, encoded length was 8, num txs: 0, I'm Some("test0")
Mar 25 12:58:28.278 DEBUG chunks: Reconstructed and decoded chunk BnusFRu283CMVGuKXMkZYLwaZqjzLCmuiSggswTod6Dq, encoded length was 8, num txs: 0, I'm Some("test0")
Mar 25 12:58:28.278 DEBUG chunks: Reconstructed and decoded chunk 3gpJYV4ZS4FemxQ4rubkvfRgoJ7Umu6dCGuqTBX56Jx1, encoded length was 8, num txs: 0, I'm Some("test0")
Mar 25 12:58:28.805 DEBUG client: Sending an approval Endorsement(`5ogz8AamjDmRmFkeGQ8hinaEkiy6WyZBLa9ztrg677cK`) from test0 to test2 for 15
```
