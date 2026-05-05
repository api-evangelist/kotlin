---
title: "Helping Decision-Makers Say Yes to Kotlin Multiplatform (KMP)"
url: "https://blog.jetbrains.com/kotlin/2026/04/helping-decision-makers-say-yes-to-kmp/"
date: "Mon, 20 Apr 2026 14:39:57 +0000"
author: "Ekaterina Volodko"
feed_url: "https://blog.jetbrains.com/kotlin/feed/"
---
<p><em>This post was written by external contributors from Touchlab</em>.</p>


    <div class="about-author ">
        <div class="about-author__box">
            <div class="row">
                                                            <div class="about-author__box-img">
                            <img alt="Justin Mancinelli" src="https://blog.jetbrains.com/wp-content/uploads/2026/04/portrait.22b39ebb3064ee3e.webp" style="width: 100% !important; height: auto !important;" />
                        </div>
                                        <div class="about-author__box-text">
                                                    <h4>Justin Mancinelli</h4>
                                                <p>Justin Mancinelli is VP of Client Services at Touchlab, where he leads client services strategy and complex technical delivery. He partners with engineering leaders on mobile apps, SDKs, developer tooling, Kotlin Multiplatform, and Compose Multiplatform. With more than 13 years of experience helping software businesses succeed, he focuses on turning product and engineering goals into delivery.</p>
<p><a href="https://www.linkedin.com/in/justinmancinelli/" rel="noopener" target="_blank">LinkedIn</a></p>
                    </div>
                            </div>
        </div>
    </div>


    <div class="about-author ">
        <div class="about-author__box">
            <div class="row">
                                                            <div class="about-author__box-img">
                            <img alt="Samuel Hill from Touchlab" src="https://blog.jetbrains.com/wp-content/uploads/2026/04/1756344765502.webp" style="width: 100% !important; height: auto !important;" />
                        </div>
                                        <div class="about-author__box-text">
                                                    <h4>Samuel Hill</h4>
                                                <p>As VP of Engineering at Touchlab, Samuel Hill leads engineering strategy and supports teams building mobile products across Android and iOS. He works with engineering leaders on Kotlin Multiplatform, architecture, development standards, and team growth. With more than 13 years of experience in mobile engineering, he focuses on strong technical delivery and cross-functional collaboration.</p>
<p><a href="https://www.linkedin.com/in/hillsamuel/" rel="noopener" target="_blank">LinkedIn</a></p>
                    </div>
                            </div>
        </div>
    </div>



<h2 class="wp-block-heading">KMP is a strategic platform</h2>



<p>In the current competitive landscape, the traditional mobile development model characterized by maintaining independent, duplicated codebases for iOS and Android is no longer a sustainable use of capital. This approach systematically introduces feature lag, technical debt, and a fragmented engineering culture that hinders organizational agility. For leadership, adopting <a href="https://kotlinlang.org/multiplatform/" rel="noopener" target="_blank">Kotlin Multiplatform (KMP)</a> must be viewed as a fundamental shift in capital allocation for mobile engineering.<br /><br />KMP is not merely an incremental technical upgrade – it is a strategic platform that enables a unified engineering organization. By sharing high-value business logic while preserving native performance and UI integrity, KMP enables organizations to drastically reduce the total cost of ownership (TCO) of their mobile ecosystem. This transition transforms mobile development from platform-specific silos into a high-velocity engine that accelerates roadmaps, mitigates delivery risks, and secures a competitive advantage. As organizations increasingly integrate AI into their products, Kotlin Multiplatform provides a reliable, JVM-native foundation for building and deploying AI-powered mobile and backend services without introducing additional language or runtime complexity.</p>



<h2 class="wp-block-heading">Quantifiable metrics for KMP adoption</h2>



<p>Understanding the strategic impact of Kotlin Multiplatform for your organization starts with modeling potential cost savings, development velocity improvements, and risk mitigations. The following data, synthesized from enterprise-scale implementations and market leaders, provides an empirical foundation for proposing, budgeting, and planning your KMP adoption initiative.</p>


<figure class="wp-block-table is-style-stripes">
<table>
<thead>
<tr>
<th><strong>Advantage</strong></th>
<th><strong>Improved metrics</strong><sup>1</sup></th>
<th><strong>Business/team impact</strong></th>
</tr>
</thead>
<tbody>
<tr>
<td><strong>Code reduction</strong></td>
<td>40–60% less code<br />80% logic shared</td>
<td>Dramatic reduction in technical debt and long-term maintenance overhead</td>
</tr>
<tr>
<td><strong>Development velocity</strong></td>
<td>20-40% faster code reviews<br />15–30% faster release cycles</td>
<td>Increased bandwidth for senior talent and faster PR throughput</td>
</tr>
<tr>
<td><strong>Quality and reliability</strong></td>
<td>40–60% fewer bugs<br />25–40% fewer platform-specific edge cases</td>
<td>Reduced QA cycles and higher customer satisfaction through consistent behavior</td>
</tr>
<tr>
<td><strong>Timeline acceleration</strong></td>
<td>50% faster implementation<br />Multi-year roadmaps realized in a single quarter</td>
<td>Drastically shortened time-to-market makes it possible to respond to market shifts in real time and execute strategic pivots under urgent deadlines</td>
</tr>
<tr>
<td colspan="3" style="padding-top: 8px;">
<p style="font-size: 90%;">1. These figures were derived from proprietary and public data gathered from Touchlab clients and community case studies (see the <a href="#provenmarketvalidation"><em>Proven market validation</em></a> section for example data). Actual results may vary depending on architecture, team structure, and project scope.</p>
</td>
</tr>
</tbody>
</table>
</figure>


<h2 class="wp-block-heading">Velocity and feature parity</h2>



<p>KMP eliminates the feature lag that historically forces businesses to delay launching on the second platform and marketing departments to delay new feature announcements. In traditional siloed development, discrepancies in business logic and implementation speed between iOS and Android teams are inevitable. KMP solves this by enabling a single, verified implementation of business rules that serves both platforms simultaneously.</p>



<p>An engineer can build and test a new feature on one platform. Subsequent platforms then simply hook up the existing data models and logic from the shared KMP code to their native UI. This groundwork reuse ensures consistency from day one.&nbsp;</p>



<p>Beyond immediate speed, this unified architecture promotes maintainability and de-risks incremental development across platforms. Future requirements, such as top-down enforced migration from one data, analytics, or streaming platform to another, are accelerated by building upon a stable, shared foundation that supports synchronized launches across the entire user ecosystem.</p>



<h2 class="wp-block-heading">Organizational risk reduction</h2>



<p>Adopting KMP is a primary driver for organizational risk reduction, enforcing a new foundation that prioritizes architectural discipline over the spaghetti often found in legacy mobile apps. By centralizing core business logic, organizations gain strategic agility that de-risks the technical roadmap. This architectural flexibility allows leadership to pivot across web and mobile ecosystems at a speed impossible when logic is trapped in platform-specific silos, enabling the engineering department to meet sudden market demands.</p>



<p>Consolidating complex calculations and business rules into a single source of truth fundamentally lowers the probability of systemic error. When logic is duplicated across disparate codebases, an organization implicitly accepts a doubled risk of regression and a fractured quality assurance cycle. KMP mitigates this operational hazard by ensuring that a single, verified enhancement or fix propagates across the entire product line, effectively slashing the technical debt and remediation costs that typically compound in traditional multi-platform environments.</p>



<p>Shared logic with KMP naturally mandates a clean separation of concerns, moving the organization away from fragile, UI-entangled code. The clear architecture empowers teams to achieve significantly higher automated test coverage, which removes the fear of the unknown that often plagues legacy systems. As the codebase becomes more predictable and less reliant on manual intervention, the organization achieves a level of stability where innovation can occur without the constant threat of destabilizing critical business functions.</p>



<h2 class="wp-block-heading">Engineering culture and talent</h2>



<p>The shift to KMP directly affects talent retention and internal mobility within the engineering organization. By moving away from platform-specific constraints, KMP allows teams to transition from isolated silos to a unified model where developers function as mobile engineers. This shift creates a more flexible and responsive technical workforce where engineering resources are allocated based on business priorities rather than purely on platform and language expertise.</p>



<p>Architectural alignment simplifies the codebase and clarifies the path to productivity for new hires. By maintaining a single logic layer instead of two separate implementations, organizations typically see a 30–50% reduction in onboarding time. Engineers can focus on mastering a well-structured system that minimizes technical debt and cognitive overhead often found in siloed environments.</p>



<h2 class="wp-block-heading" id="provenmarketvalidation">Proven market validation</h2>



<p>KMP has proven its benefit at world-class organizations that require stability and scale. The following companies have been Touchlab clients, or discussed their data publicly with Touchlab and JetBrains:</p>



<ul>
<li><a href="https://engineering.block.xyz/blog/how-bitkey-uses-cross-platform-development" rel="noopener" target="_blank">Bitkey</a> shares 95% of its mobile codebase with KMP and was able to tear down silos so that Android and iOS engineers became mobile engineers, picking up tickets no matter the platform</li>



<li><a href="http://www.blackstonepublishing.com" rel="noopener" target="_blank">Blackstone</a> achieved a 50% increase in implementation speed within six months of code consolidation, sharing ~90% of business logic with KMP.</li>



<li><a href="https://www.youtube.com/watch?v=RJtiFt5pbfs" rel="noopener" target="_blank">Duolingo</a> saved 6–12 engineer-months leveraging KMP to deliver iOS and Web implementations after the initial Android implementation. They spent five engineer-months to adopt KMP and deliver the iOS version of Adventures, then only one and a half engineer-months to deliver it to web, leveraging the same KMP codebase compared to 9 months for the initial Android implementation.&nbsp;</li>



<li><a href="https://www.forbes.com/sites/forbes-engineering/2023/11/13/forbes-mobile-app-shifts-to-kotlin-multiplatform/" rel="noopener" target="_blank">Forbes</a> achieved significant savings in engineering time and effort by consolidating over 80% of logic across platforms, sharing ~90% of business logic in total.</li>



<li><a href="https://android-developers.googleblog.com/2024/05/android-support-for-kotlin-multiplatform-to-share-business-logic-across-mobile-web-server-desktop.html" rel="noopener" target="_blank">Google</a> has been investing in and transitioning to KMP for several years, stating that KMP allows for “flexibility and speed in delivering valuable cross-platform experiences”. The <a href="https://www.youtube.com/watch?v=5lkZj4v4-ks" rel="noopener" target="_blank">Google Workspace</a> team found that iOS runtime performance and app size with KMP were on par with those of the existing code.</li>



<li><a href="https://www.youtube.com/watch?v=hZPL8QqiLi8" rel="noopener" target="_blank">Philips</a> effectively halved the time to develop features on both Android and iOS.</li>



<li>An information security company re-targeted their mobile app to the web in three weeks for a press conference after a third-party vendor blocked the release of their mobile apps. Thanks to KMP, it was very easy to call the already implemented and tested code from JavaScript.</li>



<li>A national media company built its KMP Identity SDK for use across brand apps on Android, iOS, and web, with a team half the size of that typically allocated for platform-specific projects.</li>



<li>A world leader in tabletop gaming accelerated a multi-year mobile roadmap into a single quarter with KMP to meet the needs of explosive growth and demographic shift towards mobile users.<br /></li>
</ul>



<p>For more stories discussing real-world strategies, integration approaches, and gains from KMP, check out the Kotlin Multiplatform case studies collected by JetBrains.</p>



<div class="buttons">
        <div class="buttons__row">
            <a class="ek-link jb-download-button" href="https://kotlinlang.org/case-studies/?type=multiplatform" rel="noopener" target="_blank" title="See KMP case studies">See KMP case studies</a>
         </div>
</div>



<h2 class="wp-block-heading"><strong>Strategic recommendation</strong></h2>



<p>Kotlin Multiplatform is a future-proof architectural standard developed by JetBrains and supported by Google. It offers a low-risk, high-reward path for organizations looking to modernize their mobile strategy. Most organizations that adopt KMP for shared logic see a measurable ROI within three to six months.</p>



<p>The strategic recommendation is to initiate a pilot project focusing on pure business logic areas, such as calculations, data models, and business rules. With a conservative sharing potential of 75% in these areas, scaling KMP will allow your organization to eliminate redundant effort and transition toward a high-velocity, unified engineering future.</p>



<p><strong>The Touchlab acceleration factor:</strong> While the long-term gains of KMP are inherent to the technology, expert guidance from experienced Kotlin Multiplatform practitioners, such as Touchlab, can help minimize the initial learning curve and accelerate adoption. Specialized assistance early in the adoption process prevents the trial-and-error phase that can stall pilot projects, ensuring the first success occurs quickly and the architectural benefits begin compounding immediately. When scaling challenges arise, Touchlab’s tools and experience take your KMP teams to the next level. Find out what Touchlab can do for you at <a href="https://touchlab.co" rel="noopener" target="_blank">https://touchlab.co</a>.</p>
