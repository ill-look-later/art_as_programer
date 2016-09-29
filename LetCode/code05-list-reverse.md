# 单向链表的反转

假设有链表` 0->1->2->3->4->5->6->7->8->9->null` 那么反转的结果就是`9->8->7->6->5->4->3->2->1->0->null` 
```
struct Node {
	int data;
	Node* next;
};

Node* reverse(Node* head) {
	Node* next_node = head->next;
	if (next_node == NULL) {
		head->next		
	}

}
```