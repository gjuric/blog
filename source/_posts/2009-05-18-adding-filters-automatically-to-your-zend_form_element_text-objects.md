---
title: Adding filters automatically to your Zend_Form_Element_Text objects
author: Goran Jurić
layout: post
categories:
  - Zend Framework
highlight: true
---
Although you can create a custom form element as described in [ZF manual][1] and set the properties for all instances of this element I really did not like this solution since my application already has a lot of already working forms.

I wanted to add the Zend\_Filter\_StringTrim filter to all of my text form elements so I decided to just add them on the fly.

Since my forms already extend a custom &#8220;MyApp\_Form&#8221; object (MyApp\_Form extend Zend_Form) it was just a matter of intercepting the addElement() method.

So, here it goes, in all its glory:

~~~php
public function addElement($element, $name = null, $options = null)
{
    parent::addElement($element, $name, $options);

    if (is_null($name)) {
        $name = $element->getName();
    }

    $addedElement = $this->getElement($name);

    if ($addedElement instanceof Zend_Form_Element_Text) {
        if ( ! $addedElement->getFilter('StringTrim') instanceof Zend_Filter_Interface ) {
            $addedElement->addFilter(new Zend_Filter_StringTrim());
        }
    }
}
~~~

Since the element is already added to the form using the parent method I didn&#8217;t have to check if the element is passed as a string, but does not have a name, or if it&#8217;s passed as a Zend\_Form\_Element object without the name property since the Zend_Form::addElement() already checks for this.

 [1]: http://framework.zend.com/manual/en/zend.form.elements.html#zend.form.elements.custom