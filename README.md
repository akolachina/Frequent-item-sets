# Frequent-item-sets
Python code to build frequent item sets from a transactions database where each line represents a transaction of items. Each item is represented as an integer and items are separated by space. A line that looks like: ‘1001 1002 1003' represents a single transaction consisting of products with SKUs 1001, 1002 and 1003.

# Instructions to run the code


clone the repo: git clone https://github.com/akolachina/Frequent-item-sets.git

Open *"Frequent Itemsets-Ver4.ipynb"* in Jupyter Notebook

Change the data set path to directory where you clone

Change the final output path to directory where you clone

Restart and run all cells



# Efficiency considerations

1.I read the entire data set into main memory to speed up the implementation. However, if the data set is huge, we could read one line at a time from disk, process to count frequency then go back and read next line.

2.The time taken upto identification of frequent quadruples is within 3 mins on local machine. For five and over, it takes longer. The biggest time consumption here is to look through all candidate quads for each five group and check if they are a frequent quad. If we had a cluster and memory is not a constraint, this check could be avoided to speed up the algorithm.

3. The code here is in a PoC mode, in the interest of time. If the code had to be productionized and scaled up to build frequent itemsets from bigger databases and of different support levels, I would 
* use *itertools.combinations(candidate_list, itemset_size)* to replace the nested *for-loops* in identifying candidate tuples
   * for ex:
   *for p in range(len(candidate_five_single)-4):
   
                for q in range(p+1,len(candidate_five_single)-3):
                
                    for r in range(q+1, len(candidate_five_single)-2):
                    
                        for l in range(r+1,len(candidate_five_single)-1):
                        
                            for m in range(r+1,len(candidate_five_single)):
                            
                                a = (candidate_five_single[p],candidate_five_single[q],candidate_five_single[r],candidate_five_single[l],candidate_five_single[m] )*
    may be replaced by
    
    *for a in itertools.combinations(candidate_five_single,5): 
        ""*
* Create a function to identify candidate itemsets. The arguments to function would be a list of items and the item size. The function would return candidate tuples based on itemsize, using the *itertools.combinations()* feature described above

* Wrap the functions in a while loop, so that the frequent itemset sizes of continuously bigger sizes are identified untill no more frequent itemsets are available.

  * pseudo code:
     
     i = 1
     build frequent itemset of size i
     
       while (frequent itemset of size i is not null):
       
            i+=1
            
            build frequent itemset of size i
       
     

                                
                                






# Algorithm implemented:

I leveraged the apriori algorithm to implement frequent item set identification. The algorithm runs like this:
1. Get a dump of the entire data set into main memory in a dictionary called *raw_data*
(We are able to get entire data due to small size of the data. If it were bigger, 
 we could just read one line at each pass, which would be a little bit slower 
 but more efficient in terms of memory)
 2. Capture the *no of transactions each item* isin  in a dictionary called  *single_count*
 3. Prune single_count based on the support_level, to identify individual items that are in
 more transactions than support_level. Capture them in *single_freq_items* dictionary
 4. Also, build a dictionary called *individual_items_count*. The keys of this dictionary are individual items in the transactions. Value of the key is a list. Each element of the list represents number of frequent item sets the key (individual item) is in.
 for ex, **individual_items_count['0'] is [67, 23, 27, 12, 2, 0, 0, 0]**. The way to read this is: item '0' is in 67 transactions, 23 frequent_item_pairs, 27 frequent_item_triples, 12 frequent_item_quadruples, etc
 
 **To calculate frequency of pairs:**
 1. For an item to be a frequent pair, it should be a frequent individual item.
 2. For all frequent individual items, build a candidate pair.
 3. Count number of transactions the candidate pair isin into **double_count** dictionary
 4. Prune double_count based on support_level to capture frequent pairs in **double_freq_items**
 5. Capture the number of pairs each item is in as the second element of individual_items_count
 
 **To calculate frequency of triples**
 1. For an item to be in a frequent triple, it should be in *atleast two frequent pairs*   
 **and ALL** the pairs that can be formed from triple should be a frequent pair
 2. Build candidate triples that satisfy this condition
 3. Prune triples based on support_level, to get frequent triples
 and so on for higher levels






































Efficiency considerations

1.I read the entire data set into main memory to speed up the implementation. However, if the data set is huge, we could read one line at a time from disk, process to count frequency then go back and read next line.


2.The time taken upto identification of frequent quadruples is within 3 mins on local machine. For five and over, it takes longer. The biggest time consumption here is to look through all candidate quads for each five group and check if they are a frequent quad. If we had a cluster and memory is not a constraint, this check could be avoided to speed up the algorithm.

