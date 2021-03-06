# AutoComplete

分析：

1. 实现Trie结构
2. 如果prefix不存在，返回
3. 存在的话：有子树和无子树
4. 有子树的话，从最后一个节点开始循环打印它的所有子树。用isLastNode判断。

```text
package indeed;
//Search for given query using standard Trie search algorithm.
//If query prefix itself is not present, return false to indicate the same.
//If query is present and is end of word in Trie, print query. 
// If last matching node of query has no children, return.
//Else recursively print all nodes under subtree of last matching node.
class TrieNode {
    // Initialize your data structure here.
    public TrieNode[] children;
    public boolean hasWord;

    public TrieNode() {
        children = new TrieNode[26];
        hasWord = false;
    }

    public void insert(String word) {
        TrieNode cur = this;
        for(int i = 0;  i < word.length(); i ++){
            int pos = word.charAt(i) - 'a';
            if(cur.children[pos] == null){
                cur.children[pos] = new TrieNode();
            }
            cur = cur.children[pos];
        }
        cur.hasWord = true;
    }

    public TrieNode find(String word) {
        TrieNode cur = this;
        for(int i = 0;  i < word.length(); i ++){
            int pos = word.charAt(i) - 'a';
            if(cur.children[pos] == null){
                return null;
            }
            cur = cur.children[pos];
        }
        return cur;
    }

    public boolean isLastWord(){
        TrieNode cur = this;
        for(int i = 0;  i < 26; i ++){
            if(cur.children[i] != null){
                return false;
            }
        }
        return true;
    }

}

public class AutoComplete {
    private TrieNode root;

    public AutoComplete() {
        root = new TrieNode();
    }

    // Inserts a word into the trie.
    public void insert(String word) {
        root.insert(word);
    }

    // Returns if the word is in the trie.
    public boolean search(String word) {
        TrieNode cur = root.find(word);
        return cur!= null && cur.hasWord;
    }

    // Returns if there is any word in the trie
    // that starts with the given prefix.
    public boolean startsWith(String prefix) {
        TrieNode cur = root.find(prefix);
        return cur!= null;
    }

    void printWord(TrieNode cur, String s){

        //父子都有重复动作 用递归
        if(cur.hasWord){
            System.out.println(s);
        }
        if(cur.isLastWord()){
            return;
        }
        for(char  i = 'a'; i <= 'z'; i ++){
            int pos = i - 'a';
            if(cur.children[pos] != null){
                printWord(cur.children[pos], s + i);
            }
        }
    }

    boolean printAutoSuggestions(String s){
        //先走到那个节点
        TrieNode cur = root.find(s);
        if(cur == null){
            return false;
        }
//非空的话递归打印
        if(!cur.isLastWord()){
            printWord(cur, s);
        }
        return true;
    }

    public static void main(String[] args){
        AutoComplete ac = new AutoComplete();
        ac.insert("hello");
        ac.insert("dog");
        ac.insert("hell");
        ac.insert( "cat");
        ac.insert("a");
        ac.insert( "hel");
        ac.insert( "help");
        ac.insert("helps");
        ac.insert("helping");

        boolean comp = ac.printAutoSuggestions("hel");

        if (comp)
            System.out.println("No other strings found with this prefix\n");
        else
            System.out.println("No string found with this prefix\n");
    }
}
```

