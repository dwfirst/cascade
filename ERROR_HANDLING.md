

Architecture Resilience & UI/UX States


1. Asynchronous Execution Network

To ensure system stability under high-concurrency loads, the entire pipeline must operate within a non-blocking asynchronous loop using aiogram 3.x and aiohttp. No step in the cascade may block the main thread.
• API Isolation: Every request to remote LLM endpoints (OpenAI, Anthropic, DeepSeek) must be wrapped in an individual async/await handler.
• Timeout Thresholds: A strict 30-second timeout limit is enforced per cascade stage. If a stage fails to return data within this window, a GatewayTimeout exception is triggered.

2. Fallback Router (Redundancy Matrix)

If a primary target engine drops, fails, or throws a rate-limit error, the pipeline dynamically reroutes the data to an equivalent backup model via an API gateway (e.g., OpenRouter).

Cascade Stage	Primary Model	Failover Backup Model	Action Pattern
Stage 1 (Parsing)	deepseek-chat	gpt-4o-mini	Instant hot-swap; log error internally.
Stage 2 (Constraints)	claude-3-5-sonnet	gpt-4o	Recalculate prompt parameters under failover context.
Stage 3 (Assembly)	deepseek-reasoner	o1-mini	Maintain strict reasoning token allocation.
Stage 4 (Execution)	claude-3-opus	o1	Execute compiled capsule on alternative flagship engine.


3. UI/UX State Machine & Menu Interface

The Telegram bot layout is kept strictly minimalist to focus purely on the optimization loop.
• [ Run Cascade ] — Primary entry point. Triggers the 4-step asynchronous loop. Displays an active loading state (e.g., Deconstructing... -> Building Perimeter... -> Assembling...) to manage user expectations.
• [ Show Prompt ] — Toggle Switch (State: True/False). If enabled, the bot injects the raw compiled text output from Stage 3 into the chat prior to sending the final flagship answer. Used for monitoring semantic fidelity.
• [ Latency Log ] — Metric Display. Appends a micro-footprint under the final response showing the precise execution time (in milliseconds) for each individual tier and total token consumption costs.
