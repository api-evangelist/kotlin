---
title: "Introducing Koog Integration for Spring AI: Smarter Orchestration for Your Agents"
url: "https://blog.jetbrains.com/ai/2026/04/introducing-koog-integration-for-spring-ai-smarter-orchestration-for-your-agents/"
date: "Thu, 16 Apr 2026 14:01:57 +0000"
author: "Maria Tigina"
feed_url: "https://blog.jetbrains.com/kotlin/feed/"
---
<p>Spring AI is the application-facing integration layer you may already use. <a href="https://www.jetbrains.com/koog/" rel="noreferrer noopener" target="_blank">Koog</a> is the next layer up when you need agent orchestration. Spring AI already covers the chat model API, chat memory, and vector storage for RAG, and it provides Spring Boot starters with auto-configuration. Koog’s role is not to erase that, but rather to add a stronger agent runtime, offering:</p>



<ul>
<li>Multi-step strategies and workflows for more precise control.</li>



<li>Persistence and checkpoints for fault-tolerant execution.</li>



<li>Sophisticated history management for cost-optimization.</li>



<li>Automated deterministic planning.</li>
</ul>



<p>You can now get the best of both worlds. <strong>Koog offers seamless Spring AI integration</strong> and can be easily layered on top as a higher-level agentic runtime.</p>



<h2 class="wp-block-heading">Spring AI</h2>



<p>If you already use <a href="https://spring.io/projects/spring-ai" rel="noreferrer noopener" target="_blank">Spring AI</a>, you&#8217;re familiar with its broad integration landscape: 13+ LLM providers, 18+ vector databases, and 10+ chat memory backends, all built seamlessly into the Spring ecosystem.</p>



<p>Your application likely already relies on some of these integrations and wasn&#8217;t built in isolation. But as your agent’s complexity increases and business requirements demand more reliability, you start needing things that sit above the integration layer, for example, controlled execution logic, guardrails, fault tolerance, and cost optimization. These are the problems <a href="https://www.jetbrains.com/koog/" rel="noreferrer noopener" target="_blank">Koog</a> was built to solve.</p>



<figure class="wp-block-table"><table><tbody><tr><td><strong>Capability</strong></td><td><strong>Spring AI</strong></td><td><strong>Koog</strong></td></tr><tr><td><strong>LLM providers</strong></td><td>✅ 13+</td><td>✅ 16+</td></tr><tr><td><strong>Streaming</strong></td><td>✅ Supported</td><td>✅ Supported</td></tr><tr><td><strong>Tool calling</strong></td><td>✅ Supported</td><td>✅ Supported</td></tr><tr><td><strong>Database integrations</strong></td><td>✅ 10+ (e.g. PostgreSQL and MongoDB)</td><td>Uses the underlying ecosystem with a few integrations provided out of the box (e.g. Postgres)&nbsp;</td></tr><tr><td><strong>Vector databases</strong></td><td>✅ 18+ (e.g. Milvus, Weaviate, and PGvector)</td><td>✅ Uses underlying integrations</td></tr><tr><td><strong>RAG (retrieval-augmented generation)</strong></td><td>✅ Supported via advisors and VectorStore</td><td>✅ Supported and integrated into agent workflows</td></tr><tr><td><strong>Chat memory (short-term)</strong></td><td>✅ Supported</td><td>✅ Supported</td></tr><tr><td><strong>Long-term memory</strong></td><td>✅ Supported via vector DB integrations</td><td>✅ Built-in and pluggable (semantic and structured memory)</td></tr><tr><td><strong>Observability</strong></td><td>✅ Basic observability from the Spring ecosystem (Micrometer, etc.),&nbsp;not tailored for LLM or AI observability tooling</td><td>✅ OpenTelemetry support, built-in tailored support for popular LLM or AI observability tooling (e.g. Langfuse, W&amp;B Weave, and Datadog)</td></tr><tr><td><strong>Parallel execution</strong></td><td>❌ Limited, manual</td><td>✅ Native (coroutines and concurrent node execution)</td></tr><tr><td><strong>Agent strategies</strong></td><td>❌ Basic (prompt chaining and tool calling)</td><td>✅ Advanced type-safe graph workflows (multi-step reasoning, branching, tool orchestration, domain modeling approach), advanced planners (LLM-based and GOAP)</td></tr><tr><td><strong>Persistence</strong></td><td>❌ Not built in, only the message history can be saved</td><td>✅ Built-in advanced persistence for the agent’s logic and state</td></tr><tr><td><strong>History compression</strong></td><td>❌ Not built in</td><td>✅ Native support with out-of-the-box advanced strategies (summarization, pruning, and token optimization)</td></tr></tbody></table></figure>



<p>The good news is you don&#8217;t have to choose one or the other, or dramatically change your existing setup to get there. <strong>Koog’s new Spring AI</strong> integration lets you <strong>keep your current LLM providers and databases exactly as they are</strong>, while writing your agents in Koog with minimal configuration changes. Your integration layer stays intact. <strong>Koog simply adds a powerful orchestration runtime on top of it.</strong></p>



<p>Let&#8217;s take a look at how it works. This post uses a Kotlin and Gradle setup for simplicity, but you can also use the recently released native Java Koog API (and, of course, Maven).</p>



<h2 class="wp-block-heading">Koog’s Spring AI integration</h2>



<p>Let&#8217;s say your Spring project already uses three common Spring AI interfaces: <code>ChatModel</code>, <code>ChatMemoryRepository</code>, and <code>VectorStore</code>. Adding Koog on top is just a three-step process.</p>



<p><strong>Step 1:</strong> Keep your existing Spring AI dependencies.</p>



<p></p>



<pre class="EnlighterJSRAW">// LLM
implementation("org.springframework.ai:spring-ai-starter-model-openai")

// Chat memory
implementation("org.springframework.ai:spring-ai-starter-model-chat-memory-repository-jdbc")

// Vector store
implementation("org.springframework.ai:spring-ai-starter-vector-store-pgvector")</pre>



<p><strong>Step 2:</strong> Add the Koog integration dependencies.</p>



<pre class="EnlighterJSRAW">// Koog
implementation("ai.koog:koog-agents-jvm:0.8.0")

// Bridges ChatModel to Koog's LLMClient / PromptExecutor
implementation("ai.koog:koog-spring-ai-starter-model-chat:0.8.0")

// Bridges ChatMemoryRepository to Koog's ChatHistoryProvider
implementation("ai.koog:koog-spring-ai-starter-chat-memory:0.8.0")

// Bridges VectorStore to Koog's KoogVectorStore
implementation("ai.koog:koog-spring-ai-starter-vector-store:0.8.0")</pre>



<p><strong>Step 3:</strong> Use the auto-configured Koog beans. Each Koog starter automatically exposes a Spring bean that wraps your existing Spring AI bean:</p>



<figure class="wp-block-table"><table><tbody><tr><td><strong>Spring AI interface</strong></td><td><strong>Koog bean(s)</strong></td></tr><tr><td><code>ChatModel</code>&nbsp;</td><td><code>PromptExecutor</code>, <code>LLMClient</code></td></tr><tr><td><code>ChatMemoryRepository</code></td><td><code>ChatHistoryProvider</code></td></tr><tr><td><code>VectorStore</code></td><td><code>KoogVectorStore</code></td></tr></tbody></table></figure>



<p>The beans are auto-configured by default when there is a single matching Spring AI candidate, so your existing Spring AI application config stays untouched.</p>



<p>That&#8217;s it for setup. Now let&#8217;s walk through what you can build. To make things concrete, we&#8217;ll use a customer support agent as our running example and progressively add capabilities.</p>



<h2 class="wp-block-heading">When a pure Spring AI agent reaches its limit</h2>



<p>One version of an agent that you could build in pure Spring AI would look like this:</p>



<pre class="EnlighterJSRAW">@Service
class CustomerSupportService(
    chatClientBuilder: ChatClient.Builder,
    vectorStore: VectorStore,
    chatMemory: ChatMemory,
) {
    
    // Build a fully configured ChatClient once at construction time
    private val chatClient: ChatClient = chatClientBuilder
        .defaultSystem("""
            You are an e-commerce support assistant.
            Be concise and policy-aware.
            Never invent order data.
            If order context is missing for an order-specific request, ask for it.
        """.trimIndent())
        .defaultAdvisors(
            // Vector store RAG advisor – enriches every prompt with relevant docs
            QuestionAnswerAdvisor(
                vectorStore,
                SearchRequest.builder()
                    .topK(4)
                    .similarityThreshold(0.7)
                    .build()
            ),
            // Sliding-window chat memory advisor – keeps last N turns per session
            MessageChatMemoryAdvisor(chatMemory)
        )
        .build()
    
    suspend fun createAndRunAgent(userPrompt: String, sessionId: String): String? =
        chatClient.prompt()
            .user(userPrompt)
            // Scope memory to session
            .advisorParam(ChatMemory.CONVERSATION_ID, sessionId)
            .call()
            .tools()
            .content()
}</pre>



<p>This agent implements a simple tool-calling loop that runs on top of the LLM defined in the config and inserted as a <code>ChatClient</code>. Besides this, the agent has two features. The first is <code>QuestionAnswerAdvisor</code>, which is built on top of <code>VectorStore</code> and behaves like RAG, enriching the conversation with relevant information from external docs. The second is <code>ChatMemory</code>, which keeps only a specified number of messages, helping you control the number of messages in a conversation and save tokens.</p>



<p>But what if we don’t want a window of messages but a message history summary instead? Or, increasing complexity, what if, instead of a primitive tool-calling agentic loop, we wanted a more controllable and tailored strategy with different e-commerce support scenarios, or persistence and durable execution to make our agent fault-tolerant? <strong>This is where we reach the limits of Spring AI.</strong> But these, and many other agentic features, <strong>already exist in Koog and, thanks to the integration, they can easily be built on top of what you&#8217;ve already set up for Spring AI in your project</strong>.&nbsp;</p>



<h2 class="wp-block-heading">What does Koog’s Spring AI integration enable?</h2>



<p>First of all, this is what our e-commerce agent would look like in Koog.</p>



<pre class="EnlighterJSRAW">@Service
class CustomerSupportService(
    private val promptExecutor: PromptExecutor,
    private val chatStorage: ChatHistoryProvider,
    private val knowledgeBase: SearchStorage&lt;TextDocument, SimilaritySearchRequest>
) {

    suspend fun createAndRunAgent(userPrompt: String): String {
        val agentConfig = AIAgentConfig(
            prompt = prompt("ecommerce-support") {
                system(
                    """
                        You are an e-commerce support assistant.
                        Be concise and policy-aware.
                        Never invent order data.
                        If order context is missing for an order-specific request, ask for it.
                    """.trimIndent()
                )
            },
            model = OpenAIModels.Chat.GPT5Nano,
            maxAgentIterations = 100
        )

        val toolRegistry = ToolRegistry {
            tools(EcommerceSupportTools())
        }

        val agent = AIAgent(
            promptExecutor = promptExecutor,
            agentConfig = agentConfig,
            toolRegistry = toolRegistry,
	    // Simple tool-calling loop strategy
	    strategy = singeRunStrategy()
        ) {

            // Vector store RAG advisor – enriches every prompt with relevant docs
            install(LongTermMemory) {
                retrieval {
                    storage = knowledgeBase
                    searchStrategy = SimilaritySearchStrategy(
                        topK = 4,
                        similarityThreshold = 0.70
                    )
                    promptAugmenter = UserPromptAugmenter()
                }
            }

            // Sliding-window chat memory advisor – keeps last N turns per session
            install(ChatMemory) {
                chatHistoryProvider = chatStorage
                windowSize(20)
            }
        }

        return agent.run(userPrompt)
    }
}</pre>



<p>With Koog&#8217;s Spring AI integration, the <code>PromptExecutor</code> bean is auto-configured from your existing Spring AI <code>ChatModel</code>. You inject it directly into your service – no boilerplate configuration class needed.</p>



<p>The same is true for the doc database and chat memory storage features. You don’t need to make any changes to the application config. With Koog beans, they are seamlessly injected into Koog’s <code>LongTermMemory</code> and <code>ChatMemory</code> and used under the hood.</p>



<h2 class="wp-block-heading">What can you add on top?</h2>



<h3 class="wp-block-heading">Controllable type-safe workflows</h3>



<p>Simple LLM loops lack predictability and control for enterprise scenarios. Each iteration is opaque. You can&#8217;t branch based on tool results, retry failed steps, or enforce specific conversation flows. For production support agents handling refunds, escalations, or multi-step verifications, you need explicit control over the execution path.</p>



<p>With Koog, in addition to using predefined strategies (such as default loop or ReAct), you can customize a strategy using graphs:</p>



<pre class="EnlighterJSRAW">val agent = AIAgent(
    promptExecutor = promptExecutor,
    agentConfig = agentConfig,
    toolRegistry = toolRegistry,
    // Graph strategy, can accept and return anything!
    strategy = strategy&lt;String, String>("ecommerce_support") {
        // Define graph here
    }
)</pre>



<p>Instead of putting all of the instructions in a single naive text prompt, the best way to do this is to use structured output and then append an intent-specific prompt to it. This approach reduces the amount of context and gives you more control.&nbsp;</p>



<pre class="EnlighterJSRAW">@SerialName("SupportIntent")
@Serializable
enum class SupportIntent {
    ORDER_STATUS,
    CHANGE_ADDRESS,
    REFUND,
    OTHER
}

@Serializable
@LLMDescription("Normalized support request extracted from a user message.")
data class SupportRequest(
    @property:LLMDescription("Detected support intent")
    val intent: SupportIntent,

    @property:LLMDescription("Order ID if present, otherwise null")
    val orderId: String? = null,
)

val graphStrategy = strategy&lt;String, String>("ecommerce_support") {
    // 1) Detect the intent of the request from user message
    val classifyRequest by nodeLLMRequestStructured&lt;SupportRequest>(
        examples = listOf(
            SupportRequest(
                intent = SupportIntent.ORDER_STATUS,
                orderId = "84721",
                userRequest = "Check the status of order 84721"
            )
        )
    )
}</pre>



<p>Once you know the intent, you can append intent-specific instructions or narrow down the required tools and delegate the task to a subgraph with a tool-calling loop:</p>



<pre class="EnlighterJSRAW">val graphStrategy = strategy&lt;String, String>("ecommerce_support") {
    ...
    // 2) Check that all request information is provided
    val checkRequest by node&lt;SupportRequest, CheckRequestResult> { request ->
        when {
            request.intent == SupportIntent.OTHER ->
                CheckRequestResult(
                    request = request,
                    needsMoreInfo = true,
                    clarificationQuestion = "Specify the intent: order status, refund, change address?"
                ) 
            ...
            else ->
                CheckRequestResult(
                    request = request,
                    needsMoreInfo = false
                )
        }
    }

    // 3a) Process order status request in separate subgraph with additional prompt (or tools subset)
    val orderStatusFlow by subgraphWithTask&lt;SupportRequest, String>(
        tools = EcommerceSupportTools().asTools()
    ) { req ->
        """
            Handle this request as an ORDER STATUS case.
            Use the order status tool and then answer the user clearly.
            Request: ${req.userRequest}
            Order ID: ${req.orderId}
        """.trimIndent()
    }

    // 3b) Process other intents
    ...
}</pre>



<p>Finally, you can organize each step of your workflow into a graph using type-safe edges and conditions that control your agent&#8217;s behavior:</p>



<pre class="EnlighterJSRAW">val graphStrategy = strategy&lt;String, String>("ecommerce_support") {
    ...
    // Chain all nodes by edges
    edge(nodeStart forwardTo classifyRequest)
    edge(classifyRequest forwardTo checkContext 
        onCondition { it.isSuccess } 
        transformed { it.getOrThrow().data }
    )
    edge(classifyRequest forwardTo nodeFinish 
        onCondition { it.isFailure }
        transformed { "Failed to classify request." }
    )
    // If more information is required
    edge(checkContext forwardTo nodeFinish
        onCondition { it.needsMoreInfo }
        transformed { it.clarificationQuestion }
    )
    // If we know the intent
    edge(checkContext forwardTo orderStatusFlow
        // Add intent == SupportIntent.ORDER_STATUS condition for the transition  
        onCondition { request.intent == SupportIntent.ORDER_STATUS }
        transformed { it.request }
    )
    ...
    edge(orderStatusFlow forwardTo nodeFinish)
}</pre>



<p>You have complete freedom to experiment and make the agent as complex as you need it to be.</p>



<figure class="wp-block-image size-full"><img alt="" class="wp-image-701879" src="https://blog.jetbrains.com/wp-content/uploads/2026/04/image-37.png" style="width: 100% !important; height: auto !important;" /></figure>



<h3 class="wp-block-heading">Persistence (durable execution)</h3>



<p>There&#8217;s complex logic at play, so you need to be extremely careful not to lose the execution point and state. And thanks to graphs, that&#8217;s possible. Just install and configure the <code>Persistence</code> feature, which will also use the data source from Spring!</p>



<pre class="EnlighterJSRAW">@Service
class CustomerSupportService(
    private val dataSource: DataSource,
    ...
) {
    ...
    val agent = AIAgent(
        ...
    ) {
        // Make agent fault-tolerant using Koog's persistence.
        // The agent will recover from the exact graph node where it crashed
        install(Persistence) {
            // Configure where to store the checkpoints:
            storage = PostgresJdbcPersistenceStorageProvider(dataSource)
        }
    }
}</pre>



<p>The persistence feature allows the agent to recover from the exact graph node where it failed and continue execution, which is essential for building reliable services.</p>



<h3 class="wp-block-heading">History compression</h3>



<p>Once you start scaling your AI agents to millions of users and longer-running sessions, managing LLM costs becomes critical. Each step of the agent&#8217;s execution, typically a tool call or an LLM request, adds to the message history, and every token has a price. Beyond cost, every model has a context window limit that&#8217;s easy to hit when processing large documents, handling tool outputs, or running extended sessions.</p>



<p>You don&#8217;t want to silently drop earlier messages when the window fills up. But you also don&#8217;t want to pay for irrelevant tokens or risk the model losing important context. Instead of dropping the history, you can replace it with a summary:</p>



<pre class="EnlighterJSRAW">private fun AIAgentGraphContextBase.tooManyTokensSpent(): Boolean = 
    llm.prompt.latestTokenUsage > 1000

val graphStrategy = strategy&lt;String, String>("ecommerce_support") {
    ...
    // Compress history node with compression strategy
    val compressLLMHistory by nodeLLMCompressHistory&lt;String>(
        // Substitute every 5 messages with TL;DRs
        strategy = HistoryCompressionStrategy.Chunked(5)
    )
    // Do nothing node for navigation only
    val maybeCompressHistory by nodeDoNothing&lt;String>()

    edge (orderStatusFlow forwardTo maybeCompressHistory)
    edge (maybeCompressHistory forwardTo compressLLMHistory 
        onCondition { tooManyTokensSpent() }
    )
    edge (maybeCompressHistory forwardTo nodeFinish 
        onCondition { !tooManyTokensSpent() }
    )
    edge (compressLLMHistory forwardTo nodeFinish)
}</pre>



<p>After adding history compression into the strategy, our updated graph workflow for the&nbsp; e-commerce agent would look like this:</p>



<figure class="wp-block-image size-full"><img alt="" class="wp-image-701779" src="https://blog.jetbrains.com/wp-content/uploads/2026/04/image-36.png" style="width: 100% !important; height: auto !important;" /></figure>



<p>Check out the full example in <a href="https://github.com/JetBrains/koog/blob/develop/examples/spring-ai-kotlin/src/main/kotlin/com/example/spring_ai_kotlin/service/customersupport/CustomerSupportGraphService.kt" rel="noreferrer noopener" target="_blank">Kotlin</a> or <a href="https://github.com/JetBrains/koog/blob/develop/examples/spring-ai-java/src/main/java/com/example/spring_ai_java/service/customersupport/CustomerSupportGraphService.java" rel="noreferrer noopener" target="_blank">Java</a>.</p>



<h2 class="wp-block-heading">The bottom line</h2>



<p>In this article, we saw how you can use Koog and Spring AI together to benefit from Spring AI’s model connections and database integrations, as well as the advanced production-focused orchestration layer from the Koog framework.</p>



<p>If you want to learn more about Koog, its <a href="https://www.jetbrains.com/koog/" rel="noreferrer noopener" target="_blank">product page</a> is a good place to start, and if you have any questions or feedback, be sure to join the <a href="https://github.com/JetBrains/koog/discussions" rel="noreferrer noopener" target="_blank">discussion on GitHub</a>.</p>



<p>Finally, don’t forget to join <a href="https://kotlinlang.slack.com/archives/C08SLB97W23" rel="noreferrer noopener" target="_blank">#koog-agentic-framework</a> on the Kotlin Slack<strong>&nbsp;</strong>(get an invite&nbsp;<a href="http://slack.kotlinlang.org/" rel="noreferrer noopener" target="_blank">here</a>).</p>
