---
title: "Lessons from Log4Shell: Building a CRA-Ready Log4j"
url: "https://news.apache.org/foundation/entry/lessons-from-log4shell-building-a-cra-ready-log4j"
date: "Tue, 21 Apr 2026 15:36:11 +0000"
author: "The ASF"
feed_url: "https://news.apache.org/feed"
---
<p class="wp-block-paragraph"><em>By: <a href="mailto:pkarwasz@apache.org">Piotr P. Karwasz</a>, VP Logging, Apache Software Foundation</em></p>



<p class="wp-block-paragraph">The disclosure of Log4Shell (CVE-2021-44228) on December 9, 2021 did not just expose a vulnerability: it exposed a way of building software that was no longer fit for purpose, and it helped bring the European Cyber Resilience Act into being.</p>



<p class="wp-block-paragraph">I recently hosted a session for the Open Regulatory Compliance community&#8217;s <a href="https://www.youtube.com/playlist?list=PLy7t4z5SYNaT-DjqGR0ORSSZGZYW8qmRs">CRA Monday series</a> to tell the story from the inside: what the Apache Logging team actually did in the years after Log4Shell to rebuild the project as something CRA-ready.</p>



<p class="wp-block-paragraph">This blog recaps and expands upon that session; you can also <a href="https://www.youtube.com/watch?v=ns9RBhEsz_U">watch the recording</a> or <a href="https://github.com/orcwg/orcwg/blob/main/events/cra-mondays/2026-03-16-Lessons%20from%20Log4Shell_%20building%20a%20CRA-ready%20Log4j.pdf">view the slides</a>.</p>



<h2 class="wp-block-heading">A Wake-Up Call for the Software Ecosystem</h2>



<div class="wp-block-media-text is-stacked-on-mobile"><figure class="wp-block-media-text__media"><img alt="" class="wp-image-7532 size-full" height="144" src="https://news.apache.org/wp-content/uploads/2026/04/log4jlogo.png" width="349" /></figure><div class="wp-block-media-text__content">
<p class="wp-block-paragraph">Log4Shell&#8217;s impact was unprecedented in scale. Apache Log4j is embedded so deeply across the software ecosystem that the vulnerability propagated almost everywhere at once and most organizations had no idea where they were exposed. The rush to assess risk revealed a fundamental problem: few teams maintained a reliable Software Bill of Materials (SBOM), and the question &#8220;are we affected?&#8221; had no quick answer.</p>



<p class="wp-block-paragraph">The scramble had at least one useful side effect: it pushed many teams to finally migrate from Log4j 1, already end-of-life since 2015, to Log4j 2.</p>
</div></div>



<p class="wp-block-paragraph"></p>



<h2 class="wp-block-heading">Lessons from Log4j perspective</h2>



<p class="wp-block-paragraph">Since Log4j is mostly consumed as a dependency rather than built upon, the lessons the Apache Software Foundation’s Logging Services team drew from Log4Shell were different from those of the broader ecosystem. The problems were not about visibility into our own dependencies, but about the state of the project itself:</p>



<ul class="wp-block-list">
<li><strong>Documentation</strong> was hard to navigate, with many features either undocumented or described only in terms a new contributor could not act on.</li>



<li><strong>The release process</strong> was antiquated, understood by only a handful of people, and run on personal hardware: a single point of failure that nobody had reason to address until a crisis made it unavoidable.</li>



<li><strong>Builds were slow and tests were flaky</strong>, meaning a failure late in a multi-hour process sent you back to the beginning.</li>
</ul>



<p class="wp-block-paragraph">None of these were unique to Log4j. Log4Shell made them impossible to ignore, and addressing them put us on a path that anticipates much of what the CRA now asks of software maintainers.</p>



<h2 class="wp-block-heading">Documentation: from maintainer knowledge to public record</h2>



<p class="wp-block-paragraph">Logging is not always safe. There are real security concerns: CRLF injection from unstructured logging; sensitive information leaking into debug output; and injection of Log4j formatting patterns through user-supplied strings. Before Log4Shell, much of this knowledge lived in the heads of a few maintainers: not written down, not discoverable, and not actionable for the thousands of teams depending on the library.</p>



<p class="wp-block-paragraph">We rewrote the documentation website from scratch. The goal was to turn that private knowledge base into a public record by:</p>



<ul class="wp-block-list">
<li>Covering security best practices and an explicit <a href="https://logging.apache.org/security.html"><strong>security model</strong></a></li>



<li>Providing reference documentation generated directly from code, so it stays in sync as the library evolves</li>



<li>Making Log4j&#8217;s versioning policy and support status explicit and visible, both required for CRA attestationsMoving the issue tracker from JIRA closer to the code in GitHub Issues</li>



<li>Mirroring some discussions on both GitHub Discussions and mailing lists </li>
</ul>



<p class="wp-block-paragraph">The results were measurable: more documentation pull requests, more site visits, a useful proxy for coverage and clarity, and noticeably better answers from LLMs trained on our new content.</p>



<h2 class="wp-block-heading">Release process: from manual to reproducible</h2>



<p class="wp-block-paragraph">In December 2021, Log4j&#8217;s tests ran on a Jenkins instance, binaries were built on maintainer machines, signing was manual, and builds were not reproducible. A full binary and site build literally took <strong>hours</strong>. This was not unusual for open source projects, but it created real risks around build integrity, and it was clearly not sustainable.</p>



<p class="wp-block-paragraph">By September 2024 we had migrated to GitHub Actions, achieved<strong> </strong>reproducible<strong> </strong>builds<strong> </strong>signed by a CI GPG key only known to ASF admins, parallelized tests, and reduced the build-and-deploy cycle to around 30 minutes.</p>



<p class="wp-block-paragraph">Currently:</p>



<ul class="wp-block-list">
<li>The CI pipeline now automatically stages releases up to the voting phase: the first project in The ASF to do this.</li>



<li>We are working on integration with Apache Trusted Releases, which will bring automation to the voting and publishing steps as well.</li>



<li>We are working on full-SLSA build and source attestations, which will make us one of the first ASF projects to achieve this. This includes SLSA source level 4, requiring a non-author review for every commit: a critical guarantee for a project at the center of the most significant supply-chain incident in recent memory.</li>
</ul>



<h2 class="wp-block-heading">Machine-readable metadata: SBOMs, VEX, and beyond</h2>



<p class="wp-block-paragraph">One of the most concrete CRA requirements is the expectation that software comes with machine-readable security information. We now publish CycloneDX SBOMs to Maven Central, which references a Vulnerability Disclosure Report, a machine-readable version of our CVE list, on our website. This gives downstream users a complete, well-curated source of vulnerability information, unaffected by the data loss that public vulnerability databases sometimes introduce when converting between formats. It is also open to improvements by contributors.</p>



<p class="wp-block-paragraph">The next step is Vulnerability Exploitability eXchange (VEX) statements, generated automatically through an open source <a href="https://github.com/vex-generation-toolset">toolset</a> we are developing with OpenRefactory. The system combines:</p>



<ul class="wp-block-list">
<li>An AI-backed Root Cause Service that identifies vulnerable methods</li>



<li>A Call Graph Service that maps per-component call graphs</li>



<li>A VEX Generation Service that determines the maximum reachable path and generates enriched VEX statements, which we call VEXplanations</li>
</ul>



<p class="wp-block-paragraph">We are currently testing this within Apache Solr and plan to extend it to Log4j and Commons. The goal is to give downstream users a machine-readable guarantee of no known exploitable vulnerabilities, assessed automatically rather than by hand.</p>



<p class="wp-block-paragraph">We are also planning to support Common Lifecycle Enumeration (ECMA-428), the machine-readable equivalent of our supported versions list, generated ASF-wide through Apache Trusted Releases.</p>



<h2 class="wp-block-heading">Vulnerability handling: from ad hoc to structured</h2>



<p class="wp-block-paragraph">Since Log4Shell, the Logging Services team has put in place several structural improvements to vulnerability handling:</p>



<ul class="wp-block-list">
<li>A dedicated reporting address (security@logging.apache.org) separate from the general ASF security team</li>



<li>An explicit threat model that clearly separates the security responsibilities of Log4j from those of its consumers</li>



<li>A bug bounty program hosted on YesWeHack, funded by the Sovereign Tech Resilience program</li>
</ul>



<p class="wp-block-paragraph">Since July 2024, the program has received 140 reports across Log4cxx, Log4j, and Log4net, yielding 10 CVEs.</p>



<p class="wp-block-paragraph">At the architectural level, Log4j 3 addresses the attack surface problem directly. Log4j 2 was built for a pre-Maven world and shipped as a monolithic core with many optional dependencies bundled in. Log4j 3 modularises most of that core, so each module only pulls in what it actually needs. Smaller surface area means fewer things that can go wrong.</p>



<h2 class="wp-block-heading">Community sustainability: the hardest and unsolved problem</h2>



<p class="wp-block-paragraph">All of the above is meaningless if the project burns out. And that risk is real. Log4j currently has two active maintainers doing most of the work. Log4cxx and Log4net are in a similar position. This is not a criticism of the community. It is the structural reality of most open source projects, and it is the problem that CRA compliance pressure will make worse if it is not addressed alongside the technical work.</p>



<p class="wp-block-paragraph">We are exploring two directions:</p>



<ul class="wp-block-list">
<li>The <a href="https://www.open-source-economy.com/">Open Source Economy</a> initiative: offering consulting and compliance attestations as a funded model, where fees support maintainers, infrastructure, and upstream dependencies.</li>



<li>ECMA TC-54’s <a href="https://tc54.org/contributing-yaml/">CONTRIBUTING.yaml</a> specification: a machine-readable format for describing a project&#8217;s maintenance status, contribution needs, and support expectations, so that users and organisations can understand what they are depending on.</li>
</ul>



<p class="wp-block-paragraph">There is also a cultural dimension. Many open source communities grew up in an era when maintainers could commit directly to the repository and build software on their own machines. Shifting to a model where maintainers review all contributions and CI builds everything is the right move for security, but it is genuinely less fun, and communities resist it. Making that transition while keeping contributors engaged is one of the real challenges of building a CRA-ready project.</p>



<h2 class="wp-block-heading">What CRA readiness actually looks like</h2>



<p class="wp-block-paragraph">The CRA introduces a set of &#8220;due diligence&#8221; requirements for manufacturers and voluntary attestations for OSS projects. What I hope this post makes clear is that meeting them is not a compliance exercise you bolt on at the end. It is the output of years of work on documentation, build integrity, machine-readable metadata, vulnerability processes, and community health.</p>



<p class="wp-block-paragraph">The good news is that the direction was right before the regulation arrived. Log4Shell forced hard questions that led, eventually, to a project that is genuinely more secure, more transparent, and more sustainable than it was in 2021. The CRA gives that work a formal framework and, ideally, the organisational backing to fund it. For other open source projects facing the same pressures, that is perhaps the most useful takeaway: the path to CRA readiness and the path to a healthier, more sustainable project are the same path.</p>



<h2 class="wp-block-heading">Get involved</h2>



<p class="wp-block-paragraph">The path to a CRA-ready ecosystem is not walked by one project. If any of this matters to you, here is where to start:</p>



<ul class="wp-block-list">
<li><strong>Contribute to Log4j directly.</strong> Code, documentation, and review all count. Start at <a href="https://logging.apache.org/">logging.apache.org</a>, or report security issues to security@logging.apache.org.</li>



<li><strong>Help across the ASF.</strong> The <a href="https://security.apache.org/contributing/">Contributing to Apache Security</a> guide is the way in.</li>



<li><strong>Shape OSS regulatory response.</strong> The <a href="https://orcwg.org/">Open Regulatory Compliance Working Group</a> is where CRA implementation for open source is being worked out in public.</li>



<li><strong>Define what sustainable maintenance means.</strong> <a href="https://tc54.org/contributing-yaml/">ECMA TC54 TG4</a> is building the machine-readable standards for project health and support.</li>



<li><strong>Secure the supply chain.</strong> The <a href="https://slsa.dev/community">SLSA Community</a> is advancing the build-integrity framework Log4j will soon use.</li>
</ul>
<p>The post <a href="https://news.apache.org/foundation/entry/lessons-from-log4shell-building-a-cra-ready-log4j">Lessons from Log4Shell: Building a CRA-Ready Log4j</a> appeared first on <a href="https://news.apache.org">The ASF Blog</a>.</p>
