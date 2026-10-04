---
title: "A More Reliable Compilation Scheme for Kotlin Multiplatform Modules"
url: "https://blog.jetbrains.com/kotlin/2026/09/a-more-reliable-compilation-scheme-for-kotlin-multiplatform-modules/"
date: "2026-09-28"
author: "Aleksey Zamulla"
feed_url: "https://blog.jetbrains.com/kotlin/feed/"
---
The current compilation approach to Kotlin Multiplatform projects works, but sometimes can lead to unexpected or hard-to-predict behavior. For example: With Kotlin 2.5.0-Beta1, we introduced an optional “separate compilation” approach to KMP that solves both problems: make the compilation results consistent with IDE analysis, and point more consistently to problematic calls of library code from […]
