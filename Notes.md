Notes

Kafka




JavaScript

Set
	const mySet = new Set(['1', '2', '3']); [...mySet].indexOf('2') // returns 1
Map
	var map = new Map([[ 1, 'one' ],[ 2, 'two' ]]);map.get(1) // returns 'one'



Sorting

	array’s default sort sorts an array based on Unicode
	// array of objects
	const arr = [{ name: 'Sharpe', value: 37 }, { name: 'And', value: 45 }, { name: 'The', value: -12 }];
	// sort by name string
	const st1 = arr.sort((a, b) => a.name.localeCompare(b.name));
	Object
	There is no built-in method for the sorting of the objects, but ES6 offers some interesting built-in key-sorting during the creation of the object. Object keys are sorted only based on 	    numerics/numeric-strings, all the other keys are pushed right after the numeric keys unsorted.
	const obj = { b: "one", 4: "kid", "30": "dad", 9: "son", a: "two" };
	console.log(obj) // returns {4: "kid", 9: "son", 30: "dad", b: "one", a: "two"}

      SET
	const set = new Set(['b', 'a', ‘c’]); […set].sort(); // returns ['a', 'b', 'c'] which is an array
      MAP
 	// entries spread over an array can be sorted like an array
	const map = new Map([["c", 'three'],["a", 'one'], ["b", 'two']]);
	const sortedMap = new Map([...map.entries()].sort()) // returns sorted Map(3) {"a" => "one", "b" => "three", "c" => "two"}

Removing Duplicates

	// however set doesn't remove duplicates from array of objects
	const arr = [{a:1},{b:2},{a:1}]; [...new Set(arr)]; // returns [{a:1},{b:2},{a:1}]
	// hence we can use ES6 filter and map functions to achieve the same
	arr.filter((obj, index) => { 
	  return arr.map(obj => obj['a']).indexOf(obj['a']) === index;    
	}); // removes duplicate based on the key 'a'

7 Tricks with Resting and Spreading JavaScript Objects
	Adding
		const user = { id: 100, name: 'Howard Moon’}; const userWithPass = { ...user, password: 'Password!' }
	Merge Objects
		const part1 = { id: 100, name: 'Howard Moon' } const part2 = { id: 100, password: 'Password!' } const user1 = { ...part1, ...part2 }
	Exclude Object Props.
		const noPassword = ({ password, ...rest }) => rest
		const user = {id: 100, name: 'Howard Moon', password: 'Password!'}
		noPassword(user)
        Dynamically Exclude Props
		const user1 = {id: 100,name: 'Howard Moon',password: 'Password!'}
		const removeProperty = prop => ({ [prop]: _, ...rest }) => rest
		const removePassword = removeProperty('password')
		const removeId = removeProperty('id')
		removePassword(user1) //=> { id: 100, name: 'Howard Moon' }
		removeId(user1)
	Organize Props
		const user3 = {password: 'Password!'name: 'Naboo', id: 300}
		const organize = object => ({ id: undefined, ...object })
		organize(user3) //=> { id: 300, password: 'Password!', name: 'Naboo' }

		const user3 = {password: 'Password!’,name: 'Naboo',id: 300}
		const organize = ({ password, ...object }) => ({ ...object, password }) // move password to last property
		organize(user3) //=> { name: 'Naboo', id: 300, password: 'Password!' }
	Default Props
		const user2 = { id: 200,  name: 'Vince Noir'}
		const user4 = {id: 400,  name: 'Bollo', quotes: ["I've got a bad feeling about this..."]}
		const setDefaults = ({ quotes = [], ...object}) => ({ ...object, quotes })
		setDefaults(user2)	//=> { id: 200, name: 'Vince Noir', quotes: [] }
		setDefaults(user4) // => leave original
	Remaining Properties
		const renamed = ({ ID, ...object }) => ({ id: ID, ...object })
		const user = { ID: 500, name: "Bob Fossil" }
		renamed(user) //=> { id: 5000, name: 'Bob Fossil' }

	const arrayToObject = (arr, keyField) => Object.assign({}, ...arr.map(item => ({[item[keyField]]: item})))
	const peopleObject = arrayToObject(peopleArray, "id")
	console.log(peopleObject[idToSelect])


Reactive

The term, “reactive,” refers to programming models that are built around reacting to change — network components reacting to I/O events, UI controllers reacting to mouse events, and others. In that sense, non-blocking is reactive, because, instead of being blocked, we are now in the mode of reacting to notifications as operations complete or data becomes available.
In synchronous, imperative code, blocking calls serve as a natural form of back pressure that forces the caller to wait. In non-blocking code, it becomes important to control the rate of events so that a fast producer does not overwhelm its destination.

The key expected benefit of reactive and non-blocking is the ability to scale with a small, fixed number of threads and less memory.

In Spring WebFlux (and non-blocking servers in general), it is assumed that applications do not block, and, therefore, non-blocking servers use a small, fixed-size thread pool (event loop workers) to handle requests.

https://docs.spring.io/spring/docs/current/spring-framework-reference/web-reactive.html


