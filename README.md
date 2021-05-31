# Tree

### What is a Tree?


A tree is a data structure similar to a linked list but instead of each node pointing simply to the next node in a linear fashion, each node points to a number of nodes. Tree is an example of a non- linear data structure. A tree structure is a way of representing the hierarchical nature of a structure in a graphical form.
In trees ADT (Abstract Data Type), the order of the elements is not important. If we need ordering information, linear data structures like linked lists, stacks, queues, etc. can be used.


### Tree Node 

```swift

public class TreeNode<T> {
    public var value: T
    public var children: [TreeNode] = []
    
    public init(_ value: T) {
        self.value = value
    }
    
    public func add(_ child: TreeNode) {
        children.append(child)
    }
}


let beverages = TreeNode("Beverages")

let hot = TreeNode("hot")
let cold = TreeNode("cold")

beverages.add(hot)
beverages.add(cold)

```



### depth first traversal

```swift
extension TreeNode {
    public func forEachDepthFirst(visit: (TreeNode) -> Void) {
        visit(self)
        children.forEach {
            $0.forEachDepthFirst(visit: visit)
        }
    }
}

```

// Level Order Traversal

extension TreeNode {
    public func forEachLevelOrder(visit: (TreeNode) -> Void) {
        visit(self)
        var queue = QueueArray<TreeNode>()
        children.forEach {
            queue.enqueue($0)
        }
        
        while let node = queue.dequeue() {
            visit(node)
            node.children.forEach { queue.enqueue($0) }
        }
        
    }
}


func makeBeverageTree() -> TreeNode<String> {
    let tree = TreeNode("Beverages")
    
    let hot = TreeNode("hot")
    let cold = TreeNode("cold")
    
    let tea = TreeNode("Tea")
    let coffee = TreeNode("Coffee")
    let chocolate = TreeNode("cocoa")
    
    let blackTea = TreeNode("black")
    let greenTea = TreeNode("green")
    let chai = TreeNode("chai")
    
    let soda = TreeNode("soda")
    let milk = TreeNode("milk")
    
    let gingerAle = TreeNode("ginger Ale")
    let bitterLemon = TreeNode("bitter Lemon")


    tree.add(hot)
    tree.add(cold)
    
    hot.add(tea)
    hot.add(coffee)
    hot.add(chocolate)
    
    cold.add(soda)
    cold.add(milk)
    
    tea.add(blackTea)
    tea.add(greenTea)
    tea.add(chai)
    
    soda.add(bitterLemon)
    soda.add(gingerAle)
    
    return tree
}


let tree = makeBeverageTree()
tree.forEachDepthFirst {
    print($0.value)
}

print("----------------------------------")
print("----------------------------------")
print("----------------------------------")
print("----------------------------------")
print("----------------------------------")


tree.forEachLevelOrder {
    print($0.value)
}



print("----------------------------------")
print("----------------------------------")
print("----------------------------------")
print("----------------------------------")
print("----------------------------------")

extension TreeNode where T: Equatable {
    
    public func search(_ value: T) -> TreeNode? {
        var result: TreeNode?
        
        forEachLevelOrder { node in
            if node.value == value {
                result = node
            }
        }
        
        return result
    }
}



public protocol Queue {
    associatedtype Element
    mutating func enqueue(_ element: Element) -> Bool
    mutating func dequeue() -> Element?
    var isEmpty: Bool { get }
    var peek: Element? { get }
}


public struct QueueArray<T>: Queue {

    
    public typealias Element = T
    
    
    private var array: [T] = []
    public init () {}
    
    public var isEmpty: Bool {
        return array.isEmpty
    }
    
    public var peek: T? {
        return array.first
    }
    
    public mutating func enqueue(_ element: T) -> Bool {
        array.append(element)
        return true
    }
    
    public mutating func dequeue() -> T? {
        return isEmpty ? nil : array.removeFirst()
    }
    
}

extension QueueArray: CustomStringConvertible {
    
    public var description: String {
        return String(describing: array)
    }
}


var queueArray = QueueArray<String>()
queueArray.enqueue("hi")
queueArray.enqueue("Hello")
queueArray.enqueue("How")

print(queueArray.description)


if let searchResult = tree.search("hot") {
    print("found node ", searchResult.value)
}

```

