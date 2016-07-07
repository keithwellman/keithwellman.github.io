---
layout: post
title:  "An interesting iteration to try"
date:   2016-07-06 14:13:20 -0400
---


While learning Java, I was tasked with counting the words in War in Peace with the requirements that I use HashSet and HashMap (Java), only count unique words, and count the number of times each word appears. Simple. Count the unique words. As this problem is expanded by an algorithmic set of rules, it becomes increasingly complex.

* Count each unique word only once
* Store every unique word in a hash containing the word and number of times it's used
* Don't count punctuation.
* Account for words that use a "-" to wrap to a new line
* Efficiently check if the word is already in the hash. Increment counter if it is, and if not add the word and set count to 1. 
* Try to think of edge cases.

These challenges weren't easy to overcome while using Java. The story my code told made little sense. If you're curious, here's the code:

```
/* Author: Keith Wellman
 * Date: 03-25-15
 * Purpose: to use HashSet and HashMaps to determine number of times words appear in a text file and output the top 5 words
 */

import java.util.*;
import java.io.*;

public class TopWords
{
  public static void main(String[] args) throws FileNotFoundException
  {
    
    int countHashSet = 0;
    int countHashMap = 0;
    int top = 0;
    
    String book = new Scanner( new File("FULLTEXT.txt") ).useDelimiter("\\A").next();
    String alphaOnly = book.replaceAll("[\",.'\\/*\\&^;!?(){}\\[\\]<>%\\t\\n\\r]+|- |[0-9]+","");
    PrintWriter out = new PrintWriter("FULLTEXTtemp.txt");
    String alphaOnly2 = alphaOnly.replaceAll("-","");
    out.println(alphaOnly2);
    out.close();
    
    
    ArrayList<SetList> mylist = readWords("FULLTEXTtemp.txt"); //stores the array of objects containing the set and map
    SetList mywords = mylist.get(0); //get the object out of the array
    SetList mymap = mylist.get(1); //get the object out of the array
    
    Set<String> wordSet = mywords.getSet(); //get the set out of the object
    Map<String, Integer> mapSet = mymap.getMap(); //get the map out of the object
    
    //count words in the hashset
    for (String word : wordSet)
    {
      countHashSet++;
    }
 
    //count words in the hashmap
    for (String key : mapSet.keySet())
    {
      countHashMap++;
    }
    
    myComparator comp =  new myComparator(mapSet);
    TreeMap<String, Integer> sortedmap = new TreeMap<String, Integer>(comp); //sort the map
    sortedmap.putAll(mapSet); // put the mapSet in the sortedmap
    
    // create iterator to shows the first 5 words and values in the sorted treemap
    Iterator<Map.Entry<String, Integer>> iter = sortedmap.entrySet().iterator();    
    
    String[] array = new String[5]; // arrays to hold the top 5 words and values
    
    Integer[] array1 = new Integer[5];
    for (int i = 0; i < 5; i++)
    {
      Map.Entry<String, Integer> entry = iter.next();
      array[i] = entry.getKey();
      array1[i] = entry.getValue();
      System.out.println(array[i] + " " + array1[i]);
      
    }
    
    javax.swing.JOptionPane.showMessageDialog(null, ("Number of words in HashSet: " + countHashSet + "\n" + 
                                                     "Number of words in HashMap: " + countHashMap + "\n" + "Top 5 words: " + 
                                                     "\n " + array[0] + "\n " + array[1] + "\n " + array[2] + "\n " + array[3] + 
                                                     "\n " + array[4]));
    
  }
  
  // reads the words from a file into a hashset and into a treemap with the number of times the word occurs. 
  public static ArrayList<SetList> readWords(String filename) throws FileNotFoundException
  {
    Set<String> words = new HashSet<String>();
    Map<String, Integer> wordMap = new TreeMap<String, Integer>();
      
    Scanner in = new Scanner(new File(filename));
    in.useDelimiter("[^a-zA-Z]+"); //disreguard irrelevant characters except a-z and A-Z that occur one or more times - remove all characters first before adding to the treemap
    while (in.hasNext())
    {
      
      String wordholder = in.next().toLowerCase();
      words.add(wordholder);
      
      if (wordMap.get(wordholder) == null) //if the word doesn't exsist in the map, add it
      {
        wordMap.put(wordholder, 0);
      }
      int n = wordMap.get(wordholder);
      wordMap.put(wordholder, n+1); // increase the value by 1 if it exsists in the map
      
    }
    in.close();

    // remove any word in the hashmap that occurrs only once (has a value of 1)
    wordMap.values().removeAll(Collections.singleton(1));
    
    SetList wordset = new SetList(words);
    SetList wordmap = new SetList(wordMap);
    ArrayList list = new ArrayList<SetList>();
    list.add(wordset);
    list.add(wordmap);
    
    return list; // returns an arraylist containing objects of type SetList for the set and map
  }
  
  public static class SetList // store map and set as objects that allow retrieval of the set and map
  {
    private Set<String> words;
    private Map<String, Integer> wordMap;
    
    public SetList(Set<String> words)
    {
      this.words = words;
    }
    public SetList(Map<String, Integer> wordMap)
    {
      this.wordMap = wordMap;
    }
    
    public Set<String> getSet()
    {
      return words;
    }
    public Map<String, Integer> getMap()
    {
      return wordMap;
    }
  }
  
  public static class myComparator implements Comparator<String> // implements the compare() method and returns -1 or 1 
  {
    Map<String, Integer> sortmap;
    public myComparator(Map<String, Integer> sortmap)
    {
      this.sortmap = sortmap;
    }
    
    public int compare(String a, String b)
    {
      if (sortmap.get(a) >= sortmap.get(b))
      {
        return -1;
      } 
      else 
      {
        return 1;
      } 
    }
  }
  
}
```

This kind of code almost killed by coding spirit. It's a challenge learning to think about code as abstract concepts and models. Add to that having to memorize syntactic requirements that have little meaning to any human reader. It's not an impossible task and one that I've fully embraced, however some languanges can be so much more. With Ruby, I can imagine my code as a model of things and how those things interact. This is why I enjoy writing code in languages like Ruby! The solution written in Ruby would be much more readable. 

Give it a try! I'll post my Ruby solution ... eventually. 


