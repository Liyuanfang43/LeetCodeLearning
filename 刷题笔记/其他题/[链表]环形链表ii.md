# 环形链表ii

给定一个链表的头节点  head ，返回链表开始入环的第一个节点。 如果链表无环，则返回 null。

如果链表中有某个节点，可以通过连续跟踪 next 指针再次到达，则链表中存在环。 为了表示给定链表中的环，评测系统内部使用整数 pos 来表示链表尾连接到链表中的位置（索引从 0 开始）。如果 pos 是 -1，则在该链表中没有环。注意：pos 不作为参数进行传递，仅仅是为了标识链表的实际情况。

不允许修改 链表。

解法：
1）哈希表，保存已遍历过的节点。
2）快慢指针，当快慢指针相遇时，再从head拉一个指针与slow指针一起向前移动，当二者相遇即为入环点。

解释：如下图所示，设链表中环外部分的长度为 a。slow 指针进入环后，又走了 b 的距离与 fast 相遇。此时，fast 指针已经走完了环的 n 圈，因此它走过的总距离为 a+n(b+c)+b=a+(n+1)b+nc。根据题意，任意时刻，fast 指针走过的距离都为 slow 指针的 2 倍。因此，我们有

a+(n+1)b+nc=2(a+b)⟹a=c+(n−1)(b+c)
有了 a=c+(n−1)(b+c) 的等量关系，我们会发现：从相遇点到入环点的距离加上 n−1 圈的环长，恰好等于从链表头部到入环点的距离。

作者：力扣官方题解
链接：https://leetcode.cn/problems/linked-list-cycle-ii/solutions/441131/huan-xing-lian-biao-ii-by-leetcode-solution/
来源：力扣（LeetCode）
著作权归作者所有。商业转载请联系作者获得授权，非商业转载请注明出处。

````py
class Solution:
    def detectCycle(self, head: Optional[ListNode]) -> Optional[ListNode]:
        # # 空间复杂度O(n)解法
        # node_set = set()
        # while head is not None:
        #     if head not in node_set:
        #         node_set.add(head)
        #     else:
        #         return head
        #     head = head.next
        # return None

        # 空间复杂度O(1)解法，需要推导等价关系
        fast, slow = head, head
        while fast is not None:
            slow = slow.next
            if fast.next is not None:
                fast = fast.next.next
            else:
                return None
            if fast == slow:
                base = head
                while base != slow:
                    base = base.next
                    slow = slow.next
                return base
        return None
````