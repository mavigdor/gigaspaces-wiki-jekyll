---
layout: post101
title:  Collection Index
categories: XAP101NET
parent: indexing-overview.html
weight: 300
---

An index can be defined on a Collection property (such as List). Setting such an index means that each of the Collection's items is indexed.

Setting an index on a Collection is done using the SpaceIndex.Path attribute where a Collection property should be followed by "[*]".

The following example shows how to define an index on a List of Integers:

{% highlight csharp %}
[SpaceClass]
public class CollectionIndexingExample
{
  private Integer id;
  private List<int> numbers;

  // Getter and setter methods...

  // This means that each Integer in the List is indexed.
  [SpaceIndex(Path="[*]")]
  public List<int> Numbers { get; set; }

}
{% endhighlight %}

The following query shows how to take advantage of the defined index:

{% highlight csharp %}
SqlQuery<CollectionIndexingExample> sqlQuery =
    new SqlQuery<CollectionIndexingExample>("Numbers[*] = 30");
CollectionIndexingExample[] result = spaceProxy.ReadMultiple(sqlQuery);
{% endhighlight %}

### Nested property within a Collection

Its also possible to index a nested property within a collection.

The following example shows how to define an index on a Book.id property, which resides in a Collection property in Author:

{% highlight csharp %}
[SpaceClass]
public class Author
{
  // Properties...

  // This means that each Book.Id in the Collection is indexed.
  [SpaceIndex(Path = "[*].Id")]
  [SpaceProperty(StorageType = StorageType.Document)]
  public List<Book> Books{ get; set; }
}

public class Book
{
  // Properties...

  public int? Id{ get; set; }
}
{% endhighlight %}

The following query shows how to take advantage of the defined index:

{% highlight csharp %}
SqlQuery<Author> sqlQuery = new SqlQuery<Author>("Books[*].Id = 57");
Author result = spaceProxy.Read(sqlQuery);
{% endhighlight %}

Setting an index on a Collection within a nested property is also accepted:

{% highlight csharp %}
[SpaceClass]
public class Employee
{
  // Properties...

  [SpaceIndex(Path = "PhoneNumbers[*]")]
  [SpaceProperty(StorageType = StorageType.Document)]
  public Information Information{ get; set; }
}

public class Information
{
  // Properties...
  public List<String> PhoneNumbers{ get; set; }
}
{% endhighlight %}

{% info %}
Both `[SpaceIndex(Type=SpaceIndexType.Basic)]` and `[SpaceIndex(Type=SpaceIndexType.Extended)]` are supported.
{% endinfo %}



<ul class="pager">
  <li class="previous"><a href="./indexing-nested-properties.html">&larr; Nested Property Index</a></li>
  <li class="next"><a href="./indexing-compound.html">Compound Index &rarr;</a></li>
</ul>
