---
ID: 121
post_title: >
  How to use docker-php-ext-install
  command on the docker_container of
  ansible modules.
author: killinsun
post_excerpt: ""
layout: post
permalink: https://blog.killinsun.com/?p=121
published: true
post_date: 2018-09-05 21:39:51
---
I faced any problems that is couldn't use <strong>command</strong> option of a <strong>docker_container</strong> module.

<!--more-->I need customize php-fpm container, so I used <em><strong>docker-php-ext-install</strong></em> command in docker_container's command option.
But I had any struggle points.

My struggles..
<ol>
 	<li>I didn't recognize command option's reference detail.
<ol>
 	<li>Is parameter type string? list?  fmm??</li>
</ol>
</li>
 	<li>Importance of ENTRYPOINT(docker-php-entrypoint?? what's this?)</li>
 	<li>In using <strong>command option, </strong>Docker container shutting down immediately.
<ul>
 	<li>PHP-fpm container is uses exposed port. Why shutting down????</li>
</ul>
</li>
</ol>
<h2>How to</h2>
Let me get straight the point, this playbook is a my solution.
<pre class="toolbar:2 lang:yaml decode:true crayon-selected"> - name: Deploy PHP7 on docker container
    docker_container:
      name: php7-fpm
      auto_remove: yes
      command: [
        "bash",
        "-c","
        'apt update &amp;&amp; apt install zlib1g-dev &amp;&amp; docker-php-ext-install zip &amp;&amp; docker-php-ext-install pdo_mysql &amp;&amp; php-fpm'"
      ]
      image: php:7-fpm
      exposed_ports:
        - 9000
      state: started
</pre>
&nbsp;
<h2>Answer of my struggles</h2>
<h3> 1.</h3>
A command option of docker_container can receive one of shell command.

["ping","127.0.0.1","-c","5"]

"ping 127.0.0.1 -c 5"

Both styles can use too.
<h3>2.</h3>
"docker-php-entrypoint" distinguish argument's '-'
If entrypoint option and command option that both have a some parameter, Executing that the command option is option of entrypoint's.
<h3>3.</h3>
PHP7-fpm docker container runs "php-fpm" shell command on the command that is equivalent of dockerfile's CMD directive.
However, my wrong playbook didn't have php-fpm command at last.
Therefore all commands successfully done, and container shutting down.
<pre class="lang:default decode:true ">      command: [
        "bash","-c","'apt update &amp;&amp; apt install zlib1g-dev &amp;&amp; docker-php-ext-install zip &amp;&amp; docker-php-ext-install pdo_mysql"
      ]</pre>
&nbsp;

So, bash commands option helped my solution.

<em><strong>"/bin/bash -c '{any commands of combined from &amp;&amp;}'" </strong></em>

<hr />

&nbsp;

Please your advices, comments.

thanks.