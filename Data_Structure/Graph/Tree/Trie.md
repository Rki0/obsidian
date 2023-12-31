<img width="100%" alt="image" src="https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FpYAoN%2FbtqPZJ9d7rl%2FYGhdbBzRXzLdY1ytJmsvJK%2Fimg.png">
image source : https://rebro.kr/86
# Feature
- [[Tree]]의 일종으로 문자열 검색에 특화된 자료 구조이다.
- 각 단어가 끝나는 지점에 특별한 표시를 해두는 것으로 해당 위치의 노드까지 문자열을 이루고 있음을 나타낸다.
- 접두사 검색(Prefix Search) : 사용자가 입력한 접두사를 가진 모든 가능한 단어나 문구를 찾는다.
- 자동 완성(Auto Completion) : 접두사 검색을 기반으로 동작. 사용자가 입력한 부분 문자열을 기반으로 가능한 모든 문자열을 찾아서 제안한다.
- 사전 검색(Dictionary Search) : 사용자가 완전한 단어나 문구를 입력하여 해당 단어가 사전에 있는지 확인하고 관련 정보를 제공한다.

# Why not use Array or Hash Table?
1. `Array`
	- 배열이나 리스트에 문자열을 저장하고, 검색할 때마다 전체를 순회하면서 검색해야한다.
	- 효율성이 떨어지기 때문에 큰 data set에서는 사용하기 어렵다.
2. `Hash Table`
	- 'apple'이라는 문자열을 검색하고자 한다고 했을 때, 해시 테이블에서는 해당 문자열을 검색하면 `O(1)`의 속도로 빠르게 탐색 가능!
	- 하지만, 해시 충동 시 값의 재비교, `&` 접두사, 부분 문자열 검색, 사전 순 정렬 등에 제약이 있다.
	- 예를들어, '나'('나이키' 검색을 위해), 'ple'('apple'이라는 전체 단어를 모른채 부분만 알 때) 처럼 단어의 일부분만 알고 있을 때, 이 부분을 포함하는 모든 단어('나비', '나이키', 'apple', 'people' 등)를 찾는 것이 어렵다.
	- 해시 테이블은 전체 문자열을 활용해서 해시화를 하기 때문에, 전체 문자열을 입력받지 않으면 해당 문자열을 검색할 수 없다. 따라서, 자동 완성 기능을 만들 수 없다.
	- 해시 테이블은 키를 사전 순으로 정렬하지 않는다. 따라서, 단어를 사전 순으로 보고 싶다면 별도의 작업이 필요하다.
	- 즉, 완전한 단어의 검색만 구현한다면 해시 테이블이 효율적이고, 그 외 기능을 추가해야한다면 `Trie`가 유리하다.

# Time Complexity
- O(n) : 탐색되는 트라이 부분의 노드 수

# Space Complexity
- O(n) : 재귀적 호출로 인한 문자열의 최대 길이

# Implement

```js
class Node{
	constructor(){
		this.children = {};
		this.isEndOfWord = false;
	}
}

class Trie{
	constructor(){
		this.root = new Node();
	}

	addWord(word){
	    let current = this.root;

	    for(let char of word){
	        if(!current.children[char]){
	            current.children[char] = new Node();
	        }

	        current = current.children[char];
	    }

	    current.isEndOfWord = true;
	}

	search(word){
		let current = this.root;

	    for(let char of word){
	        if(!current.children[char]){
	            return false;
	        }

	        current = current.children[char];
	    }

	    return current.isEndOfWord;
	}

	startsWith(prefix){
		let current = this.root;

	    for(let char of word){
	        if(!current.children[char]){
	            return false;
	        }

	        current = current.children[char];
	    }

	    return true;
	}

	getAllWords(){
		let words = [];

		function searchWords(node, currentWord){
			if(node.isEndOfWord){
				words.push(currentWord);
			}
		
			for(let char in node.children){
				searchWords(node.children[char], currentWord + char);
			}
		}

		searchWords(this.root, "");

		return words;
	}
}
```

## startsWith - Return all words containing the prefix
- 앞서 구현한 `startsWith()`와 다른 부분은 `prefix`를 순회한 뒤, 가능한 문자열을 전부 탐색한다는 것이다.
- `prefix`가 존재하는 문자열인지부터 판단하고, 그 후, `collectWords()`를 통해 가능한 문자열을 전부 탐색한다.
- `collectWords()`는 루트 노드가 아니라, `prefix` 순회가 끝난 노드부터 시작한다는 점에 주의하자.

```js
startsWith(prefix) {
	let current = this.root;

	let foundWords = [];

	let currentPrefix = '';

	for(let char of prefix){
		if(!current.children[char]){
			return foundWords;
		}
		
		currentPrefix += char;
		
		current = current.children[char];
	}

	function collectWords(node, currentWord){
		if(node.isEndOfWord){
			foundWords.push(currentPrefix + currentWord);
		}

		for(let char in node.children){
			collectWords(node.children[char], currentWord + char);
		}
	}

	collectWords(current, '');
	
	return foundWords;
}
```