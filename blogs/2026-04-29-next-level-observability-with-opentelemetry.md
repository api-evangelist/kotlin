---
title: "Next-Level Observability with OpenTelemetry"
url: "https://blog.jetbrains.com/kotlin/2026/04/next-level-observability-with-opentelemetry/"
date: "Wed, 29 Apr 2026 11:05:47 +0000"
author: "Viliam Sedliak"
feed_url: "https://blog.jetbrains.com/kotlin/feed/"
---
<p><em>This tutorial was written by an external contributor.</em></p>


    <div class="about-author ">
        <div class="about-author__box">
            <div class="row">
                                                            <div class="about-author__box-img">
                            <img alt="Kevin Kimani" src="https://blog.jetbrains.com/wp-content/uploads/2026/04/58376469.png" style="width: 100% !important; height: auto !important;" />
                        </div>
                                        <div class="about-author__box-text">
                                                    <h4>Kevin Kimani</h4>
                                                <p>Kevin Kimani is a software engineer and technical writer with over three years of experience in software development and documentation. He works across full-stack web development, database design, RESTful API development, and technical writing for engineer audiences. His writing focuses on clear API documentation, tutorials, and how-to guides that help developers understand and use products more easily.</p>
<p><a href="https://github.com/kimanikevin254" rel="noopener" target="_blank">GitHub</a> | <a href="https://x.com/KayveTech" rel="noopener" target="_blank">X.com</a></p>
                    </div>
                            </div>
        </div>
    </div>


            <div class="newsletter">
                                                            <article class="newsletter__post">
                                                                                    <img alt="Repository with the companion code for the tutorial" class="newsletter__post-img" src="https://blog.jetbrains.com/wp-content/uploads/2026/04/KT-social-BlogFeatured-1280x720-1-4.png" style="width: 100% !important; height: auto !important;" />
                                                                            <div class="newsletter__post-text">
                                                            <h3>Repository with the companion code for the tutorial</h3>
                                                                                                                    <a class="btn" href="https://kotl.in/q0tysp" rel="noopener" target="_blank">Go to GitHub</a>
                                                    </div>
                    </article>
                                    </div>
    


<p>As a developer, logging is usually the first technique that you reach for when something goes wrong in your application. You add a few log statements at the start and end of a function and in the exception handlers, and then you get a basic picture of what your application is doing. For simple services that run on a single instance, this approach is usually enough. You can just go through the log file, spot the error, and trace it back to its cause in just a few minutes.</p>



<p>But as your systems grow, that same approach doesn&#8217;t work. Logs start to pile up from multiple sources, executions interleave, and the error you&#8217;re looking at in the logs doesn&#8217;t provide enough information. You can clearly see the error, but you can&#8217;t trace it back to what caused it.<br /><br />In this tutorial, you&#8217;ll learn how to move beyond basic logging by instrumenting a <a href="https://kotlinlang.org/docs/server-overview.html" rel="noreferrer noopener" target="_blank">Kotlin</a> and <a href="https://spring.io/projects/spring-boot" rel="noreferrer noopener" target="_blank">Spring Boot</a> backend service with <a href="https://opentelemetry.io/" rel="noreferrer noopener" target="_blank">OpenTelemetry</a>. You&#8217;ll learn how OpenTelemetry&#8217;s tracing model gives you the execution context that logs alone can&#8217;t provide. By the end of this guide, you&#8217;ll have a working instrumented service and a clear mental model for building more observable backend systems.</p>



<h2 class="wp-block-heading">Why You Need Next-Level Observability</h2>



<p>Modern backend systems are rarely linear. One operation might fan out to several downstream services, retry on failure, or execute concurrently across multiple instances and threads. All these patterns create opportunities for failure that might be hard to explain after the fact. When a background job that processes hundreds of records across overlapping executions errors out, your logs won&#8217;t tell you much. <strong>You won&#8217;t be able to explain which execution the error belonged to</strong> or whether the other executions succeeded or failed in the same way.</p>



<p>The lack of execution context creates a gap between seeing the error in your logs and actually understanding what happened leading to it. You just have the error message and a timestamp, but no way to connect it to the broader execution that it belongs to. In a system that runs multiple concurrent job executions, logs from different runs always interleave freely, and thread names get reused by the thread pool. Without a way to uniquely identify each execution, each line of text in your log file is an isolated fact without any reliable way to group it with others that belong to the same run.</p>



<p>Observability gives you a structured view of what your system did and in what order. It does this through traces, which are records of complete operations that contain unique identifiers. You can easily filter your logs by these trace identifiers, and <strong>you&#8217;ll be able to see the entire history of a specific execution clearly</strong>. Metrics add another dimension by revealing patterns over time that no single log entry can show. Together, they help to transform debugging from guesswork into a structured investigation.</p>



<h2 class="wp-block-heading">What OpenTelemetry Provides</h2>



<p>OpenTelemetry is an open-source observability framework that defines a unified model for collecting three types of signals from your applications:</p>



<ul>
<li><a href="https://opentelemetry.io/docs/concepts/signals/traces/" rel="noreferrer noopener" target="_blank"><strong>Traces</strong></a><strong>:</strong> represent the full lifecycle of a request or an operation as it moves through your system. A trace is made up of <a href="https://opentelemetry.io/docs/concepts/signals/traces/#spans" rel="noreferrer noopener" target="_blank">spans</a>, where each span represents a unit of work within the operation, such as an HTTP call or a background task. Each span contains a trace ID and a span ID as part of its context, where the trace ID ties back the span to its parent operation (trace) and the span ID uniquely identifies the single specific step within the operation.</li>



<li><a href="https://opentelemetry.io/docs/concepts/signals/metrics/" rel="noreferrer noopener" target="_blank"><strong>Metrics</strong></a><strong>:</strong> capture aggregated measurements over time, such as how long an operation takes and the error rates. This helps to give you a statistical view of the overall system health. <a href="https://opentelemetry.io/docs/concepts/signals/logs/" rel="noopener" target="_blank">&nbsp;</a></li>



<li><a href="https://opentelemetry.io/docs/concepts/signals/logs/" rel="noreferrer noopener" target="_blank"><strong>Logs</strong></a><strong>:</strong> represent discrete events that happened at a specific point in time. When correlated with trace context, logs stop being isolated entries in a file and become anchored events within a specific execution, which makes it easy to understand exactly what happened and why.<br /></li>
</ul>



<p>The OpenTelemetry ecosystem consists of three main <a href="https://opentelemetry.io/docs/concepts/components/" rel="noreferrer noopener" target="_blank">components</a>:</p>



<ul>
<li>Instrumentation</li>



<li>The Collector</li>



<li>Exporters<br /></li>
</ul>



<p>Instrumentation is how you integrate OpenTelemetry into your application. It uses language-specific SDKs that implement the OpenTelemetry API for creating spans, recording metrics, and propagating the context.</p>



<p>The <a href="https://opentelemetry.io/docs/collector/" rel="noreferrer noopener" target="_blank">Collector</a> is an optional but powerful middleware component that defines the logic for receiving, processing, and exporting telemetry data to one or more supported backends.</p>



<p>The exporters are plugins that send your application&#8217;s telemetry data to a specific destination, such as <a href="https://prometheus.io/" rel="noreferrer noopener" target="_blank">Prometheus</a>, <a href="https://jaegertracing.io/" rel="noreferrer noopener" target="_blank">Jaeger</a>, or any other compatible backend.</p>



<p>What makes OpenTelemetry a valuable long-term observability strategy is its vendor neutrality. Before OpenTelemetry, instrumentation was tightly coupled to specific vendors, and switching always meant that you needed to rewrite your instrumentation code throughout the entire codebase. OpenTelemetry fixed this by separating the instrumentation from the destination. This means that you can now instrument your service once using the standard API and then change the exporters as your infrastructure and specific requirements evolve.</p>


            <div class="newsletter">
                                                            <article class="newsletter__post">
                                                                                    <img alt="Repository with the companion code for the tutorial" class="newsletter__post-img" src="https://blog.jetbrains.com/wp-content/uploads/2026/04/KT-social-BlogFeatured-1280x720-1-4.png" style="width: 100% !important; height: auto !important;" />
                                                                            <div class="newsletter__post-text">
                                                            <h3>Repository with the companion code for the tutorial</h3>
                                                                                                                    <a class="btn" href="https://kotl.in/e4s8j5" rel="noopener" target="_blank">Go to GitHub</a>
                                                    </div>
                    </article>
                                    </div>
    


<h2 class="wp-block-heading">Setting Up Next-Level Observability with OpenTelemetry</h2>



<p>If you want to follow along with this tutorial, you&#8217;ll need the following:</p>



<ul>
<li>An IDE, such as<a href="https://www.jetbrains.com/idea/" rel="noreferrer noopener" target="_blank"> IntelliJ IDEA</a>.</li>



<li><a href="https://www.oracle.com/africa/java/technologies/downloads/" rel="noreferrer noopener" target="_blank">JDK 17 or later</a> and <a href="https://git-scm.com/book/en/v2/Getting-Started-Installing-Git" rel="noreferrer noopener" target="_blank">Git CLI</a> installed on your local machine.<br /></li>
</ul>



<p>Here is a rough architecture diagram of what you’ll build in this guide:</p>



<figure class="wp-block-image size-full"><img alt="" class="wp-image-703428" src="https://blog.jetbrains.com/wp-content/uploads/2026/04/I5N8NCy.png" style="width: 100% !important; height: auto !important;" /></figure>



<p>The application consists of a Spring Boot Service running in a single Java Virtual Machine (JVM). The Task Scheduler triggers the <code>OrderSummaryJob</code> at regular intervals. The job reads orders from an embedded H2 database via Spring Data JPA, processes them, and writes summaries back to the database. The OpenTelemetry Java Agent sits within the JVM, automatically instrumenting the job and injecting trace context into the Mapped Diagnostic Context (MDC). This context flows through the log output, allowing you to correlate all logs from a single execution when multiple executions run concurrently.</p>



<p>Now that you have the prerequisites and understand the system you will build, it&#8217;s time to get started.</p>



<h3 class="wp-block-heading"><strong>Setting Up the Starter Template</strong></h3>



<p>To keep this tutorial focused on adding observability, we&#8217;ve prepared a Kotlin and Spring Boot application that runs a scheduled job every few minutes. Clone the application to your local machine by executing the following command:</p>



<pre class="EnlighterJSRAW">git clone --single-branch -b starter-template https://github.com/kimanikevin254/jetbrains-otel-order-summary.git</pre>



<p>Then, open the project in your code editor.</p>



<p>The most important file in this project is the <code>src/main/kotlin/com/example/order_summary/service/OrderSummaryJob.kt</code>, which defines the scheduled job. The job reads orders created in the last 24 hours from an <a href="https://www.h2database.com/html/main.html" rel="noreferrer noopener" target="_blank">H2</a> <a href="https://www.h2database.com/html/main.html" rel="noreferrer noopener" target="_blank">database</a> via Spring Data repositories, processes them one by one, and writes summaries back to the DB. The job runs every five minutes using Spring&#8217;s <code>@Scheduled</code> annotation. The summaries generated by this job can later be consumed by other parts of a larger system, such as dashboards, analytic pipelines, or downstream services that need a periodic snapshot of order volume and revenue.</p>



<p>Since this tutorial&#8217;s main focus is on observability, let&#8217;s review the logging approach used in this job:</p>



<pre class="EnlighterJSRAW">@Service
class OrderSummaryJob(
   private val orderRepository: OrderRepository,
   private val orderSummaryRepository: OrderSummaryRepository
) {
   private val logger = LoggerFactory.getLogger(OrderSummaryJob::class.java)

   @Scheduled(fixedDelay = 300000) // 5mins in ms
   fun generateSummary() {
       logger.info("Starting order summary job...")

       val periodEnd = LocalDateTime.now()
       val periodStart = periodEnd.minusHours(24)

       val orders = orderRepository.findByCreatedAtAfter(periodStart)
       if (orders.isEmpty()) {
           logger.info("No orders found in the last 24 hours. Skipping summary generation.")
           return
       }
       logger.info("Found ${orders.size} orders to process")

       var processedCount = 0
       var totalAmount = BigDecimal.ZERO

       for (order in orders) {
           try {
               logger.info("Processing order ${order.id} for customer ${order.customerId}...")

               // Simulate processing work
               Thread.sleep(2000)

               // Simulate occasional failures
               if (order.amount &gt; BigDecimal("400")) {
                   throw RuntimeException("Order amount exceeds threshold: ${order.amount}")
               }

               totalAmount = totalAmount.add(order.amount)
               processedCount++
           } catch (e: Exception) {
               logger.error("Failed to process order ${order.id}: ${e.message}")
               // Continue processing other orders
           }
       }

       val summary = OrderSummary(
           totalOrders = orders.size,
           totalAmount = totalAmount,
           periodStart = periodStart,
           periodEnd = periodEnd
       )

       orderSummaryRepository.save(summary)
       logger.info("Order summary job completed. Total: ${orders.size} orders, Amount $totalAmount")
   }
}</pre>



<p>As you can see, the logging approach is straightforward and very common. You log when the job starts, log each order being processed, log if an exception occurs, and log when the job finishes.</p>



<p>To see it in action, run the following command in your terminal:</p>



<pre class="EnlighterJSRAW">./gradlew bootRun</pre>



<p>Once the application starts, you can see the logs. Execute the command <code>tail -f logs/order-summary.log</code> in a separate terminal to stream the logs from the configured log file:</p>



<pre class="EnlighterJSRAW">2026-02-24T15:49:47.304+03:00  INFO 605417 --- [order-summary] [scheduling-1] c.e.o.service.OrderSummaryJob            : Found 12 orders to process
2026-02-24T15:49:47.305+03:00  INFO 605417 --- [order-summary] [scheduling-1] c.e.o.service.OrderSummaryJob            : Processing order 1 for customer CUST-10001...
2026-02-24T15:49:49.306+03:00  INFO 605417 --- [order-summary] [scheduling-1] c.e.o.service.OrderSummaryJob            : Processing order 3 for customer CUST-10003...
2026-02-24T15:49:51.307+03:00  INFO 605417 --- [order-summary] [scheduling-1] c.e.o.service.OrderSummaryJob            : Processing order 7 for customer CUST-10007...
2026-02-24T15:49:53.308+03:00  INFO 605417 --- [order-summary] [scheduling-1] c.e.o.service.OrderSummaryJob            : Processing order 8 for customer CUST-10008...
2026-02-24T15:49:55.308+03:00  INFO 605417 --- [order-summary] [scheduling-1] c.e.o.service.OrderSummaryJob            : Processing order 9 for customer CUST-10009...
2026-02-24T15:49:57.310+03:00 ERROR 605417 --- [order-summary] [scheduling-1] c.e.o.service.OrderSummaryJob            : Failed to process order 9: Order amount exceeds threshold: 458.23
...

2026-02-24T15:50:11.322+03:00 ERROR 605417 --- [order-summary] [scheduling-1] c.e.o.service.OrderSummaryJob            : Failed to process order 19: Order amount exceeds threshold: 427.98
2026-02-24T15:50:11.340+03:00  INFO 605417 --- [order-summary] [scheduling-1] c.e.o.service.OrderSummaryJob            : Order summary job completed. Total: 12 orders, Amount 1680.31</pre>



<p>The logs show clean, linear execution. Each step follows the previous one, and errors are easy to associate with the work being performed. This works well when the job runs infrequently, and only a single instance of the application is deployed. One execution completes before the next begins, so tracing failures back to their source is straightforward. For early-stage systems, this level of logging is sufficient.</p>



<h3 class="wp-block-heading">Introducing Complexity</h3>



<p>As a project evolves, two changes might occur:</p>



<ul>
<li>The business might demand near real-time visibility into the order metrics, which means that the job needs to run more frequently. Say, every five seconds instead of every five minutes.</li>



<li>The application may be deployed across multiple instances for high availability.<br /></li>
</ul>



<p>Let&#8217;s start by running the job more frequently to see how this affects our current logging approach. To do this, open the <code>src/main/kotlin/com/example/order_summary/OrderSummaryApplication.kt</code> file and add the following line of code to the main application class:</p>



<pre class="EnlighterJSRAW">@EnableAsync</pre>



<p>This enables async execution. Remember to add the following import statement to the same file:</p>



<pre class="EnlighterJSRAW">import org.springframework.scheduling.annotation.EnableAsync</pre>



<p>Next, open the <code>src/main/kotlin/com/example/order_summary/service/OrderSummaryJob.kt</code> file and replace <code>@Scheduled(fixedDelay = 300000) // 5mins</code> with the following:</p>



<pre class="EnlighterJSRAW">@Async
@Scheduled(fixedRate = 5000) // 5secs in ms</pre>



<p>Changing from <code>fixedDelay</code> to <code>fixedRate</code> means the job starts every five seconds regardless of whether the previous execution has finished. Adding <code>@Async</code> ensures that each execution runs on its own thread from Spring&#8217;s task executor pool, preventing slow jobs from blocking the scheduler. This is a common pattern when scaling background jobs to handle higher throughput.<br /><br />Remember to add the following import statement to the same file:</p>



<pre class="EnlighterJSRAW">@import org.springframework.scheduling.annotation.Async</pre>



<p>Restart the application and observe the logs. You should see something like this:</p>



<pre class="EnlighterJSRAW">2026-02-24T16:17:40.469+03:00  INFO 610799 --- [order-summary] [task-1] c.e.o.service.OrderSummaryJob            : Starting order summary job...
2026-02-24T16:17:40.596+03:00  INFO 610799 --- [order-summary] [task-1] c.e.o.service.OrderSummaryJob            : Found 12 orders to process
2026-02-24T16:17:40.597+03:00  INFO 610799 --- [order-summary] [task-1] c.e.o.service.OrderSummaryJob            : Processing order 1 for customer CUST-10001...
...
2026-02-24T16:17:47.468+03:00  INFO 610799 --- [order-summary] [task-2] c.e.o.service.OrderSummaryJob            : Processing order 3 for customer CUST-10003...
2026-02-24T16:17:48.602+03:00  INFO 610799 --- [order-summary] [task-1] c.e.o.service.OrderSummaryJob            : Processing order 9 for customer CUST-10009...
2026-02-24T16:17:49.469+03:00  INFO 610799 --- [order-summary] [task-2] c.e.o.service.OrderSummaryJob            : Processing order 7 for customer CUST-10007...
2026-02-24T16:17:50.460+03:00  INFO 610799 --- [order-summary] [task-3] c.e.o.service.OrderSummaryJob            : Starting order summary job...
2026-02-24T16:17:50.472+03:00  INFO 610799 --- [order-summary] [task-3] c.e.o.service.OrderSummaryJob            : Found 12 orders to process
2026-02-24T16:17:50.473+03:00  INFO 610799 --- [order-summary] [task-3] c.e.o.service.OrderSummaryJob            : Processing order 1 for customer CUST-10001...
...
2026-02-24T16:17:54.476+03:00  INFO 610799 --- [order-summary] [task-3] c.e.o.service.OrderSummaryJob            : Processing order 7 for customer CUST-10007...
2026-02-24T16:17:54.608+03:00  INFO 610799 --- [order-summary] [task-1] c.e.o.service.OrderSummaryJob            : Processing order 14 for customer CUST-10014...
2026-02-24T16:17:55.460+03:00  INFO 610799 --- [order-summary] [task-4] c.e.o.service.OrderSummaryJob            : Starting order summary job...
2026-02-24T16:17:55.473+03:00  INFO 610799 --- [order-summary] [task-4] c.e.o.service.OrderSummaryJob            : Found 12 orders to process
2026-02-24T16:17:55.474+03:00  INFO 610799 --- [order-summary] [task-4] c.e.o.service.OrderSummaryJob            : Processing order 1 for customer CUST-10001...
2026-02-24T16:17:55.475+03:00 ERROR 610799 --- [order-summary] [task-2] c.e.o.service.OrderSummaryJob            : Failed to process order 9: Order amount exceeds threshold: 458.23</pre>



<p>The logs are now completely interleaved. Executions from <code>task-1</code>, <code>task-2</code>, and <code>task-3</code> are all running simultaneously, processing the orders, and logging to the same output. When an error occurs, like the failure on order 9 at 16:17:55, it&#8217;s not easy to figure out which job execution the log belongs to and which orders were successfully processed before the error occurred in that specific execution.</p>



<p>You might think searching by thread name, such as <code>task-1</code>, would solve this, but Spring&#8217;s thread pool reuses threads. After <code>task-1</code> finishes its first execution, it picks up execution 9, then execution 17, and so on. Searching by thread name now gives you mixed logs from multiple unrelated executions. In production, where multiple application instances run behind a load balancer, thread names are no longer unique across your system.</p>



<p>This is where plain logging breaks down. You know and see that something failed, but you can&#8217;t explain what happened leading up to it.</p>



<h3 class="wp-block-heading"><strong>A Better Solution: Adding OpenTelemetry</strong></h3>



<p>To address the missing execution context, let&#8217;s adapt the application to use OpenTelemetry for log correlation. The goal is not to analyze the performance or optimize the job, but to fix the issue of associating logs with their execution context. Each job execution will be treated as a logical unit of work. A unique trace ID will be attached to that execution, and all logs emitted during the job will include that ID. This way, even when multiple executions run concurrently, and your logs get interleaved, you can filter them by trace ID and see exactly what happened in a single run from the start to the end.</p>



<p>OpenTelemetry provides <a href="https://opentelemetry.io/docs/languages/java/instrumentation/#instrumentation-categories" rel="noreferrer noopener" target="_blank">several ways</a> to instrument applications. In this guide, we&#8217;ll use the <a href="https://opentelemetry.io/docs/zero-code/java/agent/" rel="noreferrer noopener" target="_blank">Java Agent</a>, which automatically instruments your application without requiring any changes to the source code.</p>



<p>Let&#8217;s start by downloading the agent JAR file. Execute the following commands in the project root folder to create a directory for the agent and download it:</p>



<pre class="EnlighterJSRAW">mkdir -p agents
curl -L https://github.com/open-telemetry/opentelemetry-java-instrumentation/releases/latest/download/opentelemetry-javaagent.jar \
-o agents/opentelemetry-javaagent.jar</pre>



<p>Verify the download using the following command:</p>



<pre class="EnlighterJSRAW">ls -lh agents/opentelemetry-javaagent.jar</pre>



<p>You should see a file around 24MB in size.</p>



<p>Add the <code>agents/</code> directory to your <code>.gitignore</code> file so that the JAR file is not committed to version control. You can use the command <code>echo "agents/" &gt;&gt; .gitignore</code> or add it manually. <br /><br />Once you&#8217;ve confirmed that the agent was downloaded successfully, it&#8217;s time to configure it. You need to attach it to the JVM when running the application by passing it as an argument. Open the <code>build.gradle.kts</code> file and add the following configuration:</p>



<pre class="EnlighterJSRAW">tasks.bootRun {
   jvmArgs = listOf(
       "-javaagent:${projectDir}/agents/opentelemetry-javaagent.jar",
       "-Dotel.service.name=order-summary-service",
       "-Dotel.traces.exporter=logging",
       "-Dotel.metrics.exporter=none",
       "-Dotel.logs.exporter=none"
   )
}</pre>



<p>Here is what each argument does:</p>



<ul>
<li><code>-javaagent</code> tells the JVM to load the OpenTelemetry agent before your application starts</li>



<li><code>-Dotel.service.name</code> sets the name of your service in the telemetry data</li>



<li><code>-Dotel.traces.exporter=logging</code> prints trace data to the console. No external backend is needed for this guide</li>



<li><code>-Dotel.metrics.exporter=none</code> and <code>-Dotel.logs.exporter=none</code> disable metrics and log exporting since that is outside the scope of this guide<br /></li>
</ul>



<p>Lastly, you need to update the log patterns to include trace context. The OpenTelemetry agent automatically injects <code>trace_id</code> and <code>span_id</code> into the logging context (Mapped Diagnostic Context). To display these values in your application logs, open the <code>src/main/resources/application.properties</code> file and add the following:</p>



<pre class="EnlighterJSRAW">logging.pattern.console=%d{HH:mm:ss.SSS} [%thread] [trace_id=%mdc{trace_id} span_id=%mdc{span_id}] %-5level %logger{36} - %msg%n
logging.pattern.file=%d{HH:mm:ss.SSS} [%thread] [trace_id=%mdc{trace_id} span_id=%mdc{span_id}] %-5level %logger{36} - %msg%n</pre>



<p>The <code>%mdc{trace_id}</code> and <code>%mdc{span_id}</code> directives extract values from the MDC that the agent populates automatically.</p>



<p>Now, let&#8217;s restart the application and observe the logs:</p>



<pre class="EnlighterJSRAW">17:19:43.715 [task-1] [trace_id=da673f1ec49eba77264c5912584e7183 span_id=74c708e335a974e3] INFO  c.e.o.service.OrderSummaryJob - Starting order summary job...
17:19:43.856 [task-1] [trace_id=da673f1ec49eba77264c5912584e7183 span_id=74c708e335a974e3] INFO  c.e.o.service.OrderSummaryJob - Found 12 orders to process
17:19:43.857 [task-1] [trace_id=da673f1ec49eba77264c5912584e7183 span_id=74c708e335a974e3] INFO  c.e.o.service.OrderSummaryJob - Processing order 1 for customer CUST-10001...
17:19:45.860 [task-1] [trace_id=da673f1ec49eba77264c5912584e7183 span_id=74c708e335a974e3] INFO  c.e.o.service.OrderSummaryJob - Processing order 3 for customer CUST-10003...

...

17:19:53.704 [task-3] [trace_id=4a969bbb00634e0ee36b2fbda1399d8a span_id=0a602f1a58df2f71] INFO  c.e.o.service.OrderSummaryJob - Starting order summary job...
17:19:53.715 [task-3] [trace_id=4a969bbb00634e0ee36b2fbda1399d8a span_id=0a602f1a58df2f71] INFO  c.e.o.service.OrderSummaryJob - Found 12 orders to process
17:19:53.715 [task-3] [trace_id=4a969bbb00634e0ee36b2fbda1399d8a span_id=0a602f1a58df2f71] INFO  c.e.o.service.OrderSummaryJob - Processing order 1 for customer CUST-10001...
17:19:53.868 [task-1] [trace_id=da673f1ec49eba77264c5912584e7183 span_id=74c708e335a974e3] ERROR c.e.o.service.OrderSummaryJob - Failed to process order 9: Order amount exceeds threshold: 458.23</pre>



<p>All logs from the first execution share the same <code>trace_id</code> (<code>da673f1ec49eba77264c5912584e7183</code>), while logs from the third execution have a different <code>trace_id</code> (<code>4a969bbb00634e0ee36b2fbda1399d8a</code>). Even though both executions are running concurrently and their logs are interleaved, you can now filter by trace ID to isolate a single execution.</p>



<p>For example, to see logs only from the first execution, you could search for <code>trace_id=da673f1ec49eba77264c5912584e7183</code> in a log aggregation tool such as<a href="https://aws.amazon.com/cloudwatch/" rel="noreferrer noopener" target="_blank"> Amazon CloudWatch</a>.</p>



<p>All the code used in this tutorial is available on <a href="https://github.com/kimanikevin254/jetbrains-otel-order-summary" rel="noreferrer noopener" target="_blank">GitHub</a>.</p>



<h3 class="wp-block-heading">Future Considerations</h3>



<p>To make filtering even more powerful, you can update this logging strategy further by adding structured fields to logs for record IDs or job phases. For example, you could log <code>order_id</code> as a dedicated field alongside the trace ID, allowing you to query all executions that touched a specific order.</p>



<p>You can also export logs alongside traces to an observability backend like <a href="https://jaegertracing.io/" rel="noreferrer noopener" target="_blank">Jaeger</a> or<a href="https://grafana.com/" rel="noreferrer noopener" target="_blank"> Grafana</a>. This allows you to visualize the full trace as a timeline, showing how long each step took and where errors occurred. The OpenTelemetry agent supports exporting to multiple backends by changing the exporter configuration, so you can start with logging and migrate to a full observability platform later without making any changes to your instrumentation code.</p>



<p>You can also apply this same pattern to other background jobs, API handlers, or any asynchronous work in your system. Once OpenTelemetry is in place, every part of your application automatically benefits from trace context propagation, making it easier to debug complex workflows that span multiple services or components.</p>



<h2 class="wp-block-heading"><strong>Conclusion</strong></h2>



<p>In this guide, you built a Kotlin and Spring Boot application with a scheduled background job, observed how plain logging breaks down under concurrency, and solved the problem by instrumenting the application with OpenTelemetry. You learned how the OpenTelemetry Java Agent automatically injects trace context into logs without requiring code changes, and how trace IDs enable you to correlate logs from a single execution even when multiple executions run concurrently.</p>



<p>Observability isn&#8217;t just about adding more logs; it&#8217;s about adding structure and context to the signals your system already emits. With OpenTelemetry, you can turn interleaved, confusing logs into isolated, queryable execution traces.</p>



<p>But the benefits go beyond debugging. Building systems with observability in mind changes how you design them. Traces reveal where boundaries should exist and help you set realistic timeouts based on actual production data. Metrics show trends over time, which make it easier to plan capacity, define SLOs, and alert on deviations before your users are affected. Additionally, well-instrumented code is more readable. When every operation is traced, you think more carefully about what constitutes a meaningful unit of work, which makes the system clearer for both observability tools and other developers.</p>
