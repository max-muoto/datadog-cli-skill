# DataDog Agent Skill

Agent skill for interacting with Datadog's [pup CLI](https://github.com/datadog-labs/pup). Focuses on giving the agent the most relevant set of Datadog `pup` commands for querying logs, metrics, and dashboards while leaving commands less likely to be used (e.g., managing synthetic tests, users, organizations) to a separate reference document the agent can reference as needed, to avoid overpopulating the context window initially.

Currently DataDog's CLI is lacking some key features including support for traces/profiling, so this skill will need to evolve with the growing feature-set of `pup` over time.
