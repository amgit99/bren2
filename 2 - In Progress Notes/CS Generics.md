<span style="color:#e1db3d">Generics</span> are like Templates from C++, it can be imagined as a function structure that takes in types as arguments as well. Generics are very powerful, the full extent can only be explained with examples, Generics is how the very well developed [[CS LINQ]] library is developed.

<span style="color:#00b050">Example</span>: Given a list of heros (object with attributes, FirstName, LastName, HeroName and CanFly) we are performing few filtering queries, here we see incrementally, how we can write efficient code leading up-to what the LINQ library offers.
```
List<Hero> FilterHerosWithNoLastName(List<Hero> heros) {
	
	var resultList = new List<Hero> ();
		
	foreach (var hero in heros){
		if (string.IsNullOrEmpty(hero.LastName)){
			resultList.Add(hero);
		}
	}
	return resultList;
}

List<Hero> FilterHerosThatCanFly(List<Hero> heros) {
	
	var resultList = new List<Hero> ();
		
	foreach (var hero in heros){
		if (hero.CanFly){
			resultList.Add(hero);
		}
	}
	return resultList;
}
```
Given above are two methods used to filter out Heros without last names and the ones that can fly. we have separate functions. We can use delegates and pass on lambdas to make this even more cleaner.
```
List<Hero> filterHeros(List<Hero> heros, Filter f){
	
	var resultList = new List<Hero> ();
		
	foreach(var hero in heros){
		if(f(hero)){
			resultList.Add(hero);
		}
	}
	return resultList;
}

delegate bool Filter(Hero h);

var result1 = filterHeros(heros, (hero) => hero.Canfly);
var result2 = filterHeros(heros, (hero) => {string.IsNullOrEmpty(hero.LastName)};
```
Now we have a delegate for the filter, and for the delegate we pass on a lambdas according to our requirement. So just one method is used.

`IEnumerable` interface is one that is implemented by any <span style="color:#e1db3d">iterable collection</span> that has the <span style="color:#e1db3d">forward only</span> and <span style="color:#e1db3d">read only</span> property. So if there is a function that takes in a collection as a parameter and that collection need not be modified and iterated in a forward only manner, then we can define the type of the object as <span style="color:#e1db3d">IEnumerable</span>.
This is used to make the function more general(generic). Like the filter use-case in the above example, we can extend this to make it general for querying any kind of collection that checks these conditions and returns a collection of filtered entities. 
```
IEnumerable<T> filter(IEnumerable<T> items, Filter<T> f){
	
	var resultList = new List<T> ();
		
	foreach(var item in items){
		if(f(item)){
			resultList.Add(item);
		}
	}
	return resultList;
}

delegate bool Filter<T> (T t);

var result1 = filter (heros, (hero) => hero.CanFly);
var result2 = filter (cars, (car) => car.isAutomatic);
var result3 = filter (phones, (phone) => phone.isAndroid); --------- (1)
```
Now, we have a filter function that can work with any forward iterable, read-only data structure and query its elements based on our requirements. We have the LINQ where clause.
<span style="color:#e1db3d">(1)</span> C# has implicit type resolution and hence we don't need to specify the generic type <\Hero>, <\Car>, <\Phone>, for results of the queries.


