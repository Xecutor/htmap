# htmap

htmap is associative container based on 4-ary hash tree, interface compatible with unordered_map.
Performance-wise it's somewhere between std::map and std::unordered_map for complex keys.
Each insert/lookup costs at most one comparison of keys, unless collision is encountered.
For integer keys std::map is somewhat faster because comparison of ints is basically one cpu instruction.

There are 3 types of nodes in hash tree.
1. Inner node that consists of array of 4 pointers to other nodes.
2. Leaf node that contains key and value pair.
3. Leaf node that contains list of type 2 nodes, used in case of collision.

Lookup works like this:
calculate 32-bit hash code.
Pick lower 2 bits as index and shift remaining code to the left by two bits.
Now index array in inner node with that index.
Repeat until leaf node is encountered.
In case of normal leaf node just compare lookup key with the key in node.
In case of list leaf node, traverse the list and compare lookup key with the keys in list.
However, collision in hash tree means equality of two 32-bit hash codes.
Which is much much rares sutiation than in the hash table.

Leaf nodes in hash tree are combined into double linked list.
So, in addition to associative array sematnics it's also works as fifo container.
i.e. iteration over it gives you elements in order of insertion.
