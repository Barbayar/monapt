katana
============

Like Scala APIs for TypeScript and JavaScript

## Setup

With Bower:

```
$ bower install git://github.com/yaakaito/katana.git --save
```

## APIs

## katana.Option<A>

```javascript
var valueOption = map.get('key');
valueOption.getOrElse(() => 'defaultValue');
```


```javascript
valueOption.map(v => v * 2).filter(v => v > 10).match({
    Some: v  => console.log(v),
    None: () => console.log('None!')
})
```

### katana.Some / katana.None

```javascript
new katana.Some('value')
new katana.None<string>()
```

### Properties / Methods

* `isEmpty: boolean`
* `get(): A`
* `getOrElse(defaultValue: () => A): A`
* `orElse(alternative: () => Option<A>): Option<A>`
* `match(matcher: IOptionMatcher<A>): void`
* `map<B>(f: (value: A) => B): Option<B>`
* `flatMap<B>(f: (value: A) => Option<B>): Option<B>`
* `filter(predicate: (value: A) => boolean): Option<A>`
* `reject(predicate: (value: A) => boolean): Option<A>`
* `foreach(f: (value: A) => void): void`


#### katana.IOptionMatcher<A>

```javascript
interface IOptionMatcher<A> {
    Some?(value: A): void;
    None?(): void;
}
```

## katana.Try<T>

### katana.Success / katana.Failure


## katana.Future<T>

```javascript
katana.future<string>(promise => {
    api.get((error, value) => {
        if (error) {
            promise.failure(error);
        }
        else {
          promise.success(value);
        }
    });
}).onComplete({
    Success: v => console.log(v),
    Failure: e => console.log(e)
})
```

Mix futures:
```javascript
var macbook = katana.future<string>(promise => {
    setTimeout(() => {
        promise.success('MacBook');
    }, 100);
});
 
var pro = katana.future<string>(promise => {
    setTimeout(() => {
        promise.success('Pro');
    }, 100);
});
 
var macbookPro = macbook.flatMap<string>(mb => {
    return pro.map<string>((pro, promise) => {
        promise.success(mb + pro);
    });
});
 
macbookPro.onSuccess(v => {
    console.log(v); // MacBookPro
});
```