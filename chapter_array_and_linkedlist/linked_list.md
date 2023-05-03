---
comments: true
---

# 4.2. &nbsp; 链表

内存空间是所有程序的公共资源，排除已被占用的内存空间，空闲内存空间通常散落在内存各处。在上一节中，我们提到存储数组的内存空间必须是连续的，而当我们需要申请一个非常大的数组时，空闲内存中可能没有这么大的连续空间。与数组相比，链表更具灵活性，它可以被存储在非连续的内存空间中。

「链表 Linked List」是一种线性数据结构，其每个元素都是一个节点对象，各个节点之间通过指针连接，从当前节点通过指针可以访问到下一个节点。**由于指针记录了下个节点的内存地址，因此无需保证内存地址的连续性**，从而可以将各个节点分散存储在内存各处。

链表「节点 Node」包含两项数据，一是节点「值 Value」，二是指向下一节点的「指针 Pointer」，或称「引用 Reference」。

![链表定义与存储方式](linked_list.assets/linkedlist_definition.png)

<p align="center"> Fig. 链表定义与存储方式 </p>

=== "Java"

    ```java title=""
    /* 链表节点类 */
    class ListNode {
        int val;        // 节点值
        ListNode next;  // 指向下一节点的指针（引用）
        ListNode(int x) { val = x; }  // 构造函数
    }
    ```

=== "C++"

    ```cpp title=""
    /* 链表节点结构体 */
    struct ListNode {
        int val;         // 节点值
        ListNode *next;  // 指向下一节点的指针（引用）
        ListNode(int x) : val(x), next(nullptr) {}  // 构造函数
    };
    ```

=== "Python"

    ```python title=""
    class ListNode:
        """链表节点类"""
        def __init__(self, val: int):
            self.val: int = val                  # 节点值
            self.next: Optional[ListNode] = None # 指向下一节点的指针（引用）
    ```

=== "Go"

    ```go title=""
    /* 链表节点结构体 */
    type ListNode struct {
        Val  int       // 节点值
        Next *ListNode // 指向下一节点的指针（引用）
    }
    
    // NewListNode 构造函数，创建一个新的链表
    func NewListNode(val int) *ListNode {
        return &ListNode{
            Val:  val,
            Next: nil,
        }
    }
    ```

=== "JavaScript"

    ```javascript title=""
    /* 链表节点类 */
    class ListNode {
        val;
        next;
        constructor(val, next) {
            this.val = (val === undefined ? 0 : val);       // 节点值
            this.next = (next === undefined ? null : next); // 指向下一节点的引用
        }
    }
    ```

=== "TypeScript"

    ```typescript title=""
    /* 链表节点类 */
    class ListNode {
        val: number;
        next: ListNode | null;
        constructor(val?: number, next?: ListNode | null) {
            this.val = val === undefined ? 0 : val;        // 节点值
            this.next = next === undefined ? null : next;  // 指向下一节点的引用
        }
    }
    ```

=== "C"

    ```c title=""
    /* 链表节点结构体 */
    struct ListNode {
        int val;               // 节点值
        struct ListNode *next; // 指向下一节点的指针（引用）
    };

    typedef struct ListNode ListNode;

    /* 构造函数 */
    ListNode *newListNode(int val) {
        ListNode *node, *next;
        node = (ListNode *) malloc(sizeof(ListNode));
        node->val = val;
        node->next = NULL;
        return node;
    }
    ```

=== "C#"

    ```csharp title=""
    /* 链表节点类 */
    class ListNode
    {
        int val;         // 节点值
        ListNode next;   // 指向下一节点的引用
        ListNode(int x) => val = x;  //构造函数
    }
    ```

=== "Swift"

    ```swift title=""
    /* 链表节点类 */
    class ListNode {
        var val: Int // 节点值
        var next: ListNode? // 指向下一节点的指针（引用）

        init(x: Int) { // 构造函数
            val = x
        }
    }
    ```

=== "Zig"

    ```zig title=""
    // 链表节点类
    pub fn ListNode(comptime T: type) type {
        return struct {
            const Self = @This();
            
            val: T = 0, // 节点值
            next: ?*Self = null, // 指向下一节点的指针（引用）

            // 构造函数
            pub fn init(self: *Self, x: i32) void {
                self.val = x;
                self.next = null;
            }
        };
    }
    ```

!!! question "尾节点指向什么？"

    我们将链表的最后一个节点称为「尾节点」，其指向的是“空”，在 Java, C++, Python 中分别记为 $\text{null}$ , $\text{nullptr}$ , $\text{None}$ 。在不引起歧义的前提下，本书都使用 $\text{null}$ 来表示空。

!!! question "如何称呼链表？"

    在编程语言中，数组整体就是一个变量，例如数组 `nums` ，包含各个元素 `nums[0]` , `nums[1]` 等等。而链表是由多个节点对象组成，我们通常将头节点当作链表的代称，例如头节点 `head` 和链表 `head` 实际上是同义的。

**链表初始化方法**。建立链表分为两步，第一步是初始化各个节点对象，第二步是构建引用指向关系。完成后，即可以从链表的头节点（即首个节点）出发，通过指针 `next` 依次访问所有节点。

=== "Java"

    ```java title="linked_list.java"
    /* 初始化链表 1 -> 3 -> 2 -> 5 -> 4 */
    // 初始化各个节点 
    ListNode n0 = new ListNode(1);
    ListNode n1 = new ListNode(3);
    ListNode n2 = new ListNode(2);
    ListNode n3 = new ListNode(5);
    ListNode n4 = new ListNode(4);
    // 构建引用指向
    n0.next = n1;
    n1.next = n2;
    n2.next = n3;
    n3.next = n4;
    ```

=== "C++"

    ```cpp title="linked_list.cpp"
    /* 初始化链表 1 -> 3 -> 2 -> 5 -> 4 */
    // 初始化各个节点 
    ListNode* n0 = new ListNode(1);
    ListNode* n1 = new ListNode(3);
    ListNode* n2 = new ListNode(2);
    ListNode* n3 = new ListNode(5);
    ListNode* n4 = new ListNode(4);
    // 构建引用指向
    n0->next = n1;
    n1->next = n2;
    n2->next = n3;
    n3->next = n4;
    ```

=== "Python"

    ```python title="linked_list.py"
    # 初始化链表 1 -> 3 -> 2 -> 5 -> 4
    # 初始化各个节点 
    n0 = ListNode(1)
    n1 = ListNode(3)
    n2 = ListNode(2)
    n3 = ListNode(5)
    n4 = ListNode(4)
    # 构建引用指向
    n0.next = n1
    n1.next = n2
    n2.next = n3
    n3.next = n4
    ```

=== "Go"

    ```go title="linked_list.go"
    /* 初始化链表 1 -> 3 -> 2 -> 5 -> 4 */
    // 初始化各个节点
    n0 := NewListNode(1)
    n1 := NewListNode(3)
    n2 := NewListNode(2)
    n3 := NewListNode(5)
    n4 := NewListNode(4)
    // 构建引用指向
    n0.Next = n1
    n1.Next = n2
    n2.Next = n3
    n3.Next = n4
    ```

=== "JavaScript"

    ```javascript title="linked_list.js"
    /* 初始化链表 1 -> 3 -> 2 -> 5 -> 4 */
    // 初始化各个节点
    const n0 = new ListNode(1);
    const n1 = new ListNode(3);
    const n2 = new ListNode(2);
    const n3 = new ListNode(5);
    const n4 = new ListNode(4);
    // 构建引用指向
    n0.next = n1;
    n1.next = n2;
    n2.next = n3;
    n3.next = n4;
    ```

=== "TypeScript"

    ```typescript title="linked_list.ts"
    /* 初始化链表 1 -> 3 -> 2 -> 5 -> 4 */
    // 初始化各个节点
    const n0 = new ListNode(1);
    const n1 = new ListNode(3);
    const n2 = new ListNode(2);
    const n3 = new ListNode(5);
    const n4 = new ListNode(4);
    // 构建引用指向
    n0.next = n1;
    n1.next = n2;
    n2.next = n3;
    n3.next = n4;
    ```

=== "C"

    ```c title="linked_list.c"
    /* 初始化链表 1 -> 3 -> 2 -> 5 -> 4 */
    // 初始化各个节点 
    ListNode* n0 = newListNode(1);
    ListNode* n1 = newListNode(3);
    ListNode* n2 = newListNode(2);
    ListNode* n3 = newListNode(5);
    ListNode* n4 = newListNode(4);
    // 构建引用指向
    n0->next = n1;
    n1->next = n2;
    n2->next = n3;
    n3->next = n4;
    ```

=== "C#"

    ```csharp title="linked_list.cs"
    /* 初始化链表 1 -> 3 -> 2 -> 5 -> 4 */
    // 初始化各个节点 
    ListNode n0 = new ListNode(1);
    ListNode n1 = new ListNode(3);
    ListNode n2 = new ListNode(2);
    ListNode n3 = new ListNode(5);
    ListNode n4 = new ListNode(4);
    // 构建引用指向
    n0.next = n1;
    n1.next = n2;
    n2.next = n3;
    n3.next = n4;
    ```

=== "Swift"

    ```swift title="linked_list.swift"
    /* 初始化链表 1 -> 3 -> 2 -> 5 -> 4 */
    // 初始化各个节点
    let n0 = ListNode(x: 1)
    let n1 = ListNode(x: 3)
    let n2 = ListNode(x: 2)
    let n3 = ListNode(x: 5)
    let n4 = ListNode(x: 4)
    // 构建引用指向
    n0.next = n1
    n1.next = n2
    n2.next = n3
    n3.next = n4
    ```

=== "Zig"

    ```zig title="linked_list.zig"
    // 初始化链表
    // 初始化各个节点 
    var n0 = inc.ListNode(i32){.val = 1};
    var n1 = inc.ListNode(i32){.val = 3};
    var n2 = inc.ListNode(i32){.val = 2};
    var n3 = inc.ListNode(i32){.val = 5};
    var n4 = inc.ListNode(i32){.val = 4};
    // 构建引用指向
    n0.next = &n1;
    n1.next = &n2;
    n2.next = &n3;
    n3.next = &n4;
    ```

## 4.2.1. &nbsp; 链表优点

**链表中插入与删除节点的操作效率高**。例如，如果我们想在链表中间的两个节点 `A` , `B` 之间插入一个新节点 `P` ，我们只需要改变两个节点指针即可，时间复杂度为 $O(1)$ ；相比之下，数组的插入操作效率要低得多。

![链表插入节点](linked_list.assets/linkedlist_insert_node.png)

<p align="center"> Fig. 链表插入节点 </p>

=== "Java"

    ```java title="linked_list.java"
    /* 在链表的节点 n0 之后插入节点 P */
    void insert(ListNode n0, ListNode P) {
        ListNode n1 = n0.next;
        P.next = n1;
        n0.next = P;
    }
    ```

=== "C++"

    ```cpp title="linked_list.cpp"
    /* 在链表的节点 n0 之后插入节点 P */
    void insert(ListNode *n0, ListNode *P) {
        ListNode *n1 = n0->next;
        P->next = n1;
        n0->next = P;
    }
    ```

=== "Python"

    ```python title="linked_list.py"
    def insert(n0: ListNode, P: ListNode) -> None:
        """在链表的节点 n0 之后插入节点 P"""
        n1 = n0.next
        P.next = n1
        n0.next = P
    ```

=== "Go"

    ```go title="linked_list.go"
    /* 在链表的节点 n0 之后插入节点 P */
    func insertNode(n0 *ListNode, P *ListNode) {
        n1 := n0.Next
        P.Next = n1
        n0.Next = P
    }
    ```

=== "JavaScript"

    ```javascript title="linked_list.js"
    /* 在链表的节点 n0 之后插入节点 P */
    function insert(n0, P) {
        const n1 = n0.next;
        P.next = n1;
        n0.next = P;
    }
    ```

=== "TypeScript"

    ```typescript title="linked_list.ts"
    /* 在链表的节点 n0 之后插入节点 P */
    function insert(n0: ListNode, P: ListNode): void {
        const n1 = n0.next;
        P.next = n1;
        n0.next = P;
    }
    ```

=== "C"

    ```c title="linked_list.c"
    /* 在链表的节点 n0 之后插入节点 P */
    void insert(ListNode *n0, ListNode *P) {
        ListNode *n1 = n0->next;
        P->next = n1;
        n0->next = P;
    }
    ```

=== "C#"

    ```csharp title="linked_list.cs"
    /* 在链表的节点 n0 之后插入节点 P */
    void insert(ListNode n0, ListNode P) {
        ListNode? n1 = n0.next;
        P.next = n1;
        n0.next = P;
    }
    ```

=== "Swift"

    ```swift title="linked_list.swift"
    /* 在链表的节点 n0 之后插入节点 P */
    func insert(n0: ListNode, P: ListNode) {
        let n1 = n0.next
        P.next = n1
        n0.next = P
    }
    ```

=== "Zig"

    ```zig title="linked_list.zig"
    // 在链表的节点 n0 之后插入节点 P
    fn insert(n0: ?*inc.ListNode(i32), P: ?*inc.ListNode(i32)) void {
        var n1 = n0.?.next;
        P.?.next = n1;
        n0.?.next = P;
    }
    ```

在链表中删除节点也非常方便，只需改变一个节点的指针即可。如下图所示，尽管在删除操作完成后，节点 `P` 仍然指向 `n1` ，但实际上 `P` 已经不再属于此链表，因为遍历此链表时无法访问到 `P` 。

![链表删除节点](linked_list.assets/linkedlist_remove_node.png)

<p align="center"> Fig. 链表删除节点 </p>

=== "Java"

    ```java title="linked_list.java"
    /* 删除链表的节点 n0 之后的首个节点 */
    void remove(ListNode n0) {
        if (n0.next == null)
            return;
        // n0 -> P -> n1
        ListNode P = n0.next;
        ListNode n1 = P.next;
        n0.next = n1;
    }
    ```

=== "C++"

    ```cpp title="linked_list.cpp"
    /* 删除链表的节点 n0 之后的首个节点 */
    void remove(ListNode *n0) {
        if (n0->next == nullptr)
            return;
        // n0 -> P -> n1
        ListNode *P = n0->next;
        ListNode *n1 = P->next;
        n0->next = n1;
        // 释放内存
        delete P;
    }
    ```

=== "Python"

    ```python title="linked_list.py"
    def remove(n0: ListNode) -> None:
        """删除链表的节点 n0 之后的首个节点"""
        if not n0.next:
            return
        # n0 -> P -> n1
        P = n0.next
        n1 = P.next
        n0.next = n1
    ```

=== "Go"

    ```go title="linked_list.go"
    /* 删除链表的节点 n0 之后的首个节点 */
    func removeNode(n0 *ListNode) {
        if n0.Next == nil {
            return
        }
        // n0 -> P -> n1
        P := n0.Next
        n1 := P.Next
        n0.Next = n1
    }
    ```

=== "JavaScript"

    ```javascript title="linked_list.js"
    /* 删除链表的节点 n0 之后的首个节点 */
    function remove(n0) {
        if (!n0.next) return;
        // n0 -> P -> n1
        const P = n0.next;
        const n1 = P.next;
        n0.next = n1;
    }
    ```

=== "TypeScript"

    ```typescript title="linked_list.ts"
    /* 删除链表的节点 n0 之后的首个节点 */
    function remove(n0: ListNode): void {
        if (!n0.next) {
            return;
        }
        // n0 -> P -> n1
        const P = n0.next;
        const n1 = P.next;
        n0.next = n1;
    }
    ```

=== "C"

    ```c title="linked_list.c"
    /* 删除链表的节点 n0 之后的首个节点 */
    // 注意：stdio.h 占用了 remove 关键词
    void removeNode(ListNode *n0) {
        if (!n0->next)
            return;
        // n0 -> P -> n1
        ListNode *P = n0->next;
        ListNode *n1 = P->next;
        n0->next = n1;
        // 释放内存
        free(P);
    }
    ```

=== "C#"

    ```csharp title="linked_list.cs"
    /* 删除链表的节点 n0 之后的首个节点 */
    void remove(ListNode n0) {
        if (n0.next == null)
            return;
        // n0 -> P -> n1
        ListNode P = n0.next;
        ListNode? n1 = P.next;
        n0.next = n1;
    }
    ```

=== "Swift"

    ```swift title="linked_list.swift"
    /* 删除链表的节点 n0 之后的首个节点 */
    func remove(n0: ListNode) {
        if n0.next == nil {
            return
        }
        // n0 -> P -> n1
        let P = n0.next
        let n1 = P?.next
        n0.next = n1
        P?.next = nil
    }
    ```

=== "Zig"

    ```zig title="linked_list.zig"
    // 删除链表的节点 n0 之后的首个节点
    fn remove(n0: ?*inc.ListNode(i32)) void {
        if (n0.?.next == null) return;
        // n0 -> P -> n1
        var P = n0.?.next;
        var n1 = P.?.next;
        n0.?.next = n1;
    }
    ```

## 4.2.2. &nbsp; 链表缺点

**链表访问节点效率较低**。如上节所述，数组可以在 $O(1)$ 时间下访问任意元素。然而，链表无法直接访问任意节点，这是因为系统需要从头节点出发，逐个向后遍历直至找到目标节点。例如，若要访问链表索引为 `index`（即第 `index + 1` 个）的节点，则需要向后遍历 `index` 轮。

=== "Java"

    ```java title="linked_list.java"
    /* 访问链表中索引为 index 的节点 */
    ListNode access(ListNode head, int index) {
        for (int i = 0; i < index; i++) {
            if (head == null)
                return null;
            head = head.next;
        }
        return head;
    }
    ```

=== "C++"

    ```cpp title="linked_list.cpp"
    /* 访问链表中索引为 index 的节点 */
    ListNode *access(ListNode *head, int index) {
        for (int i = 0; i < index; i++) {
            if (head == nullptr)
                return nullptr;
            head = head->next;
        }
        return head;
    }
    ```

=== "Python"

    ```python title="linked_list.py"
    def access(head: ListNode, index: int) -> ListNode | None:
        """访问链表中索引为 index 的节点"""
        for _ in range(index):
            if not head:
                return None
            head = head.next
        return head
    ```

=== "Go"

    ```go title="linked_list.go"
    /* 访问链表中索引为 index 的节点 */
    func access(head *ListNode, index int) *ListNode {
        for i := 0; i < index; i++ {
            if head == nil {
                return nil
            }
            head = head.Next
        }
        return head
    }
    ```

=== "JavaScript"

    ```javascript title="linked_list.js"
    /* 访问链表中索引为 index 的节点 */
    function access(head, index) {
        for (let i = 0; i < index; i++) {
            if (!head) {
                return null;
            }
            head = head.next;
        }
        return head;
    }
    ```

=== "TypeScript"

    ```typescript title="linked_list.ts"
    /* 访问链表中索引为 index 的节点 */
    function access(head: ListNode | null, index: number): ListNode | null {
        for (let i = 0; i < index; i++) {
            if (!head) {
                return null;
            }
            head = head.next;
        }
        return head;
    }
    ```

=== "C"

    ```c title="linked_list.c"
    /* 访问链表中索引为 index 的节点 */
    ListNode *access(ListNode *head, int index) {
        while (head && head->next && index) {
            head = head->next;
            index--;
        }
        return head;
    }
    ```

=== "C#"

    ```csharp title="linked_list.cs"
    /* 访问链表中索引为 index 的节点 */
    ListNode? access(ListNode head, int index) {
        for (int i = 0; i < index; i++) {
            if (head == null)
                return null;
            head = head.next;
        }
        return head;
    }
    ```

=== "Swift"

    ```swift title="linked_list.swift"
    /* 访问链表中索引为 index 的节点 */
    func access(head: ListNode, index: Int) -> ListNode? {
        var head: ListNode? = head
        for _ in 0 ..< index {
            if head == nil {
                return nil
            }
            head = head?.next
        }
        return head
    }
    ```

=== "Zig"

    ```zig title="linked_list.zig"
    // 访问链表中索引为 index 的节点
    fn access(node: ?*inc.ListNode(i32), index: i32) ?*inc.ListNode(i32) {
        var head = node;
        var i: i32 = 0;
        while (i < index) : (i += 1) {
            head = head.?.next;
            if (head == null) return null;
        }
        return head;
    }
    ```

**链表的内存占用较大**。链表以节点为单位，每个节点除了保存值之外，还需额外保存指针（引用）。这意味着在相同数据量的情况下，链表比数组需要占用更多的内存空间。

## 4.2.3. &nbsp; 链表常用操作

**遍历链表查找**。遍历链表，查找链表内值为 `target` 的节点，输出节点在链表中的索引。

=== "Java"

    ```java title="linked_list.java"
    /* 在链表中查找值为 target 的首个节点 */
    int find(ListNode head, int target) {
        int index = 0;
        while (head != null) {
            if (head.val == target)
                return index;
            head = head.next;
            index++;
        }
        return -1;
    }
    ```

=== "C++"

    ```cpp title="linked_list.cpp"
    /* 在链表中查找值为 target 的首个节点 */
    int find(ListNode *head, int target) {
        int index = 0;
        while (head != nullptr) {
            if (head->val == target)
                return index;
            head = head->next;
            index++;
        }
        return -1;
    }
    ```

=== "Python"

    ```python title="linked_list.py"
    def find(head: ListNode, target: int) -> int:
        """在链表中查找值为 target 的首个节点"""
        index = 0
        while head:
            if head.val == target:
                return index
            head = head.next
            index += 1
        return -1
    ```

=== "Go"

    ```go title="linked_list.go"
    /* 在链表中查找值为 target 的首个节点 */
    func findNode(head *ListNode, target int) int {
        index := 0
        for head != nil {
            if head.Val == target {
                return index
            }
            head = head.Next
            index++
        }
        return -1
    }
    ```

=== "JavaScript"

    ```javascript title="linked_list.js"
    /* 在链表中查找值为 target 的首个节点 */
    function find(head, target) {
        let index = 0;
        while (head !== null) {
            if (head.val === target) {
                return index;
            }
            head = head.next;
            index += 1;
        }
        return -1;
    }
    ```

=== "TypeScript"

    ```typescript title="linked_list.ts"
    /* 在链表中查找值为 target 的首个节点 */
    function find(head: ListNode | null, target: number): number {
        let index = 0;
        while (head !== null) {
            if (head.val === target) {
                return index;
            }
            head = head.next;
            index += 1;
        }
        return -1;
    }
    ```

=== "C"

    ```c title="linked_list.c"
    /* 在链表中查找值为 target 的首个节点 */
    int find(ListNode *head, int target) {
        int index = 0;
        while (head) {
            if (head->val == target)
                return index;
            head = head->next;
            index++;
        }
        return -1;
    }
    ```

=== "C#"

    ```csharp title="linked_list.cs"
    /* 在链表中查找值为 target 的首个节点 */
    int find(ListNode head, int target) {
        int index = 0;
        while (head != null) {
            if (head.val == target)
                return index;
            head = head.next;
            index++;
        }
        return -1;
    }
    ```

=== "Swift"

    ```swift title="linked_list.swift"
    /* 在链表中查找值为 target 的首个节点 */
    func find(head: ListNode, target: Int) -> Int {
        var head: ListNode? = head
        var index = 0
        while head != nil {
            if head?.val == target {
                return index
            }
            head = head?.next
            index += 1
        }
        return -1
    }
    ```

=== "Zig"

    ```zig title="linked_list.zig"
    // 在链表中查找值为 target 的首个节点
    fn find(node: ?*inc.ListNode(i32), target: i32) i32 {
        var head = node;
        var index: i32 = 0;
        while (head != null) {
            if (head.?.val == target) return index;
            head = head.?.next;
            index += 1;
        }
        return -1;
    }
    ```

## 4.2.4. &nbsp; 常见链表类型

**单向链表**。即上述介绍的普通链表。单向链表的节点包含值和指向下一节点的指针（引用）两项数据。我们将首个节点称为头节点，将最后一个节点成为尾节点，尾节点指向 $\text{null}$ 。

**环形链表**。如果我们令单向链表的尾节点指向头节点（即首尾相接），则得到一个环形链表。在环形链表中，任意节点都可以视作头节点。

**双向链表**。与单向链表相比，双向链表记录了两个方向的指针（引用）。双向链表的节点定义同时包含指向后继节点（下一节点）和前驱节点（上一节点）的指针。相较于单向链表，双向链表更具灵活性，可以朝两个方向遍历链表，但相应地也需要占用更多的内存空间。

=== "Java"

    ```java title=""
    /* 双向链表节点类 */
    class ListNode {
        int val;        // 节点值
        ListNode next;  // 指向后继节点的指针（引用）
        ListNode prev;  // 指向前驱节点的指针（引用）
        ListNode(int x) { val = x; }  // 构造函数
    }
    ```

=== "C++"

    ```cpp title=""
    /* 双向链表节点结构体 */
    struct ListNode {
        int val;         // 节点值
        ListNode *next;  // 指向后继节点的指针（引用）
        ListNode *prev;  // 指向前驱节点的指针（引用）
        ListNode(int x) : val(x), next(nullptr), prev(nullptr) {}  // 构造函数
    };
    ```

=== "Python"

    ```python title=""
    class ListNode:
        """双向链表节点类"""
        def __init__(self, val: int):
            self.val: int = val                   # 节点值
            self.next: Optional[ListNode] = None  # 指向后继节点的指针（引用）
            self.prev: Optional[ListNode] = None  # 指向前驱节点的指针（引用）
    ```

=== "Go"

    ```go title=""
    /* 双向链表节点结构体 */
    type DoublyListNode struct {
        Val  int             // 节点值
        Next *DoublyListNode // 指向后继节点的指针（引用）
        Prev *DoublyListNode // 指向前驱节点的指针（引用）
    }
    
    // NewDoublyListNode 初始化
    func NewDoublyListNode(val int) *DoublyListNode {
        return &DoublyListNode{
            Val:  val,
            Next: nil,
            Prev: nil,
        }
    }
    ```

=== "JavaScript"

    ```javascript title=""
    /* 双向链表节点类 */
    class ListNode {
        val;
        next;
        prev;
        constructor(val, next) {
            this.val = val  ===  undefined ? 0 : val;        // 节点值
            this.next = next  ===  undefined ? null : next;  // 指向后继节点的指针（引用）
            this.prev = prev  ===  undefined ? null : prev;  // 指向前驱节点的指针（引用）
        }
    }
    ```

=== "TypeScript"

    ```typescript title=""
    /* 双向链表节点类 */
    class ListNode {
        val: number;
        next: ListNode | null;
        prev: ListNode | null;
        constructor(val?: number, next?: ListNode | null, prev?: ListNode | null) {
            this.val = val  ===  undefined ? 0 : val;        // 节点值
            this.next = next  ===  undefined ? null : next;  // 指向后继节点的指针（引用）
            this.prev = prev  ===  undefined ? null : prev;  // 指向前驱节点的指针（引用）
        }
    }
    ```

=== "C"

    ```c title=""
    /* 双向链表节点结构体 */
    struct ListNode {
        int val;               // 节点值
        struct ListNode *next; // 指向后继节点的指针（引用）
        struct ListNode *prev; // 指向前驱节点的指针（引用）
    };

    typedef struct ListNode ListNode;

    /* 构造函数 */
    ListNode *newListNode(int val) {
        ListNode *node, *next;
        node = (ListNode *) malloc(sizeof(ListNode));
        node->val = val;
        node->next = NULL;
        node->prev = NULL;
        return node;
    }
    ```

=== "C#"

    ```csharp title=""
    /* 双向链表节点类 */
    class ListNode {
        int val;        // 节点值
        ListNode next;  // 指向后继节点的指针（引用）
        ListNode prev;  // 指向前驱节点的指针（引用）
        ListNode(int x) => val = x;  // 构造函数
    }
    ```

=== "Swift"

    ```swift title=""
    /* 双向链表节点类 */
    class ListNode {
        var val: Int // 节点值
        var next: ListNode? // 指向后继节点的指针（引用）
        var prev: ListNode? // 指向前驱节点的指针（引用）

        init(x: Int) { // 构造函数
            val = x
        }
    }
    ```

=== "Zig"

    ```zig title=""
    // 双向链表节点类
    pub fn ListNode(comptime T: type) type {
        return struct {
            const Self = @This();
            
            val: T = 0, // 节点值
            next: ?*Self = null, // 指向后继节点的指针（引用）
            prev: ?*Self = null, // 指向前驱节点的指针（引用）

            // 构造函数
            pub fn init(self: *Self, x: i32) void {
                self.val = x;
                self.next = null;
                self.prev = null;
            }
        };
    }
    ```

![常见链表种类](linked_list.assets/linkedlist_common_types.png)

<p align="center"> Fig. 常见链表种类 </p>