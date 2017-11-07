# Functional Programming Concepts

## Functor

```haskell
class Functor f where
    fmap :: (a -> b) -> f a - f b
```

### Some Instances

```haskell

//List
instance Functor [] where
    fmap = map

//Maybe
instance Functor Maybe where
    fmap f (Just x) = Just (f x)
    fmap f Nothing = Nothing

//Either
instance Functor (Either a) where
    fmap f (Right x) = Right (f x)
    fmap f (Left x) = Left x

```

### Functor Laws

1. The first functor law states that if we map the id function over a functor, the functor that we get back should be the same as the original functor. 

```haskell
fmap id = id
```

2. The second law says that composing two functions and then mapping the resulting function over a functor should be the same as first mapping one function over the functor and then mapping the other one. 
```haskell
fmap (f . g) = fmap f . fmap g

//or 
fmap (f . g) F = fmap f (fmap g F)
```


## Applicative

```haskell
class (Functor f) => Applicative f where  
    pure :: a -> f a  
    (<*>) :: f (a -> b) -> f a -> f b  
```

### Some Instances
```haskell

//Maybe
instance Applicative Maybe where
    pure = Just
    Nothing <*> _ = Nothing
    (Just f) <*> something = fmap f something

//List
instance Applicative [] where
    pure x = [x]
    fs <*> xs = [f x | f <- fs, x <- xs]

//IO
instance Applicative IO where
    pure = return
    a <*> b = do
        f <- a
        x <- b
        return (f x)
```


## Monoid
```haskell
class Monoid m where
    mempty :: m
    mappend :: m -> m -> m
    mconcat :: [m] -> m
    mconcat = foldr mappend mempty
```

### Laws
- mempty `mappend` x = x
- x `mapend` mempty = x
- (x `mappend` y) `mappend` z = x `mappend` (y `mappend` z)

### Some Instances
```haskell
//Maybe
instance Mnoid a => Monoid (Maybe a) where
    memtpy = Nothing
    Nothing `mappend` m = m
    m `mappend` Nothing = m 
    Just m1 `mappend` Just m2 = Just (m1 `append` m2)

//List
instance Monoid [a] where
    mempty = []
    mappend = (++)
```

## Monad
```haskell
class Monad m where  
    return :: a -> m a  
  
    (>>=) :: m a -> (a -> m b) -> m b  
  
    (>>) :: m a -> m b -> m b  
    x >> y = x >>= \_ -> y  
  
    fail :: String -> m a  
    fail msg = error msg  
```

### Some Instances
```haskell

//Maybe
instance Monad Maybe where
    return x = Just x
    Nothing >>= f = Nothing
    Just x >>= f = f x
    fail _ = Nothing

//List
instance Monad [] where
    return x = [x]
    xs >>= f = concat (map f xs)
    fail _ = []
```

### Laws

- **Left identity**, `return x >= f` is same as `f x`
- **Right identity**, `m >>= return` is same as `m`
- **Associativity**, `(m >>= f) >>= g` is same as `m >>= (\x -> f x >>= g)`