## 클래스로 구현하기

```python
class Node:
    def __init__(self, data):
        self.left = None
        self.right = None
        self.data = data


root = Node(10)
root.left = Node(34)
root.right = Node(89)
root.left.left = Node(45)
root.left.right = Node(50)

def preorder(node):
    if node:
        print(node.data)
        preorder(node.left)
        preorder(node.right)

preorder(root)
print('---------------------------')

def inorder(node):
    if node:
        inorder(node.left)
        print(node.data)
        inorder(node.right)

inorder(root)
print('---------------------------')

def postorder(node):
    if node:
        postorder(node.left)
        postorder(node.right)
        print(node.data)

postorder(root)
```



### python 라이브러리 활용

`from anytree import Node, RenderTree`