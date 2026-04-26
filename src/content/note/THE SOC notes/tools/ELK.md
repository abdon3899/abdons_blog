---
title: "ELK"
description: "My personal SOC notes and research on ELK."
publishDate: "2026-04-19T00:00:00Z"
tags: ["tools"]
---

# ELK

**Kibana Query Language (KQL)**:

A user-friendly query language developed by Elastic specifically for Kibana.

- Features:
    
    Autocomplete suggestions.
    
    Supports filtering using various operators and functions.
    

**Lucene Query Syntax**:

A more powerful query language powered by the open-source Lucene search engine library.

- Features:
    
    More advanced and flexible than KQL.
    
    Harder to learn for beginners.
    
    Used as the backend for Elasticsearch.
    

**Choosing Between KQL and Lucene**:

- The choice depends on the situation and the type of data being searched.
- In practice, you may switch between the two depending on the complexity of the query.

### Special Characters :

Reserved Characters: +, -, =, &&, ||, &, |, ! , must be escaped to avoid errors. Here’s how to handle them: Precede the reserved character with a backslash (\).
Example: Searching for User+1 in the username field:
Incorrect: username:User+1 (will result in an error).
Correct: username:User\+1.

### Wildcards :

 used to match specific characters within a field value. They are useful for flexible searching:

***  Wildcard:** Matches any number of characters (including zero).
Example:
Searching for monit* in the product_name field will match: monitor, monitors , monitoring

**?  Wildcard:**
Matches a single character.
Example:
Searching for J?n in the name field will match: Jan, Jen, Jon

### **Range Query Syntax**

To search for documents where a field's value falls within a specific range, use the following operators:

`>=` (greater than or equal to)

`<=` (less than or equal to)

`>` (greater than)

`<` (less than)

### **Fuzzy Query:**

a technique used to find documents with typos, inconsistencies, or slight variations in the data by allowing a specified number of character differences (called the **fuzziness value**) between the search term and the actual field value. 

To perform a fuzzy search, use the `~` character followed by the **fuzziness value**. The format is: `field_name:search_term~fuzziness_value`

**`field_name`**: The field to search in.

**`search_term`**: The term to search for.

**`fuzziness_value`**: The number of allowed character differences (e.g., 1 or 2).

For example, searching for `server~1` can match terms like `serber` or `server1`

### **Proximity Search:**

allows you to find documents where two or more specified terms appear within a certain distance (measured in words) of each other in a field. This is useful for locating phrases or terms that are close together but not necessarily adjacent.

In KQL, you use the `match_phrase` query with the `slop` parameter to perform a proximity search. The `slop` value defines the maximum number of words that can separate the terms.

`field_name:"term1 term2"~slop_value`

**`field_name`**: The field to search in.

**`term1 term2`**: The terms you want to search for.

**`slop_value`**: The maximum allowed distance (in words) between the terms.