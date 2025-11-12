# Consensus

At a glance:

* **Goal:** Achieve high throughput and fast recovery for block finality.
* **Protocol:** Autobahn (Hyli’s consensus layer).
* **How:** Parallel data lanes + BFT-based snapshot consensus.
* **Benefits:** Scales horizontally, tolerates faults, and recovers instantly.

## Overview

Autobahn is Hyli’s next-generation consensus protocol. It ensures high throughput and seamless recovery, even under network disruption or validator faults.

Unlike traditional consensus mechanisms that stall during leader failures or latency spikes, Autobahn separates **data dissemination** from **consensus finalization**. This allows Hyli to process large volumes of data with stable finality.

## Why traditional consensus falls short

Most legacy protocols (PBFT, HotStuff, Tendermint) assume failures are rare and fast to recover from.
In practice, they face:

* **Network disruptions** and latency variance.
* **Leader bottlenecks** in message propagation.
* **Post-fault hangovers**, where uncommitted data must be reprocessed.

These constraints limit scalability and responsiveness.

## How Autobahn works

Autobahn introduces a **two-layer architecture**:

| Layer                  | Purpose                                        | Key Feature                                              |
| - | - | -- |
| **Data dissemination** | Validators broadcast data in parallel “lanes.” | Each validator maintains its own lane.                   |
| **Consensus**          | Finalizes snapshots of all lanes (cuts).       | Message size remains constant regardless of data volume. |

![Data lanes diagram](../assets/img/autobahn/data-lanes.png)

### Data dissemination: parallel lanes

Each validator maintains a **lane**, a local chain of data proposals. Availability is guaranteed before consensus through **proofs of availability (PoA)** signed by one-third of validators.

Why this matters:

* **No waiting for data:** Validators confirm availability upfront.
* **Batch finalization:** Consensus can finalize many blocks at once.
* **Fast recovery:** Even if a leader fails, other validators continue broadcasting.

### Consensus: finality with tip cuts

A rotating leader collects the latest PoAs and hashes for each lane and proposes a **cut** — a snapshot representing progress across all lanes.

All validators participate in a two-round BFT vote to finalize the cut.

![Cut proposal diagram](../assets/img/autobahn/data-lane-cut.png)

Once finalized, all blocks before the cut are considered committed.

## Developer benefits

* **Predictable throughput** for high-volume apps.
* **Instant recovery** from network issues.
* **Cheaper node operation** due to lightweight message structure.
* **Low latency** when combined with [pipelined proving](./pipelined-proving.md).

## Performance and scaling

Autobahn scales horizontally: the more validators you have, the more data lanes exist.
This increases bandwidth proportionally to network size.

Benchmarks and ongoing testnet data will be published after Hyli’s current testnet phase.
See [Sei Labs’ Autobahn benchmark](https://blog.sei.io/sei-giga-achieving-5-gigagas-with-autobahn-consensus/) for early results.

## Security model

Autobahn ensures **safety and liveness** under partial asynchrony:

* Safe if **≥⅔** validators are honest.
* PoA signatures are **threshold-based** and **slashable**.
* Byzantine faults are mitigated through snapshot consensus.

## Resources

* [Autobahn whitepaper (arXiv)](https://arxiv.org/abs/2401.10369)
* [Commonware: Ordered Broadcast](https://commonware.xyz/blogs/commonware-runtime.html)
* [Sei Labs Autobahn Benchmark](https://blog.sei.io/content/images/size/w2000/2025/02/5362633a-39ec-45ec-a66b-0e72fda1a11e.png)
