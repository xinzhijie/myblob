<div class="hljs-center">
<h2><a id="vue_2"></a>vue热加载失效</h2>
</div>
<p>段落引用在Linux系统中打开vue 项目，手动修改并保存文件时，控制台自动刷新，但是浏览器并不刷新且没有修改后的效果。</p>
<p>原因是因为Linux系统单个进程可分配的最大文件数过低<br />
执行以下命令：</p>
<pre><code class="lang-">echo fs.inotify.max_user_watches=524288 | 
sudo tee -a /etc/sysctl.conf &amp;&amp; sudo sysctl -p

</code></pre>
