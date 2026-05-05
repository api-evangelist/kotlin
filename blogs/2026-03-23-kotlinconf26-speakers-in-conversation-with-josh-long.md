---
title: "KotlinConf’26 Speakers: In Conversation with Josh Long"
url: "https://blog.jetbrains.com/kotlin/2026/03/kotlinconf-26-speakers-in-conversation-with-josh-long/"
date: "Mon, 23 Mar 2026 14:51:05 +0000"
author: "Daria Voronina"
feed_url: "https://blog.jetbrains.com/kotlin/feed/"
---
<h3 class="wp-block-heading"><em><strong>“There’s never been a better time to be a JVM or Spring developer.”</strong></em></h3>


    <div class="about-author ">
        <div class="about-author__box">
            <div class="row">
                                                            <div class="about-author__box-img">
                            <img alt="KotlinConf’26 Speakers: In Conversation with Josh Long" src="https://blog.jetbrains.com/wp-content/uploads/2026/03/josh-long.png" style="width: 100% !important; height: auto !important;" />
                        </div>
                                        <div class="about-author__box-text">
                                                    <h4>Josh Long, Spring Developer Advocate</h4>
                                                <p><span style="font-weight: 400;">Josh Long is the first Spring Developer Advocate, starting in 2010. Josh is a Java Champion, author of 7 books (including </span><i><span style="font-weight: 400;">Reactive Spring</span></i><span style="font-weight: 400;">) and numerous bestselling video trainings (including </span><i><span style="font-weight: 400;">Building Microservices with Spring Boot Livelessons</span></i><span style="font-weight: 400;"> with Spring Boot co-founder Phil Webb), and an open-source contributor (Spring Boot, Spring Integration, Axon, Spring Cloud, Activiti, Vaadin, and others), a YouTuber (Coffee + Software with Josh Long and his Spring Tips series), and a podcaster (</span><i><span style="font-weight: 400;">A Bootiful Podcast</span></i><span style="font-weight: 400;">).</span></p>
                    </div>
                            </div>
        </div>
    </div>



<p><strong>The Spring ecosystem has evolved dramatically over the past decade, from traditional enterprise applications to microservices, distributed systems, and now AI-powered services. Few people have witnessed that evolution as closely as Josh Long, who has served as Spring’s first Developer Advocate since 2010.</strong></p>



<p><strong>Ahead of <a href="https://kotlinconf.com/workshops/?utm_source=blogpost&amp;utm_medium=referral&amp;utm_campaign=firstspeakers" rel="noreferrer noopener" target="_blank">KotlinConf’26</a>, we spoke with Josh about how the Spring community has grown, why Kotlin has become such a natural fit for Spring developers, and why he believes there’s never been a better time to build on the JVM.</strong></p>



<div class="buttons">
        <div class="buttons__row">
            <a class="ek-link jb-download-button" href="https://kotlinconf.com/schedule/?utm_source=blog&#038;utm_medium=button&#038;utm_campaign=interview-josh-long&#038;day=2026-05-21&#038;session=03a85869-fa3c-52b4-ad32-05b318e8f3a9" rel="noopener" target="_blank" title="Meet Josh Long at KotlinConf’26">Meet Josh Long at KotlinConf’26</a>
         </div>
</div>



<h3 class="wp-block-heading"><strong>Q: You were the first Spring Developer Advocate, starting in 2010. How has the community around Spring changed during that time?</strong></h3>



<p><strong>Josh Long:</strong> Back then, most of the things people built were basically web applications. Nowadays, there are web services and backend server-side applications, and those applications are expected to do many more things.</p>



<p>So, the use cases that people introduce into their applications have grown. Before, Spring was very narrowly focused on the enterprise server-side world. Today, we talk about microservices, distributed computing systems, batch processing, integration, and all kinds of security.</p>



<p>And now we talk about AI.</p>



<p>These used to be different jobs and different career paths, but today they can all be done with Spring very naturally – quite elegantly in a lot of cases, compared to some of the alternatives.</p>



<p>So, the community has changed accordingly. The kinds of things people are doing have expanded, and the community around that has grown as well.</p>



<p>Put another way, it’s not that the people who were doing things in 2010 stopped doing those things. It’s more that people who were doing other kinds of work joined the community.</p>



<p>You see this represented in open-source projects, in language choices, Kotlin, for example, and across the whole ecosystem. It’s this wonderful open-source diaspora.</p>


    <div class="blockquote">
                    <blockquote><p>“<i>The galaxy of things people need to do has grown, and so has the community.</i>”</p></blockquote>
            <div class="blockquote__author">
                                    <img alt="KotlinConf’26 Speakers: In Conversation with Josh Long" class="blockquote__author-img" src="https://blog.jetbrains.com/wp-content/uploads/2026/03/josh-long.png" style="width: 100% !important; height: auto !important;" />
                                <div class="blockquote__author-info">
                                                        </div>
            </div>
            </div>



<h3 class="wp-block-heading"><strong>Q: As you mentioned, the Spring and Kotlin teams have worked hard to make sure that Kotlin and Spring Boot are a first-class experience. From your perspective, what makes a language truly first-class within a framework ecosystem?</strong></h3>



<p><strong>Josh:</strong> Spring is a framework built on top of the JVM. Most of Spring itself is written in Java, because Java was the only language people used when we created Spring back in 2001.</p>



<p>But we’ve always tried to excel at integration. We want Spring to be a well-behaved citizen on top of the JVM and the languages that run on it.</p>



<p>If you’re a Java developer, we want Spring to feel natural and idiomatic. Someone who understands Java should look at Spring code and immediately understand what’s going on.</p>



<p>The same is true for all integrations. Spring works with dozens of libraries and technologies, and we want those integrations to feel coherent and consistent.</p>



<p>The same principle applies to languages.</p>


    <div class="blockquote">
                    <blockquote><p>“<i>If we support a language, we want Spring to feel natural for people who already use that language. That’s definitely true for Kotlin.</i>”</p></blockquote>
            <div class="blockquote__author">
                                    <img alt="KotlinConf’26 Speakers: In Conversation with Josh Long" class="blockquote__author-img" src="https://blog.jetbrains.com/wp-content/uploads/2026/03/josh-long.png" style="width: 100% !important; height: auto !important;" />
                                <div class="blockquote__author-info">
                                                        </div>
            </div>
            </div>



<p>For the longest time, our goal was simply to be a good citizen on top of these languages. We didn’t expect the languages to adapt to us.</p>



<p>When the relationship between the Spring and Kotlin teams began developing more than ten years ago, we discovered that they were incredibly pragmatic and collaborative. They genuinely wanted Kotlin to work well for Spring developers.</p>



<p>That partnership has been a real honor.</p>



<p>One of my favorite examples is the Kotlin all-open plugin.</p>



<p>In Kotlin, classes are final by default. But frameworks like Spring and Hibernate rely on subclassing.</p>



<p>So normally you’d have to declare everything as <code>open</code>. The Kotlin team solved this by creating a compiler plugin. When you use Spring annotations, the classes are implicitly <code>open</code> behind the scenes.</p>



<p>Developers don’t have to change anything – if you go to start.spring.io, it’s already configured.</p>


    <div class="blockquote">
                    <blockquote><p>“<i>It’s a thousand small changes like this that make it clear the language wants to make Spring developers feel comfortable. I feel warm, grateful, and happy thinking about this wonderful teamwork.</i>”</p></blockquote>
            <div class="blockquote__author">
                                    <img alt="KotlinConf’26 Speakers: In Conversation with Josh Long" class="blockquote__author-img" src="https://blog.jetbrains.com/wp-content/uploads/2026/03/josh-long.png" style="width: 100% !important; height: auto !important;" />
                                <div class="blockquote__author-info">
                                                        </div>
            </div>
            </div>



<h3 class="wp-block-heading"><strong>Q: When you’re actually building a Spring application in Kotlin, where does it feel noticeably different from building it in Java?</strong></h3>



<p><strong>Josh:</strong> Spring has DSLs. These DSLs are about as elegant as they can be in Java, but Kotlin has a much more expressive language for designing DSLs. That’s not a controversial thing to suggest – it’s just empirically true.</p>



<p>The Spring team has embraced Kotlin. We actually have Kotlin code in Spring itself. We’ve written parts of Spring in Kotlin.</p>



<p>There are several DSLs that we provide in Java that also have sister DSLs written in Kotlin, and those Kotlin DSLs are much nicer.</p>



<p>For example, Spring Cloud Gateway, functional HTTP routes, and the new BeanRegistrar API in Spring Framework 7. There are lots of them. Spring Security has one as well. They’re everywhere.</p>



<p>It’s just a really nice, elegant little language.</p>



<p>And we’ve essentially done the work of building DSLs twice – once for Java and then again in Kotlin – because we wanted the Kotlin version to feel nice and idiomatic and natural. It feels really good.</p>



<div class="buttons">
        <div class="buttons__row">
            <a class="ek-link jb-download-button" href="https://pretix.eu/jetbrains/kotlinconf2026/c/AdBopK2LB/?utm_source=blog&#038;utm_medium=button&#038;utm_campaign=interview-josh-long" rel="noopener" target="_blank" title="Join us at KotlinConf’26">Join us at KotlinConf’26</a>
         </div>
</div>



<h3 class="wp-block-heading"><strong>Q: For Kotlin developers who are new to Spring, what’s one misconception they often have, and what’s one feature that usually wins them over? For those who haven’t tried Kotlin yet but are big fans of Spring, why should they give it a shot?</strong></h3>



<p><strong>Josh:</strong> I imagine the misconceptions those developers might have are the same ones anyone might have.</p>



<p>If you’re using Spring, you certainly don’t have to use just Java. Spring has always tried to embrace different languages.</p>


    <div class="blockquote">
                    <blockquote><p>“<i>Kotlin is by far the best story we’ve had there.</i>”</p></blockquote>
            <div class="blockquote__author">
                                    <img alt="KotlinConf’26 Speakers: In Conversation with Josh Long" class="blockquote__author-img" src="https://blog.jetbrains.com/wp-content/uploads/2026/03/josh-long.png" style="width: 100% !important; height: auto !important;" />
                                <div class="blockquote__author-info">
                                                        </div>
            </div>
            </div>



<p></p>



<p>People may not realize this, but we had a Spring for Scala project about fifteen years ago. We also tried Groovy. You can still use Groovy today, although I personally never do.</p>



<p>Kotlin is just a really natural fit.</p>



<p>The only language where Spring has actually added that language to the Spring Framework itself, as part of the codebase, is Kotlin.</p>



<p>So, I use Kotlin all the time.</p>



<h3 class="wp-block-heading"><strong>Q: You’ve spent years helping developers navigate new technologies. What excites you most right now about building on the JVM?</strong></h3>



<p><strong>Josh:</strong> First of all, the languages we have on the JVM today are very competitive.</p>



<p>If you&#8217;re building something today, languages like Kotlin are just as concise, small, and efficient as many other modern languages. They’re easy to reason about, but they also come with a lot of additional benefits because they run on the JVM.</p>



<p>The JVM itself is incredibly fast and very scalable. It’s one of the few places where you can have a program that is both very small and very fast.</p>



<p>There used to be a kind of trade-off. If you wanted to write something quickly, you used a scripting language like Python or Perl, at least when I first started working. If you wanted performance, you used something like C++.</p>



<p>But now, with languages like Kotlin running on the JVM, you can have both. You can write programs that are as concise as scripting languages while performing close to native languages.</p>



<p>We live in an amazing time.</p>



<p>There’s a second part to this. Because we have this amazing runtime infrastructure and language ecosystem, people are building incredible tools and frameworks on top of it to support new kinds of applications.</p>



<p>For example, I spend a lot of time talking to people about AI. Spring AI is a really nice way to build AI integrations, agentic systems, and integrations with AI models.</p>


    <div class="blockquote">
                    <blockquote><p>“<i>If you had told me 10 or 15 years ago that we would be writing five-line Spring applications in Kotlin that talk to AI models and do interesting things, I would have laughed. That would have sounded impossible. The world is very different now. There has also never been a better time to be a JVM developer or a Spring developer.</i>”</p></blockquote>
            <div class="blockquote__author">
                                    <img alt="KotlinConf’26 Speakers: In Conversation with Josh Long" class="blockquote__author-img" src="https://blog.jetbrains.com/wp-content/uploads/2026/03/josh-long.png" style="width: 100% !important; height: auto !important;" />
                                <div class="blockquote__author-info">
                                                        </div>
            </div>
            </div>



<h3 class="wp-block-heading"><strong>Q: With rapid growth in AI-driven applications, what does building AI-powered systems on the JVM look like today, and where do Kotlin and Spring play a role?</strong></h3>



<p><strong>Josh:</strong> I think people are sometimes misguided about AI.</p>



<p>When people talk about AI, they often mix up two very different use cases.&nbsp;</p>



<p>One involves building and training models. That kind of work often uses tools that don’t really exist on the JVM today.</p>


    <div class="blockquote">
                    <blockquote><p>“<i>Building your own models, training them, and doing data science is a very rare and a small use case compared to what 99% of the ecosystem will be doing, which is integrating these models into their business applications. Most of these models are just REST APIs, which means that this is an integration problem.</i>”</p></blockquote>
            <div class="blockquote__author">
                                    <img alt="KotlinConf’26 Speakers: In Conversation with Josh Long" class="blockquote__author-img" src="https://blog.jetbrains.com/wp-content/uploads/2026/03/josh-long.png" style="width: 100% !important; height: auto !important;" />
                                <div class="blockquote__author-info">
                                                        </div>
            </div>
            </div>



<p>For most applications, leveraging AI is about integrating those models into existing systems, and the JVM has always been extremely good at that.</p>



<p>That’s why enterprises use it – it can talk to anything.</p>



<p>Today, we have Spring AI, which makes it easier to build these integrations. Of course, there are other ecosystems on the JVM that have their own approaches to building AI-based applications.</p>



<p>There are lots of good options.</p>



<p>But the important thing is that the JVM is not just as good as something like Python or TypeScript for building these systems. In many cases, it’s actually much better.</p>



<p>There was a benchmark that came out recently looking at the performance of Model Context Protocol implementations. The JVM came out on top. Spring Boot and Spring AI had the best performance.</p>



<p>They compared implementations in Go, Python, and TypeScript, and the JVM performed the best.</p>



<p>So it’s not just a question of whether you can do this work on the JVM. In many cases, it’s much more performant. We also have better security and stronger integration with existing systems.</p>



<p>It’s a really big opportunity for developers in this ecosystem.</p>



<p>Another thing people often miss is that many AI projects fail because they don’t integrate properly with existing systems.</p>



<p>There was an MIT study that suggested something like 90% of AI integrations fail.</p>



<p>That’s not surprising – many teams build AI workflows as completely separate systems, often in Python, that don’t integrate well with the rest of their infrastructure.</p>



<p>But if you extend the systems where the business logic already lives, which is often on the JVM, things tend to work much better.</p>



<p>If you extend those existing services with tools like Spring AI and Kotlin, you’ll usually have a much better experience.</p>



<p>So it’s not just about being as good as other ecosystems. In many cases, the JVM is simply better for this kind of work.</p>



<p><strong>As Josh notes, many new technologies, including AI, ultimately come down to how well they integrate with the systems developers already use.</strong></p>



<p><strong>Josh will dive deeper into how Kotlin and Spring Boot work together to create a cleaner, more productive developer experience in his <a href="https://kotlinconf.com/schedule/?day=2026-05-21&amp;session=03a85869-fa3c-52b4-ad32-05b318e8f3a9" rel="noreferrer noopener" target="_blank">KotlinConf’26 talk “Bootiful Kotlin.”</a></strong></p>



<p><strong>Don’t miss Josh Long at KotlinConf’26!</strong></p>



<div class="buttons">
        <div class="buttons__row">
            <a class="ek-link jb-download-button" href="https://pretix.eu/jetbrains/kotlinconf2026/c/AdBopK2LB/?utm_source=blog&#038;utm_medium=button&#038;utm_campaign=interview-josh-long" rel="noopener" target="_blank" title="Join us at KotlinConf’26">Join us at KotlinConf’26</a>
         </div>
</div>
