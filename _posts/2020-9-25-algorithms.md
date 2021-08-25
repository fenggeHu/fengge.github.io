# algorithms

本文算法用Java实现 持续更新。

linearithmic:NlogN linear:N

## union-find
Dynamic connectivity:Union Find

多个集合，将两个元素合并到一个集合（union），看两个元素是否在同一集合（find）

只需要一个数组记录每个元素所属集合号。

三种算法

Quick-find 这种算法每一次union都需要将一个集合中所有元素的记号改成另一个集合。所以他查找特别方便

~~~java
public class QuickFindUF {
    private int[] id;

    public QuickFindUF(int N) {
        id = new int[N];
        for (int i = 0; i < N; i++)
            id[i] = i;
    }

    public boolean connected(int p, int q) {
        return id[p] == id[q];
    }

    public void union(int p, int q) {
        int pid = id[p];
        int qid = id[q];
        for (int i = 0; i < id.length; i++)
            if (id[i] == pid)
                id[i] = qid;
    }
}
~~~



Quick-union 这种算法union 特别方便，只要把一个元素的记号改成另一个。就形成一种类似树的结构。而find就比较麻烦，需要找到元素记号的根。

~~~java
public class QuickUnionUF {
    private int[] id;

    public QuickUnionUF(int N) {
        id = new int[N];
        for (int i = 0; i < N; i++)
            id[i] = i;
    }

    private int root(int i) {
        while (i != id[i])
            i = id[i];
        return i;
    }

    public boolean connected(int p, int q) {
        return root(p) == root(q);
    }

    public void union(int p, int q) {
        int i = root(p);
        int j = root(q);
        id[i] = j;
    }
}
~~~



Weighted quick-union 这个算法在Quick-union上修改，因为Quick-union每次记号随便改的，这样树可能会特别长，不利于find。所以我们每次都把小的树插到大的树上。

~~~java
public class WeightedQuickUnionUF {
    private int[] id;
    private int[] sz;

    public WeightedQuickUnionUF(int N) {
        id = new int[N];
        for (int i = 0; i < N; i++)
            id[i] = i;
        sz = new int[N];
        for (int i = 0; i < N; i++)
            sz[i] = 1;
    }

    private int root(int i) {
        while (i != id[i])
            i = id[i];
        return i;
    }

    public boolean connected(int p, int q) {
        return root(p) == root(q);
    }

    public void union(int p, int q) {
        int i = root(p);
        int j = root(q);
        if (i == j)
            return;
        if (sz[i] < sz[j]) {
            id[i] = j;
            sz[j] += sz[i];
        } else {
            id[j] = i;
            sz[i] += sz[j];
        }
    }
}
~~~

Quick Union with path compression

~~~java
public class QuickUnionPathCompression {
    private int[] id;

    public QuickUnionPathCompression(int N) {
        id = new int[N];
        for (int i = 0; i < N; i++)
            id[i] = i;
    }

    private int root(int i) {
        while (i != id[i]) {
            id[i] = id[id[i]];
            i = id[i];
        }
        return i;
    }

    public boolean connected(int p, int q) {
        return root(p) == root(q);
    }

    public void union(int p, int q) {
        int i = root(p);
        int j = root(q);
        id[i] = j;
    }
}
~~~



| algorithm   | initialize | union | connected |
| ----------- | ---------- | ----- | --------- |
| quick-find  | N          | N     | 1         |
| quick-union | N          | N     | N         |
| weighted QU | N          | logN  | logN      |

| algorithm                      | worst-case time |
| ------------------------------ | --------------- |
| quick-find                     | MN              |
| quick-union                    | MN              |
| weighted QU                    | N+MlogN         |
| QU + path compression          | N+MlogN         |
| weighted QU + path compression | N+MlgN          |

## ANALYSIS OF ALGORITHMS

three sum

~~~java
public class ThreeSum {
    public static int count(int[] a) {
        int N = a.length;
        int count = 0;
        for (int i = 0; i < N; i++)
            for (int j = i + 1; j < N; j++)
                for (int k = j + 1; k < N; k++)
                    if (a[i] + a[j] + a[k] == 0)
                        count++;
        return count;
    }
}

~~~



cost model:Use some basic operation as a proxy for running time.

tilde notation:Ignore lower order terms

Ex 1. ⅙ N 3 + 20 N + 16	 ~ ⅙ N 3



Binary Search

~~~java
public class BinarySearch {
    public static int rank(int key, int[] a)
    {
        int lo = 0;
        int hi = a.length-1;
        while(lo <= hi){
            int mid = lo + (hi-lo)/2;
            if(key<a[mid]) hi = mid-1;
            else if(key > a[mid]) lo = mid+1;
            else return mid;
        }
        return -1;
    }
}
~~~

 uses at most 1 + lg N key compares to search in a sorted array of size N

T (N) 

≤ T (N / 2) + 1 

≤ T (N / 4) + 1 + 1

≤ T (N / 8) + 1 + 1 + 1 . . . 

≤ T (N / N) + 1 + 1 + … + 1 = 1 + lg N 



3-sum fast

~~~java
public class ThreeSumFast {
    public static int count(int[] a){
        Arrays.sort(a);
        int N = a.length;
        int cnt = 0;
        for(int i = 0; i < N; i++)
            for(int j = 0; j < N; j++)
                if(BinarySearch.rank(-a[i]-a[j],a)>j)
                    cnt++;
        return cnt;      
    }
}
~~~

N 2 log N



Best case,Worst case,Average case

Big Theta Θ(N2) asymptotic order of growth

Big Oh O(N2) upper

Big Omega Ω(N2) lower



| 内置类型 | 大小 bytes |
| -------- | ---------- |
| boolean  | 1          |
| char     | 2          |
| int      | 4          |
| float    | 4          |
| double   | 8          |
| long     | 8          |

object 一般有16B overhead  大小为8B的倍数

Integer 24B 16(overhead)+4(int)+4(padding)

Date 32B 16(over)+3×4(3 int)+4(padding)

Reference (可以理解成pointer 强reference即为class 类似cpp智能指针) 8B (64bit machine)

Node 40B = 16(over) + 2×8(reference item, next Node) + 8(extra overhead)

Stack Integer 32+64N; 

32 = 16(over) + 8(reference) +4(int) +4(padding)

64 = 40(Node) + 24(Integer)

Array header 24B= 16(over) + 4(int length) +4(padding)

Array int 24+4N

Array object Date 24+8N(reference)+32N(Date)

Array two-dimensional二维 M×N double 24B+8M(row's reference)+24M(row's header)+8MN(double)

Array two-dimensional二维 M×N Object 24+32M+8MN

String 64+2N = 40+(24+2N) = 8(reference) + 3×4(3 int offset length hash) + 16(over) +4(padding) +(24+2N) (char array)

subString 40B reuse char[] 

## Sort

Any compare-based sorting algorithm must use at least lg ( N ! ) ~ N lg N compares in the worst-case.

### Merge

~~~java
public class Merge {
    private static Comparable[] aux;

    public static void sort(Comparable[] a) {
        aux = new Comparable[a.length];
        sort(a, 0, a.length - 1);
    }

    private static void sort(Comparable[] a, int lo, int hi) {
        if (hi <= lo)
            return;
        int mid = lo + (hi - lo) / 2;
        sort(a, lo, mid);
        sort(a, mid + 1, hi);
        merge(a, lo, mid, hi);
    }

    public static void merge(Comparable[] a, int lo, int mid, int hi) {
        int i = lo, j = mid + 1;

        for (int k = lo; k <= hi; k++)
            aux[k] = a[k];

        for (int k = lo; k <= hi; k++) {
            if (i > mid)
                a[k] = aux[j++];
            else if (j > hi)
                a[k] = aux[i++];
            else if (aux[j].compareTo(aux[i])<0)
                a[k] = aux[j++];
            else
                a[k] = aux[i++];
        }
    }
}
~~~

at most N lg N compares and 6 N lg N array accesses to sort any array of size N.

extra space proportional to N

Bottom-up Merge 

~~~java
public class MergeBU {
    private static Comparable[] aux;

    public static void sort(Comparable[] a){
        int N = a.length;
        aux = new Comparable[N];
        for(int sz = 1; sz < N; sz = sz+sz)
            for(int lo = 0; lo < N-sz; lo += sz+sz)
                merge(a,lo,lo+sz-1,Math.min(lo+sz+sz-1,N-1));
    }

    public static void merge(Comparable[] a, int lo, int mid, int hi) {
        int i = lo, j = mid + 1;

        for (int k = lo; k <= hi; k++)
            aux[k] = a[k];

        for (int k = lo; k <= hi; k++) {
            if (i > mid)
                a[k] = aux[j++];
            else if (j > hi)
                a[k] = aux[i++];
            else if (aux[j].compareTo(aux[i])<0)
                a[k] = aux[j++];
            else
                a[k] = aux[i++];
        }
    }
}

~~~



### Quick

内循环短：优于mergesort   shell sort，它们需要移动元素在内循环

更少的比较

average1.29NlgN

best NlgN