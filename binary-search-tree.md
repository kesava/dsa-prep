## Binary Search Tree
```js
//// 6.3 Binary/Binary Search tree
const EmptyTree = null;
const MakeTree = (v, l, r) => ({
  v, l, r
});
const root = tree => {
  let {v, l, r} = tree;
  return v;
}
const left = tree => {
  let {v, l, r} = tree;
  return l;
}
const right = tree => {
  let {v, l, r} = tree;
  return r;
}

const isEmpty = t => t === EmptyTree;

const Leaf = (v) => MakeTree(v, EmptyTree, EmptyTree); 
const isLeaf = t => {
  if (isEmpty(t)) return false;
  let {v, l, r} = t;
  if (isNull(l) && isNull(r)) return true;
  return false;
}

const t = MakeTree(10, Leaf(3), Leaf(4));
const size = t => isEmpty(t) ? 0 : (1 + size(left(t)) + size(right(t)));
size(t) // 3

const insert = (x, t) => {
  if (isEmpty(t)) {
    return Leaf(x);
  } else {
    let {v, l, r} = t;
    if (x < root(t)) {
      return MakeTree(v, insert(x, l), r);
    } else {
      return MakeTree(v, l, insert(x, r));
    }
  }
}

const del = (t, x) => {
  if (isEmpty(t)) {
    return t;
  } else {
    let {v, l, r} = t;
    if (x === v) {
      if (isEmpty(l)) {
        return r;
      } else if (isEmpty(r)) {
        return l;
      } else {
        console.log({ smallestNode: smallestNode(r) })
        return MakeTree(smallestNode(r), l, removeSmallestNode(r));
      }
    } else if (x < v) {
      return MakeTree(v, del(l, x), r);
    } else {
      return MakeTree(v, l, del(r, x));
    }
  }
}

const smallestNode = t => {
  if (isEmpty(t)) return t;
  let {v, l, r} = t;
  if (isNull(l)) {
    return v;
  } else if (isLeaf(l)) {
    return root(l);
  } else {
    return smallestNode(l);
  }
}

const removeSmallestNode = t => {
  if (isEmpty(t)) return t;
  let {v, l, r} = t;
  if (isNull(l)) {
    return r;
  } else if (isLeaf(l)) {
    return MakeTree(v, EmptyTree, r);
  } else {
    return MakeTree(v, removeSmallestNode(l), r);
  }
}

const floor = (t, x) => {
  if (isEmpty(t)) return EmptyTree;
  let {v, l, r} = t;
  switch(true) {
    case (v === x): return x;
    case (v > x): return floor(l, x);
    case (v < x): 
      let fl = floor(r, x);
      if (fl) return (x >= fl) ? fl : v;
      return v;
  }
}

const height = t => {
  if (isEmpty(t)) {
    return 0;
  } else {
    let {v, l, r} = t;
    return Math.max(height(l), height(r)) + 1;
  }
} 

const flatten = arr => {
  let output = [];
  // if (!arr) return [];
  arr.forEach(element => {
    if (Array.isArray(element)) {
      output = output.concat(flatten(element));
    } else if (element !== null) {
      output.push(element);
    }
  });
  return output;
}

const isNull = t => t === null

const inorder = t => {
  if (isEmpty(t)) return t;
  let {v, l, r} = t;
  switch(true) {
    case (!isNull(l) && !isNull(r)):
      return flatten([inorder(l), v, inorder(r)]);
    case (!isNull(l)):
      return flatten([inorder(l), v]);
    case (!isNull(r)):
      return flatten([v, inorder(r)]);
    default:
      return v;
  } 
}

const randomNumbers = (n, max=100) => ({
    [Symbol.iterator]() {
      let i = 0;
      return {
        next() {
        if (i < n) {
          i++;
          return { value: Math.floor(Math.random() * max), done: false }
        } else {
          return { done: true }
        }
      }
    }
    }
});

const str = 'hello world and some more and then even more'



const buildTree = list => list.reduce((acc, element) => insert(element, acc), EmptyTree);
const t2 = buildTree([3,2,4,9])
t2
// {
//   v: 3,
//   l: { v: 2, l: null, r: null },
//   r: { v: 4, l: null, r: { v: 9, l: null, r: null } }
// }


smallestNode(t2) // 2

removeSmallestNode(t2)
// {
//   v: 3,
//   l: null,
//   r: { v: 4, l: null, r: { v: 9, l: null, r: null } }
// }

const findBinaryTree = (tree, element) => {
  if (isEmpty(tree)) return false;
  let {v, l, r} = tree;
  switch(true) {
    case (element === v): return true;
    case (element < v): return findBinaryTree(l, element);
    case (element > v): return findBinaryTree(r, element);
  }
}
const find = (list, element) => findBinaryTree(buildTree(list), element);
find([3,1,7,9,2,8], 3); // true
find([3,1,7,9,2,8], 6); // false
floor(buildTree([3,1,7,9,2,8]), 4);
const r3 = [...randomNumbers(100, 10000)];
const t3 = del(buildTree(r3), r3[1]);
r3.length // 100
inorder(buildTree(r3)).length // 100
inorder(t3).length // 99

const binsort = arr => inorder(buildTree(arr));
binsort([...randomNumbers(100, 10000)])
// [ 101, 303, 389, 437, 813, 908, 909, 958, 1047, 1096, 1293, 1306, 1400, 1535, 2013, 2049, 2091, 2187, 2220, 2433, 2462, 2495, 2567, 2573, 2587, 2594, 2780, 2898, 3026, 3060, 3082, 3425, 3529, 3774, 3805, 4169, 4209, 4391, 4891, 4939, 5475, 5623, 5635, 5653, 5670, 5972, 5991, 5993, 6139, 6538, 6549, 6551, 6796, 6810, 6907, 7008, 7019, 7048, 7116, 7144, 7219, 7328, 7353, 7362, 7397, 7428, 7542, 7594, 7609, 7640, 7654, 7713, 7864, 7885, 7897, 8025, 8034, 8215, 8221, 8569, 8776, 8809, 8863, 8867, 9031, 9081, 9087, 9155, 9165, 9253, 9318, 9394, 9603, 9615, 9645, 9743, 9753, 9834, 9915, 9968 ]

const nonTrivialTree = t => {
  if (isLeaf(t)) return false;
  let {v, l, r} = t;
  if (isEmpty(l) || isEmpty(r)) return false;
  return true;
}

const rotateLeft = t => {
  if (isEmpty(t)) {
    return t;
  } else {
    let {v: y, l: left, r: c} = t;
    if (nonTrivialTree(left)) {
      let {v: x, l: a, r: b} = left;
      return MakeTree(x, a, MakeTree(y, b, c));
    }
    return t;
  }
}

const rotateRight = t => {
  if (isEmpty(t)) {
    return t;
  } else {
    let {v: x, l: a, r: right} = t;
    if (nonTrivialTree(right)) {
      let {v: y, l: b, r: c} = right;
      return MakeTree(y, MakeTree(x, a, b), c);
    }
    return t;
  }
}

const t1 = MakeTree('y', MakeTree('x', Leaf('a'), Leaf('b')), Leaf('c'));
const t2 = rotateRight(rotateLeft(t1));

```
