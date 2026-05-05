---
title: "Amper 0.10 – JDK Provisioning, a Maven Converter, Custom Compiler Plugins, and More"
url: "https://blog.jetbrains.com/amper/2026/03/amper-0-10/"
date: "Tue, 31 Mar 2026 08:20:01 +0000"
author: "Kirill Likhodedov"
feed_url: "https://blog.jetbrains.com/kotlin/feed/"
---
<p><a href="https://github.com/JetBrains/amper/releases/tag/v0.10.0" rel="noopener" target="_blank">Amper 0.10.0</a> is out, and it brings a variety of new features, such as JDK provisioning, custom Kotlin compiler plugins, a Maven-to-Amper converter, and numerous IDE improvements! Read on for all of the details, and see <a href="https://github.com/JetBrains/amper/releases/tag/v0.10.0" rel="noopener" target="_blank">the release notes</a> for the full list of changes and bug fixes.</p>



<p><em>To get support for Amper’s latest features, use </em><a href="https://www.jetbrains.com/idea/download/other/#releases-2025" rel="noopener" target="_blank"><em>IntelliJ IDEA 2025.3.4 </em></a><em>&nbsp;or</em><a href="https://www.jetbrains.com/idea/" rel="noopener" target="_blank"><em> IntelliJ IDEA 2026.1</em></a><em> (or newer). Make sure the latest version of the Amper plugin is installed.&nbsp;</em></p>



<h2 class="wp-block-heading">JDK provisioning</h2>



<p>Amper needs a JDK (Java Development Kit) in order to perform various tasks in the project: compile Kotlin and Java sources, run tests, run JVM apps, etc.</p>



<p>Our philosophy is that you should be able to run your project without manually installing anything on your machine or having to configure anything. This is why Amper is able to provision a JDK automatically for you – JDK 21 by default.&nbsp;</p>



<p>However, some projects require specific JDK versions. You can now specify the criteria for the necessary JDK in <code>module.yaml</code>, and Amper will download and install the matching JDK.</p>



<pre class="EnlighterJSRAW">settings:
  jvm:
    jdk:
      version: 21 # major version
      distributions: [ zulu, temurin ] # acceptable distributions</pre>



<p>Amper also takes the <code>JAVA_HOME</code> environment variable into account, since it is a common way to set the JDK to be used on the machine. You can read more about Amper’s JDK provisioning behavior <a href="https://amper.org/0.10/user-guide/advanced/jdk-provisioning/#jdk-provisioning" rel="noopener" target="_blank">in the documentation</a>.</p>



<h2 class="wp-block-heading">Maven converter and Maven plugin compatibility</h2>



<p>If you have an existing Maven project, you don’t have to rewrite your build configuration from scratch. This release introduces a semi-automated conversion tool that reads your <code>pom.xml</code> files, including those in multi-module reactor projects, and generates the corresponding <code>project.yaml</code> and <code>module.yaml</code> files for you. To use it, simply run:</p>



<pre class="EnlighterJSRAW">./amper tool convert-project</pre>



<p>The converter maps your dependencies, BOMs, repositories, publishing coordinates, compiler flags, and other settings to their Amper equivalents. To support using both build systems during the transition, it sets <code>layout: maven-like</code> in every module so that your source directory structure, including <code>src/main/java</code> and <code>src/main/kotlin</code>, stays the same and no files need to be moved.</p>



<p>Well-known Maven plugins such as <code>maven-compiler-plugin</code> and <code>spring-boot-maven-plugin</code> are translated into built-in Amper settings. Other Maven plugins are added to the new <code>mavenPlugins</code> configuration section in <code>module.yaml</code>, and Amper can execute them during the build process through our <a href="https://amper.org/0.10/user-guide/advanced/maven-plugins/" rel="noopener" target="_blank">Maven plugin compatibility layer</a>.</p>



<p>The conversion is best-effort, so some projects may require tweaks afterward. For a full walkthrough and a list of limitations, see <a href="https://amper.org/0.10/getting-started/migrating-from-maven/" rel="noopener" target="_blank">the documentation</a>.</p>



<h2 class="wp-block-heading">Kotlin compiler plugins</h2>



<p>This release brings support for <a href="https://amper.org/0.10/user-guide/advanced/kotlin-compiler-plugins/#third-party-compiler-plugins" rel="noopener" target="_blank">third-party Kotlin compiler plugins</a>. Enabling this support is as easy as adding the following to <code>module.yaml</code>:</p>



<pre class="EnlighterJSRAW">settings:
  kotlin:
    compilerPlugins:
      - id: org.example.my.plugin
        dependency: org.example:my-plugin:1.0.0
        options:
          myKey1: myValue1
          myKey2: myValue2</pre>



<p>See <a href="https://amper.org/0.10/user-guide/advanced/kotlin-compiler-plugins/#third-party-compiler-plugins" rel="noopener" target="_blank">the documentation</a> for examples and how to enable IDE support for custom plugins.</p>



<p>We also added built-in support for the <a href="https://amper.org/0.10/user-guide/builtin-tech/kotlinx-rpc/" rel="noopener" target="_blank">kotlinx.rpc</a> and <a href="https://amper.org/0.10/user-guide/advanced/kotlin-compiler-plugins/#js-plain-objects" rel="noopener" target="_blank">JsPlainObjects</a> compiler plugins.&nbsp;</p>



<h2 class="wp-block-heading">IDE improvements</h2>



<h3 class="wp-block-heading">Reworked UX for running Amper commands</h3>



<p>We’ve revisited the UI for creating and editing run configurations in the IDE. New custom views allow you to configure the options for <code>run</code> and <code>test</code> commands in a more convenient way:</p>



<figure class="wp-block-image size-full"><img alt="" class="wp-image-695104" src="https://blog.jetbrains.com/wp-content/uploads/2026/03/image-55.png" style="width: 100% !important; height: auto !important;" /></figure>



<figure class="wp-block-image size-full"><img alt="" class="wp-image-695325" src="https://blog.jetbrains.com/wp-content/uploads/2026/03/test.png" style="width: 100% !important; height: auto !important;" /></figure>



<p>Additionally, you can now create a configuration for any Amper command by choosing <em>Amper</em> in the <em>Add New Configuration</em> menu:</p>



<figure class="wp-block-image size-full"><img alt="" class="wp-image-695336" src="https://blog.jetbrains.com/wp-content/uploads/2026/03/build.png" style="width: 100% !important; height: auto !important;" /></figure>



<p>If you want to run a command in an ad-hoc way, you can use <a href="https://www.jetbrains.com/help/idea/running-anything.html" rel="noopener" target="_blank"><em>Run Anything</em></a> (<em>Ctrl+Ctrl</em>) and prepend your command with amper:</p>



<figure class="wp-block-image size-full"><img alt="" class="wp-image-695347" src="https://blog.jetbrains.com/wp-content/uploads/2026/03/runanything.png" style="width: 100% !important; height: auto !important;" /></figure>



<h3 class="wp-block-heading">Run gutters for native applications in module.yaml</h3>



<p>Native applications (<code>linux/app</code>, <code>macos/app</code>, <code>windows/app</code>) can now be run from the IDE via the gutter:</p>



<figure class="wp-block-image size-full is-resized"><img alt="" class="wp-image-695143" src="https://blog.jetbrains.com/wp-content/uploads/2026/03/image-57.png" style="width: 430px; height: auto; width: 100% !important; height: auto !important;" /></figure>



<h3 class="wp-block-heading">Better test names in the <em>Test </em>tool window</h3>



<p>The <code>@DisplayName</code> and <code>@ParameterizedTest.name</code> JUnit 5 annotations are now respected in the <em>Test </em>tool window when showing the test execution tree.<br /></p>



<pre class="EnlighterJSRAW">@ParameterizedTest(name = "Test #{0}")
@DisplayName("My parameterized test")
@ValueSource(ints = [1, 2, 3])
fun parameterized(i: Int) {}</pre>



<figure class="wp-block-image size-full is-resized"><img alt="" class="wp-image-695358" src="https://blog.jetbrains.com/wp-content/uploads/2026/03/testwindow.png" style="width: 510px; height: auto; width: 100% !important; height: auto !important;" /></figure>



<h3 class="wp-block-heading">Ktor plugin assistance</h3>



<p>If your module has the Ktor server dependency, the <code>module.yaml</code> file provides support for searching and adding plugins via the <em>Add Plugins…</em> inlay:</p>



<figure class="wp-block-image size-full is-resized"><img alt="" class="wp-image-695244" src="https://blog.jetbrains.com/wp-content/uploads/2026/03/image-60.png" style="width: 387px; height: auto; width: 100% !important; height: auto !important;" /></figure>



<p>Alternatively, you can use <a href="https://www.jetbrains.com/help/idea/ktor.html#generate" rel="noopener" target="_blank">completion in the Kotlin code</a>, which will add all the necessary dependencies to the module without you even having to touch the <code>module.yaml</code> file:</p>



<figure class="wp-block-image size-full is-resized"><img alt="" class="wp-image-695202" src="https://blog.jetbrains.com/wp-content/uploads/2026/03/image-58.png" style="width: 537px; height: auto; width: 100% !important; height: auto !important;" /></figure>



<h3 class="wp-block-heading">Support for profiling JVM applications</h3>



<p><em>Note: This feature requires IntelliJ IDEA Ultimate.</em></p>



<p>The configuration for the run command in jvm/app modules can now be run using <a href="https://www.jetbrains.com/help/idea/profiler-intro.html" rel="noopener" target="_blank">IntelliJ IDEA’s support for profilers</a>:</p>



<figure class="wp-block-image size-full is-resized"><img alt="" class="wp-image-695216" src="https://blog.jetbrains.com/wp-content/uploads/2026/03/image-59.png" style="width: 435px; height: auto; width: 100% !important; height: auto !important;" /></figure>



<h2 class="wp-block-heading">Amper plugin development</h2>



<p><a href="https://blog.jetbrains.com/amper/2025/11/amper-update-november-2025/">The previous release of Amper</a> brought the preview of Amper’s extensibility system. We received a lot of feedback, and we are working on extending the capabilities of plugins. While the ability to publish and share plugins is still a work in progress, a valuable improvement is already available in this release: you can now reference the module settings from the plugin using <code>${module.settings}</code> in <code>plugin.yaml</code>:</p>



<figure class="wp-block-image size-full"><img alt="" class="wp-image-695125" src="https://blog.jetbrains.com/wp-content/uploads/2026/03/image-56.png" style="width: 100% !important; height: auto !important;" /></figure>



<h2 class="wp-block-heading">Other improvements</h2>



<p>Starting with version 0.10, Amper supports<a href="https://maven.apache.org/guides/introduction/introduction-to-profiles.html#Profiles_in_POMs" rel="noopener" target="_blank"> Maven profiles</a> declared in the POM files of transitive dependencies.</p>



<p>In this release, we’ve also introduced the ability to add module descriptions in <code>module.yaml</code>. The description is formatted in Markdown and can occupy multiple lines. This text is used by the <code>./amper show modules</code> command in the CLI, as well as by the IDE to show information about the module. For libraries, it is also used as a description in published metadata by default.</p>



<figure class="wp-block-image size-full is-resized"><img alt="" class="wp-image-695369" src="https://blog.jetbrains.com/wp-content/uploads/2026/03/showmodules.png" style="width: 517px; height: auto; width: 100% !important; height: auto !important;" /></figure>



<h2 class="wp-block-heading">Updated default versions</h2>



<p>We updated some of our default versions for toolchains and frameworks:</p>



<ul>
<li>Kotlin: 2.3.20</li>



<li>Android minimum SDK: 23</li>



<li>Compose: 1.10.3</li>



<li>KSP: 2.3.6</li>



<li>Ktor: 3.4.1</li>



<li>Spring Boot: 4.0.5</li>
</ul>



<h2 class="wp-block-heading">Try Amper 0.10.0</h2>



<p>To update an existing project, use the <code>./amper update</code> command.</p>



<p>To get started with Amper, check out our <a href="https://jb.gg/amper/get-started" rel="noopener" target="_blank"><em>Getting started</em></a> guide. Take a look at some examples, follow a tutorial, or read the comprehensive user guide depending on your learning style.</p>


    <div class="buttons">
        <div class="buttons__row">
                                                <a class="btn" href="https://amper.org" rel="noopener" target="">Try Amper</a>
                                                    </div>
    </div>







<h2 class="wp-block-heading">Share your feedback</h2>



<p>Amper is still experimental and under active development. You can provide feedback about your experience by joining the discussion in the <a href="https://slack-chats.kotlinlang.org/c/amper" rel="noopener" target="_blank">Kotlinlang Slack’s #amper channel</a> or sharing your suggestions and ideas in a <a href="https://youtrack.jetbrains.com/issues/AMPER" rel="noopener" target="_blank">YouTrack issue</a>. Your input and use cases help shape the future of Amper!</p>
