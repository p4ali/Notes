### Category
According to this [Haskell wiki](https://en.wikibooks.org/wiki/Haskell/Category_theory), it's like this: 
> A category is a collection of objects and morphisms (functions, or arrow), where the morphisms are from objects in a category to other objects in that category. A functor is a function which maps objects and morphisms from one category to objects and morphisms in another

Representation:
* morphisms: If `f` is a morphism with source object C and target object B, we write `f: C->B`
* composition of these morphisms: If `g: A->B` and `f: B->C` are two morphisms, they can be composed, resulting in a morphism `f ○ g: A->C`.

According the [text book from opencourse textbook](https://ocw.mit.edu/courses/mathematics/18-s097-applied-category-theory-january-iap-2019/lecture-videos-and-readings/18-s097iap19textbook.pdf):
> A category C consists of four pieces of data—objects, homomorphisms(e.g., (f:c->d), where c,d in same cagtegory), identities, and a
composition rule—satisfying two properties: unitality and associativity.


### functor
A functor is a mapping between categories. It sends objects to objects and morphisms to morphisms, all while preserving identities and composition. So given categories C and D, a functor `F: C->D` (C also called `domain` and D also called `codomain`):
* Maps any object A in C to `F(A)`, in D.
* Maps morphisms `f:A->B in C to `F(f):F(A)->F(B)` in D.

A functor is a function from structures to structures; that is, a functor accepts one or more arguments, which are usually 
structures of a given signature, and produces a struacture (of the same signature) as its result. see also [Functor(函子), 
Applicative, and Monad(单子, or 一价原子)](http://adit.io/posts/2013-04-17-functors,_applicatives,_and_monads_in_pictures.html) 
and [category thoery(范畴论), morphism(态射), objects(物件)](http://zh.wikipedia.org/wiki/%E8%8C%83%E7%95%B4%E8%AE%BA)

### Monoid
Getting warmer. What is a monoid?
Monoid can be defined as a:

* single set S 
* with an associative binary operation ○
* and an identity element e
* following two laws (where ○ means `compose`):
    * (a ○ b) ○ c = a ○ (b ○ c)
    * a ○ e = e ○ a = a
 
### Monad(单子)
Functor that maps a category back to that same category is called an [**endofunctor**](https://www.quora.com/What-is-an-endofunctor). A monad is just a monoid in the category of endofunctors.

an abstract datay type in functional programming. It use to present computation not the data. It contains 2 opration **bind** (```>=``` in Haskell) and **return**, and a type constructor **M**. See also [Monads for functional programming](http://homepages.inf.ed.ac.uk/wadler/papers/marktoberdorf/baastad.pdf)

Monad in scala:
* [Monad is Elephant](http://james-iry.blogspot.com/2007/09/monads-are-elephants-part-1.html)
* [Introduction to Monads in Scala](http://www.slideshare.net/stasimus/introduction-to-monads-in-scala-1)

[How to make List monad](https://rosettacode.org/wiki/Monads/List_monad)

### [abstract algebra](http://w3.math.sinica.edu.tw/math_media/d362/36204.pdf) and [Group](http://math.ntnu.edu.tw/~li/algebra-html/node4.html)
abstract algebra 主要研究對象是代數結構,比如群(Groups)、環(Rings)、體(Fields)、模(Modules)、 向量空間(Vector Spaces)和代數(Algebras.

集合S上的二元運算*的種種性質:
* 運算∗是可結合的(associative);若
  (a∗b)∗c = a∗(b∗c), ∀a, b, c ∈ S
* 運算∗是可交換的(commutative);若
  a∗b = b∗a, ∀a, b ∈ S
* 元素e ∈ S稱之為運算∗的一個單位元素(identity element);若
  a∗e = e*a = a, ∀a ∈ S
* 若集合S擁有運算∗的一個單位元素e;則我們說元素u ∈ S在集合S中具有反元素(inverse),如果存在v ∈ S使得
  u*v = v*u = e
* 若 a, b ∈ S 則 a*b ∈ S. 這個性質就是所謂的封閉性 closed.

加法運算(+)在複數集C上具有封閉性(封)、結合性(結)並擁有單位元素0+0i(單)且每一元素a+bi都有反元素(−a) + (−b)i(反),這就是所謂的群(group)的代數結構。一般而言,集合S上的一個二元運算∗;若滿足上述的封、結、 單、 反四個性質,我們就說S在運算∗之下形成一個群或說(S,∗)是一個群。 如果運算∗是可交換的;那麼理所當然,我們就說(S,∗)是一個交換群(commutative group)。通常又稱為阿貝爾群(abelian group) ,為的是要紀念數學家阿貝爾8。複數集(C,+)在加法運算之下當然是一個阿貝爾群

### Reference
* [Mondad in picture - chinese](http://jiyinyiyong.github.io/monads-in-pictures/)
* [Monad in picture - english](http://adit.io/posts/2013-04-17-functors,_applicatives,_and_monads_in_pictures.html)
* [Monda for functional programming](http://homepages.inf.ed.ac.uk/wadler/papers/marktoberdorf/baastad.pdf)
* [What's a functor in js](https://medium.com/@dtinth/what-is-a-functor-dcf510b098b6)
* [Applied theory open course in MIT](https://ocw.mit.edu/courses/mathematics/18-s097-applied-category-theory-january-iap-2019/)
