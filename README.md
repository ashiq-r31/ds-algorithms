# JS Data Structures & Algorithms
## Trees
```javascript
function Tree(value) {
  this.value = value
  this.children = []
  // unlike linked lists we don't need to keep track of head and tail of each node
  // since its way too much metadata to track
}

Tree.prototype.addChild = function(value) {
  const child = new Tree(value)
  this.children.push(child)
  return child // print instance, allows you to access the child's methods as well
}
```

## Binary Search Trees
```javascript
function BinarySearchTree(value) {
  this.value = value
  this.left = null
  this.right = null
}

BinarySearchTree.prototype.insert = function(value) {
  if(value > this.value) {
    if(this.right) this.right.insert(value)
    else this.right = new BinarySearchTree(value)
  }
  if(value < this.value) {
    if(this.left) this.left.insert(value)
    else this.left = new BinarySearchTree(value)
  }
  return this
}

BinarySearchTree.prototype.contains = function(value) {
  if(value === this.value) return true
  if(value > this.value && !!this.right) {
    this.right.contains(value)
  }
  if(value < this.value && !!this.left) {
    this.left.contains(value)
  }
  return false
}

BinarySearchTree.prototype.inOrderTraversal = function(fn) {
  if (!this.left && !this.right) return fn(this.value) // print leaf node at the bottom
  if(!!this.left) this.left.inOrderTraversal(fn)
  fn(this.value) // print parent node after all its left nodes returned 
  if(!!this.right) this.right.inOrderTraversal(fn)
}

BinarySearchTree.prototype.preOrderTraversal = function(fn) {
  fn(this.value) // print parent node
  if(!!this.left) this.left.preOrderTraversal(fn) // print left 
  if(!!this.right) this.right.preOrderTraversal(fn) // print right
}

BinarySearchTree.prototype.postOrderTraversal = function(fn) {
  if(!!this.left) this.left.preOrderTraversal(fn)
  if(!!this.right) this.right.preOrderTraversal(fn)
  fn(this.value) // print parent node when no more nodes left
}
  
```

## Graphs
Implemented with an adjacency list. Link to gist [here](https://gist.github.com/ashiq-r31/fb0d6774200d82d3523969d3e23743d5).
```javascript
function Graph() {
  this.nodes = []
}

Graph.prototype.addVertex = function(value) {
  if(value === undefined) return
  // if vertex already exists then set itself, else set empty array
  this.nodes[value] = this.nodes[value] || []
}

Graph.prototype.addEdge = function(vertex1, vertex2) {
  if(!this.nodes[value1] || !this.nodes[value2]) return 'invalid node'
  // bi-directional edge for vertices
  this.nodes[value1].push(value2)
  this.nodes[value2].push(value1)
}

Graph.prototype.traverseDepthFirst = function(value, fn, visited) {
  if(!this.nodes[vertex] === undefined || typeof fn !== 'function') return 'invalid node or function'
  // initialze visited vertices lookup object
  visited = visited || {}
  // set current vertex as visited
  visited[value] = true
  fn(value)
  // iterate over neighbors of the current vertex
  this.nodes[value].forEach(function(neighbor) {
    if(!visited[neighbor]) {
      // recursively call self to traverse each neighbor's edges
      this.traverseDepthFirst(neighbor, fn, visited)
    }
    // exit iteration if already visited
    return
  }, this)
}

Graph.prototype.traverseBreadthFirst = function(value, fn) {
  if(!this.nodes[vertex] === undefined || typeof fn !== 'function') return 'invalid node or function'
  // initialze visited vertices lookup object
  let visited = {}
  // initialize queue
  let queue = [value]
  
  while(queue.length) {
    // take first vertex from queue
    let node = queue.shift()
    fn(node)
    this.nodes[node].forEach(function(neighbor) {
      if(!visited[neighbor]) {
        // set each neighbor vertex as visited
        visited[neighbor] = true
        // add neighbor to top of queue, traverse it's edges
        queue.push(neighbor)
      }
      return
    })
  }
}
```
