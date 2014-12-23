### functor
A functor is a container of type a that, when subjected to a function that maps from `a->b`, yields a container of type b.

Unlike the abstracted-function-pointer use in C++, here the functor is not the function; rather, it's something that behaves 
consistently when subjected to a function.

According to this Haskell wiki: en.wikibooks.org/wiki/Haskell/Category_theory , it's like this: 
> A category is a collection of objects and morphisms (functions), where the morphisms are from objects in a category to other objects in that category. A functor is a function which maps objects and morphisms from one category to objects and morphisms in another

A functor is a function from structures to structures; that is, a functor accepts one or more arguments, which are usually 
structures of a given signature, and produces a struacture (of the same signature) as its result. see also [Functor(函子), 
Applicative, and Monad(单子, or 一价原子)](http://adit.io/posts/2013-04-17-functors,_applicatives,_and_monads_in_pictures.html) 
and [category thoery(范畴论), morphism(态射), objects(物件)](http://zh.wikipedia.org/wiki/%E8%8C%83%E7%95%B4%E8%AE%BA)

### Monad(单子)
an abstract datay type in functional programming. It use to present computation not the data. It contains 2 opration **bind** (```>=``` in Haskell) and **return**, and a type constructor **M**. See also [Monads for functional programming](http://homepages.inf.ed.ac.uk/wadler/papers/marktoberdorf/baastad.pdf)

