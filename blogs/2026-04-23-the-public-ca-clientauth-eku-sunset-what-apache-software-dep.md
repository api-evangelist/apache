---
title: "The Public CA clientAuth EKU Sunset: What Apache Software Deployers Need to Know"
url: "https://news.apache.org/foundation/entry/the-public-ca-clientauth-eku-sunset-what-apache-software-deployers-need-to-know"
date: "Thu, 23 Apr 2026 19:48:25 +0000"
author: "The ASF"
feed_url: "https://news.apache.org/feed"
---
<p class="wp-block-paragraph"><em>By: <a href="https://jinwoohwang.com"><em>Jinwoo Hwang</em></a>, Lead Developer, Project Lead, and Release Manager, Apache Geode 2.0</em></p>



<h2 class="wp-block-heading"><strong>What Apache Software Deployers Need to Know Before May 2026</strong></h2>



<p class="wp-block-paragraph">Effective May 2026, major public Certificate Authorities (CAs) will stop including the Client Authentication Extended Key Usage (EKU) in public TLS certificates.</p>



<p class="wp-block-paragraph">This is not a routine policy update; it is a structural change to how trust is defined on the internet. And for the software ecosystem, it introduces a clear and time-bound risk for deployments relying on mutual TLS. Mutual TLS (or <em>mTLS</em>) is an alternative that verifies identities of both the client and server, rather than just the server as with standard TLS.</p>



<p class="wp-block-paragraph">Deployers of any Apache project whose deployment relies on mutual TLS (mTLS) with public-CA-issued client certificates — including Apache Kafka, Apache Cassandra, Apache ZooKeeper, Apache Pulsar, Apache Ignite, and Apache Geode — face the same outcome: certificates renewed after this date will no longer include the clientAuth EKU required for client authentication.</p>



<p class="wp-block-paragraph">Nothing in your code needs to change for this to break. The failure will arrive with a routine certificate renewal, and when it does, it will look like a sudden loss of connectivity across otherwise stable systems. Deployers who rely on the clientAuth EKU need to switch to an internal or enterprise CA.&nbsp;</p>



<h2 class="wp-block-heading"><strong>When Industry Standards Shift Beneath Your Feet</strong></h2>



<p class="wp-block-paragraph">In the evolving landscape of internet security, certificate authorities serve as critical trust anchors. Major public CAs — including Let&#8217;s Encrypt, DigiCert, Sectigo, and GlobalSign — are removing the clientAuth EKU from publicly issued-leaf certificates.</p>



<p class="wp-block-paragraph">This is not a narrow or isolated policy decision. It reflects a coordinated realignment of what a Public Key Infrastructure (PKI) is meant to do. The CA/Browser Forum has concluded that client authentication is a private identity concern and one that should be managed by internal infrastructure, not public trust anchors designed for internet-facing services.</p>



<p class="wp-block-paragraph">For the broader software industry, and for the Apache ecosystem, this realignment carries immediate operational consequences.</p>



<p class="wp-block-paragraph">Many widely deployed open source systems (from high-throughput messaging platforms to distributed databases) support mTLS as a first-class mechanism for authenticating clients and cluster members. In practice, many of these deployments have relied on public CAs for convenience.</p>



<p class="wp-block-paragraph">Because public CAs are removing the clientAuth EKU from their certificates, a single publicly-issued certificate will no longer work for both server and client authentication. Deployers who relied on this convenience will need to source their client certificates separately — typically from an internal or enterprise CA.</p>



<h2 class="wp-block-heading"><strong>The Common Failure Mode Across Apache Deployments</strong></h2>



<p class="wp-block-paragraph">Across the software ecosystem — whether a project is built in Java, Go, Python, C, or Rust — any deployment using mTLS is affected. The TLS libraries in every major language enforce the clientAuth EKU check during client certificate validation. When public CAs stop including that EKU, the handshake will fail regardless of the underlying stack.</p>



<p class="wp-block-paragraph">When a TLS connection requires client authentication, the TLS stack verifies that the presented certificate includes the clientAuth EKU. If it does not, the handshake is rejected. This is standard behavior across all major TLS implementations — Java (JSSE), Go (crypto/tls), Rust (rustls), and OpenSSL all enforce this check. It is not a quirk of any specific runtime; it is how TLS and X.509 are designed to work, ensuring that a certificate is only accepted for its intended purpose.</p>



<p class="wp-block-paragraph">Without the clientAuth EKU, mTLS failures will manifest as:</p>



<ul class="wp-block-list">
<li>Brokers rejecting clients</li>



<li>Cluster members failing to authenticate with each other</li>



<li>Services losing connectivity despite “valid” certificates</li>
</ul>



<p class="wp-block-paragraph">Because every major TLS implementation enforces the clientAuth EKU check, this is not isolated to any single language, runtime, or project — any mTLS deployment using public CA-issued client certificates will be affected.&nbsp;</p>



<p class="wp-block-paragraph">The fix is not to weaken the TLS check, but to issue client certificates from a CA that includes the correct EKU, such as an internal or enterprise CA. The next section covers exactly when and how these failures will surface.</p>



<h2 class="wp-block-heading"><strong>What Will Break and When</strong></h2>



<p class="wp-block-paragraph">The most dangerous aspect of this change is that failures are delayed.</p>



<p class="wp-block-paragraph">Deployments will not break when the policy takes effect. They will break when the operator renews their client certificates through a public CA after May 2026. A certificate issued before the cutoff will continue working until it expires. But once it is renewed, the replacement will lack the clientAuth EKU, and the next mTLS handshake will be rejected — even though the previous certificate worked without issue.</p>



<p class="wp-block-paragraph">This creates a scenario where:</p>



<ul class="wp-block-list">
<li>Deployments appear stable for weeks or months after the policy change</li>



<li>Routine certificate renewal triggers unexpected outages</li>



<li>Failures are difficult to diagnose because the certificate looks valid in every other respect</li>
</ul>



<p class="wp-block-paragraph">For distributed systems, where certificate rotation is often automated, a single renewal cycle can trigger cascading authentication failures across an entire cluster.</p>



<h2 class="wp-block-heading"><strong>How to Know If You&#8217;re Affected</strong></h2>



<p class="wp-block-paragraph">The short answer: if you didn&#8217;t explicitly configure mutual TLS (mTLS), you are almost certainly not affected.</p>



<p class="wp-block-paragraph">Client certificate authentication is not enabled by default in any of the Apache projects discussed in this article. For example, Apache Kafka&#8217;s ssl.client.auth defaults to none, and Apache Cassandra ships with client_encryption_options.enabled: false and require_client_auth: false. Standard TLS — where only the server presents a certificate — is the default for Apache Kafka, Cassandra, ZooKeeper, Pulsar, NiFi, and others. Mutual TLS is always an opt-in configuration that someone in your organization would have deliberately enabled. If no one configured mTLS, this change does not affect you.</p>



<p class="wp-block-paragraph">You are affected if all three of the following are true:</p>



<ul class="wp-block-list">
<li>Your deployment requires clients to present certificates. Look for configuration like ssl.client.auth=required (Kafka), client_encryption_options.require_client_auth: true (Cassandra), or equivalent settings in your project&#8217;s TLS configuration.</li>
</ul>



<ul class="wp-block-list">
<li>Those client certificates were issued by a public CA (e.g., Let&#8217;s Encrypt, DigiCert, Sectigo) rather than an internal or enterprise CA.</li>
</ul>



<ul class="wp-block-list">
<li>Those certificates include the clientAuth EKU today — which they will lose upon renewal after May 2026.</li>
</ul>



<h2 class="wp-block-heading">How to check your certificates</h2>



<h4 class="wp-block-heading">Inspect any certificate with OpenSSL:</h4>



<p class="wp-block-paragraph">openssl x509 -in your-cert.pem -noout -text | grep -A1 &#8220;Extended Key Usage&#8221;</p>



<p class="wp-block-paragraph">If you see TLS Web Client Authentication, the certificate currently includes the clientAuth EKU. If it was issued by a public CA, it will not include this EKU after renewal.</p>



<h4 class="wp-block-heading">For Java keystores:</h4>



<p class="wp-block-paragraph">keytool -list -v -keystore your-keystore.jks -storepass changeit | grep -A3 &#8220;ExtendedKeyUsages&#8221;</p>



<p class="wp-block-paragraph">If you only use standard TLS (server authentication only), this change does not affect you. Your public CA certificates will continue to work exactly as they do today.</p>



<h2 class="wp-block-heading"><strong>Three Proven Paths Forward</strong></h2>



<p class="wp-block-paragraph">While the problem is broad, the solution space is well understood. Three primary approaches have emerged across Apache deployments.</p>



<h3 class="wp-block-heading"><strong>1) Internal / Enterprise CA for Full mTLS</strong></h3>



<p class="wp-block-paragraph">This approach embraces a simple principle: client identity should be managed internally.</p>



<p class="wp-block-paragraph">Organizations operate their own certificate authority and issue certificates for both servers and clients, ensuring the correct EKUs are always present.</p>



<p class="wp-block-paragraph"><strong>Why choose this:</strong></p>



<ul class="wp-block-list">
<li>Preserves strong, certificate-based authentication</li>



<li>Provides full control over certificate lifecycle and policy</li>
</ul>



<p class="wp-block-paragraph"><strong>Trade-offs:</strong></p>



<ul class="wp-block-list">
<li>Requires PKI infrastructure and operational expertise</li>



<li>Introduces additional automation and management overhead</li>
</ul>



<h3 class="wp-block-heading"><strong>2) Hybrid Model: Public CA Servers, Private CA Clients</strong></h3>



<p class="wp-block-paragraph">This model splits trust responsibilities.</p>



<ul class="wp-block-list">
<li>Server certificates remain issued by public CAs</li>



<li>Client certificates are issued by an internal CA</li>
</ul>



<p class="wp-block-paragraph">This preserves external trust for servers while ensuring client certificates include the required clientAuth EKU.</p>



<p class="wp-block-paragraph"><strong>Why choose this:</strong></p>



<ul class="wp-block-list">
<li>Maintains compatibility with systems that rely on public trust stores</li>



<li>Keeps client identity under organizational control</li>
</ul>



<p class="wp-block-paragraph"><strong>Trade-offs:</strong></p>



<ul class="wp-block-list">
<li>More complex truststore configuration</li>



<li>Requires managing two certificate hierarchies</li>
</ul>



<h3 class="wp-block-heading"><strong>3) Server-Only TLS with Application-Layer Authentication</strong></h3>



<p class="wp-block-paragraph">This approach separates transport security from identity.</p>



<p class="wp-block-paragraph">TLS is still used to encrypt traffic and authenticate servers, but client identity is handled at the application layer (e.g., passwords, tokens, OAuth).</p>



<p class="wp-block-paragraph">It is important to note: disabling client certificate authentication does not disable encryption. Data remains fully protected in transit.</p>



<p class="wp-block-paragraph"><strong>Why choose this:</strong></p>



<ul class="wp-block-list">
<li>Simplifies certificate management</li>



<li>Leverages existing identity systems</li>
</ul>



<p class="wp-block-paragraph"><strong>Trade-offs:</strong></p>



<ul class="wp-block-list">
<li>Requires robust application-layer authentication</li>



<li>Moves identity enforcement out of the TLS layer</li>
</ul>



<h2 class="wp-block-heading"><strong>What You Should Do Now</strong></h2>



<p class="wp-block-paragraph">Regardless of approach, the most important step is to begin planning.</p>



<p class="wp-block-paragraph"><strong>Inventory your environment</strong></p>



<ul class="wp-block-list">
<li>Identify where mTLS is enabled</li>



<li>Determine which certificates are issued by public CAs</li>
</ul>



<p class="wp-block-paragraph"><strong>Assess exposure</strong></p>



<ul class="wp-block-list">
<li>Are client certificates used for authentication?</li>



<li>Are they dependent on public CA issuance?</li>
</ul>



<p class="wp-block-paragraph"><strong>Test before it matters</strong></p>



<ul class="wp-block-list">
<li>Validate your chosen approach in non-production environments</li>



<li>Simulate certificate renewal scenarios</li>
</ul>



<p class="wp-block-paragraph"><strong>Plan for phased migration</strong></p>



<ul class="wp-block-list">
<li>Avoid last-minute changes</li>



<li>Allow time for validation, rollback, and coordination</li>
</ul>



<h2 class="wp-block-heading"><strong>A Moment for the Ecosystem</strong></h2>



<p class="wp-block-paragraph">The removal of clientAuth EKU from public certificates is not just a technical adjustment. It is a shift in responsibility.</p>



<p class="wp-block-paragraph">Client identity is moving out of the public trust layer and back into the hands of the organizations that operate these systems. For the Apache ecosystem, that means rethinking long-standing assumptions about how mTLS is deployed and managed.</p>



<p class="wp-block-paragraph">The impact is broad because every major TLS implementation enforces the same EKU check. Any mTLS deployment using public CA-issued client certificates will experience the same failure — often at the least convenient moment: certificate renewal.&nbsp;</p>



<p class="wp-block-paragraph">But the path forward is clear. Whether through private PKI, hybrid trust models, or application-layer authentication, the solutions are well understood and already in use across the ecosystem.</p>



<p class="wp-block-paragraph">What matters now is​​ acting immediately.</p>



<p class="wp-block-paragraph">The policy takes effect in May 2026, but the real exposure window extends well beyond that — every certificate renewal over the following 12 months is a potential breakage point. If your next renewal is weeks away, this is urgent. If it&#8217;s months away, you have time to plan — but not to postpone. Deployments that begin migrating now can transition deliberately. Those that wait will likely discover this change through a production outage triggered by something as routine as a certificate renewal.</p>



<p class="wp-block-paragraph">This is an opportunity to modernize how trust and identity are managed across Apache systems.</p>



<h2 class="wp-block-heading"><strong>Where to Get Help</strong></h2>



<ul class="wp-block-list">
<li>Check your project&#8217;s documentation — Most Apache projects with mTLS support document their TLS configuration. For example, Apache Pulsar&#8217;s mTLS authentication guide (<a href="https://pulsar.apache.org/docs/next/security-tls-authentication/">https://pulsar.apache.org/docs/next/security-tls-authentication/</a>) walks through the full setup. Start there to understand your current setup and available authentication options.<br /></li>



<li>Ask on the user mailing list — If you need project-specific guidance, post to your project&#8217;s user list (e.g., users@kafka.apache.org, users@nifi.apache.org, users@cassandra.apache.org). The community can help you evaluate which migration path fits your deployment.<br /></li>



<li>Contact your CA provider — Ask your CA for their specific timeline for removing the clientAuth EKU. For example, DigiCert has announced that on March 1, 2027, it will fully remove the clientAuth EKU from its public TLS certificate issuance process — including renewals, reissues, and duplicates. Your CA may have a different deadline, so confirm when your next renewal will be affected.</li>
</ul>
<p>The post <a href="https://news.apache.org/foundation/entry/the-public-ca-clientauth-eku-sunset-what-apache-software-deployers-need-to-know">The Public CA clientAuth EKU Sunset: What Apache Software Deployers Need to Know</a> appeared first on <a href="https://news.apache.org">The ASF Blog</a>.</p>
