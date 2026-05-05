---
title: "ASF Project Spotlight: Apache Iceberg"
url: "https://news.apache.org/foundation/entry/asf-project-spotlight-apache-iceberg"
date: "Wed, 29 Apr 2026 16:22:08 +0000"
author: "The ASF"
feed_url: "https://news.apache.org/feed"
---
<p class="wp-block-paragraph"><em>Dipankar Mazumdar is the Director of Developer Relations at Cloudera, leading global developer initiatives across lakehouse architecture and AI. He previously held advocacy and engineering roles at Dremio, Onehouse, and Qlik, contributing to open source projects including Apache Iceberg, Apache Hudi, and Apache XTable (incubating) and building communities. His work focuses on the intersection of data engineering and AI. He is the author of </em>Engineering Lakehouses with Open Table Formats<em> and a contributor to </em>Apache Iceberg: The Definitive Guide. </p>



<p class="wp-block-paragraph">Apache Iceberg has quickly become a foundational technology in modern data architectures—but its impact goes far beyond performance and scale. This conversation with Dipankar explores how Iceberg redefined the data lake, and how community, education, and open collaboration fueled its adoption.</p>



<figure class="wp-block-image size-full"><img alt="" class="wp-image-7570" height="196" src="https://news.apache.org/wp-content/uploads/2026/04/iceberg.png" width="720" /></figure>



<h2 class="wp-block-heading">What Is Apache Iceberg and Why It Exists</h2>



<p class="wp-block-paragraph"><strong>Q: Can you tell us a bit about Apache Iceberg?</strong><strong><br /></strong><strong>A:</strong> Apache Iceberg is a high-performance open table format for huge analytic datasets. It was designed to bring reliability and simplicity to data lakes, allowing multiple engines to safely read and write to the same datasets with strong guarantees. By introducing a table abstraction on top of raw data files, Iceberg helps organizations manage large-scale data with the consistency typically associated with data warehouses while retaining the flexibility of data lakes.</p>



<p class="wp-block-paragraph"><strong>Q: What did the data ecosystem look like before Apache Iceberg, and what gaps did you see early on?</strong><strong><br /></strong><strong>A:</strong> Before Iceberg, most data lakes relied on tables built on technologies like Apache Hive, with Apache Parquet as the underlying storage format. These systems worked well in the Hadoop era, but as workloads diversified and organizations were moving to cloud object stores (like Amazon S3), a number of structural limitations began to surface. Updates were unreliable, partitioning strategies were brittle, and schema evolution was difficult to manage. Metadata handling also became increasingly expensive, especially with large numbers of files, and query performance would degrade over time.</p>



<p class="wp-block-paragraph">At the same time, data warehouses abstracted all of this away behind proprietary systems, so many engineers never had to think about these problems directly. However, they were dealing with significant cost issues and entering a vendor-locked environment.&nbsp;</p>



<p class="wp-block-paragraph">These limitations/issues in both data lakes and warehouses made it clear that a new approach was needed that treated tables as first-class objects rather than just collections of files.</p>



<h2 class="wp-block-heading">From Netflix to Apache: How Iceberg Took Shape</h2>



<p class="wp-block-paragraph"><strong>Q: When was Iceberg started and why?</strong><strong><br /></strong><strong>A:</strong> Iceberg was originally developed at Netflix to address these large-scale data challenges. The team needed a solution that could handle massive datasets reliably while supporting evolving data requirements. Recognizing that these challenges were industry-wide, the project was later open sourced and contributed to The Apache Software Foundation (ASF) in 2018 to foster broader collaboration and adoption.</p>



<p class="wp-block-paragraph"><strong>Q: What technology problem is Apache Iceberg solving?</strong><strong><br /></strong><strong>A:</strong> Iceberg addresses several fundamental issues in traditional data lakes:</p>



<ul class="wp-block-list">
<li>Lack of consistency when multiple engines works on the same data</li>



<li>Complex and brittle partitioning strategies</li>



<li>Interoperability at the storage layer</li>



<li>Challenges with schema and partition evolution</li>
</ul>



<p class="wp-block-paragraph">By rethinking how tables are defined and managed, Iceberg enables scalable, reliable data operations without the overhead and fragility of legacy approaches.</p>



<p class="wp-block-paragraph"><strong>Q: What were some of the most important design decisions that shaped Iceberg early on?</strong><strong><br /></strong><strong>A:</strong> A few key principles guided Iceberg’s design:</p>



<ul class="wp-block-list">
<li>Treating metadata as a first-class concern for performance and scalability</li>



<li>Decoupling logical table structure from physical storage layout</li>



<li>Supporting schema evolution as a core feature, not an afterthought</li>



<li>Enabling engine-agnostic access to the data</li>
</ul>



<p class="wp-block-paragraph">These decisions allowed Iceberg to avoid many of the constraints that limited earlier systems.</p>



<h2 class="wp-block-heading">Real-World Use and Impact</h2>



<p class="wp-block-paragraph"><strong>Q: Iceberg is known for working across multiple compute engines. Why was that so important from the start?<br />A:</strong> Interoperability is essential because modern data ecosystems rely on multiple processing engines. Iceberg was designed to act as a shared table layer, enabling different tools to safely access the same data without tight coupling. This approach gives organizations flexibility and helps prevent vendor lock-in.</p>



<p class="wp-block-paragraph"><strong>Q: Are there any use cases you would like to tell us about?</strong><strong><br /></strong><strong>A:</strong> Iceberg is used across industries for:</p>



<ul class="wp-block-list">
<li>Large-scale analytics and reporting</li>



<li>AI pipelines</li>



<li>Streaming and batch data processing</li>
</ul>



<p class="wp-block-paragraph">It enables teams to unify different workloads on a single, reliable data foundation.</p>



<h2 class="wp-block-heading">The Challenge: Explaining a New Layer of the Data Stack</h2>



<p class="wp-block-paragraph"><strong>Q: What made evangelizing Iceberg particularly challenging in those early days?</strong><strong><br /></strong><strong>A:</strong> The challenge wasn’t just that Iceberg was new &#8211; it was that the problem it addressed wasn’t clearly recognized.</p>



<p class="wp-block-paragraph">From the outside, most systems appeared to be working. Data warehouses handled structured workloads, data lakes handled large-scale storage, and teams had already built processes around them. So when we started talking about open table formats and formal specifications, it didn’t feel like solving an urgent problem.</p>



<p class="wp-block-paragraph">Even when issues did exist, they weren’t attributed to the table abstraction itself. Slow jobs, partitioning limitations, schema breakages, or data corruption from concurrent writes were seen as isolated operational problems. Teams would patch them with scripts, conventions, or workarounds rather than questioning the underlying design.</p>



<p class="wp-block-paragraph">That made the conversation harder. We weren’t just introducing a new approach &#8211; we were pointing out that a foundational layer people relied on had limitations they hadn’t fully understood yet.</p>



<h2 class="wp-block-heading">Building Understanding: Advocacy, Education, and Content</h2>



<p class="wp-block-paragraph"><strong>Q: How did advocacy around Iceberg evolve beyond the core PMC &amp; Committers?</strong><strong><br /></strong><strong>A</strong>: In the beginning, most of the evangelism came from the engineers building the project. There wasn’t a structured effort around storytelling or community building.</p>



<p class="wp-block-paragraph">That started to change when dedicated developer advocacy roles emerged with a focus on Iceberg. I was lucky enough to have been in that boat. Back in 2021-2022, there was no established playbook for how to evangelize an open table format. The approach was largely experimental, i.e. learning in public, translating technical concepts into something practical, and consistently engaging with the community.</p>



<p class="wp-block-paragraph">Over time, that effort became more deliberate. If the abstraction wasn’t obvious to most people, we had to make it understandable. And in places where the ecosystem didn’t even have the right words, we had to build that vocabulary ourselves.</p>



<p class="wp-block-paragraph"><strong>Q: At what point did you realize that community and education would play a central role?</strong><strong><br /></strong><strong>A:</strong> It became clear fairly early that adoption was not going to happen through feature awareness alone. We were asking people to think about a layer they had historically never needed to reason about. Most engineers were focused on pipelines, performance, and reliability. The storage layer was abstracted away, and the issues they encountered, like slow jobs, partitioning problems, schema breakages, were treated as isolated operational concerns rather than symptoms of a deeper limitation.</p>



<p class="wp-block-paragraph">So before we could talk about Iceberg, we had to establish why the table abstraction itself mattered. That meant explaining what a table actually represents in a data lake, how metadata governs behavior, and why things like snapshot isolation or partition evolution are not features, but necessary primitives for running multiple workloads safely.</p>



<p class="wp-block-paragraph">Again, those are not surface-level concepts. They require building mental models from the ground up. That’s where community and education became central!</p>



<p class="wp-block-paragraph"><strong>Q: You mentioned technical content playing a huge role in Iceberg’s early adoption. Can you share more?</strong><strong><br /></strong><strong>A: </strong>Absolutely!<strong> </strong>Long-form technical content became the foundation for our advocacy goals. We definitely were more interested in explaining how the system actually works &#8211; what happens under the hood, how reads and writes are executed, and how design decisions translate into real-world behavior.</p>



<p class="wp-block-paragraph">At the same time, it became clear that reading alone wasn’t enough for engineers. They needed to run things themselves and understand what Iceberg brought to the table. That led to hands-on exercises where people could experiment with things like table creation, schema evolution, partitioning, and table optimization to see the behavior directly.</p>



<p class="wp-block-paragraph">Deep explanations paired with practical exploration helped bridge the gap between theory and adoption.</p>



<h2 class="wp-block-heading">Community in Action: From Conversations to Momentum</h2>



<p class="wp-block-paragraph"><strong>Q: How did live engagement (talks, office hours, etc.) shape the evolution of the Iceberg community?</strong><strong><br /></strong><strong>A: </strong>Webinars, conferences, and live sessions enabled a space for real-time interaction. Early on, a lot of those sessions were spent clarifying fundamentals &#8211; what Iceberg is and what it isn’t.</p>



<p class="wp-block-paragraph">But over time, the nature of the conversation changed. Engineers began bringing their own systems, workloads, and constraints into the discussion. And the questions shifted from conceptual to more operational &#8211; which was great to see. That’s what led to introducing dedicated office hours. Instead of one-to-many sessions, these became open forums where people could discuss real production scenarios. Those conversations became one of the most important feedback loops, shaping how we explained Iceberg and highlighting gaps in understanding.</p>



<p class="wp-block-paragraph">Conferences became places where the ecosystem came together. And the conversations moved from understanding the technology to discussing how it was being used in production. People started comparing approaches, sharing operational strategies, and aligning on best practices. That’s when you could see the shift that Iceberg was no longer just a technology that came out of Netflix- it was becoming a shared foundation that multiple organizations were building on.</p>



<p class="wp-block-paragraph"><strong>Q: Was there a moment when you realized Iceberg was gaining real momentum?</strong><strong><br /></strong><strong>A: </strong>The inflection point came when the community itself started shaping the narrative. Iceberg was never positioned as a vendor-owned format, and its specification evolved in the open.</p>



<p class="wp-block-paragraph">As more contributors, adopters, and organizations got involved, the conversation expanded beyond any single perspective. Integrations, use cases, and improvements came from different directions, each grounded in real production needs. At that point, growth was no longer something being driven &#8211; it was something emerging from the open source community itself. And that was a huge success for us!</p>



<p class="wp-block-paragraph"><strong>Q: How did you contribute to building and growing the Apache Iceberg community?</strong><strong><br /></strong><strong>A: </strong>A lot of the early Iceberg activity was fragmented. There were contributors building the system, users trying it out, and discussions happening across Slack, mailing lists, and conferences &#8211; but these weren’t always connected. We were quite deliberate about bringing that together.</p>



<p class="wp-block-paragraph">That meant consistently creating places where those interactions could happen. We were actively helping users on Slack, running webinars, publishing blogs and books, setting up office hours, and making sure the people actually building and using Iceberg were part of those conversations. Instead of keeping discussions isolated, we tried to pull them into shared spaces.</p>



<p class="wp-block-paragraph">Over time, that started to compound. We turned questions from users into topics for talks. There were production use cases that would show up in webinars or conferences. Conversations that happened in one place would get carried into others. It gave the community a kind of continuity that wasn’t there before.</p>



<p class="wp-block-paragraph">It also changed the level of discussion. Early on, most conversations were about understanding what Iceberg is. As more people got involved, it shifted toward how it behaves under real workloads &#8211; scale, concurrency, and multi-engine access. This shift came from repeatedly bringing practitioners into the same loop and letting those discussions build on each other.</p>



<p class="wp-block-paragraph">That’s really where we had the most impact &#8211; creating that loop and keeping it active.</p>



<h2 class="wp-block-heading">The Apache Way: Governance and Community Over Code</h2>



<p class="wp-block-paragraph"><strong>Q: How has moving to The ASF influenced the project’s growth and direction?</strong><strong><br /></strong><strong>A:</strong> Becoming part of The ASF reinforced Iceberg’s commitment to open governance and vendor neutrality. It created a foundation for long-term sustainability and encouraged broader participation from across the industry.</p>



<p class="wp-block-paragraph"><strong>Q: The ASF’s mission is to provide software for the public good. In what ways does your project embody that mission and the “community over code” ethos?</strong><strong><br /></strong><strong>A:</strong> Iceberg is developed in the open, with decisions shaped by a diverse community rather than a single organization. This collaborative approach ensures the technology serves a wide range of users and use cases. The emphasis on shared ownership and transparency reflects the ASF’s “community over code” philosophy.</p>



<h2 class="wp-block-heading">Getting Started and Getting Involved</h2>



<p class="wp-block-paragraph"><strong>Q: What advice would you give to teams considering adopting Iceberg today?</strong><strong><br /></strong><strong>A:</strong> Start by understanding your current data challenges and how Iceberg (and the open lakehouse ecosystem) align with your needs. Take advantage of its interoperability to experiment within your existing ecosystem and tools and engage with the community to learn from others’ experiences.</p>



<p class="wp-block-paragraph"><strong>Q: What’s the best way to learn about the project and try it out?</strong><strong><br /></strong><strong>A:</strong> The best way to get started is through the official documentation and by experimenting with Iceberg using your preferred data processing engine. Community channels (Slack), Dev mailing list, and conference talks also provide valuable guidance for new users (see links below).There are also other avenues like <a href="https://community.cloudera.com/t5/Blogs/ct-p/blogs">Cloudera Community</a>, <a href="https://youtube.com/playlist?list=PLe-h9HrA9qfDEdAxiPj9TqEAavd79Aneu&amp;si=T2kR5lHKquAHGXrZ">Developer Playlist</a>, and <a href="https://www.dremio.com/blog/apache-iceberg-101-your-guide-to-learning-apache-iceberg-concepts-and-practices/">Iceberg 101 (Dremio)</a> that focus on Iceberg’s education.&nbsp;</p>



<p class="wp-block-paragraph"><strong>Q: How can others contribute to this project (code contributions being only one of the ways)?</strong><strong><br /></strong><strong>A:</strong> Contributions come in many forms, including improving documentation, sharing use cases, participating in discussions, and helping others adopt the technology. These efforts are just as important as code in building a strong, sustainable project.</p>



<h2 class="wp-block-heading">What’s Next for Apache Iceberg</h2>



<p class="wp-block-paragraph"><strong>Q: What does the future hold for the project?</strong><strong><br /></strong><strong>A:</strong> A big part of where Iceberg is heading is around supporting newer workloads, especially AI-driven ones. We are starting to see more demand for things like wider schemas, semi-structured data, and access patterns that go beyond traditional analytics. To support that, there’s growing interest in areas like vector-based indexing and more flexible data representations.</p>



<p class="wp-block-paragraph">At the same time, there’s continued focus on making the system more efficient. That includes improvements to metadata handling and commit performance, which become more important as tables scale. There’s also work around indexing. Instead of relying only on file-level stats, newer proposals are exploring primary, secondary, and even vector-based indexes, especially as access patterns shift.</p>



<p class="wp-block-paragraph">Overall, the future direction is about evolving the core pieces, so Iceberg can support a broader set of workloads while keeping the same design principles.</p>



<h2 class="wp-block-heading">Iceberg Resources</h2>



<ul class="wp-block-list">
<li><a href="https://iceberg.apache.org/">Official Iceberg documentation</a></li>



<li><a href="https://iceberg.apache.org/community/">Join the Iceberg community&nbsp;</a></li>



<li><a href="https://apache-iceberg.slack.com/join/shared_invite/zt-3tkrk9gpf-1eFZ8ozS2In0~zM_BeZiRQ#/shared-invite/email">Discuss on Slack</a></li>
</ul>



<hr class="wp-block-separator has-alpha-channel-opacity" />



<p class="wp-block-paragraph">The ASF is home to nearly 9,000 committers contributing to more than 320 active projects including Apache Airflow, Apache Camel, Apache Flink, Apache HTTP Server, Apache Kafka, and Apache Superset. With the support of volunteers, developers, stewards, and more than 75 sponsors, ASF projects create open source software that is used ubiquitously around the world. This work helps us realize our mission of providing software for the public good.</p>



<p class="wp-block-paragraph">In the midst of hosting community events, engaging in collaboration, producing code and so much more, we often forget to take a moment to recognize and adequately showcase the important work being done across the ASF ecosystem. This blog series aims to do just that: shine a spotlight on the projects that help make the ASF community vibrant, diverse and long lasting. We want to share stories, use cases and resources among the ASF community and beyond so that the hard work of ASF communities and their contributors is not overlooked.&nbsp;</p>



<h4 class="wp-block-heading has-text-align-center">If you are part of an ASF project and would like to be showcased, please reach out to <a href="mailto:markpub@aparche.org">markpub@apache.org</a>. </h4>



<h2 class="wp-block-heading">Connect with The ASF</h2>



<ul class="wp-block-list">
<li>Follow The ASF on social: <a href="https://twitter.com/TheASF">X</a>, <a href="https://www.linkedin.com/company/the-apache-software-foundation/">LinkedIn</a>, <a href="https://bsky.app/profile/apache.org">Bluesky</a>, <a href="https://fosstodon.org/@TheASF">Fosstodon</a>, and <a href="https://www.youtube.com/c/TheApacheFoundation">YouTube</a></li>



<li><a href="https://www.apache.org/projects/">Host a Project&nbsp;</a></li>



<li><a href="https://www.apache.org/foundation/sponsorship.html">Become an ASF Sponsor </a>&nbsp;</li>



<li><a href="https://apache.org/community-resources/">Community Resources</a></li>



<li><a href="https://communityovercode.org/">Attend Community Over Code</a></li>
</ul>
<p>The post <a href="https://news.apache.org/foundation/entry/asf-project-spotlight-apache-iceberg">ASF Project Spotlight: Apache Iceberg   </a> appeared first on <a href="https://news.apache.org">The ASF Blog</a>.</p>
