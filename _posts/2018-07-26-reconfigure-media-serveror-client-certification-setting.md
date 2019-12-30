---
ID: 103
post_title: >
  Reconfigure media server(or client)
  certification setting.
author: 首無しキリン
post_excerpt: ""
layout: post
permalink: >
  https://blog.killinsun.com/2018/07/reconfigure-media-serveror-client-certification-setting/
published: true
post_date: 2018-07-26 11:01:02
---
I faced problem that is "The vnetd proxy encountered an error".

My situations are these.
<ul>
 	<li>Re-deployed new master server instead of old one.</li>
 	<li>I added existing media server with "nbemmcmd " command at new master server.</li>
 	<li>Can resolve hostname, and connect ping.</li>
</ul>
<h2>Cause</h2>
Existing media server require re-config new Master server's CACert and Cert.
<h2>Solution</h2>
<ol>
 	<li>Check CA Cert file between master server and media server.
run at media server, and masterserver.
<pre class="toolbar:2 lang:sh decode:true "> /usr/openv/netbackup/bin/nbcertcmd  -listCACertDetails
       Subject Name : /CN=nbatd/OU=root@myserver.local/O=vx
        Start Date : Jul 23 06:29:14 2018 GMT
       Expiry Date : Jul 18 07:44:14 2038 GMT
  SHA1 Fingerprint : ** Check incorrect between master and media. **</pre>
&nbsp;</li>
 	<li>**If incorrect**, delete old CA Cert file on media server.
run at media server.
<pre class="toolbar:2 lang:sh decode:true">/usr/openv/netbackup/bin/nbcertcmd  -removeCACertificate -fingerPrint {old SHA1 fingerprint on media server}</pre>
&nbsp;</li>
 	<li>Re configure CACertfile
run at media server
<pre class="toolbar:2 lang:sh decode:true">/usr/openv/netbackup/bin/nbcertcmd  -getCACertificate</pre>
&nbsp;</li>
 	<li>Next, check your media server's cert file. If it's too old, you should reconfig cert file
run at media server
<pre class="toolbar:2 lang:sh decode:true ">/usr/openv/netbackup/bin/nbcertcmd -listCertDetails</pre>
&nbsp;</li>
 	<li>Delete all old cert file. (CAUTION: If you need specifc cert file, you delete individual old cert file with other command.)
run at media server
<pre class="toolbar:2 lang:sh decode:true ">/usr/openv/netbackup/bin/nbcertcmd  -deleteAllCertificates</pre>
&nbsp;</li>
 	<li>the following next command, you can reconfig new cert file.
run at media server
<pre class="toolbar:2 lang:sh decode:true">/usr/openv/netbackup/bin/nbcertcmd -getCertificate</pre>
but, an error message print, you need re-issue token.
If print above error message, generate reissue token at master server.
<pre class="toolbar:2 lang:sh decode:true crayon-selected">/usr/openv/netbackup/bin/nbcertcmd -createToken -name {your_token_name} -
reissue -host {mediaserver_hostname}</pre>
After generate reissue token, rerun and add option this command at media server.
<pre class="toolbar:2 lang:sh decode:true ">/usr/openv/netbackup/bin/nbcertcmd -getCertificate -token</pre>
Copy and paste generated token.

<span style="font-size: 1rem;">Finally, you can connect master server to media server.</span></li>
</ol>
For more details, check official support page.

<a href="https://www.veritas.com/support/en_US/article.000127129" target="_blank" rel="noopener noreferrer">How to manually obtain a host ID Certificate.</a>