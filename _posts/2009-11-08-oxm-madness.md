---
title: "Object-XML Mapping in Spring 3.0: when indirection gets out of hand"
layout: post
og_description: "When indirection gets out of hand."
og_image: "2009/oxm-madness/cookies-1024x768.jpg"
---

<a href="{{site.baseurl}}/img/2009/oxm-madness/cookies-1024x768.jpg"><img title="how many layers?" src="{{site.baseurl}}/img/2009/oxm-madness/cookies-300x225.jpg" alt="how many layers?" width="300" height="225" /></a>

<a href="http://www.ibm.com/developerworks/java/library/x-springXOM/index.html?ca=drs-">Meet the Object/XML mapping support in Spring</a> is an introduction to the <a title="Spring WS OXM documentation" href="http://static.springsource.org/spring-ws/site/reference/html/oxm.html">Marshaller and Unmarshaller interfaces</a> that have been rolled into Spring 3.0 from Spring WS.</p>

In summary:

1. Application code uses the two interfaces and delegates the object-to-XML-and-back (or OXM) work to them.
2. The application context is configured to use Spring's wrapper around an OXM framework.
3. The wrapper is injected into the bean.
4. The OXM framework also needs to be added to the classpath and configured.

To me, there are two levels of indirection too many here, already. Add another level if the application wraps this up in an XmlService interface and corresponding implementation.

Furthermore, it turns out that the <a href="http://www.joelonsoftware.com/articles/LeakyAbstractions.html">abstraction is leaky</a>! From the Spring WS docs:

> Although the marshal method accepts a plain object as its first parameter, most Marshaller implementations cannot handle arbitrary objects. Instead, an object class must be mapped in a mapping file, registered with the marshaller, or have a common base class. Refer to the further sections in this chapter to determine how your O/X technology of choice manages this.

Despite all that indirection, you still need to know which OXM is being used. If you have to know what you're using, why not work more directly with the OXM and avoid the <a title="a paraphrase of Security Theater" href="http://en.wikipedia.org/wiki/Security_theater">abstraction theater</a>?

I would go so far as to argue that, not only is the indirection not buying you anything, it's probably going to end up costing you, by making the application's wiring more difficult to understand and maintain.

I've been working with <a href="http://code.google.com/p/google-guice/">Guice</a> of late. Its focus on programmatic configuration encourages you to "touch" 3rd-party APIs directly. This feels lean and natural compared to Spring's often unnecessary infinite indirection.

Apply DAO design to XML and you'll find that a few simple methods are enough to decouple you from your OXM in the same way a DAO decouples your from your ORM. For example:

<pre>void save(Object entity) <span style="color: #008000;">// serialises to XML</span>
&lt;T&gt; T get(Class&lt;T&gt; myClass, String xml) <span style="color: #008000;">/* converts from XML to object of type T. Replace String with File or InputStream as needed */</span></pre>
<p>Even if for some bizarre reason you need two OXM frameworks at the same time, the XML DAO should be injected with both OXM classes and decide which one to use based on the class of the object passed into the save() method.</p>
<p>To be really fancy, apply the Interface Segregation Principle (<a href="http://www.objectmentor.com/resources/articles/isp.pdf">PDF</a>): have the XML DAO implement two interfaces and let clients choose the one they're interested in. These might be drawn from the problem domain (UserXmlDao, ArticleXmlDao) or the solution domain (CastorXmlDao, JaxbXmlDao) [1].</p>

In conclusion: indirection has advantages, but it also has a cost. Using it indiscriminately for trivial things leads to <a href="http://97things.oreilly.com/wiki/index.php/Simplify_essential_complexity;_diminish_accidental_complexity">accidental complexity</a> and confusion through over-engineering.

[1] See Tim Ottinger's <a href="http://www.objectmentor.com/resources/articles/naming.htm">paper on variable naming</a> for the concept of problem and solution domains
