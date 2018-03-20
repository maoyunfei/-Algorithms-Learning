<script type="text/javascript"
   src="http://cdn.mathjax.org/mathjax/latest/MathJax.js?config=TeX-AMS-MML_HTMLorMML">
</script>
## 查找给定值的k个最接近的元素

给定一个有序数组arr[]和一个值X，在arr[]中找到与X最接近的k个元素。请注意，如果元素存在于数组中，则不应该输出，只需要其他最接近的元素。

算法思路：

 * 首先用二分搜索找到最接近X的交叉点（交叉点之前的元素小于或等于X，之后的元素大于或等于X）。这一步需要$O\left( \log n\right)$次。
 * 一旦我们找到交叉点，我们可以比较交叉点两侧的元素来打印k个最接近的元素。这一步需要$O\left( k\right)$次。

 ```java
 // Java program to find k closest elements to a given value
public class KClosest {
    /* Function to find the cross over point (the point before
       which elements are smaller than or equal to x and after
       which greater than x)*/
    int findCrossOver(int arr[], int low, int high, int x) {
        // Base cases
        if (arr[high] <= x) // x is greater than all
            return high;
        if (arr[low] > x)  // x is smaller than all
            return low;

        // Find the middle point
        int mid = (low + high) / 2;  /* low + (high - low)/2 */

        /* If x is same as middle element, then return mid */
        if (arr[mid] <= x && arr[mid + 1] >= x)
            return mid;

        /* If x is greater than arr[mid], then either arr[mid + 1]
          is ceiling of x or ceiling lies in arr[mid+1...high] */
        if (arr[mid] < x)
            return findCrossOver(arr, mid + 1, high, x);

        return findCrossOver(arr, low, mid - 1, x);
    }

    // This function prints k closest elements to x in arr[].
    // n is the number of elements in arr[]
    void printKclosest(int arr[], int x, int k, int n) {
        // Find the crossover point
        int l = findCrossOver(arr, 0, n - 1, x);
        int r = l + 1;   // Right index to search
        int count = 0; // To keep track of count of elements
        // already printed

        // If x equals to arr[l], then reduce left index
        while (l >= 0 && arr[l] == x) l--;
        // If x equals to arr[r], then increase right index
        while (r < n && arr[r] == x) r++;


        // Compare elements on left and right of crossover
        // point to find the k closest elements
        while (l >= 0 && r < n && count < k) {
            if (x - arr[l] < arr[r] - x)
                System.out.print(arr[l--] + " ");
            else
                System.out.print(arr[r++] + " ");
            count++;
        }

        // If there are no more elements on right side, then
        // print left elements
        while (count < k && l >= 0) {
            System.out.print(arr[l--] + " ");
            count++;
        }


        // If there are no more elements on left side, then
        // print right elements
        while (count < k && r < n) {
            System.out.print(arr[r++] + " ");
            count++;
        }
    }

    /* Driver program to check above functions */
    public static void main(String args[]) {
        KClosest ob = new KClosest();
        int arr[] = {1, 2, 2, 2, 5, 6};
        int n = arr.length;
        int x = 2, k = 2;
        ob.printKclosest(arr, x, k, n);
    }
}
 ```
时间复杂度：$O\left( \log n+k\right)$