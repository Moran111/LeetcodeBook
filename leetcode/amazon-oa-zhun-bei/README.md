# Amazon准备

## F**etch Items To Display**

Given a list of items, the item includes (name, relevance, price). They want to ask how many item will be displayed in given page. Given one page can display n items and they want to sort the item by **(0: ascending, 1:descending).** SortParameter means the item should be sorted by which attribute. SortParamerter = 1 means sorted by relevance.&#x20;

```
Sample Input:
numOfItems = 3
items: ["item1",10,15],["item2",3,4],["item3",17,8]
sortParameter: 1
sortOrder: 0 
itemsPerPage: 2
pageNumber: 1
Output:
{"item3"}


public List<String> fetchItemsToDisplay(
int numOfItems, 
Map<String, Pair<Integer, Integer>> items, 
int sortParameter,
int sortOrder,
int itemsPerPage,
int pageNumber) {
    // 1. having an Item class and a List<Item> 
    List<int[]> itemList = new ArrayList<>();
    
    // 2. based on sortParameter to sort the Item in sort Order
    if (sortOrder == 0) {
        
    }
    
    List<String> res = new ArrayList<>();
    
    // we want to find ith item
    int itemPosition = pageNumber * itemsPerPage;
    
    // ith item in index i-1
    int itemIndex = itemPosition - 1;
    for (int i = itemIndex; i < itemIndex + itemPerPage; i++) {
        res.add(list.get(itemPosition - 1)[0]);
    }
    return res;
}

private void sortItem(int sortParameter, int sortOrder) {
    if (sortOrder == 0) {
        // sort in ascending order
        Collection.sort(itemList, (a, b) -> a[sortParamter] - b[sortPramater]);
    } else {
        Collection.sort(itemList, (a, b) -> b[sortPramater] - a[sortParamter]);
    }
}
```

## Number of Combinations

## Optimize Memory Usage

