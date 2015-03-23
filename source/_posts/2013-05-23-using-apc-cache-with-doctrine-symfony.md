---
title: 'Using APC cache with Doctrine & Symfony? Check again!'
author: Goran Jurić
layout: post
highlight: true
categories:
  - Uncategorized
---
If you have read the documentation you probably know that having a Metadata cache for Doctrine is something you really want to have in production.

The setup in Symfony is pretty easy, you just have to edit the `config_prod.yml`:

~~~yaml
orm:
    metadata_cache_driver: apc
    query_cache_driver: apc
~~~

The problem is that the Dependency Injection Container is generated through the Command Line Interface, and if you do not have `apc.enable_cli=1` in your `php.ini` DIC will use `FileCacheReader` instead.

You can check your `/app/cache/prod/appProdProjectContainer.php` to see if you are using APC cache.

~~~php
    protected function getDoctrine_Orm_DefaultEntityManagerService()
    {
        $a = $this->get('annotation_reader');
        $b = new \Doctrine\Common\Cache\ApcCache();
        $b->setNamespace('sf2orm_default_5cdc3404d84577b226d7772ca9818908');
        $c = new \Doctrine\Common\Cache\ApcCache();
        $c->setNamespace('sf2orm_default_5cdc3404d84577b226d7772ca9818908');
// ...
        $g = new \Doctrine\ORM\Configuration();
        $g->setMetadataCacheImpl($b);
        $g->setQueryCacheImpl($c);
// ...
    }
~~~

If you can not find `\Doctrine\Common\Cache\ApcCache` you are not using APC to cache your metadata.

For a simple page with one query the difference between APC and File based caching can be quite big. Take a look at the numbers I got from XHProf for a simple page:

<table>
  <tr>
    <th>
      Cache type
    </th>
    
    <th>
      Wall time
    </th>
    
    <th>
      CPU time
    </th>
    
    <th>
      Memory usage
    </th>
  </tr>
  
  <tr>
    <td>
      APC
    </td>
    
    <td>
      254,972 µs
    </td>
    
    <td>
      242,020 µs
    </td>
    
    <td>
      10,352,536 bytes
    </td>
  </tr>
  
  <tr>
    <td>
      File
    </td>
    
    <td>
      355,617 µs
    </td>
    
    <td>
      325,320 µs
    </td>
    
    <td>
      11,579,568 bytes
    </td>
  </tr>
  
  <tr>
    <td>
      <strong>Difference</strong>
    </td>
    
    <td>
      39.47 %
    </td>
    
    <td>
      34.42 %
    </td>
    
    <td>
      11.85 %
    </td>
  </tr>
</table>