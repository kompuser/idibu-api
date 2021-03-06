<p>A request is submitted using an HTTP POST to the address:</p>
<pre>
<code>
http://ws.idibu.com/clients/api/REMOTE/V3/[INSERT LOGIN HASH HERE]
</code></pre>
<p class="p1">The client hash is part of the the url used to post the information to, and it is mandatory that the request uses the POST HTTP method. GET request will fail with an error indicating that the payload is missing.&nbsp;</p>
<p class="p1">The xml data should be sent in a variable called:<br><br>
<b>&ldquo;xml_text&rdquo;</b><br><br>and must URL encoded. Make sure your application, that will link up with idibu, does not post the jobs as text/xml with ascii encoding, but is of application/x-www-form-urlencoded type. Please use the iso-8859-1 coding when URL encoding. Alternatively - and preferably! - you can use the utf-8 encoding provided you indicate the encoding in the configuration tag (utf8_enable sub-tag.)). As the encoding rules might differ between multiple job sites, please use HTML entities (like &quot;&amp;pound;&quot;, &quot;&amp;euro;&quot;) instead of standard ISO-8859-1 symbols (&quot;&pound;&quot;, &quot;&euro;&quot;) - this shouldn't be an issue when using UTF-8 though. You can see an XML with special characters and their correct encoding in the <a href="https://github.com/oneworldmarket/idibu-api/tree/master/api-v3/examples">XML3 examples section</a>.</p>
<h1 class="p1">
	Response Messages</h1>
<p class="p1">All messages are sent back as XML, and for succesful postings the system outputs significant logging information.</p>
<h2>
	Response Messages for Successful Postings</h2>
<p>For success posts you will receive a message similar to this one:</p>
<pre>
<code>
&lt;?xml version=&quot;1.0&quot; encoding=&quot;UTF-8&quot;?&gt;
&lt;IDIBU&gt;
&lt;JOB  ID=&quot;31002849&quot; STATUS=&quot;ADDED&quot;&gt;
&lt;POSTS&gt;
&lt;POST STATUS=&#39;PENDING&#39; BOARDID=&#39;54&#39; QUEUEID=&#39;846&#39; TYPE=&#39;ADD&#39; PUBLISH=&#39;2009-06-12 18:00&#39; DURATION=&#39;7&#39; /&gt;
&lt;/POSTS&gt;
&lt;/JOB&gt;
&lt;/IDIBU&gt;
</code></pre>
<p class="p1">Depending on the data posted, the system may inform you of warnings/problems with the posting, even if it was successful:</p>
<pre>
<code>
&lt;?xml version=&quot;1.0&quot; encoding=&quot;UTF-8&quot;?&gt;
&lt;IDIBU&gt;
&lt;JOB  ID=&quot;31002847&quot; STATUS=&quot;ADDED&quot;&gt;
&lt;POSTS&gt;
&lt;POST STATUS=&#39;PENDING&#39; BOARDID=&#39;54&#39; QUEUEID=&#39;844&#39; TYPE=&#39;ADD&#39; PUBLISH=&#39;2009-06-12 18:00&#39; DURATION=&#39;7&#39; /&gt;
&lt;/POSTS&gt;
&lt;/JOB&gt;
&lt;LOG&gt;
&lt;WARNING FIELD=&#39;SCR_City&#39; TYPE=&#39;text&#39; BOARDID=&#39;54&#39;&gt;Field &#39;SCR_City&#39; is missing, an empty string, or contains only whitespace.&lt;/WARNING&gt;
&lt;/LOG&gt;
&lt;/IDIBU&gt;
</code></pre>
<h2>
	Response Messages for Failed Postings</h2>
<p>In event of a posting issue you will be informed of the error. You can find below several different examples:</p>
<h3>
	Core Field missing</h3>
<pre>
<code>
&lt;?xml version=&quot;1.0&quot; encoding=&quot;UTF-8&quot;?&gt;
&lt;IDIBU&gt;
&lt;JOB  STATUS=&quot;FAILED&quot;&gt;
&lt;ERROR FIELD=&quot;TITLE&quot;&gt;Value was missing&lt;/ERROR&gt;
__posts__&lt;/JOB&gt;
&lt;/IDIBU&gt;
</code></pre>
<h3>
	Incorrect Sender</h3>
<pre>
<code>
&lt;?xml version=&quot;1.0&quot; encoding=&quot;UTF-8&quot;?&gt;&lt;IDIBU&gt;
&lt;JOB  STATUS=&quot;FAILED&quot;&gt;
&lt;ERROR FIELD=&quot;SENDPROFILE&quot;&gt;6028 is not a valid sender for you.&lt;/ERROR&gt;
&lt;/JOB&gt;
&lt;/IDIBU&gt;
</code></pre>
<h3>
	Required Extra Field missing or with incorrect value</h3>
<pre>
<code>
&lt;?xml version=&quot;1.0&quot; encoding=&quot;UTF-8&quot;?&gt;
&lt;IDIBU&gt;
&lt;JOB STATUS=&quot;FAILED&quot;&gt;&lt;ERROR&gt;There were errors in the data supplied. Please check the message log.&lt;/ERROR&gt;&lt;/JOB&gt;
&lt;LOG&gt;
&lt;ERROR FIELD=&#39;SCRlocation&#39; TYPE=&#39;doublemultiselect&#39; BOARDID=&#39;54&#39;&gt;Field &#39;SCRlocation&#39; was not supplied, or was supplied with an option that is an empty string or contains only whitespace.&lt;/ERROR&gt;
&lt;/LOG&gt;
&lt;/IDIBU&gt;
</code></pre>
<h3>
	Response messages for delayed postings</h3>
<p>Most often the system will respond with a Post Completion Page URL - this can be passed back via the XML, or sent to the person responsible for completing the advert via email. The url links to the page where idibu asks for the remaining data to complete the posting.</p>
<pre>
<code>
&lt;?xml version=&quot;1.0&quot; encoding=&quot;UTF-8&quot; ?&gt;
&lt;idibu&gt;
&nbsp;&lt;job status=&quot;delayed&quot; id=&quot;31003254&quot; postingid=&quot;235&quot;&gt;
&nbsp;&nbsp;&lt;postcompletionurl&gt;http://adpost.idibu.com/cBzdX.html&lt;/postcompletionurl&gt;
&nbsp;&lt;/job&gt;
&nbsp;&lt;log&gt;
&nbsp;&lt;errors&gt;
&nbsp;&nbsp;&lt;error field=&#39;sco_sector&#39; type=&#39;select&#39; boardid=&#39;214&#39;&gt;Field &#39;sco_sector&#39; is missing, an empty string, or contains only whitespace.&lt;/error&gt;
&nbsp;&lt;/errors&gt;
&nbsp;&lt;warnings&gt;
&nbsp;&nbsp;&lt;warning&gt;The provided team id doesn&#39;t exists or is wrong(ID provided: 33000169)&lt;/warning&gt;
&nbsp;&nbsp;&lt;warning field=&#39;sco_location&#39; type=&#39;doubleSelect&#39; boardid=&#39;214&#39;&gt;Field &#39;sco_location&#39; was missing from the payload, but was mapped internally by the system&lt;/warning&gt;
&nbsp;&nbsp;&lt;warning field=&#39;sco_featured&#39; type=&#39;select&#39; boardid=&#39;214&#39;&gt;Field &#39;sco_featured&#39; is missing, an empty string, or contains only whitespace.&lt;/warning&gt;
&nbsp;&nbsp;&lt;warning field=&#39;idibudts_cat&#39; type=&#39;select&#39; boardid=&#39;517&#39;&gt;Field &#39;idibudts_cat&#39; was missing from the payload, but was mapped internally by the system&lt;/warning&gt;
&nbsp;&lt;/warnings&gt;
&nbsp;&lt;/log&gt;
&lt;/idibu&gt;
&lt;/code&gt;</pre>
<p>We offer a great deal of configuration in this area:</p>
<ul>
	<li>
		We can setup a private domain with full rebranding for your users to post jobs out from</li>
	<li>
		The system can be configured to use any SMTP email account to provide seamless branding.</li>
	<li>
		The system can send out a branded confirmation email to let users know the post was successful.</li>
</ul>
Click <a href="https://github.com/oneworldmarket/idibu-api/blob/master/api-v3/quick-rep-job.md">here</a> to learn about quick reposting.
