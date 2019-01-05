---
ID: 94
post_title: >
  Master Server cert revoked on NetBackup
  8.1
author: killinsun
post_excerpt: ""
layout: post
permalink: https://blog.killinsun.com/?p=94
published: true
post_date: 2018-07-26 09:53:01
---
I had a mistake with revoked for master server's cert on NetBackup 8.1 .

In below, my solution.
<h2>Issue</h2>
<a class="ab-item" href="http://blog.killinsun.com/wp-admin/post-new.php">投稿</a><img class="alignnone wp-image-99 size-full" src="http://blog.killinsun.com/wp-content/uploads/2018/07/01-1.png" alt="" width="1787" height="412" />
<pre class="toolbar:2 lang:sh decode:true">/usr/openv/netbackup/bin/nbcertcmd -hostSelfCheck
 Certificate is revoked.</pre>
<ul>
 	<li>And you can not logon after you logout and disconnect session from Administration Console,</li>
</ul>
<h2>Solution</h2>
<ol>
 	<li>Create Reissue token
<pre class="toolbar:2 lang:sh decode:true">/usr/openv/netbackup/bin/bpnbat -login -logintype WEB
/usr/openv/netbackup/bin/nbcertcmd -createToken -name {token_name} -reissue -host {hostname of masterserver}
#Copy it generate token.</pre>
</li>
 	<li>Reconfigure certificate
<pre class="toolbar:2 lang:sh decode:true">/usr/openv/netbackup/bin/nbcertcmd -getCertificate -server {hostname of masterserver} -force -token</pre>
</li>
 	<li>Verify
<pre class="toolbar:2 lang:sh decode:true crayon-selected"> /usr/openv/netbackup/bin/nbcertcmd -hostSelfCheck</pre>
Check message "is not revoked" is ok</li>
</ol>
&nbsp;