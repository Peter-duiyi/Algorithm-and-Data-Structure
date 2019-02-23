## Tree Implementation
Suppose we want to build a tree for some tree algorithms implementation。
Normally we build a tree by using two classes, one node class to define tree Node and the other one tree class to build the tree.
You can also see people use a struct and a class, there is no difference between two methods.  

```
struct TreeNode {
	int _val;
	TreeNode* _left;
	TreeNode* _right;
	TreeNode() : _left(nullptr), _right(nullptr){}
	TreeNode(int val)  : _val(val), _left(nullptr), _right(nullptr) {}
};

```

Then we also need a Tree class 

```
class Tree
{
public:
	TreeNode* _root;
	Tree() {
		_root = new TreeNode();
	};
	Tree(int val) {
		_root = new TreeNode(val);
	}
};

```
You can also encapulate the tree class and node struct if you want. Maybe I will add it to my code in the future.
