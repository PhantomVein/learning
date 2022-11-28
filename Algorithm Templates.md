* 位运算
```
bit操作

& 符号，x & y ，会将两个十进制数在二进制下进行与运算
| 符号，x | y ，会将两个十进制数在二进制下进行或运算
^ 符号，x ^ y ，会将两个十进制数在二进制下进行异或运算
<< 符号，x << y 左移操作，最右边用 0 填充
>> 符号，x >> y 右移操作，最左边用 0 填充
~ 符号，~x ，按位取反操作，将 x 在二进制下的每一位取反

整数集合set位运算
整数集合做标志时，比如回溯时的visited标志数组

visited 访问 i visited | (1 << i)
visited 离开 i visited & ~(1 << i)
visited 不包含 i : not visited & (1 << i)

并集 ：A | B
交集 ：A & B
全集 ：(1 << n) - 1
补集 ：((1 << n) - 1) ^ A
子集 ：(A & B) == B
判断是否是 2 的幂 ：A & (A - 1) == 0
最低位的 1 变为 0 ：n &= (n - 1)
最低位的 1：A & (-A)，最低位的 1 一般记为 lowbit(A)
```

* 并查集
```python
class DisjointSetUnion:
    def __init__(self, n: int):
        self.n = n
        self.rank = [1] * n
        self.f = list(range(n))
    
    def find(self, x: int) -> int:
        if self.f[x] == x:
            return x
        self.f[x] = self.find(self.f[x])
        return self.f[x]
    
    def unionSet(self, x: int, y: int):  # 按秩合并
        root_x, root_y = self.find(x), self.find(y)
        if root_x == root_y:
            return
        if self.rank[root_x] < self.rank[root_y]:
            root_x, root_y = root_y, root_x
        self.rank[root_x] += self.rank[root_y]
        self.f[root_y] = root_x
```

* 非递归遍历
```python
def inorderTraversal(root: Node):
    stack = []
    if root:
        stack.append(root)
    while stack:
        current = stack.pop()
        if current:
            if current.right:
                stack.append(current.right)  # right
            stack.append(current)
            stack.append(None)
            if current.left:
                stack.append(current.left)  # left
        else:
            current = stack.pop()
            print(current)
```

* 快排
```python
def quick_sort(nums: List[int]):
    def quick(left, right):
        if left >= right:
            return nums
        pivot = left
        i = left
        j = right
        while i < j:
            while i < j and nums[j] > nums[pivot]:
                j -= 1
            while i < j and nums[i] <= nums[pivot]:
                i += 1
            nums[i], nums[j] = nums[j], nums[i]
        nums[pivot], nums[j] = nums[j], nums[pivot]
        quick(left, j - 1)
        quick(j + 1, right)
        return nums

    return quick(0, len(nums) - 1)
```
* 二分查找
```python
# lower_bound / bisect_left 求非降序范围[first, last)内第一个不小于value的值的位置
# 本身找的是x >= value的下界，若为last则不存在
# 所以最好判断返回值是否为last，为last则不能作为index
def lower_bound(array, first, last, value):
    while first < last: # 搜索区间[first, last)不为空
        mid = first + (last - first) // 2  # 防溢出
        if array[mid] < value: first = mid + 1 
        else: last = mid
    return first  # last也行，因为[first, last)为空的时候它们重合

# upper_bound / bisect_right / bisect 找的是x > value的下界，若为last则不存在
# 代码为把 array[mid] < value 改为 array[mid] <= value
```

* 字典树
```python
class TrieNode:
    def __init__(self):
        self.children = defaultdict(TrieNode)
        self.is_word = False

class Trie:
    def __init__(self):
        self.root = TrieNode()

    # Inserts a word into the trie.
    def insert(self, word: str) -> None:
        node = self.root
        for s in word:
            node = node.children[s]
        node.is_word = True

    # Returns if the word is in the trie.
    def search(self, word: str) -> bool:
        node = self.root
        for s in word:
            if s in node.children:
                node = node.children[s]
            else:
                return False
        return node.is_word

    # Returns if there is any word in the trie that starts with the given prefix.
    def startsWith(self, prefix: str) -> bool:
        node = self.root
        for s in prefix:
            if s in node.children:
                node = node.children[s]
            else:
                return False
        return True
        

# Your Trie object will be instantiated and called as such:
# obj = Trie()
# obj.insert(word)
# param_2 = obj.search(word)
# param_3 = obj.startsWith(prefix)
```

* 最短路 Dijkstra
```python
```

* 最小生成树 Prim
```python
```
