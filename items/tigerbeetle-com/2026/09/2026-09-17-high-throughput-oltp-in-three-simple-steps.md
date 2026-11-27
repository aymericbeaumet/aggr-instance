---
title: High-Throughput OLTP in Three Simple Steps
link: https://tigerbeetle.com/blog/2026-09-17-performant-use-of-tigerbeetle
source: tigerbeetle-com
published: 2026-09-17T00:00:00Z
updated: 2026-09-17T00:00:00Z
first_seen: 2026-11-27T00:01:05.630507547Z
authors:
- Tobi
- Maxi
summary: 'TigerBeetle is a transaction-processing database. Its double-entry accounting primitives are simple and powerful: they can represent the movement or exchange of any quantity, including financial transactions such as real-time payments, billing, or credits, and non-financial transactions such as energy, inventory, or positions.'
content: extracted
html: 2026-09-17-high-throughput-oltp-in-three-simple-steps.html
remote_preview:
  url: https://tigerbeetle.com/blog/2026-09-17-performant-use-of-tigerbeetle/banner-e9cd62bd.webp
---

TigerBeetle is a transaction-processing database. Its double-entry accounting primitives are simple and powerful: they can represent the movement or exchange of any quantity, including financial transactions such as real-time payments, billing, or credits, and non-financial transactions such as energy, inventory, or positions.

If you’re coming from a general-purpose SQL (OLGP) database and translate your data model to TigerBeetle one-to-one, you might leave a lot of performance on the table. In this post, we use an [all-time favorite transaction-processing workload](https://jimgray.azurewebsites.net/papers/AMeasureOfTransactionProcessingPower.pdf) to illustrate why this is the case and how you can unlock a 100x performance improvement with TigerBeetle using three techniques: choosing the right data model and primitives, utilizing autobatching, and using TigerBeetle’s time-based identifiers.

Our sample [workload](https://www.tpc.org/TPC_Documents_Current_Versions/pdf/TPC-A_v2.0.0.pdf) models a bank with branches, tellers (ATMs), and customer accounts. Each transaction:

- changes a customer’s balance;
- records the transaction in a history table;
- changes the teller’s balance; and
- changes the branch’s balance.

This workload is conceptually simple but presents a performance challenge: transactions update only a few branches and tellers, causing contention.

In SQL-like pseudocode, each transaction looks like this:

```
begin transaction
                                              
  update account where account_id = aid:
    read account_balance from account
     set account_balance = account_balance + amount
   write account_balance to account
                                               
  update teller where teller_id = tid:
     set teller_balance = teller_balance + amount
   write teller_balance to teller

  update branch where branch_id = bid:
     set branch_balance = branch_balance + amount
   write branch_balance to branch
                                               
  write to history: aid, tid, bid, amount, timestamp
                                               
commit transaction
```

TigerBeetle represents the same logic differently. For this example, we represent the deposit as two linked transfers:

- value moves from the bank’s aggregate account to the branch account; and
- value moves from the teller account to the customer account.

The two transfers form one logical transaction and must either both succeed or both fail.

In Go-like pseudocode using the [TigerBeetle client](https://docs.tigerbeetle.com/coding/clients/go/), the logic looks like this:

```
transfers := []Transfer{
    {
        ID:              nextID(),
        DebitAccountID:  bankAccountID,
        CreditAccountID: branchAccountID,
        Amount:          amount,
        Flags:           Linked, // Succeed or fail together.
    },
    {
        ID:              nextID(),
        DebitAccountID:  tellerAccountID,
        CreditAccountID: customerAccountID,
        Amount:          amount,
    },
}

results := client.CreateTransfers(transfers)
```

The [`Linked`](https://docs.tigerbeetle.com/coding/linked-events/#linked-events) flag on the first transfer links it to the next transfer in the transfer batch. TigerBeetle treats the chain atomically: if either transfer fails, neither transfer is committed. Note that TigerBeetle is [immutable](https://docs.tigerbeetle.com/concepts/debit-credit/#immutability-is-essential) and automatically records the transfer history, so there is nothing further to enable or model.

Two details are important here.

First, `CreateTransfers` accepts a collection rather than a single transfer. TigerBeetle is designed to process [multiple operations](https://docs.tigerbeetle.com/concepts/performance/#batching-batching-batching) in one request.

Second, the bank account acts as an aggregate account. Its balance gives us the total amount represented across the bank without requiring a query that scans and sums every branch.

In a conventional design, updating one aggregate row from every transaction would create severe contention. TigerBeetle is designed specifically for high-contention workloads, so aggregate accounts are practical rather than prohibitive.

The debit/credit schema is powerful, and we use it to model highly complex use cases with our customers.

Changing the data model is half the migration. The next question is how to submit transactions efficiently.

Applications built on general-purpose (OLGP) SQL databases often use a connection pool. Each incoming request acquires a connection, performs one transaction, and releases the connection back to the pool. Increasing the number of concurrent active connections can increase throughput until the database reaches its contention, CPU, or I/O limit.

We can transfer this architecture directly to TigerBeetle (but don’t do this in practice!): For every application request, we create two linked transfers and submit them immediately. We use many [goroutines](https://go.dev/tour/concurrency/1) and multiple TigerBeetle client instances in an *attempt* to increase “concurrency”.

The result is disappointing: more clients don’t improve performance at all!

> Using the Right Tool Incorrectly Doesn’t Make It the Wrong Tool

What we tried to do just now is increase concurrency by using many clients. But we don’t need to; TigerBeetle already has an inherently concurrent interface: Batches!

A client request can contain up to 8 189 operations in one round trip, such as transfers in a `create_transfers` request or account IDs in a `lookup_accounts` request ([and more](https://docs.tigerbeetle.com/reference/requests/)). Batching amortizes the fixed costs of a single request (network, replication, processing), and it enables other cool things, such as auto-vectorization, efficient cache utilization, and batches as transactions (see linked transfers above).

So in the following, we use a single TigerBeetle client instance and instead scale the number of transactions within a batch:

This looks much better! With large batches, TigerBeetle processes 454 518 transactions per second; that is 909 162 transfers per second, since each logical transaction uses two transfers.

That’s great, but how do you achieve batching? Isn’t it a lot of code?

As it turns out, no: while you can collect these large batches yourself, as an application that performs bulk inserts might, you don’t need to.

TigerBeetle allows only one outstanding request per client. And sends this request immediately. No delay. As soon as the current request completes, the client submits the next batch. The client uses this time window to batch operations automatically. While waiting for the server’s response, it collects new operations and groups them into the next batch. This adds no artificial delay because batching occurs while the client is already waiting for the pending request to complete. No special batching logic is required in your application; TigerBeetle clients do this for you automatically. However, if you have batches in the rest of your system (and you should try to design your interfaces accordingly), you’ll go even faster.

Our Go example takes advantage of autobatching simply by spawning many goroutines that use the same TigerBeetle client instance. The TigerBeetle client is thread-safe for exactly this usage pattern, so you don’t need to wrap it in a `mutex`.

Most users can achieve their required throughput with a single client, though we recommend using multiple (e.g., 4) physically separate clients[^1] to avoid a single point of failure. If you need more than one client just for performance, you are either already working with us or S&P 500-scale – or both!

We’ll leave you with one more tip: One common mistake we see that leaves a lot of performance on the table is the use of random IDs (often `UUID`s) for transfers. In fact, if you do this, you’ll likely fall far short of the 909 162 transfers per second from above! To understand why this is the case, we need to dive into TigerBeetle’s internals a bit.

TigerBeetle helps you build correct systems by automatically checking the idempotency of every transfer ID you create. If the transfer ID already exists, TigerBeetle rejects the transfer, preventing duplicate transfers even when the client retries.

If new transfer IDs are always larger than older ones (monotonically increasing), this check is cheap: Just confirm that the new transfer ID is larger than all old ones. If, on the other hand, transfer IDs are random, TigerBeetle needs to search through its transfer index structure to confirm the transfer ID does not exist yet, which takes time.

So which 128-bit ID type should you use?

The TigerBeetle client provides an `ID()` method, which returns a `ULID`-like number we call [TigerBeetle Time-Based Identifier](https://docs.tigerbeetle.com/coding/data-modeling/#tigerbeetle-time-based-identifiers-recommended). `TBID`s combine many nice properties of `UUID`s while also ensuring monotonicity: a `TBID` is roughly a concatenation of a client’s local timestamp and a random number. If multiple transfers are created in the same millisecond, the `TBID`’s random component increments by one to generate the next ID; when the next millisecond starts, the `TBID` generator increments the timestamp component and generates a new random part. This ensures that, on the server side, incoming IDs are approximately monotonic, allowing for fast idempotency checks.

TigerBeetle could also make random IDs faster on average with Bloom filters, but we deliberately choose to optimize for the best usage pattern. Not being OLGP, TigerBeetle has a laser focus on making transaction processing as fast as possible, and that necessarily involves application and database co-design.

> It’s better to start with ordered IDs from day 1 of using TigerBeetle!

Using TigerBeetle correctly unlocks high-throughput performance.

By applying the three techniques outlined in this post, we reached roughly 450k transactions per second with TigerBeetle in our benchmark. By contrast, the relational database with stored procedures achieved about 7k transactions per second:

These figures should be treated as rough order-of-magnitude indicators: we naturally have more experience tuning TigerBeetle than the relational database used in the comparison[^2].

Still, the size of the gap is fundamentally architectural: TigerBeetle avoids row-level locking, has batched execution, and parallelizes I/O on the server.

[^1]: We usually refer to these as gateways: physically separate machines that host TigerBeetle clients. They act as application endpoints, collecting transfers and submitting them to the TigerBeetle database. We recommend deploying four gateways so the system can continue operating if one or more of them fail.

[^2]: We used stored procedures, connection pooling and set `shared_buffers` to 64 GiB.
