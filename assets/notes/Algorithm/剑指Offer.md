## 剑指Offer

### 1. 替换空格

请实现一个函数，将一个字符串中的每个空格替换成“%20”。例如，当字符串为We Are Happy.则经过替换之后的字符串为We%20Are%20Happy。

```c++
class Solution {
public:
	void replaceSpace(char *str,int length) {
	}
};
```

<hr>


这题其实就是在给定地str中找到空格，并替换成%20，由于是一个字符替换成三个字符，所以需要移动元素，如果从前往后移动，假设有n个空格，则第n个空格后面的元素都要被移动n次，很浪费时间，所以从后往前移动，并且不需要额外的数组，先计算出填补后的长度`newlength=oldnumber+n*2`,然后从后遍历直接`str[pNewlength--]=str[pOldlength];`

```c++
class Solution {
public:
	void replaceSpace(char *str,int length) {
        if (str == NULL || length < 0)
      return;
        int i=0,oldLength=strlen(str),newLength=0,blanks=0;
        while(str[i]!='\0'){
            //oldLength++;
            if(str[i]==' ')
            blanks++;
            i++;
        }
        newLength=oldLength+blanks*2;
        if(newLength>length) return;
        while(newLength>oldLength&&oldLength>=0){
            if(str[oldLength]==' '){
                str[newLength--]='0';
                str[newLength--]='2';
                str[newLength--]='%';
            }else{
                str[newLength]=str[oldLength];
                newLength--;
            }
            oldLength--;
        }
	}
};
```

### 2. 从头到尾打印链表

输入一个链表，按链表从尾到头的顺序返回一个ArrayList。

<hr>


```c++
class Solution {
public:
    vector<int> printListFromTailToHead(ListNode* head) {
        vector<int> arr;
        while(head){
            arr.insert(arr.begin(),head->val);
            head=head->next;
        }
        return arr;
    }
};
```

### 3. 重建二叉树

输入某二叉树的前序遍历和中序遍历的结果，请重建出该二叉树。假设输入的前序遍历和中序遍历的结果中都不含重复的数字。例如输入前序遍历序列{1,2,4,7,3,5,6,8}和中序遍历序列{4,7,2,1,5,3,8,6}，则重建二叉树并返回。

```c++
/**
 * Definition for binary tree
 * struct TreeNode {
 *     int val;
 *     TreeNode *left;
 *     TreeNode *right;
 *     TreeNode(int x) : val(x), left(NULL), right(NULL) {}
 * };
 */
class Solution {
public:
    TreeNode* reConstructBinaryTree(vector<int> pre,vector<int> vin) {

    }
};
```

<hr>


![图片说明](../../../../../Desktop/Algorithm.assets/807319133_1566201171550_B40B59C37DAF61726E7EDC919A086425.png)

![图片说明](../../../../../Desktop/Algorithm.assets/807319133_1566201156302_8051F21823FC87CE1C97D7AE4FAD5B08.png)

思路：

1. 由先序序列第一个**`pre[0]`**在中序序列中找到根节点位置**`gen`** 

2. 以

   `gen`

   为中心遍历

   - `0~gen`

     左子树

     - 子中序序列：**`0~gen-1`**，放入**`vin_left[]`** 
     - 子先序序列：**`1~gen`**放入**`pre_left[]`**，**`+1`**可以看图，因为头部有根节点 

   - `gen+1~vinlen`

     为右子树

     - 子中序序列：**`gen+1 ~ vinlen-1`**放入**`vin_right[]`** 
     - 子先序序列：**`gen+1 ~ vinlen-1`**放入**`pre_right[]`** 

3. 由先序序列**`pre[0]`**创建根节点 

4. 连接左子树，按照左子树子序列递归（**`pre_left[]`**和**`vin_left[]`**） 

5. 连接右子树，按照右子树子序列递归（**`pre_right[]`**和**`vin_right[]`**） 

6. 返回根节点

```c++
/**
 * Definition for binary tree
 * struct TreeNode {
 *     int val;
 *     TreeNode *left;
 *     TreeNode *right;
 *     TreeNode(int x) : val(x), left(NULL), right(NULL) {}
 * };
 */
class Solution {
public:
    TreeNode* reConstructBinaryTree(vector<int> pre,vector<int> vin) {
        int mid=0,len=vin.size();
        if(len==0) return NULL;
        vector<int> preL,preR,vinL,vinR;
        
        TreeNode* root=new TreeNode(pre[0]);
        
        for(int i=0;i<len;i++){
            if(vin[i]==pre[0]){
                mid=i;
                break;
            }
        }
        
        for(int i=0;i<mid;i++){
            preL.push_back(pre[i+1]);
            vinL.push_back(vin[i]);
        }
        for(int i=mid+1;i<len;i++){
            preR.push_back(pre[i]);
            vinR.push_back(vin[i]);
        }
        root->left=reConstructBinaryTree(preL,vinL);
        root->right=reConstructBinaryTree(preR,vinR);
        return root;
    }
};
```

### 4. 两个栈实现队列

用两个栈来实现一个队列，完成队列的Push和Pop操作。 队列中的元素为int类型。

<hr>


```c++
class Solution
{
public:
    void push(int node) {
        if(stack2.empty()&&stack1.empty()){
            stack2.push(node);
        }else
        stack1.push(node);
    }

    int pop() {
        if(!stack2.empty()){
            
        }else{
            if(stack1.empty()){
                return -1;
            }
            trans();
        }
        int ans= stack2.top();
        stack2.pop();
        if(stack2.empty()){
            trans();
        }
        return ans;
    }
    
    void trans(){
        while(!stack1.empty()){
            stack2.push(stack1.top());
            stack1.pop();
        }
    }

private:
    stack<int> stack1;
    stack<int> stack2;
};
```

### 5.二进制中1的个数

输入一个整数，该数二进制表示中1的个数。其中负数用补码表示。

<hr>


1.利用一个结论：一个二进制数n减1后与原二进制数进行&运算( 即n&(n-1) )会消去最右边的1。
2.这个结论怎么来的？

- 假设二进制数101进行减1运算，刚好最右边是1，则得到100，此时用100跟101做&运算，得到的是100，故消去了101左右边的1。 
- 100减1呢？最低位是0,跟十进制减法一样啊，向高位借。(可以脑补一下十进制100减1的过程) 

**所以二进制100减1的运算过程如下：** 

- 最右边的0向右数第二位借1得：2-1=1， 
- 右数第二位还是0，却要借给最右边那位1，所以它也得向高位借。 
- 这样右数第三位的1借给它1之后变成0，右数第二位借1得：2-1=1。 
- 所以得到新数为011。 

**观察一下刚刚的运算过程可以发现：** 

- 如果最右边刚好是1(如101)，进行减1运算就不用向高位借，直接得0，高位则保持原样不变(得100)。 
- 再把减1后得到的数与原数&运算(即101&100=100)可知高位都不变那就是消去最右边的1！(由这可能还不太明显是消去最右边的1，继续看下面的) 
- 如果最右边是0(如100)，进行减1运算，则需要像高位借，而最终会导致最近的高位1因为被借走1而变0，而比它高的高位保持原样不变(得011),再跟原来的数进行&运算(即100&011=000)； 
- 所以由以上两点可知二进制数每次减1后与原数进行&运算会消去最右边的1。

```c++
class Solution {
public:
     int  NumberOf1(int n) {
         int count=0;
         while(n!=0)
         {
             n=n&(n-1);
             count++;
         }
         return count;
     }
};

```

### 6. 调整数组顺序

输入一个整数数组，实现一个函数来调整该数组中数字的顺序，使得所有的奇数位于数组的前半部分，所有的偶数位于数组的后半部分，并保证奇数和奇数，偶数和偶数之间的相对位置不变。

<hr>


我写了个很弱智的方法,时间复杂度不高空间挺复杂的

```c++
class Solution {
public:
    void reOrderArray(vector<int> &array) {
        vector<int> tmp;
        vector<int> tmp2;
        for(int i=0;i<array.size();i++){
            if(array[i]&1){
                tmp.push_back(array[i]);
            }else{
                tmp2.push_back(array[i]);
            }
        }
        for(int i=0;i<tmp2.size();i++){
            tmp.push_back(tmp2[i]);
        }
        array.swap(tmp);
    }
};
```

### 7. 链表中倒数第k个结点

输入一个链表，输出该链表中倒数第k个结点。

<hr>


```c++
class Solution {
public:
    ListNode* FindKthToTail(ListNode* pListHead, unsigned int k) {
        int cnt=0;
        ListNode * L;
        ListNode * ans;
        L=pListHead;ans=pListHead;
        while(L){
            cnt++;
            if(cnt>k){
                ans=ans->next;
            }
            L=L->next;
        }
        if(cnt<k){
            return NULL;
        }
        return ans;
    }
};
```

没啥难度，但是指针定义那一块要小心` ListNode * L,ans;`是错误的，应为` ListNode * L,*ans;`

### 8. 反转链表

输入一个链表，反转链表后，输出新链表的表头。

<hr>


```c++
class Solution {
public:
    ListNode* ReverseList(ListNode* pHead) {
        ListNode * p0=nullptr,*p1=pHead,*p2=nullptr;
        while(p1){
            p2=p1->next;
            p1->next=p0;
            p0=p1;
            p1=p2;
        }
        return p0;
    }
};
```

没啥难度，但要小心。

![反转链表](../../../../../Desktop/Algorithm.assets/815472960_1566642098849_C3B4D3532589FC2990420BCEA62A8DBF.jfif)

### 9. 合并两个排序的链表

输入两个单调递增的链表，输出两个链表合成后的链表，当然我们需要合成后的链表满足单调不减规则。

<hr>


搜集了一个递归写法，不算太懂

```c++
class Solution {
public:
    ListNode* Merge(ListNode* pHead1, ListNode* pHead2)
    {
        if(pHead1==NULL)
            return pHead2;
        else if(pHead2==NULL)
            return pHead1;
        ListNode* p1=pHead1;
        ListNode* p2=pHead2;
        ListNode* newHead=NULL;
        if(p1->val<p2->val){
            newHead=p1;
            newHead->next=Merge(p1->next,p2);
        }
        else{
            newHead=p2;
            newHead->next=Merge(p2->next,p1);
        }
        return newHead;
    }
};
```

这其实就是归并排序，等回头数据结构学完得好好写一下。

### 10. 顺时针打印矩阵

输入一个矩阵，按照从外向里以顺时针的顺序依次打印出每一个数字，例如，如果输入如下4 X 4矩阵： 1 2 3 4 5 6 7 8 9 10 11 12 13 14 15 16 则依次打印出数字1,2,3,4,8,12,16,15,14,13,9,5,6,7,11,10.

<hr>


```c++
class Solution {
public:
    vector<int> printMatrix(vector<vector<int> > matrix) {
        int dir[4][2]={{0,1},{1,0},{0,-1},{-1,0}},di=0,i=0,j=0,cnt=0;
        int vis[matrix.size()][matrix[0].size()],num=matrix.size()*matrix[0].size();
        vector<int> arr;
        if(num==0){
            return arr;
        }
        
        while(1){
                //cout<<matrix[i][j];
            arr.push_back(matrix[i][j]);
            vis[i][j]=1;
            i+=dir[di][0];
            j+=dir[di][1];
          //判断增加后是否超出数组范围或者下一个元素已被访问，则转向
            if(vis[i][j]==1||i==matrix.size()||j==matrix[0].size()||i<0||j<0){
                    i-=dir[di][0];
                    j-=dir[di][1];
                    di=(di+1)%4;
                    i+=dir[di][0];
                    j+=dir[di][1];
            }
            if(++cnt==num){//最后判断输出个数达到数组元素个数时则结束
                break;
            }
        }
    return arr;
    }
};
```

其实有点找回以前刷题时用dir方向数组+vis数组的感觉了，题目在以前算简单，现在居然花了好一会才出来。

### 11.包含min函数的栈

定义栈的数据结构，请在该类型中实现一个能够得到栈中所含最小元素的min函数（时间复杂度应为O（1））。

<hr>


```c++
class Solution {
public:
    void push(int value) {
        if(ptr>=9999) return;
        if(value<minNum) {
            stack[++ptr]=minNum;
            minNum=value;
        }
        stack[++ptr]=value;
    }
    void pop() {
        if(ptr==-1) return;
        if(stack[ptr]==minNum){
            ptr--;
            minNum=stack[ptr--];
        }else{
            ptr--;
        }
    }
    int top() {
        if(ptr==-1) return -1;
        return stack[ptr];
    }
    int min() {
        return minNum==0xffffff?-1:minNum;
    }
    int minNum=0xffffff;
    int ptr=-1;
    int stack[10000];
    
};
```

本来对于目前的最小值被pop后如何处理没什么办法，查看题解后明白可以在插入时判断，如果此值为新的最小值，则在插入此值之前先插入上一个最小值，在pop时判断取出的是不是当前最小值，若是则pop两次，第二次pop出的则为当前的最小值。

### 12. 栈的压入弹出

要看一个序列是否可以作为一个给定入栈序列的弹出序列，可以用一个栈进行模拟（也可用数组代替，我就偷个懒），设一个指向popV第一个元素的指针j，按照pushV的顺序插入，插入时检索插入的值和popV[J]是否一样，若一样则说明当前出栈序列正好出到这个值，那就直接弹出，j++（匹配出栈序列的下一个），最后pushV全插入后，若j未到popV末尾，说明栈中剩下的内容一次性全部弹出，直接边弹出边和popV中的作比较即可。

```c++
class Solution {
public:
    bool IsPopOrder(vector<int> pushV,vector<int> popV) {
        stack<int> sta;
        int i=0,j=0;
        while(i!=pushV.size()){
            sta.push(pushV[i]);
            if(pushV[i]==popV[j]){
                sta.pop();
                j++;
            }
            i++;
        }
        for(;j<popV.size();j++){
            if(sta.top()!=popV[j]){
                return false;
            }else{
                sta.pop();
            }
        }
        return true;
    }
};
```

### 13.字符串的排列

输入一个字符串,按字典序打印出该字符串中字符的所有排列。例如输入字符串abc,则打印出由字符a,b,c所能排列出来的所有字符串abc,acb,bac,bca,cab和cba。

<hr>


```c++
class Solution {
public:
    vector<string> Permutation(string str) {
        if (str.empty()) return {};
        sort(str.begin(), str.end());
        vector<string> ans;
        ans.push_back(str);
        while (next_permutation(str.begin(), str.end()))
            ans.push_back(str);
        return ans;
    }
};
```

### 14. 数组中出现次数超过一半的数字

数组中有一个数字出现的次数超过数组长度的一半，请找出这个数字。例如输入一个长度为9的数组{1,2,3,2,2,2,5,4,2}。由于数字2在数组中出现了5次，超过数组长度的一半，因此输出2。如果不存在则输出0。

<hr>


```c++
class Solution {
public:
    int MoreThanHalfNum_Solution(vector<int> numbers) {
        sort(numbers.begin(),numbers.end());
        int len=numbers.size()/2;
        int tmp,j;
        for(int i=0;i<numbers.size();i++){
            j=i+len;
            if(j<numbers.size()){
                if(numbers[j]==numbers[i]){
                    return numbers[i];
                }
            }else{
                return 0;
            }
        }
    }
};
```

这个办法不是很满意，看了看他人的，普遍有找中间数的方法

```java
public int MoreThanHalfNum_Solution(int [] array) {
    Arrays.sort(array);
    int i=array[array.length/2];
    return IntStream.of(array).filter(k->k==i).count()>array.length/2?i:0;
}
```

或者用count++

```c++
class Solution {
public:
    int MoreThanHalfNum(vector<int> numbers) {
        int n = 0;
        int ret;
        for(size_t i=0;i<numbers.size();i++){
            if(n == 0){
                ret = numbers[i];
                n = 1;
            }else{
                if(ret == numbers[i])
                    n ++;
                else
                    n--;  
            }
        }
        return ret;
    }
};
```

### 15.连续子数组的最大和

HZ偶尔会拿些专业问题来忽悠那些非计算机专业的同学。今天测试组开完会后,他又发话了:在古老的一维模式识别中,常常需要计算连续子向量的最大和,当向量全为正数的时候,问题很好解决。但是,如果向量中包含负数,是否应该包含某个负数,并期望旁边的正数会弥补它呢？例如:{6,-3,-2,7,-15,1,2,2},连续子向量的最大和为8(从第0个开始,到第3个为止)。给一个数组，返回它的最大连续子序列的和，你会不会被他忽悠住？(子向量的长度至少是1)

<hr>


```c++
class Solution {
public:
    int FindGreatestSumOfSubArray(vector<int> array) {
        int dp[array.size()],maxn=array[0];
        dp[0]=array[0];
        for(int i=1;i<array.size();i++){
            dp[i]=max(array[i],dp[i-1]+array[i]);
            maxn=max(maxn,dp[i]);
        }
        return maxn;
    }
}; 
```

最简单的动态规划，dp作为规划数组，dp[i]存储从前面最佳位置到array[i]的和，由于dp[i-1]存储从前面最佳位置到array[i-1]的和，所以如果dp[i-1]+array[i]<array[i]，即前面最佳位置到i-1的值是负的，说明还不如从i位置重新开始寻找，此时dp[i]=array[i]，同时maxn密切追踪最大值，最后的值就是整个的最大值。

也可以待循环结束后遍历整个dp数组找到最大值。

### 16. 丑数

把只包含质因子2、3和5的数称作丑数（Ugly Number）。例如6、8都是丑数，但14不是，因为它包含质因子7。 习惯上我们把1当做是第一个丑数。求按从小到大的顺序的第N个丑数。

<hr>


```c++
class Solution {
public:
    int GetUglyNumber_Solution(int index) {
        int ans[index+1];
        //ans[0]=0;
        memset(ans,0,sizeof(ans));
        ans[1]=1;
        int p2=1,p3=1,p5=1;
        for(int i=2;i<=index;i++){
            ans[i]=min(ans[p2]*2,min(ans[p3]*3,ans[p5]*5));
            if(ans[p2]*2==ans[i]) p2++;
            if(ans[p3]*3==ans[i]) p3++;
            if(ans[p5]*5==ans[i]) p5++;
        }
        return ans[index];
    }
};
```

利用动态规划，由于每个丑数可以理解为``2^x3^y5^z`的形式,用ans[i]表示第i个丑数，则ans[i]等于ans[a]\*2、ans[b]\*3、ans[c]*5中最小的一个，a、b、c均小于i。所以用三个指针记录a、b、c即可，由于有可能ans[a]\*2==ans[b]\*3，所以最后指针++时得用三个独立的if。

### 17.数字在排序数组中出现的次数

统计一个数字在排序数组中出现的次数。

```c++
class Solution {
public:
int GetNumberOfK(vector<int> data, int k)
{
};
```

<hr>


一道三分钟的题被我写成了三小时。

自己想法是先二分找到这个数然后再遍历寻找个数。主要是好久不写二分，一些细节都忘了。

```c++
class Solution {
public:
int GetNumberOfK(vector<int> data, int k)
{
  int le = 0, ri = data.size() - 1;
  int mid, cnt = 0;
  while ( le <= ri){
    mid = (ri + le) >> 1;
    if (data[mid] > k){
      ri = mid - 1;//注意需要-1，不然可能出现13+14/2=13一直访问不到14的情况
    }
    else if (data[mid] < k){
      le = mid + 1;
    }else{
      break;
    }
  }
  if (mid>=data.size()||mid<0)//判断是找到退出还是le>ri退出时，不能用data[mid]==k，因为mid可能<0或==data.size()造成越界
    return cnt;
  else
    cnt = 0;
  for (int i = mid; i < data.size() && data[i] == k; i++)
  {
    cnt++;
  }
  for (int i = mid - 1; i >= 0 && data[i] == k; i--)
  {
    cnt++;
  }
  return cnt;
}
};
```

还有更好的方法是，直接upper_bound-lower_bound就是个数，相当于两次都用二分。

```java
private int upper_bound(int[] array, int val) {
        int l = 0, r = array.length - 1, mid;
        while (l <= r) {
            mid = (l + r) >> 1;
            if (array[mid] <= val) {
                l = mid + 1;
            } else {
                r = mid - 1;
            }
        }
        return l;
    }
 
    private int lower_bound(int[] array, int val) {
        int l = 0, r = array.length - 1, mid;
        while (l <= r) {
            mid = (l + r) >> 1;
            if (array[mid] < val) {
                l = mid + 1;
            } else {
                r = mid - 1;
            }
        }
        return l;
    }
```

### 18.二叉树深度

输入一棵二叉树，求该树的深度。从根结点到叶结点依次经过的结点（含根、叶结点）形成树的一条路径，最长路径的长度为树的深度。

```c++
/*
struct TreeNode {
	int val;
	struct TreeNode *left;
	struct TreeNode *right;
	TreeNode(int x) :
			val(x), left(NULL), right(NULL) {
	}
};*/
class Solution {
public:
    int maxDeep=0;
    void getD(TreeNode* pRoot,int dep){
        if(pRoot==NULL){
            maxDeep=max(maxDeep,dep);
            return;
        }
        dep+=1;
        getD(pRoot->right,dep);
        getD(pRoot->left,dep);
    };
    int TreeDepth(TreeNode* pRoot)
    {
        maxDeep=0;
        if(pRoot){
            getD(pRoot,0);
            return maxDeep;
        }else{
            return 0;
        }
    };
};
```

