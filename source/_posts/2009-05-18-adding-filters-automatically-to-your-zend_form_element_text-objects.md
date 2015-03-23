---
title: Adding filters automatically to your Zend_Form_Element_Text objects
author: Goran Jurić
layout: post
categories:
  - Zend Framework
---
Although you can create a custom form element as described in [ZF manual][1] and set the properties for all instances of this element I really did not like this solution since my application already has a lot of already working forms.

I wanted to add the Zend\_Filter\_StringTrim filter to all of my text form elements so I decided to just add them on the fly.

Since my forms already extend a custom &#8220;MyApp\_Form&#8221; object (MyApp\_Form extend Zend_Form) it was just a matter of intercepting the addElement() method.

So, here it goes, in all its glory:

<pre class="php"><span class="kw2">public</span> <span class="kw2">function</span> addElement<span class="br0">&#40;</span><span class="re0">$element</span><span class="sy0">,</span> <span class="re0">$name</span> <span class="sy0">=</span> <span class="kw4">null</span><span class="sy0">,</span> <span class="re0">$options</span> <span class="sy0">=</span> <span class="kw4">null</span><span class="br0">&#41;</span>
<span class="br0">&#123;</span>
    parent<span class="sy0">::</span><span class="me2">addElement</span><span class="br0">&#40;</span><span class="re0">$element</span><span class="sy0">,</span> <span class="re0">$name</span><span class="sy0">,</span> <span class="re0">$options</span><span class="br0">&#41;</span><span class="sy0">;</span>

    <span class="kw1">if</span> <span class="br0">&#40;</span><span class="kw3">is_null</span><span class="br0">&#40;</span><span class="re0">$name</span><span class="br0">&#41;</span><span class="br0">&#41;</span> <span class="br0">&#123;</span>
        <span class="re0">$name</span> <span class="sy0">=</span> <span class="re0">$element</span><span class="sy0">-&gt;</span><span class="me1">getName</span><span class="br0">&#40;</span><span class="br0">&#41;</span><span class="sy0">;</span>
    <span class="br0">&#125;</span>

    <span class="re0">$addedElement</span> <span class="sy0">=</span> <span class="re0">$this</span><span class="sy0">-&gt;</span><span class="me1">getElement</span><span class="br0">&#40;</span><span class="re0">$name</span><span class="br0">&#41;</span><span class="sy0">;</span>

    <span class="kw1">if</span> <span class="br0">&#40;</span><span class="re0">$addedElement</span> instanceof Zend_Form_Element_Text<span class="br0">&#41;</span> <span class="br0">&#123;</span>
        <span class="kw1">if</span> <span class="br0">&#40;</span> <span class="sy0">!</span> <span class="re0">$addedElement</span><span class="sy0">-&gt;</span><span class="me1">getFilter</span><span class="br0">&#40;</span><span class="st_h">'StringTrim'</span><span class="br0">&#41;</span> instanceof Zend_Filter_Interface <span class="br0">&#41;</span> <span class="br0">&#123;</span>
            <span class="re0">$addedElement</span><span class="sy0">-&gt;</span><span class="me1">addFilter</span><span class="br0">&#40;</span><span class="kw2">new</span> Zend_Filter_StringTrim<span class="br0">&#40;</span><span class="br0">&#41;</span><span class="br0">&#41;</span><span class="sy0">;</span>
        <span class="br0">&#125;</span>
    <span class="br0">&#125;</span>
<span class="br0">&#125;</span></pre>

Since the element is already added to the form using the parent method I didn&#8217;t have to check if the element is passed as a string, but does not have a name, or if it&#8217;s passed as a Zend\_Form\_Element object without the name property since the Zend_Form::addElement() already checks for this.

 [1]: http://framework.zend.com/manual/en/zend.form.elements.html#zend.form.elements.custom