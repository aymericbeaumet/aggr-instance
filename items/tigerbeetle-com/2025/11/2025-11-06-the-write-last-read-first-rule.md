---
title: The Write Last, Read First Rule
link: https://tigerbeetle.com/blog/2025-11-06-the-write-last-read-first-rule
source: tigerbeetle-com
published: 2025-11-06T00:00:00Z
updated: 2025-11-06T00:00:00Z
first_seen: 2026-11-27T00:01:05.630507547Z
authors:
- Dominik Tornow
summary: 'TigerBeetle is a financial transactions database built for correctness. Yet, building a correct system from correct components remains a challenge:'
content: extracted
html: 2025-11-06-the-write-last-read-first-rule.html
remote_preview:
  url: https://tigerbeetle.com/blog/2025-11-06-the-write-last-read-first-rule/banner-2832e13d.webp
---

TigerBeetle is a financial transactions database built for correctness. Yet, building a correct system from correct components remains a challenge:

Composing systems, each correct in isolation, does not necessarily yield a correct system. In this post, we’ll explore how to maintain consistency in the absence of transactions, how to reason about correctness when intermediate states are externalized, and how to recover from partial failures.

TigerBeetle is a financial transactions database that offers two primitives for double-entry bookkeeping: accounts and transfers. A separate data store, such as Postgres, stores master data, such as name and address of the account holder or terms and conditions of the account.

This separation enables transfers to scale independently of general purpose master data (for example dealing with Black Friday events) and solves different security, compliance, or retention requirements of the independent data sets (for example enforce immutability of transfers).

![Architecture](https://tigerbeetle.com/blog/2025-11-06-the-write-last-read-first-rule/architecture-dark-50320be3.svg)\
Architecture

Just as a bank may have need for both a filing cabinet and a bank vault, Postgres specializes in strings and describing entities (master data), while TigerBeetle specializes in integers and moving integers between these entities.

A transaction is a sequence of operations ending in a commit, where all operations take effect, or an abort, where no operation takes effect. The completion is instant, intermediate state is not externalized and is not observable. Disruptions (such as process failure or network failure) are mitigated transparently.

![Transaction Boundaries](https://tigerbeetle.com/blog/2025-11-06-the-write-last-read-first-rule/transaction-boundary-1-dark-112d4f89.svg)

However, the sequential composition of two transactions is not itself a transaction. The completion of the entire sequence is (at best) eventual, intermediate state is externalized and observable. Disruptions are not mitigated transparently.

![Transaction Boundaries](https://tigerbeetle.com/blog/2025-11-06-the-write-last-read-first-rule/transaction-boundary-2-dark-72771bbd.svg)

Since Postgres and TigerBeetle do not share a transaction boundary, the application must ensure consistency through repeated attempts at completion and coordination, not transactions.

To reason about such coordination, we need to understand the guarantees we expect our system to uphold.

A system is characterized by a set of safety and liveness properties. A safety property states that nothing bad ever happens, while a liveness property states that something good eventually happens.

In this post, we will focus on two safety properties:

- **Consistent**\
   We consider the system consistent if every account in Postgres has an account in TigerBeetle, and vice versa.

```
Consistent =
  ∧ ∀ a₁ ∈ PG: ∃ a₂ ∈ TB: id(a₁) = id(a₂)
  ∧ ∀ a₁ ∈ TB: ∃ a₂ ∈ PG: id(a₁) = id(a₂)
```

- **Traceable**\
   We consider the system traceable if every account in TigerBeetle with a positive balance corresponds to an account in Postgres.

```
Traceable = ∀ a₁ ∈ TB: balance(a₁) > 0 => ∃ a₂ ∈ PG: id(a₁) = id(a₂)
```

In the absence of transactions, the system may be *temporarily* inconsistent. However, the system must *always* remain traceable to avoid the possibility of losing—or, more precisely, orphaning—money.

In the absence of transactions, we need to make an explicit architectural decision that transactions used to make implicitly: Which system determines the existence of an account? In other words: Which system is the source of truth?

We must designate a:

- **System of Record**. The champion. If the account exists here, the account exists on a system level.

- **System of Reference**. The supporter. If the account exists here but not in the system of record, the account does not exist on a system level.

So which system is the system of record and which is the system of reference? That is an architectural decision that depends on your requirements and the properties of the subsystems. In this case, TigerBeetle is the system of record:

- If the account is present in Postgres, the account is not able to process transfers, so the account in Postgres merely represents a staged record.

- If the account is present in TigerBeetle, the account is able to process transfers, so the account in TigerBeetle represents a committed record.

In other words, as soon as the account is created in TigerBeetle, the account exists system wide.

Once the system of record is chosen, correctness depends on performing operations in the right order.

Since the system of reference doesn’t determine existence, we can safely write to it first without committing anything. Only when we write to the system of record does the account spring into existence.

Conversely, when reading to check existence, we must consult the system of record, because reading from the system of reference tells us nothing about whether the account actually exists.

**This principle—Write Last, Read First—ensures that we maintain application level consistency.**

Remarkably, if the system of record provides strict serializability, like TigerBeetle, and if ordering is correctly applied, then the system as a whole preserves strict serializability, leading to a delightful developer experience.

Choosing the correct system of record and the correct order of operations is not just a philosophical exercise. If we designate the wrong system as the source of truth and perform operations in the wrong order, we may quickly violate safety properties.

For example, if we create the account in TigerBeetle but not in Postgres, the system may start processing transfers without containing any information about who this account belongs to. If the system crashes and forensics do not surface the necessary information to establish ownership, we violated the golden rule: traceability.

However, if we create the account in Postgres but subsequently not in TigerBeetle, no harm, no foul. Any transfer attempt is simply rejected by TigerBeetle, money cannot flow to an account that doesn’t exist in the ledger.

Clients interact with the system exclusively via the Application Programming Interface exposed by the application layer, which in turn interacts via the interfaces exposed by the subsystems, Postgres and TigerBeetle.

The Application Programming Interface has two responsibilities, Orchestration and aggregation: The API determines the order of operations and aggregates operation results into application level semantics.

We will implement the API with Resonate’s durable execution framework, Distributed Async Await. Distributed Async Await guarantees eventual completion simplifying reaching consistency even in the absence of transactions.

Resonate guarantees eventual completion via language integrated checkpointing and reliable resumption in case of disruptions: Executions resume where they left off by restarting from the beginning and skipping steps that have already been recorded (see Figure 4.)

![Idempotence](https://tigerbeetle.com/blog/2025-11-06-the-write-last-read-first-rule/idempotence-dark-94ff03b9.svg)

However, we must consider a subtle issue inherent to checkpointing: In the event of a disruption, after performing an operation but before recording its completion, the operation will be performed again.

**Therefore, every operation must be idempotent, i.e. the repeated application of an operation does not have any effects beyond the initial application.**

For each subsystem, Postgres and TigerBeetle, we implement an idempotent function to create an account. In our case, both the Postgres and TigerBeetle account creation functions return whether the account was created, already existed with the same values, or already existed with different values:

```
type Result =
  | { type: “created” }
  | { type: “exists_same” }
  | { type: “exists_diff” }

// Create account in Postgres
async function pgCreateAccount(uuid: string, data: any): Promise<Result>

// Create Account in TigerBeetle
async function tbCreateAccount(uuid: string, data: any): Promise<Result>
```

The Listing below illustrates `tbCreateAccount`.

```
async function tbCreateAccount(context: Context, guid: number) {

  const client = context.getDependency("client");

  // Construct account object
  const account: Account = {
    ...
  };

  const errors = await client.createAccounts([account]);

  // Success case: account was created
  if (errors.length === 0) {
    return { type: “created” };
  }

  const error = errors[0];

  // Account exists with the same properties (idempotent)
  if (error.result === CreateAccountError.exists) {
    return { type: “exists_same” };
  }

  // Account exists with different properties
  if (
    error.result === CreateAccountError.exists_with_different_flags ||
    error.result === ...)
    return { type: “exists_diff” };
  }

  // For any other error, throw
  throw new Error(`Failed to create account: ${JSON.stringify(error)}`);
}
```

At the system level, the application composes these idempotent building blocks, interpreting subsystem responses and translating them into application-level semantics.

For example, Postgres or TigerBeetle may return that an account already exists but with different values. The application layer must determine whether this represents a success or a failure, translating platform level semantics into application level semantics.

In our case, because operations may be repeated, both created or already exists with the same values constitutes success, but exists with different values indicates a bug in the system. Additionally, due to write last, read first, if the Postgres account was created but the TigerBeetle account already existed, an ordering violation occurred.

| Postgres Result | TigerBeetle Result | System Result | Scenario          |
| --------------- | ------------------ | ------------- | ----------------- |
| Created         | Created            | Success       |                   |
| Created         | Exists/Same        | Panic         | Violates ordering |
| Created         | Exists/Diff        | Panic         | Violates ordering |
| Exists/Same     | Created            | Success       | Recovery          |
| Exists/Same     | Exists/Same        | Success       | Recovery          |
| Exists/Same     | Exists/Diff        | Panic         | Conflict          |
| Exists/Diff     | Any                | Panic         | Conflict          |

The Listing below illustrates `createAccount`:

```
function* createAccount(context: Context, uuid: string, data: any) {

  // Generate internal account ID for TigerBeetle
  const guid = yield* context.run(generateId);

  // Create account in Postgres
  const pgResult =
    yield* context.run(pgCreateAccount, uuid, { ...data, guid: guid });

  // Panic and alert the operator if the account exists
  // but with different values
  yield* context.panic(pgResult.type == “exists_diff”);

  // Create account in TigerBeetle
  const tbResult =
    yield* context.run(tbCreateAccount, guid);

  // Panic and alert the operator if the account exists
  // but with different values
  yield* context.panic(tbResult.type == “exists_diff”);

  // Panic and alert the operator if ordering was violated
  yield* context.panic(pgResult.type == "created" &&
                       tbResult.type == “exists_same”);

  return {uuid, guid};

}
```

We need to detect and mitigate correctness violations, here by refusing to proceed and inform the operator.

To create an account, the developer executes createAccount with a unique id, for example the account id

```
await resonate.run(`create-${uuid}`, createAccount, uuid, {...});
```

The unique identifier `create-${uuid}` assigned a unique identity to the top level execution and transitively to every sub level execution, ensuring consistent checkpointing and eliminating the need for explicit recovery logic.

In the absence of transaction, we turn to coordination to ensure the correctness of our applications. A powerful framework to reason about correctness is to delineate the system into system of record and system of reference and ordering operations according to your requirements.

While correctness still requires careful design, with durable executions, intentional ordering, and idempotent operations, we can build correct systems from correct components.

Note: To run the example, visit the GitHub repository at [https://github.com/resonatehq-examples/example-tigerbeetle-account-creation-ts](https://github.com/resonatehq-examples/example-tigerbeetle-account-creation-ts).

Thanks to [Dominik Tornow](https://www.dtornow.com), Founder and CEO of [Resonate](https://www.resonatehq.io), for penning this guest post!
