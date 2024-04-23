---
aliases:
  - ADT Tutorial
tags:
  - tutorial
  - fundamentals
---
Algebraic data types are one of the powerful constructs we've borrowed from other functional languages, like Haskell. Using these container types we can model complex domains logically.

There are built-in data types listed in [[Core - Data Types|Data Types]], this document will aim to provide a specific tutorial on concrete usage of these ADTs.
## Example

Let's say that we are running a bakery and we have pastries that we want to be able to sell. These could be modeled as a [[Core - Types#Records|Record]] with possibly invalid values:

```
alias Pastry = {
  // what we call it
  name :: String,
  // what we charge for it
  price :: Float,
  // how many we have
  stock :: Integer,
  // how many are sold at a time
  unit :: Integer,
} 
```

The above structure would allow us to model part of our domain, but we've currently coupled our inventory such that our possible offerings are tied to how many of them we have at a given time. This means that if we have a new product or we want to update our current offerings, we have to think about representing this information with concrete quantities. (What happens if I have one cake in stock and I try to sell two? Do I have a negative value?) This _may_ be what we want, but likely it is better to decouple these values so that we can elide over parts of the domain, like so:

```
// pastries have a name (String) and price (Float)
type BakedGood = Pastry(String, Float)

// products have a pastry, stock (Integer) and how many are sold at a time (Integer)
// Invalid products have a reason (String)
type Product = Product(BakedGood, Integer, Integer) | InvalidProduct(String)
```

Now we can model the kind of baked good independently from its concrete value, as well as a few other useful features.

Consider the following:

```
findProductInStock :: List Product -> String -> Integer -> Product
findProductInStock = (inventory, name, total) => pipe(
  List.find(
    where {
      Product(Pastry(givenName, _), stock, units) =>
        (units * total <= stock && name == givenName)

      InvalidProduct(_) =>
        false
    },
  ),
  fromMaybe(InvalidProduct("Not in stock")),
)(inventory)

```

We can trivially iterate over a list of products and search for one by name, and we can ensure that the requested number of a given product wouldn't result in trying to sell more than we have in stock.