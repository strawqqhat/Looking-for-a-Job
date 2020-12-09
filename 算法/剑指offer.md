#### 剑指offer

[TOC]



#### 03 数组中重复的数字

```java
class Solution {
    public int findRepeatNumber(int[] nums) {
        int temp;
        for(int i=0; i<nums.length; i++){
            while(nums[i] != i){
                if(nums[i] == nums[nums[i]]){
                    return nums[i];
                }
                temp = nums[i];
                nums[i] = nums[temp];
                nums[temp] = temp;
            }
        }
        return -1;
    }
}
```



#### 04 二维数组中的查找

```java
class Solution {
    public boolean findNumberIn2DArray(int[][] matrix, int target) {
        int i = matrix.length-1;
        int j = 0;
        for(; i>=0&&j<matrix[0].length;){
            if(matrix[i][j] == target){
                return true;
            }
            else if(matrix[i][j]<target){
                j++;
            }else{
                i--;
            }
        }
        return false;
    }
}
```



####  05 替换空格

```java
class Solution {
    public String replaceSpace(String s) {
        StringBuilder sb = new StringBuilder();
        for(int i=0; i<s.length(); i++){
            if(s.charAt(i) != ' '){
                sb.append(s.charAt(i));
            }else{
                sb.append("%20");
            }
        }
        return sb.toString();
    }
}
```



#### 06 从尾到头打印链表

```java
class Solution {
    public int[] reversePrint(ListNode head) {
        int count = 0;
        Stack<ListNode> stack = new Stack<>();
        while(head != null){
            stack.push(head);
            count++;
            head=head.next;
        }
        int[] res = new int[count];
        int i = 0;
        while(!stack.isEmpty()){
            res[i++] = stack.pop().val;
        }
        return res;
    }
}
```

```java
class Solution {
    ArrayList<Integer> tmp = new ArrayList<>();
    public int[] reversePrint(ListNode head) {
        recur(head);
        int[] res = new int[tmp.size()];
        for(int i=0; i<tmp.size(); i++){
            res[i] = tmp.get(i);
        }
        return res;
    }
    private void recur(ListNode head){
        if(head == null){
            return;
        }
        recur(head.next);
        tmp.add(head.val);
    }
}
```



#### 07 重建二叉树

```java
/**
 * Definition for a binary tree node.
 * public class TreeNode {
 *     int val;
 *     TreeNode left;
 *     TreeNode right;
 *     TreeNode(int x) { val = x; }
 * }
 */
class Solution {
    public TreeNode buildTree(int[] preorder, int[] inorder) {
        TreeNode root = reConstructTree(preorder,0,preorder.length-1,inorder,0,inorder.length-1);
        return root;
    }

    public TreeNode reConstructTree(int[] preorder,int startPre,int endPre,int[] inorder,int startIn,int endIn){
        if(startPre > endPre || startIn > endIn){
            return null;
        }
        TreeNode root = new TreeNode(preorder[startPre]);
        for(int i=startIn; i<=endIn; i++){
            if(inorder[i] == preorder[startPre]){
                root.left = reConstructTree(preorder,startPre+1,startPre+i-startIn,inorder,startIn,i-1);
                root.right = reConstructTree(preorder,startPre+i-startIn+1,endPre,inorder,i+1,endIn);
            }
        }
        return root;
    }
}
```



#### 09 用两个栈实现队列

```java
class CQueue {

    Stack<Integer> s1;
    Stack<Integer> s2;

    public CQueue() {
        this.s1 = new Stack<Integer>();
        this.s2 = new Stack<Integer>();
    }
    
    public void appendTail(int value) {
        s2.push(value);
    }
    
    public int deleteHead() {
        if(s1.isEmpty()){
            while(!s2.empty()){
                s1.push(s2.pop());
            }
        }
        if(!s1.empty()){
            return s1.pop();
        }
        return -1;
    }
}

/**
 * Your CQueue object will be instantiated and called as such:
 * CQueue obj = new CQueue();
 * obj.appendTail(value);
 * int param_2 = obj.deleteHead();
 */
```



#### 10-I 斐波那契数列

```java
class CQueue {

    Stack<Integer> s1;
    Stack<Integer> s2;

    public CQueue() {
        this.s1 = new Stack<Integer>();
        this.s2 = new Stack<Integer>();
    }
    
    public void appendTail(int value) {
        s2.push(value);
    }
    
    public int deleteHead() {
        if(s1.isEmpty()){
            while(!s2.empty()){
                s1.push(s2.pop());
            }
        }
        if(!s1.empty()){
            return s1.pop();
        }
        return -1;
    }
}

/**
 * Your CQueue object will be instantiated and called as such:
 * CQueue obj = new CQueue();
 * obj.appendTail(value);
 * int param_2 = obj.deleteHead();
 */
```



#### 10-II 青蛙跳台阶

```java
class Solution {
    public int numWays(int n) {
        if(n == 0){
            return 1;
        }
        if(n == 1){
            return 1;
        }
        int left = 1, right = 1;
        for(int i=2; i<=n; i++){
            int sum = (left + right)%1000000007;
            left = right;
            right = sum;
        }
        return right;
    }
}
```



#### 11 旋转数组的最小数字

```java
class Solution {
    public int minArray(int[] numbers) {
        int left = 0, right = numbers.length-1;
        
        while(left <= right){
            int mid = left + (right-left)/2;
            if(numbers[mid] > numbers[right]){
                left = mid+1;
            }else if(numbers[mid] == numbers[right]){
                right = right-1;
            }else{
                right = mid;
            }
        }
        return numbers[left];
    }
}
```



#### 12 矩阵中的路径

```java
class Solution {
    public boolean exist(char[][] board, String word) {
        char[] words = word.toCharArray();
        for(int i=0; i<board.length; i++){
            for(int j=0; j<board[0].length; j++){
                if(dfs(board, words, i, j, 0)){
                    return true;
                }
            }
        }
        return false;
    }

    private boolean dfs(char[][] board,char[] words,int i, int j, int k){
        if(i>=board.length||i<0||j>=board[0].length||j<0||board[i][j]!=words[k]){
            return false;
        }
        if(k == words.length-1){
            return true;
        }
        board[i][j] = '\0';
        boolean res = dfs(board,words,i+1,j,k+1)||dfs(board,words,i,j+1,k+1)||dfs(board,words,i-1,j,k+1)||dfs(board,words,i,j-1,k+1);
        board[i][j] = words[k];
        return res;
    }
}
```



#### 13 机器人的运动范围

```java
class Solution {
    boolean[][] visited;
    int m,n,k;
    public int movingCount(int m, int n, int k) {
        this.m = m;
        this.n = n;
        this.k = k;
        this.visited = new boolean[m][n];
        return dfs(0,0,0,0);
    }
    private int dfs(int i,int j,int si, int sj){
        if(i>=m||j>=n||visited[i][j]==true||k<si+sj){
            return 0;
        }
        visited[i][j] = true;
        return 1+dfs(i+1,j,(i+1)%10!=0?si+1:si-8,sj)+dfs(i,j+1,si,(j+1)%10!=0?sj+1:sj-8);
    }
}
```



#### 14-I 剪绳子

```java
class Solution {
    public int cuttingRope(int n) {
        if(n <= 3){
            return n-1;
        }
        int a = n/3;
        int b = n%3;
        if(b==0){
            return (int)Math.pow(3,a);
        }
        if(b==1){
            return (int)Math.pow(3,a-1)*4;
        }
        return (int)Math.pow(3,a)*2;
    }
}
```



#### 14-II 剪绳子2

```java
class Solution {
    public int cuttingRope(int n) {
        if(n<=3){
            return n-1;
        }
        int b = n%3;
        int p = 1000000007;
        long rem = 1;
        for(int a = n/3-1; a>0; a--){
            rem = (rem*3)%p;
        }
        if(b==0){
            return (int)(rem*3%p);
        }
        if(b==1){
            return (int)(rem*4%p);
        }
        return (int)(rem*6%p);
    }
}
```



#### 15 二进制中1的个数

```java
public class Solution {
    // you need to treat n as an unsigned value
    public int hammingWeight(int n) {
        int count=0;
        while(n!=0){
            count++;
            n = n&(n-1);
        }
        return count;
    }
}
```



#### 16 数值的整数次方

```java
class Solution {
    public double myPow(double x, int n) {
        if(x==0){
            return 0;
        }
        long b = n;
        double res = 1.0;
        if(n < 0){
            b = -b;
            x = 1/x;
        }
        while(b > 0){
            if((b&1)==1){
                res*=x;
            }
            x*=x;
            b>>=1;
        }
        return res;
    }
}
```



#### 17 打印从1到最大的n位数

```java
class Solution {
    int[] res;
    char[] num;
    int count = 0, n;
    public int[] printNumbers(int n) {
        this.n = n;
        res = new int[(int)Math.pow(10,n)-1];
        num = new char[n];
        dfs(0);
        return res;
    }

    private void dfs(int x){
        if(x == n){
            String tmp = String.valueOf(num);
            int curNum = Integer.parseInt(tmp);
            if(curNum != 0){
                res[count++] = curNum;
            }
            return;
        }
        for(char i='0'; i<='9'; i++){
            num[x] = i;
            dfs(x+1);
        }
    }
}
```



#### 18 删除链表的节点

```java
/**
 * Definition for singly-linked list.
 * public class ListNode {
 *     int val;
 *     ListNode next;
 *     ListNode(int x) { val = x; }
 * }
 */
class Solution {
    public ListNode deleteNode(ListNode head, int val) {
        if(head == null){
            return null;
        }
        if(head.val == val){
            return head.next;
        }
        ListNode cur = head;
        while(cur.next!=null&&cur.next.val!=val){
            cur = cur.next;
        }
        cur.next = cur.next.next;
        return head;
    }
}
```



#### 19 正则表达式匹配

```java
class Solution {
    public boolean isMatch(String s, String p) {
        if(s==null&&p==null){
            return false;
        }
        int rows=s.length(), columns=p.length();
        boolean[][] dp = new boolean[rows+1][columns+1];
        dp[0][0]=true;
        for(int j=1; j<=columns; j++){
            if(p.charAt(j-1)=='*'&&dp[0][j-2]){
                dp[0][j] = true;
            }
        }
        for(int i=1; i<=rows; i++){
            for(int j=1; j<=columns; j++){
                char nows = s.charAt(i-1);
                char nowp = p.charAt(j-1);
                if(nowp == nows){
                    dp[i][j]=dp[i-1][j-1];
                }else{
                    if(nowp == '.'){
                        dp[i][j] = dp[i-1][j-1];
                    }
                    else if(nowp == '*'){
                        if(j>=2){
                            char nowpLast = p.charAt(j-2);
                            if(nowpLast==nows||nowpLast=='.'){
                                dp[i][j] = dp[i][j-1]||dp[i-1][j];
                            }
                            dp[i][j]=dp[i][j]||dp[i][j-2];
                        }
                    }else{
                        dp[i][j] = false;
                    }
                }
            }
        }
        return dp[rows][columns];
    }
}
```



#### 20 表示数值的字符串

```java
class Solution {
    public boolean isNumber(String s) {
        if(s==null||s.length()==0){
            return false;
        }
        boolean isNum = false, isDot = false, ise_or_E = false;
        char[] str = s.trim().toCharArray();
        for(int i=0; i<str.length; i++){
            if(str[i]>='0'&&str[i]<='9') isNum=true;
            else if(str[i]=='.') {
                if(isDot||ise_or_E) return false;
                isDot=true;
            }else if(str[i]=='e'||str[i]=='E'){
                if(!isNum||ise_or_E) return false;
                ise_or_E=true;
                isNum=false;
            }else if(str[i]=='-'||str[i]=='+'){
                if(i!=0&&str[i-1]!='e'&&str[i-1]!='E') return false;
            }else{
                return false;
            }
        }
        return isNum;
    }
}
```



 #### 21 调整数组顺序使奇数位于偶数前面

```java
class Solution {
    public int[] exchange(int[] nums) {
        int i=0, j=nums.length-1;
        while(i < j){
            while(i<j&&(nums[i]&1)==1){
                i++;
            }
            while(i<j&&(nums[j]&1)==0){
                j--;
            }
            int temp = nums[i];
            nums[i] = nums[j];
            nums[j] = temp;
        }
        return nums;
    }
}
```



#### 22 链表中倒数第k个节点

```java
/**
 * Definition for singly-linked list.
 * public class ListNode {
 *     int val;
 *     ListNode next;
 *     ListNode(int x) { val = x; }
 * }
 */
class Solution {
    public ListNode getKthFromEnd(ListNode head, int k) {
        if(head == null){
            return null;
        }
        int len = 0;
        ListNode left = head;
        ListNode right = head;
        while(head != null){
            len++;
            head = head.next;
        }
        if(len <= k){
            return left; 
        }
        for(int i=0; i<k; i++){
            right = right.next;
        }
        while(right != null){
            left = left.next;
            right = right.next;
        }
        return left;
    }
}
```



#### 24 反转链表

```java
/**
 * Definition for singly-linked list.
 * public class ListNode {
 *     int val;
 *     ListNode next;
 *     ListNode(int x) { val = x; }
 * }
 */
class Solution {
    public ListNode reverseList(ListNode head) {
        if(head == null || head.next == null){
            return head;
        }
        ListNode last = reverseList(head.next);
        head.next.next = head;
        head.next = null;
        return last;
    }
}
```



#### 25 合并两个排序的链表

```java
/**
 * Definition for singly-linked list.
 * public class ListNode {
 *     int val;
 *     ListNode next;
 *     ListNode(int x) { val = x; }
 * }
 */
class Solution {
    public ListNode mergeTwoLists(ListNode l1, ListNode l2) {
        ListNode res = new ListNode(0);
        ListNode ans = res;
        ListNode p = l1, q = l2;
        while(p != null && q != null){
            if(p.val < q.val){
                res.next = new ListNode(p.val);
                p = p.next;
                res = res.next;
            }else{
                res.next = new ListNode(q.val);
                q = q.next;
                res = res.next;
            }
        }
        if(p != null){
            res.next = p;
        }
        if(q != null){
            res.next = q;
        }
        return ans.next;
    }
}
```



#### 26 树的子结构

```java
/**
 * Definition for a binary tree node.
 * public class TreeNode {
 *     int val;
 *     TreeNode left;
 *     TreeNode right;
 *     TreeNode(int x) { val = x; }
 * }
 */
class Solution {
    public boolean isSubStructure(TreeNode A, TreeNode B) {
        if(A == null || B == null){
            return false;
        }
        return recur(A,B)||isSubStructure(A.left,B)||isSubStructure(A.right,B);
    }
    private boolean recur(TreeNode A, TreeNode B){
        if(B == null){
            return true;
        }
        if(A == null || A.val != B.val){
            return false;
        }
        return recur(A.left, B.left)&&recur(A.right, B.right);
    }
}
```



#### 27 二叉树的映像

```java
/**
 * Definition for a binary tree node.
 * public class TreeNode {
 *     int val;
 *     TreeNode left;
 *     TreeNode right;
 *     TreeNode(int x) { val = x; }
 * }
 */
class Solution {
    public TreeNode mirrorTree(TreeNode root) {
        if(root == null){
            return null;
        }
        TreeNode temp = root.left;
        root.left = mirrorTree(root.right);
        root.right = mirrorTree(temp);
        return root;
    }
}
```



#### 28 对称的二叉树

```java
/**
 * Definition for a binary tree node.
 * public class TreeNode {
 *     int val;
 *     TreeNode left;
 *     TreeNode right;
 *     TreeNode(int x) { val = x; }
 * }
 */
class Solution {
    public boolean isSymmetric(TreeNode root) {
        if(root == null){
            return true;
        }
        return areSymmetric(root.left, root.right);
    }
    private boolean areSymmetric(TreeNode left, TreeNode right){
        if(left == null && right == null){
            return true;
        }
        if(left == null){
            return false;
        }
        if(right == null){
            return false;
        }
        if(left.val != right.val){
            return false;
        }
        return areSymmetric(left.right, right.left)&&areSymmetric(left.left,right.right);
    }
}
```



#### 29 顺时针打印矩阵

```java
class Solution {
    public int[] spiralOrder(int[][] matrix) {
        if(matrix.length == 0){
            return new int[0];
        }
        int l=0,r=matrix[0].length-1,t=0,b=matrix.length-1,count=0;
        int[] res = new int[(r+1)*(b+1)];
        while(true){
            for(int i=l; i<=r; i++){
                res[count++] = matrix[t][i];
            }
            if(++t>b){
                break;
            }
            for(int i=t; i<=b; i++){
                res[count++] = matrix[i][r];
            }
            if(l>--r){
                break;
            }
            for(int i=r; i>=l; i--){
                res[count++] = matrix[b][i];
            }
            if(t>--b){
                break;
            }
            for(int i=b; i>=t; i--){
                res[count++] = matrix[i][l];
            }
            if(++l>r){
                break;
            }
        }
        return res;
    }
}
```



#### 30 包含min函数的栈

```java
class MinStack {

    Stack<Integer> stack1;
    Stack<Integer> stack2;
    /** initialize your data structure here. */
    public MinStack() {
        this.stack1 = new Stack<>();
        this.stack2 = new Stack<>();
    }
    
    public void push(int x) {
        stack1.push(x);
        if(stack2.empty()||stack2.peek()>=x){
            stack2.add(x);
        }
    }
    
    public void pop() {
        if(stack1.peek().equals(stack2.peek())){
            stack2.pop();
        }
        stack1.pop();
    }
    
    public int top() {
        return stack1.peek();
    }
    
    public int min() {
        return stack2.peek();
    }
}

/**
 * Your MinStack object will be instantiated and called as such:
 * MinStack obj = new MinStack();
 * obj.push(x);
 * obj.pop();
 * int param_3 = obj.top();
 * int param_4 = obj.min();
 */
```



#### 31 栈的压入、弹出序列

```java
class Solution {
    public boolean validateStackSequences(int[] pushed, int[] popped) {
        Stack<Integer> stack = new Stack<>();
        for(int i=0,j=0; i<pushed.length;){
            stack.push(pushed[i++]);
            while(!stack.isEmpty()&&stack.peek()==popped[j]){
                stack.pop();
                j++;
            }
        }
        return stack.isEmpty();
    }
}
```



#### 32-I 从上到下打印二叉树

```java
/**
 * Definition for a binary tree node.
 * public class TreeNode {
 *     int val;
 *     TreeNode left;
 *     TreeNode right;
 *     TreeNode(int x) { val = x; }
 * }
 */
class Solution {
    public int[] levelOrder(TreeNode root) {
        List<Integer> list = new ArrayList<>();
        Queue<TreeNode> queue = new LinkedList<>();
        if(root == null){
            return new int[0];
        }
        queue.offer(root);
        while(!queue.isEmpty()){
            int size = queue.size();
            for(int i=0; i<size; i++){
                TreeNode node = queue.poll();
                list.add(node.val);
                if(node.left != null){
                    queue.offer(node.left);
                }
                if(node.right != null){
                    queue.offer(node.right);
                }
            }
        }
        int[] res = new int[list.size()];
        for(int i=0; i<list.size(); i++){
            res[i] = list.get(i);
        }
        return res;
    }
}
```



#### 32-II 从上到下打印二叉树II

```java
/**
 * Definition for a binary tree node.
 * public class TreeNode {
 *     int val;
 *     TreeNode left;
 *     TreeNode right;
 *     TreeNode(int x) { val = x; }
 * }
 */
class Solution {
    public List<List<Integer>> levelOrder(TreeNode root) {
        Queue<TreeNode> q = new LinkedList<TreeNode>();
        if(root != null) q.add(root);
        List<List<Integer>> res = new ArrayList<>();
        while(!q.isEmpty()){
            int size = q.size();
            List<Integer> temp = new ArrayList<>();
            for(int i=0; i<size; i++){
                TreeNode node = q.remove();
                if(node.left != null){
                    q.add(node.left);
                }
                if(node.right != null){
                    q.add(node.right);
                }
                temp.add(node.val);
            }
            res.add(temp);
        }
        return res;
    }
}
```



#### 32-III 从上到下打印二叉树III

```java
/**
 * Definition for a binary tree node.
 * public class TreeNode {
 *     int val;
 *     TreeNode left;
 *     TreeNode right;
 *     TreeNode(int x) { val = x; }
 * }
 */
class Solution {
    public List<List<Integer>> levelOrder(TreeNode root) {
        Queue<TreeNode> queue = new LinkedList<>();
        List<List<Integer>> res = new ArrayList<>();
        if(root != null){
            queue.add(root);
        }
        while(!queue.isEmpty()){
            LinkedList<Integer> tmp = new LinkedList<>();
            for(int i=queue.size(); i>0; i--){
                TreeNode node = queue.poll();
                if(res.size()%2==0){
                    tmp.addLast(node.val);
                }else{
                    tmp.addFirst(node.val);
                }
                if(node.left != null){
                    queue.add(node.left);
                }
                if(node.right != null){
                    queue.add(node.right);
                }
                
            }
            res.add(tmp);
        }
        return res;
    }
}
```



#### 33 二叉搜索树的后序遍历序列

```java
class Solution {
    public boolean verifyPostorder(int[] postorder) {
        if(postorder.length == 0){
            return true;
        }
        return judge(postorder,0,postorder.length-1);
    }
    private boolean judge(int[] postorder, int left, int right){
        if(left >= right){
            return true;
        }
        int key = postorder[right];
        int i = 0;
        for(i=left; i<right; i++){
            if(postorder[i] > key){
                break;
            }
        }
        for(int j=i; j<right; j++){
            if(postorder[j] < key){
                return false;
            }
        }
        return judge(postorder,left,i-1)&&judge(postorder,i,right-1);
    }
}
```



#### 34 二叉树中和为某一值的路径

```java
/**
 * Definition for a binary tree node.
 * public class TreeNode {
 *     int val;
 *     TreeNode left;
 *     TreeNode right;
 *     TreeNode(int x) { val = x; }
 * }
 */
class Solution {
    List<List<Integer>> res = new LinkedList<>();
    LinkedList<Integer> path = new LinkedList<>();
    public List<List<Integer>> pathSum(TreeNode root, int sum) {
        if(root == null){
            return res;
        }
        path.addLast(root.val);
        sum -= root.val;
        if(sum == 0&&root.left==null&&root.right==null){
            res.add(new LinkedList<>(path));
        }
        pathSum(root.left, sum);
        pathSum(root.right, sum);
        path.removeLast();
        return res;
    }
}
```



#### 35 复杂链表的复制

```java
/*
// Definition for a Node.
class Node {
    int val;
    Node next;
    Node random;

    public Node(int val) {
        this.val = val;
        this.next = null;
        this.random = null;
    }
}
*/
class Solution {
    public Node copyRandomList(Node head) {
        if(head == null){
            return null;
        }
        Node cur = head;
        while(cur != null){
            Node tmp = new Node(cur.val);
            tmp.next = cur.next;
            cur.next = tmp;
            cur = tmp.next;
        }
        cur = head;
        while(cur != null){
            if(cur.random != null){
                cur.next.random = cur.random.next;
            }
            cur = cur.next.next;
        }
        cur = head.next;
        Node pre = head, res = head.next;
        while(cur.next != null){
            pre.next = pre.next.next;
            cur.next = cur.next.next;
            pre = pre.next;
            cur = cur.next;
        }
        pre.next = null;
        return res;
    }
}
```



#### 36 二叉搜索树与双向链表

```java
/*
// Definition for a Node.
class Node {
    public int val;
    public Node left;
    public Node right;

    public Node() {}

    public Node(int _val) {
        val = _val;
    }

    public Node(int _val,Node _left,Node _right) {
        val = _val;
        left = _left;
        right = _right;
    }
};
*/
class Solution {
    Node pre, head;
    public Node treeToDoublyList(Node root) {
        if(root == null){
            return null;
        }
        dfs(root);
        head.left = pre;
        pre.right = head;
        return head;
    }

    private void dfs(Node cur){
        if(cur == null) return;
        dfs(cur.left);
        if(pre != null){
            pre.right = cur;
        }else{
            head = cur;
        }
        cur.left = pre;
        pre = cur;
        dfs(cur.right);
    }
}
```



#### 37 序列化二叉树

```java
/**
 * Definition for a binary tree node.
 * public class TreeNode {
 *     int val;
 *     TreeNode left;
 *     TreeNode right;
 *     TreeNode(int x) { val = x; }
 * }
 */
public class Codec {

    // Encodes a tree to a single string.
    public String serialize(TreeNode root) {
        if(root == null){
            return "[]";
        }
        StringBuilder res = new StringBuilder("[");
        Queue<TreeNode> queue = new LinkedList<>();
        queue.add(root);
        while(!queue.isEmpty()) {
            TreeNode node = queue.poll();
            if(node != null) {
                res.append(node.val + ",");
                queue.add(node.left);
                queue.add(node.right);
            }
            else res.append("null,");
        }
        res.deleteCharAt(res.length()-1);
        res.append("]");
        return res.toString();
    }

    // Decodes your encoded data to tree.
    public TreeNode deserialize(String data) {
        if(data.equals("[]")) return null;
        String[] vals = data.substring(1, data.length() - 1).split(",");
        TreeNode root = new TreeNode(Integer.parseInt(vals[0]));
        Queue<TreeNode> queue = new LinkedList<>();
        queue.add(root);
        int i = 1;
        while(!queue.isEmpty()){
            TreeNode node = queue.poll();
            if(!vals[i].equals("null")){
                node.left = new TreeNode(Integer.parseInt(vals[i]));
                queue.add(node.left);
            }
            i++;
            if(!vals[i].equals("null")){
                node.right = new TreeNode(Integer.parseInt(vals[i]));
                queue.add(node.right);
            }
            i++;
        }
        return root;
    }
}

// Your Codec object will be instantiated and called as such:
// Codec codec = new Codec();
// codec.deserialize(codec.serialize(root));
```



#### 38 字符串的序列

```java
class Solution {
    List<String> res = new LinkedList<>();
    char[] c;
    public String[] permutation(String s) {
        c = s.toCharArray();
        dfs(0);
        return res.toArray(new String[res.size()]);
    }
    private void dfs(int x){
        if(x == c.length-1){
            res.add(String.valueOf(c));
            return;
        }
        HashSet<Character> set = new HashSet<>();
        for(int i=x; i<c.length; i++){
            if(set.contains(c[i])) continue;
            set.add(c[i]);
            swap(i, x);
            dfs(x+1);
            swap(i, x);
        }

    }
    private void swap(int a, int b){
        char tmp = c[a];
        c[a] = c[b];
        c[b] = tmp;
    }
}
```



#### 39 数组中出现次数超过一半的数字

```java
class Solution {
    public int majorityElement(int[] nums) {
        int x = 0, votes = 0;
        for(int num : nums){
            if(votes == 0){
                x = num;
            }
            votes += (num==x?1:-1);
        }
        return x;
    }
}
```



#### 40 最小的k个数

```java
class Solution {
    public int[] getLeastNumbers(int[] arr, int k) {
        Queue<Integer> pq = new PriorityQueue<>(k,new Comparator<Integer>(){
            public int compare(Integer a, Integer b){
                return b-a;
            }
        });
        if(arr.length==0 || k == 0){
            return new int[0];
        }
        for(int num : arr){
            pq.offer(num);
        }
        int[] res = new int[arr.length];
        int index = 0;
        for(int num : pq){
            res[index++] = num;
        }
        return res;
    }
}
```



#### 41 数据流中的中位数

```java
class MedianFinder {

    Queue<Integer> A, B;
    /** initialize your data structure here. */
    public MedianFinder() {
        A = new PriorityQueue<>();
        B = new PriorityQueue<>((x,y)->(y-x));
    }
    
    public void addNum(int num) {
        if(A.size()!=B.size()){
            A.add(num);
            B.add(A.poll());
        }else{
            B.add(num);
            A.add(B.poll());
        }
    }
    
    public double findMedian() {
        return A.size()!=B.size()?A.peek():(A.peek()+B.peek())/2.0;
    }
}

/**
 * Your MedianFinder object will be instantiated and called as such:
 * MedianFinder obj = new MedianFinder();
 * obj.addNum(num);
 * double param_2 = obj.findMedian();
 */
```



#### 42 连续子数组的最大和

```java
class Solution {
    public int maxSubArray(int[] nums) {
        int res = nums[0];
        int temp = nums[0];
        for(int i=1; i<nums.length; i++){
            temp = Math.max(nums[i]+temp, nums[i]);
            res = Math.max(temp, res);
        }
        return res;
    }
}
```



#### 43 1~n整数中1出现的次数

```java
class Solution {
    public int countDigitOne(int n) {
        int digit=1, res=0;
        int high=n/10, cur=n%10, low=0;
        while(high!=0||cur!=0){
            if(cur==0){
                res+=high*digit;
            }else if(cur == 1){
                res+=high*digit+low+1;
            }else{
                res+=(high+1)*digit;
            }
            low+=cur*digit;
            cur=high%10;
            high/=10;
            digit*=10;
        }
        return res;
    }
}
```



#### 44 数字序列中某一位的数字

```java
class Solution {
    public int findNthDigit(int n) {
        int digit = 1;
        long start = 1;
        long count = 9;
        while(n>count){
            n -= count;
            digit += 1;
            start *= 10;
            count = digit*start*9;
        }
        long num = start+(n-1)/digit;
        return Long.toString(num).charAt((n-1)%digit)-'0';
    }
}
```



#### 45 把数组排成最小的数

```java
class Solution {

    public String minNumber(int[] nums) {
        String[] strs = new String[nums.length];
        for(int i=0; i<nums.length; i++){
            strs[i] = String.valueOf(nums[i]);
        }
        Arrays.sort(strs, new Comparator<String>(){
            public int compare(String a, String b){
                if((a+b).compareTo(b+a)>0){
                    return 1;
                }else{
                    return -1;
                }
            }
        });
        StringBuilder sb = new StringBuilder();
        for(String s : strs){
            sb.append(s);
        }
        return sb.toString();
    }
}
```



#### 46 把数字翻译成字符串

```java
class Solution {
    public int translateNum(int num) {
        String s = String.valueOf(num);
        int a=1, b=1;
        for(int i=2; i<=s.length(); i++){
            String tmp = s.substring(i-2, i);
            int c = tmp.compareTo("10")>=0&&tmp.compareTo("25")<=0?a+b:a;
            b=a;
            a=c;
        }
        return a;
    }
}
```



#### 47 礼物的最大价值

```java
class Solution {
    public int maxValue(int[][] grid) {
        int m = grid.length, n = grid[0].length;
        for(int i=0; i<m; i++){
            for(int j=0; j<n; j++){
                if(i == 0 && j == 0){
                    continue;
                }
                if(i == 0){
                    grid[i][j] += grid[i][j-1];
                }else if(j == 0){
                    grid[i][j] += grid[i-1][j];
                }else{
                    grid[i][j] += Math.max(grid[i-1][j], grid[i][j-1]);
                }
            }
        }
        return grid[m-1][n-1];
    }
}
```



#### 48 最长不含重复字符的子字符串

```java
class Solution {
    public int lengthOfLongestSubstring(String s) {
        Map<Character, Integer> dic = new HashMap<>();
        int i=-1, res = 0;
        for(int j=0; j<s.length(); j++){
            if(dic.containsKey(s.charAt(j))){
                i = Math.max(i, dic.get(s.charAt(j)));
            }
            dic.put(s.charAt(j), j);
            res = Math.max(res, j-i);
        }
        return res;
    }
}
```



#### 49 丑数

```java
class Solution {
    public int nthUglyNumber(int n) {
        int[] dp = new int[n];
        dp[0] = 1;
        int p2=0, p3=0, p5=0;
        for(int i=1; i<n; i++){
            dp[i] = Math.min(dp[p2]*2,Math.min(dp[p3]*3, dp[p5]*5));
            if(dp[i] == 2*dp[p2]) p2++;
            if(dp[i] == 3*dp[p3]) p3++;
            if(dp[i] == 5*dp[p5]) p5++;
        }
        return dp[n-1];
    }
}
```



#### 50 第一个只出现一次的字符

```java
class Solution {
    public char firstUniqChar(String s) {
        int[] flag = new int[26];
        for(int i=0; i<s.length(); i++){
            flag[s.charAt(i)-'a']++;
        }
        for(int i=0; i<s.length(); i++){
            if(flag[s.charAt(i)-'a'] == 1){
                return s.charAt(i);
            }
        }
        return ' ';
    }
}
```



#### 51 数组中的逆序对

```java
class Solution {
    public int reversePairs(int[] nums) {
        int len=nums.length;
        if(len<2){
            return 0;
        }
        int[] copy=new int[len];
        for(int i=0; i<len; i++){
            copy[i]=nums[i];
        }
        int[] temp = new int[len];
        return reversePairs(copy, 0, len-1, temp);
    }

    private int reversePairs(int[] nums, int left, int right, int[] temp){
        if(left == right){
            return 0;
        }
        int mid = left+(right-left)/2;
        int leftPairs =  reversePairs(nums, left, mid, temp);
        int rightPairs = reversePairs(nums, mid+1, right, temp);
        if(nums[mid]<=nums[mid+1]){
            return leftPairs+rightPairs;
        }
        int crossPairs = mergeAndCount(nums,left,mid,right,temp);
        return leftPairs+rightPairs+crossPairs;
    }

    private int mergeAndCount(int[] nums, int left, int mid, int right, int[] temp){
        for(int i=left; i<=right; i++){
            temp[i] = nums[i];
        }
        int i=left, j=mid+1;
        int count = 0;
        for(int k=left; k<=right; k++){
            if(i==mid+1){
                nums[k]=temp[j];
                j++;
            }else if(j==right+1){
                nums[k]=temp[i];
                i++;
            }else if(temp[i]<=temp[j]){
                nums[k]=temp[i];
                i++;
            }else{
                nums[k]=temp[j];
                j++;
                count+=(mid-i+1);
            }
        }
        return count;
    }
}
```



#### 52 两个链表的第一个公共节点

```java
/**
 * Definition for singly-linked list.
 * public class ListNode {
 *     int val;
 *     ListNode next;
 *     ListNode(int x) {
 *         val = x;
 *         next = null;
 *     }
 * }
 */
public class Solution {
    public ListNode getIntersectionNode(ListNode headA, ListNode headB) {
        int lenA = 0, lenB = 0;
        ListNode p = headA, q = headB;
        while(headA != null){
            lenA++;
            headA = headA.next;
        }
        while(headB != null){
            lenB++;
            headB = headB.next;
        }
        while(lenA != lenB){
            if(lenA > lenB){
                lenA--;
                p = p.next;
            }else{
                lenB--;
                q = q.next;
            }
        }
        while(p!=q){
            p = p.next;
            q = q.next;
        }
        return p;
    }
}
```



#### 53-I 在排序数组中查找数字I

```java
class Solution {
    public int search(int[] nums, int target) {
        int count = 0;
        for(int num : nums){
            if(num == target){
                count++;
            }
        }
        return count;
    }
}
```



#### 53-II 0~n-1中缺失的数字

```java
class Solution {
    public int missingNumber(int[] nums) {
        int i = 0, j = nums.length-1;
        while(i <= j){
            int mid = i+(j-i)/2;
            if(mid == nums[mid]){
                i = mid+1;
            }else{
                j = mid-1;
            }
        }
        return i;
    }
}
```



#### 54 二叉搜索树的第k大节点

```java
/**
 * Definition for a binary tree node.
 * public class TreeNode {
 *     int val;
 *     TreeNode left;
 *     TreeNode right;
 *     TreeNode(int x) { val = x; }
 * }
 */
class Solution {
    int res, k;
    public int kthLargest(TreeNode root, int k) {
        this.k = k;
        dfs(root);
        return res;
    }
    private void dfs(TreeNode root){
        if(root == null){
            return;
        }
        dfs(root.right);
        if(k == 0){
            return;
        }
        if(--k == 0){
            res = root.val;
        }
        dfs(root.left);
    }
}
```



#### 55-I 二叉树的深度

```java
/**
 * Definition for a binary tree node.
 * public class TreeNode {
 *     int val;
 *     TreeNode left;
 *     TreeNode right;
 *     TreeNode(int x) { val = x; }
 * }
 */
class Solution {
    public int maxDepth(TreeNode root) {
        if(root == null){
            return 0;
        }
        return Math.max(maxDepth(root.left)+1, maxDepth(root.right)+1);
    }
}
```



#### 55-II 平衡二叉树

```java
/**
 * Definition for a binary tree node.
 * public class TreeNode {
 *     int val;
 *     TreeNode left;
 *     TreeNode right;
 *     TreeNode(int x) { val = x; }
 * }
 */
class Solution {
    public boolean isBalanced(TreeNode root) {
        if(root == null){
            return true;
        }
        return Math.abs(depth(root.left)-depth(root.right))<=1&&isBalanced(root.left)&&isBalanced(root.right);
    }
    private int depth(TreeNode root){
        if(root == null){
            return 0;
        }
        return Math.max(depth(root.left)+1, depth(root.right)+1);
    }
}
```



#### 56-I 数组中数字出现的次数

```java
class Solution {
    public int[] singleNumbers(int[] nums) {
        int ret = 0;
        for(int num : nums){
            ret ^= num;
        }
        int div = 1;
        while((div&ret)==0){
            div <<= 1;
        }
        int a = 0, b = 0;
        for(int num: nums){
            if((div&num)==0){
                a^=num;
            }else{
                b^=num;
            }
        }
        return new int[]{a,b};
    }
}
```



#### 56-II 数组中数字出现的次数II	

```java
class Solution {
    public int singleNumber(int[] nums) {
        if(nums.length == 0){
            return -1;
        }
        int[] bitSum = new int[32];
        int res = 0;
        for(int num : nums){
            int bitMask = 1;
            for(int i=31; i>=0; i--){
                if((num&bitMask)!=0) bitSum[i]++;
                bitMask <<= 1;
            }
        }
        for(int i=0; i<32; i++){
            res=res<<1;
            res+=bitSum[i]%3;
        }
        return res;
    }
}
```



#### 57 和为s的两个数字

```java
class Solution {
    public int[] twoSum(int[] nums, int target) {
        int[] res = new int[2];
        int i = 0, j = nums.length-1;
        while(i < j){
            int left = nums[i];
            int right = nums[j];
            if(left + right == target){
                res[0] = left;
                res[1] = right;
                return res;
            }else if(left + right > target){
                j--;
            }else{
                i++;
            }
        }
        return res;
    }
}
```



#### 57-II 和为s的连续正数序列

```java
class Solution {
    public int[][] findContinuousSequence(int target) {
        int i = 1;
        int j = 1;
        int sum = 0;
        List<int[]> res = new ArrayList<>();
        while(i<=target/2){
            if(sum < target){
                sum+=j;
                j++;
            }else if(sum > target){
                sum-=i;
                i++;
            }else{
                int[] arr = new int[j-i];
                for(int k=i; k<j; k++){
                    arr[k-i] = k;
                }
                res.add(arr);
                sum-=i;
                i++;
            }
        }
        return res.toArray(new int[res.size()][]);
    }
}
```



#### 58-I 翻转单词顺序

```java
class Solution {
    public String reverseWords(String s) {
        String[] strs = s.trim().split(" ");
        StringBuilder sb = new StringBuilder();
        for(int i=strs.length-1; i>=0; i--){
            if(strs[i].equals("")) continue;
            sb.append(strs[i]+" ");
        }
        return sb.toString().trim();
    }
}
```



#### 58-II 左旋转字符串

```java
class Solution {
    public String reverseLeftWords(String s, int n) {
        StringBuilder sb = new StringBuilder();
        String latter = s.substring(n,s.length());
        String former = s.substring(0,n);
        sb.append(latter);
        sb.append(former);
        return sb.toString();
    }
}
```



#### 59-I 滑动窗口的最大值

```java
class Solution {
    public int[] maxSlidingWindow(int[] nums, int k) {
        if(nums.length==0||k==0){
            return new int[0];
        }
        Deque<Integer> deque = new LinkedList<>();
        int[] res = new int[nums.length-k+1];
        for(int j=0,i=1-k; j<nums.length; i++,j++){
            if(i>0&&deque.peekFirst()==nums[i-1]){
                deque.pollFirst();
            }
            while(!deque.isEmpty()&&deque.peekLast()<nums[j]){
                deque.pollLast();
            }
            deque.offerLast(nums[j]);
            if(i>=0){
                res[i] = deque.peekFirst();
            }
        }
        return res;
    }
}
```



#### 59-II 队列的最大值

```java
class MaxQueue {

    Queue<Integer> q;
    Deque<Integer> d;
    public MaxQueue() {
        q = new LinkedList<Integer>();
        d = new LinkedList<Integer>();
    }
    
    public int max_value() {
        if(d.isEmpty()){
            return -1;
        }
        return d.peekFirst();
    }
    
    public void push_back(int value) {
        while(!d.isEmpty()&&d.peekLast()<value){
            d.pollLast();
        }
        d.offerLast(value);
        q.offer(value);
    }
    
    public int pop_front() {
        if(q.isEmpty()){
            return -1;
        }
        int ans = q.poll();
        if(d.peekFirst() == ans){
            d.pollFirst();
        }
        return ans;
    }
}

/**
 * Your MaxQueue object will be instantiated and called as such:
 * MaxQueue obj = new MaxQueue();
 * int param_1 = obj.max_value();
 * obj.push_back(value);
 * int param_3 = obj.pop_front();
 */
```



#### 60 n个筛子的点数

```java
class Solution {
    public double[] dicesProbability(int n) {
        int[][] dp = new int[12][72];
        for(int i=1; i<=6; i++){
            dp[1][i] = 1;
        }
        for(int i=2; i<=n; i++){
            for(int j=i; j<=6*i; j++){
                for(int cur=1; cur<=6; cur++){
                    if(j-cur<=0){
                        break;
                    }
                    dp[i][j] += dp[i-1][j-cur];
                }
            }
        }
        double sum = Math.pow(6,n);
        double[] res = new double[6*n-n+1];
        int count = 0;
        for(int i=n; i<=6*n; i++){
            res[count++] = dp[n][i]*1.0/sum;
        }
        return res;
    }
}
```



#### 61 扑克牌中的顺子

```java
class Solution {
    public boolean isStraight(int[] nums) {
        Set<Integer> repeat = new HashSet<>();
        int max = 0, min = 14;
        for(int num : nums){
            if(num == 0){
                continue;
            }
            max = Math.max(max, num);
            min = Math.min(min, num);
            if(repeat.contains(num)){
                return false;
            }
            repeat.add(num);
        }
        return max-min<5;
    }
}
```



#### 62 圆圈中最后剩下的顺子

```java
class Solution {
    public int lastRemaining(int n, int m) {
        int pos = 0;
        for(int i=2; i<=n; i++){
            pos = (pos+m)%i;
        }
        return pos;
    }
}
```



#### 63 股票的最大利润

```java
class Solution {
    public int maxProfit(int[] prices) {
        int cost = Integer.MAX_VALUE, profit = 0;
        for(int i=0; i<prices.length; i++){
            cost = Math.min(cost, prices[i]);
            profit = Math.max(profit, prices[i]-cost);
        }
        return profit;
    }
}
```



#### 64 求1+2+...+n

```java
class Solution {
    int res = 0;
    public int sumNums(int n) {
        boolean flag = n>1&&sumNums(n-1)>0;
        res+=n;
        return res;
    }
}
```



#### 65 不用加减乘除做加法

```java
class Solution {
    public int add(int a, int b) {
        while(b!=0){
            int c = (a&b)<<1;
            a^=b;
            b=c;
        }
        return a;
    }
}
```



#### 66 构建乘积数组

```java
class Solution {
    public int[] constructArr(int[] a) {
        if(a.length == 0) return new int[0];
        int[] b = new int[a.length];
        b[0] = 1;
        int tmp = 1;
        for(int i = 1; i < a.length; i++) {
            b[i] = b[i - 1] * a[i - 1];
        }
        for(int i = a.length - 2; i >= 0; i--) {
            tmp *= a[i + 1];
            b[i] *= tmp;
        }
        return b;
    }
}
```



#### 67 把字符串转换成整数

```java
class Solution {
    public int strToInt(String str) {
        char[] c = str.trim().toCharArray();
        if(c.length == 0){
            return 0;
        }
        int res = 0, bndry = Integer.MAX_VALUE/10;
        int i = 1, sign = 1;
        if(c[0] == '-'){
            sign = -1;
        }else if(c[0] != '+'){
            i = 0;
        }
        for(int j=i; j<c.length; j++){
            if(c[j]<'0'||c[j]>'9'){
                break;
            }
            if(res > bndry || res == bndry&&c[j] > '7'){
                return sign == 1?Integer.MAX_VALUE:Integer.MIN_VALUE;
            }
            res = res*10+(c[j]-'0');
        }
        return sign*res;
    }
}
```



#### 68-I 二叉搜索树的最近公共祖先

```java
/**
 * Definition for a binary tree node.
 * public class TreeNode {
 *     int val;
 *     TreeNode left;
 *     TreeNode right;
 *     TreeNode(int x) { val = x; }
 * }
 */
class Solution {
    public TreeNode lowestCommonAncestor(TreeNode root, TreeNode p, TreeNode q) {
        int pVal = p.val, qVal = q.val;
        if(p.val <= root.val && root.val <= q.val||q.val<=root.val && root.val<=p.val){
            return root;
        }else if(p.val<root.val&&q.val<root.val){
            return lowestCommonAncestor(root.left, p, q);
        }else{
            return lowestCommonAncestor(root.right, p, q);
        }
    }
}
```



#### 68-II 二叉树的最近公共祖先

```java
/**
 * Definition for a binary tree node.
 * public class TreeNode {
 *     int val;
 *     TreeNode left;
 *     TreeNode right;
 *     TreeNode(int x) { val = x; }
 * }
 */
class Solution {
    public TreeNode lowestCommonAncestor(TreeNode root, TreeNode p, TreeNode q) {
        if(root==null||root==p||root==q){
            return root;
        }
        TreeNode left = lowestCommonAncestor(root.left, p, q);
        TreeNode right = lowestCommonAncestor(root.right, p, q);
        if(left == null){
            return right;
        }
        if(right == null){
            return left;
        }
        return root;
    }
}
```



