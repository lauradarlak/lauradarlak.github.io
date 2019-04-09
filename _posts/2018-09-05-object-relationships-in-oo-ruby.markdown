---
title: "Object Relationships in OO Ruby"
categories: [ruby]
---

## Modeling Object Relationships through Mycology

The Fullstack Web Development curriculum at the Flatiron School introduces Ruby through a procedural paradigm. Procedural programming approaches programming as a series of steps, or a list of instructions for the computer to process, from beginning to end. However, Ruby is intrinsically an object-oriented programming (OOP) language.  Object-oriented programming employs abstraction by encapsulating data and behavior or instructions into objects. Abstraction removes the details from programming in order to reveal the impeccable essence of our code. Object relationships in ruby is an illustrative topic for understanding Ruby as object-orient programming.

A goal of OOP is to represent real world situations through our code. I am a mycophile and will demonstrate object relationships of “belongs to” and “has many” by modeling fungi in the natural world.

In Ruby, classes serve as the blueprint for creating objects and are also capable of creating individual objects, or instances of a class. I like to think of a class as spiritual devas. Devas are often referred to in nature as the group soul of each living entity. For example, although there are countless individual fungi that exist at any one moment on this planet, every unique, fungi is connected to and is a reflection of the Fungi Spirit Deva.

### Belongs To Relationship

Object relationships involve connecting objects through attributes. An object can belong to another object. This “belongs to” relationship is reflected below by the class Fungi and class Phylum. Every mushroom belongs to one particular taxonomic division, known as a phylum.

~~~ ruby
class Fungi
  attr_accessor :common_name, :phylum

  def initialize(common_name)
    @common_name = common_name
  end

end

class Phylum
  attr_accessor :name

  def initialize(name)
    @name = name
  end

end

chicken_woods = Fungi.new("Chicken of the Woods")
basidiomycota_phylum = Phylum.new("Basidiomycota")


chicken_wood.phylum = basidiomycota_phylum # a Fungi belongs to a Phylum
~~~

The variable chicken_woods stores an instance of the Fungi class and belongs to the Basidiomycota phylum of fungi. The above example connects the Fungi class object to the Phylum class object through the phylum attribute.

### Has Many Relationship

A phylum has many fungi, for example the Basidiomycota phylum contains over 31,000 species of fungi. The "has many" relationship is modeled below.

~~~ruby
class Fungi
  attr_accessor :common_name, :phylum

  def initialize(common_name)
    @common_name = common_name
  end

end

class Phylum
  attr_accessor :name
  attr_reader :fungus

  def initialize(name)
    @name = name
    @fungus = [] # writer method, create plural attribute and set to an empty array
  end

  def add_fungi(fungi)
    @fungus << fungi # add the fungi objects to the fungus array

  end

end

chicken_woods = Fungi.new("Chicken of the Woods")
basidiomycota_phylum = Phylum.new("Basidiomycota")

basidiomycota_phylum.add_fungi(chicken_woods)
~~~

By relating objects, our program becomes more dynamic and is able to be modified in ways that would otherwise be complicated by procedural programming.
